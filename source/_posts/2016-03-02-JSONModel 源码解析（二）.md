---
title: JSONModel 源码解析（二）
date: 2016-03-02
---

Header 引入文件列表
```objectivec
	#import <objc/runtime.h
	#import <objc/message.h>


	static const char * kMapperObjectKey; // 自定义 keyMapper
	static const char * kClassPropertiesKey; // 
	static const char * kClassRequiredPropertyNamesKey; // 
	static const char * kIndexPropertyNameKey; // 索引字段名称

	allowedJSONTypes
	allowedPrimitiveTypes
	valueTransformer
	JSONModelClass

	globalKeyMapper
```


## allowedJSONTypes
包含 NSString, NSNumber, NSDecimalNumber, NSArray, NSDictionary, NSNull, NSMutableString, NSMutableArray, NSMutableDictionary

## allowedPrimitiveTypes
BOOL, float, int, long, double, short, NSInteger, NSUInteger, Block

## JSONValueTransformer
隐式值类型转换

## JSONModelClass

根据 `kClassPropertiesKey` 判断是否需要扫描 model 属性
根据 `kMapperObjectKey` 保存自定义 keyMapper

JSONModelErrorDomain，包含几种类型错误：
1. kJSONModelErrorModelIsInvalid
2. kJSONModelErrorBadJSON
3. kJSONModelErrorNilInput
4. kJSONModelErrorInvalidData

最终，都是经过 `initWithData:error:` 处理，由 `NSJSONSerialization` 解析成 NSDictionary，再中转到 `initWithDictionary:error:` 处理。

1. 检查输入数据结构 `__doesDictionary:matchModelWithKeyMapper:error:`
2. 输入的 NSDictionary 导入值 `__importDictionary:withKeyMapper:validation:error:`
3. 执行数据校验 `validate:`

### `__requiredPropertyNames` 
返回 model keys 集合，NSMutableSet 类型  
通过 `kClassRequiredPropertyNamesKey` 属性获得包含的 keys 集合，懒加载方式，首先判断是否获取过，不然遍历 `__properties__` 方法返回的数组，判断 `isOptional` 属性值，添加 `p.name` 到 keys 集合  

### `__properties__` 
获取所有的 keys 数组的方式，依赖属性扫描过程 `__inspectProperies`，保存到 `kClassPropertiesKey` 动态属性  

### `__inspectProperties`
通过 NSScanner 方式检查  
从子类开始查找，向上遍历，判断条件是 `class != [JSONModel class]`  
用到了一个 runtime.h 里的方法 `class_copyPropertyList`
	unsigned int propertyCount;
	objc_property_t *properties = class_copyPropertyList(class, &propertyCount);

然后，从 `objc_property_t` 类型，通过 `property_getName` 和 `property_getAttributes` 方法，提取出 propertyName 和 propertyAttributes，封装成 `JSONModelClassProperty`。  

#### 具体策略：
1. 过滤到 *R* 属性的只读属性
2. 检查 *T* 的具体类型，区分三种类型，保存到 propertyType 变量里:
	1. NSObject  
		例如：T@"NSString",&,N,V_s1  
		首先扫描属性名称，其次扫描协议名称，协议判断是否属于 Optional, Index, ConvertOnDemand, Ignore 等协议
	2. structure  
		如果检查到存在 structure，则标识为非标准 JSON 类型，并将结构名保存到 structName
	3. primitive  
		通过 `valueTransformer.primitivesNames` 字典里获取 primitive 值对应的 propertyType，并判断是否在 `allowedPrimitiveTypes` 字典，否则抛出 **NSException**
3. 判断属性是否 `propertyIsOptional`
4. 判断属性是否 `propertyIsIgnored`
5. 判断属性如果是 NSArray，元素是否对应于协议 `protocolForArrayProperty`
6. 判断属性是否是 Block  

### `__doesDictionary:matchModelWithKeyMapper:error:`
检查输入数据的结构，依赖于 dict 和 keyMapper
dict 是经过 NSJSONSerialize 处理之后待解析的 NSDictionary 类型
经过 keyMapper 转换过属性名之后，存在 `@try/@catch` 过程，步骤如下：
1. 试图解析 `valueForKeyPath:`
	2. 试图解析 `valueForKey:`
	3. 如果值 value 存在，则保存到 transformedIncomingKeys
	4. 如果 requiredProperties 包含的属性值 **多于** incomingKeys，则将多余的属性名从 requiredProperties 里移除，并返回错误 NSError，标明 Keys missing

### `__mapString:withKeyMapper:importing:`
用于 JSON 和 Model 之间属性名之间映射转换，依赖于 keyMapper，存在优先级，如果自定义 keyMapper 和全局 keyMapper 同时存在，如果先采用自定义 keyMapper 转换之后属性名一样，则接着采用全局 keyMapper 方式继续转换

#### keyMapper
双向的数据结构映射组件，用于 JSON 和 Model 之间的属性名映射，可以通过重写 `keyMapper` 方法指定自定义 keyMapper，还可以设置全局 keyMapper

`typedef NSString* (^JSONModelKeyMapBlock)(NSString* keyName);`
主要就是这个 Block 来实现字段名映射

同样是通过字典方式缓存映射 Map，包含 toModelMap 和 toJSONMap，而且构造函数里包含 JSONToModelBlock 和 ModelToJSONBlock

如果通过 `initWithDictionary:` 初始化方法构造的话，toModelMap 采用入参 map，toJSONMap 采用 swappedMap

