---
title: iOS 代码格式化 clang-format 使用说明
date: 2016-04-10
---

在工程目录下创建 _clang-format 文件，是 clang-format 代码格式化工具的配置文件。
使用该格式化配置文件需要安装Xcode插件：BBUncrustifyPlugin-Xcode，也可通过 Xcode 插件管理器 Alcatraz 搜索进行安装。或者安装另一款插件 ClangFormat-Xcode。

以下对 _clang-format 文件配置进行逐行解释：

- AccessModifierOffset: 0
类的访问修饰关键字(private,public,protected···)缩进
- AlignAfterOpenBracket: true
在未封闭(括号的开始和结束不在同一行)的括号中的代码是否对齐
- AlignEscapedNewlinesLeft: false
在(),[],{}中代码不少于一行且换行情况下。如果true则，第二行起代码会尽量向左对齐，否则向最右边对齐
- AlignOperands: true
如果为true，水平对齐二元和三元表达式的操作数。
- AlignTrailingComments: true
如果为true，对齐各行尾部注释
- AllowAllParametersOfDeclarationOnNextLine: true
如果为true，函数申明多个参数时，允许换行
- AllowShortBlocksOnASingleLine: false
如果true，较短的代码片段允许格式化为一行
- AllowShortCaseLabelsOnASingleLine: false
是否允许短switch的case 语句在一行写完
- AllowShortIfStatementsOnASingleLine: false
是否允许短if else语句在一行写完
- AllowShortLoopsOnASingleLine: false
是否允许短的循环在一行写完
- AllowShortFunctionsOnASingleLine: All
是否允许短的方法实现在一行写完
- AlwaysBreakAfterDefinitionReturnType: false
定义函数返回类型之后换行
- AlwaysBreakTemplateDeclarations: false
定义模板之后换行
- AlwaysBreakBeforeMultilineStrings: false
多行字符串之前换行
- BreakBeforeBinaryOperators: None
二元操作符之前换行
- BreakBeforeTernaryOperators: true
三元操作符之前换行
- BreakConstructorInitializersBeforeComma: false
在构造函数初始化时按逗号断行，并以冒号对齐
- BinPackParameters: true
如果false，函数的定义或声明的参数要么是全部占同一行，要么一个参数占一行
- BinPackArguments: true
如果false，函数调用时的参数要么是全部占同一行，要么一个参数占一行
- ColumnLimit: 160
一行代码长度的限制，0为无限制
- ConstructorInitializerAllOnOneLineOrOnePerLine: false
如果true，构造函数的初始化无法适应于一行以内，那么每个参数占一行
- ConstructorInitializerIndentWidth: 4
构造函数初始化的缩进值
- DerivePointerAlignment: false
如果true，将用使用PointerAlignment的值为指针类型进行格式化
- ExperimentalAutoDetectBinPacking: false
如果true，clang-format检测方法的定义和调用是否被格式化为一个参数占据一行
- IndentCaseLabels: false
case语句的位置总是在switch语句后缩进一级
- IndentWrappedFunctionNames: false
- IndentFunctionDeclarationAfterType: false
If true, indent when breaking function declarations which are not also definitions after the type
- MaxEmptyLinesToKeep: 2
持续空行的最大数量
- KeepEmptyLinesAtTheStartOfBlocks: true
如果true，在一块代码前的空行将会被保留
- NamespaceIndentation: None 
namespace的缩进
NI_None 所有的namespace均不缩进
NI_Inner 只在内部namespace缩进
NI_All 所有的namespace缩进
- ObjCBlockIndentWidth: 4
OC的block缩进宽度
- ObjCSpaceAfterProperty: false
OC的property后空格是否存在，如果true，那么@property(readonly) 代替 @property (readonly)
- ObjCSpaceBeforeProtocolList: true
OC中协议列表前的空格是否存在，如果true，那么 Person 代替 Person
- PenaltyBreakBeforeFirstCallParameter: 19
- PenaltyBreakComment: 300
- PenaltyBreakString: 1000
- PenaltyBreakFirstLessLess: 160
- PenaltyExcessCharacter: 1000000
最多能超出ColumnLimit多少个字符
- PenaltyReturnTypeOnItsOwnLine: 60
- PointerAlignment: Right
指针在类型那边还是在变量名那边还是在中间
- SpacesBeforeTrailingComments: 1
单行注释前的空格数
- Cpp11BracedListStyle: true
如果true，将大括号的列表格式化为最适合C++11的格式
- Standard: Cpp11
按照改Cpp11格式化 A> 代替 A >
- IndentWidth: 4
缩进的列数
- TabWidth: 4
制表位列数
- UseTab: Never
是否使用tab进行缩进
- BreakBeforeBraces: Attach
括号的断行模式，此处为括号紧贴代码片段
- SpacesInParentheses: false
如果true，在非空的括号中插入空格
- SpacesInSquareBrackets: false
如果true，[]中间插入空格
- SpacesInAngles: false
如果true，在<>中间插入空格
- SpaceInEmptyParentheses: false
如果true 空括号中加空格
- SpacesInCStyleCastParentheses: false
- SpaceAfterCStyleCast: false
- SpacesInContainerLiterals: true
是否在容器字面量(@[@"1",@"2"])中插入空格
- SpaceBeforeAssignmentOperators: true
在=号前加空格
- ContinuationIndentWidth: 4
在续行(下一行)时的缩进长度
- CommentPragmas: '^ IWYU pragma:'
一个描述特殊意义的正则表达式。
- ForEachMacros: [ foreach, Q_FOREACH, BOOST_FOREACH ]
A vector of macros that should be interpreted as foreach loops instead of as function calls
- SpaceBeforeParens: ControlStatements
是否在括号前加上空格，此处只是在控制语句之前添加(if/while/for...)
- DisableFormat: false
禁用当前format文件

参考文档：http://clang.llvm.org/docs/ClangFormatStyleOptions.html
