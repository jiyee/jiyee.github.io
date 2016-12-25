---
title: JSONModel 最佳实践
date: 2016-03-05
---

- 属性类型选用 NS 类型，避免选用基本数据类型（Primitive Types）
	> 基本数据类型在解析时直接调用的是 `setValue:forKey:` 方法，如果传入类型不一致的值，会抛出异常。
- 属性协议合理选用
	- 有条件地设置 `<Optional>`，尽量避免设置全部属性均为 Optional
	- 对自定义属性设置 `<Ignore>`，避免解析覆盖
	- 对索引属性设置 `<Index>`，保证值唯一，便于比较
	- 对解析耗时的 NSArray 或者 NSDictionary 类型属性设置 `<convertsOnDemands>`，延时解析
- 设置统一的 KeyMapper，并结合 `mapper:withExceptions:` 方法添加例外条件
- `-initWithDictionary:error:` 用于设置 `<Ignore>` 类型属性的初始值，或者设置属性的默认值，或者对经过解析之后的属性值进行值变换（避免类型变换，类型变换通过自定义 setter 方法完成）
- `-validate` 用于检验经过解析之后的 Model 是否在业务逻辑上正常
- 对于需要类型转换的属性，如果 JSONValueTransformer.h 里没有定义兼容转型方法，可以设置自定义 setter 方法，格式如 `setXXXWithYYY:`（ XXX 是属性名称，YYY 是 JSON 类型名）。如果需要从 Model 转回 JSON 的话，需要同时设置自定义 getter 方法，格式如 `JSONObjectFromXXX:`
- 全局性类型兼容转型，通过扩展 JSONValueTransformer 类实现，设置 `XXXFromYYY:` 方法（ XXX 是 Model 类型名，YYY 是 JSON 类型名）
- 空置或者解析失败处理
	> 所有 NSObject 类型的属性如果设置了 Optional，其属性值都可能为 nil，允许直接判断 `value == nil`。经过 JSONModel 成功解析之后的属性值不可能是 `NSNull` 类型。如果 JSON 值是 null，则一定为 nil。
- 如果是 NSDictionary 类型，则遍历 keys 和 values，将 value 作为协议类型执行解析，如同 NSArray 方式解析
- BOOL, NSInteger, ENUM 等基本数据类型（Primitive Types），如果传入 NSArray, NSDictionary 等非预期类型值的时候，都将抛出异常。
	> 例如，属性 enabled 是一个映射到 JSON 的 BOOL 值，为了兼容 1/0/"1"/"0"/true/false/"true"/"false" 等常见 BOOL 设定值，同时，想防止传入其他值导致异常。
	> 建议按照如下方式：
		
```objectivec
		@property (nonatomic, assign) BOOL enabled;
		@property (nonatomic, strong) NSNumber *__numberOfEnabled;
```
		

KeyMapper 设置为 `@"enabled" : @"__numberOfEnabled"`

在 initWithDictionary:error: 方法
	
```objectivec
	self.enabled = NO;
	self = [super initWithDictionary:dict error:error]; 
	
	self.enabled = self.__numberOfEnabled.boolValue;
```
	