`convertValue:isImportingToModel:` 方法，则直接通过 Block 求值返回

`mapperFromUnderscoreCaseToCamelCase` 方法，用于构造 Underscore Case转到 Camel Case

`mapperFromUpperCaseToLowerCase` 方法，用于构造 Upper Case 转到 Lower Case

### `__importDictionary:withKeyMapper:validation:error:`
1. 通过 keyMapper 转换成 jsonKeyPath
	2. 试图解析 `valueForKeyPath:`
	3. 试图解析 `valueForKey:`
	4. 判断是否属于 isNull
	5. 如果是 Null，判断是否可选或是否待验证 validation
	6. 如果错误，直接返回 NSEror，标明 Value of required model key xxx is nul
	7. 获取 jsonValueClass，判断 jsonValueClass 是否属于 allowedJSONTypes 元素的子类之列
	8. 如果错误，直接返回 NSError，标明 Type xxx is not allowed in JSON
	9. 上述步骤都是检查步骤，下面开始走赋值步骤
	10. **判断是否有自定义赋值方法，命名方式为 `set(PropertyName)With(NSString|NSNumber|NSArray|NSDictionary|NSDate)`，源类名要根据值类型来指定**
	11. 如果 type == nil 并且 structName == nil，则 为 primitives，直接赋值为 jsonValue
	12. 如果 Null，直接赋值为 nil
	13. 如果是 JSONModel 子类，递归解析，调用 `initWithDictionary:error:`
	14. 如果有协议指定，转而调用 `__transform:forProperty:error:`
	15. 如果是标准 JSON 类型且 jsonValue 是 type 类型，直接赋值，如果是 mutable 则赋值之前先 mutableCopy
	16. 如果 jsonValue 不是 type 类型，且 jsonValue 不为空，或者是 mutable，或者是 structure，那么黑科技来了。首先，获取 jsonValue 类型对应的类型，将 Cluster 子类型转换为标准类型；拼装 selectorName，格式为 `(PropertyName)From(SourceClass)`，如果指定了此方法，就用此方法，如果没有指定，则试图查找 `__(selectorName)` 方法是否存在，如果存在使用找到的方法转换值，否则返回 NSException，标明是 Type not allowed
	17. 其他情况就直接赋值

### `__transform:forProperty:error:`
1. 如果指定的协议不存在，当 NSArray 类型时就抛出异常，其他则直接返回
2. 如果指定的协议属于 JSONModel 子类
	1. 如果是 NSArray 类型且 value 类型也是数组，则根据是否需要 convertsOnDemand 选择走不同的逻辑，如果需要 convertsOnDemand，通过 JSONModelArray 对象初始化，经过 JSONModelArray 封装之后，可以实现需要访问子元素的时候再执行解析过程；如果不需要则通过 `arrayOfModelsFromDictionaries` 方法，一次性初始化完成，还是通过循环遍历方式初始化
	2. **如果是 NSDictionary 类型，则遍历 keys，将 value 作为协议类型执行初始化。这样就可以如果 NSArray 指定协议方式来扩展实现 NSDictionary**

## JSONValueTransformer
如果属性 isStandardJSONType == YES 时，不需要执行任何 valueTransform，直接赋值。
是否标准 JSON 类型，通过属性类型是否在 `allowedJSONTypes` 数组里来判断。
初始化过程中定义了 **Declared property type encodings** 缩写跟实际变量名对应字典，包含如下：
	{@"f":@“float",@"i":@"int", @"d":@"double", @"l":@"long", @"c":@"BOOL", @"s":@"short", @"q":@"long", @"I":@"NSInteger", @"Q":@"NSUInteger", @"B":@"BOOL", @?":@"Block"}

### `classByResolvingClusterClasses`
通过 sourceClass 来归一化输入类的类型判断，检查各种类型的变种，用于类簇子类型的类型判断，主要用 `isSubclassOfClass:` 方法判断。

另外，包含了许多的 NSObject 基本类型的之间相互转换，虽然看起来没有直接用到，但是在 JSONModel 的 `toDictionary` 方法里，如果需要使用 valueTransformer 的时候，将动态执行 valueTransformer 里定义的 `xxxFromYYY` 方法。

> 为什么要进行JSONValueTransformer的转换呢，是因为在iOS的视线中，由于抽象工厂的存在，构建了大量的簇类，比如NSArray, NSNumber, NSDictionary等等，他们只是对外暴露的一层皮，实质上底层对于真正的类。比如NSArrayI \<=\> NSArray等等。因此，我们需要通过JSONValueTransformer得到真正的Class Type，同时通过class Type找到最合适的转换方法，在JSONValueTransformer.m的文件中，我们能找到一大堆xxxFromYYY的函数。


### Declared property type encodings
| Code | Meaning |
| --- | --- |
| R | The property is read-only (readonly). |
| C | The property is a copy of the value last assigned (copy). |
| & | The property is a reference to the value last assigned (returnain). |
| N | The property is non-atomic (nonatomic). |
| G\<name\> | The property defines a custom getter selector name. The name follows the G (for example, GcustomGetter,). |
| S\<name\> | The property defines a custom setter selector name. The name follows the S (for example, ScustomSetter:,). |
| D | The property is dynamic (@dynamic). |
| W | The property is a weak reference (`__weak`). |
| P | The property is eligible for garbage collection. |
| t\<encoding\> | Specifies the type using old-style encoding. |
