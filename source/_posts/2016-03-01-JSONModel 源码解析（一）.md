---
title: JSONModel 源码解析（一）
date: 2016-03-01
---

## Property Protocols
- Ignore
- Optional
- Index
- ConvertOnDemand (仅对 NSArray 有效）

## AbstractJSONModelProtocol
- initWithDictionary:error:
- initWithData:error:
- toDictionary
- toDictionaryWithKeys:

## JSONModel
实现了 `AbstractJSONModelProtocol` 和 `NSSecureCoding` 协议。
增加了对于 NSString 类型的 JSON 字符串解析支持:
- initWithString:error:
- initWithString:usingEncoding:error:
- toJSONString
- toJSONStringWithKeys:
- toJSONData
- toJSONDataWithKeys:

对于 JSONModel 构成的数组，还增加了批量方法，用于循环解析输入数组，输出包含解析之后的 Model 的数组
- arrayOfModelsFromDictionaries:
- arrayOfDictionariesFromModels:

比较 Model 的方法：
- indexPropertyName 输出索引字段名称
- isEqual: 比较索引字段是否相等
- compare: 如果没有索引字段或者索引字段不能比较，则抛出异常，`NSString` 和 `NSNumber` 可比较，如果是自定义类型，要求实现 `compare:` 方法

验证 Model 有效性：
- validate:
	重写此方法，能够支持有效性验证，如果检查出无效情况，返回 NO，并且设置 error 引用参数
- keyMapper
	重写此方法，如果属性名称不对应 JSON key names
- setGlobalKeyMapper:
	影响所有 JSONModel 对象解析，自定义 keyMapper 的优先级高于全局 keyMapper
- propertyIsOptional:
- propertyIsIgnored:
- protocolForArrayProperty:
	获取 NSArray 对象定义的元素实现的协议
- mergeFromDictionary:useKeyMapping:
	将 dict 包含的值合并到 model 实例，useKeyMapping 标识是否使用自定义 keyMapper 和 全局 keyMapper

