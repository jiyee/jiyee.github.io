---
title: NSJSONSerialization 
date: 2015-03-10
---

# NSJSONSerialization，JSON数据解析API
---

JSON和Foundation objects之间相互转换的API, 支持ARC, iOS 5+

JSON文档的要求：所有的key都是双引号，不能使用单引号或没有引号

object的要求：

1. 顶级节点的数据类型必须是 \_\_NSArray\_\_ 或者 \_\_NSDictionary\_\_
2. 所有节点必须是以下实例： \_\_NSString__, __NSNumber__, __NSArray__, __NSDictionary__, or __NSNull\_\_
3. 所有节点的key必须是 \_\_NSString\_\_
4. __Numbers__ 不能是 __NaN__ 或者 __infinity__

`isValidJSONObject:`，判断是否可以正确转换

`JSONObjectWithData:options:error:`，A Foundation object from the JSON data in data, or __nil__ if an error occurs.
JSON 文档的字符编码只能是 UTF 系

`NSJSONReadingOptions` 包含三种类型：

- NSJSONReadingMutableContainers，返回可变容器，\_\_NSMutableDictionary\_\_ 或 \_\_NSMutableArray\_\_
- NSJSONReadingMutableLeaves，叶节点都转成 \_\_NSMutableString\_\_，目前在 iOS 7 上测试不好用，应该是个 bug，参见：- [http://stackoverflow.com/questions/19345864/nsjsonreadingmutableleaves-option-is-not-working ](http://stackoverflow.com/questions/19345864/nsjsonreadingmutableleaves-option-is-not-working%20)
- NSJSONReadingAllowFragments，允许顶层节点不是 \_\_NSArray\_\_ 或者 \_\_NSDictionary\_\_，但必须是有效的 JSON Fragment，如解析 `@"123"` 这样的字符串。
	> That’s because 32 is a valid JSON fragment (a number), but abcd is not a valid JSON fragment since all strings must be quoted.
	> `NSString *num=@"32";`
	> `NSString *num=@"\"abcd\"";`


`JSONObjectWithStream:options:error:`，通过数据流解析
\_\_NSInputStream\_\_ 数据流必须是可被打开和配置。

`dataWithJSONObject:options:error:`，创建 JSON data
如果对象不能正确生成 JSON data，将抛出一个 programming error，并非 internal error，This exception is thrown prior to parsing and represents a programming error。

而 API 里设定的 \_\_error\_\_，If an internal error occurs, upon return contains an NSError object that describes the problem.

所以，在生成 JSON data 之前，需要检查是否能够正确 `isValidJSONObject:`

`NSJSONWritingOptions`，
- NSJSONWritingPrettyPrinted，指定输出 JSON data 是否执行 pretty print.
