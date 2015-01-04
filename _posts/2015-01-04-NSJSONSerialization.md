---
layout: page
title: NSJSONSerialization 
---

# NSJSONSerialization，iOS自带JSON数据解析API
---

JSON和Foundation objects之间相互转换的API, 支持ARC, iOS 5+

JSON文档的要求：所有的key都是双引号，不能使用单引号或没有引号

object的要求：

1. 顶级节点的数据类型必须是__NSArray__或者__NSDictionary__
2. 所有节点必须是以下实例：__NSString__, __NSNumber__, __NSArray__, __NSDictionary__, or __NSNull__
3. 所有节点的key必须是__NSString__
4. __Numbers__不能是__NaN__或者__infinity__

`isValidJSONObject:`，判断是否可以正确转换

`JSONObjectWithData:options:error:`，A Foundation object from the JSON data in data, or __nil__ if an error occurs.
JSON文档的字符编码只能是UTF系

`NSJSONReadingOptions`包含三种类型：

- NSJSONReadingMutableContainers，返回可变容器，__NSMutableDictionary__或__NSMutableArray__
- NSJSONReadingMutableLeaves，叶节点都转成__NSMutableString__，目前在iOS 7上测试不好用，应该是个bug，参见：- [http://stackoverflow.com/questions/19345864/nsjsonreadingmutableleaves-option-is-not-working ](http://stackoverflow.com/questions/19345864/nsjsonreadingmutableleaves-option-is-not-working%20)
- NSJSONReadingAllowFragments，允许顶层节点不是__NSArray__或者__NSDictionary__，但必须是有效的JSON Fragment，如解析`@"123"`这样的字符串。
	> That’s because 32 is a valid JSON fragment (a number), but abcd is not a valid JSON fragment since all strings must be quoted.
	> `NSString *num=@"32";`
	> `NSString *num=@"\"abcd\"";`


`JSONObjectWithStream:options:error:`，通过数据流解析
__NSInputStream__数据流必须是可被打开和配置。

`dataWithJSONObject:options:error:`，创建JSON data
如果对象不能正确生成JSON data，将抛出一个programming error，并非internal error，This exception is thrown prior to parsing and represents a programming error。

而API里设定的__error__，If an internal error occurs, upon return contains an NSError object that describes the problem.

所以，在生成JSON data之前，需要检查是否能够正确`isValidJSONObject:`

`NSJSONWritingOptions`，
- NSJSONWritingPrettyPrinted，指定输出JSON data是否执行pretty print.




