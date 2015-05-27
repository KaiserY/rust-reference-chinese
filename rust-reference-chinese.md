# Rust 参考手册

* [1.介绍](#Introduction)
* [2.标记法](#Notation)
  * [2.1.Unicode组合](#UnicodeProductions)
  * [2.2.字符串表组合](#StringTableProductions)
* [3.词法结构](#LexicalStructure)
  * [3.1.输入格式](#InputFormat)
  * [3.2.标识符](#Identifiers)
  * [3.3.注释](#Comments)
  * [3.4.空白](#Whitespace)
  * [3.5.记号](#Tokens)
    * [3.5.1.关键字](#Keywords)
    * [3.5.2.常量](#Literals)
      * [3.5.2.1.例子](#Examples)
        * [3.5.2.1.1.字符和字符串](#CharactersAndStrings)
        * [3.5.2.1.2.字节转义](#ByteEscapes)
        * [3.5.2.1.3.Unicode转义](#UnicodeEscapes)
        * [3.5.2.1.4.数字](#Numbers)
        * [3.5.2.1.5.后缀](#Suffixes)
      * [3.5.2.2.字符和字符串常量](#CharacterAndStringLiterals)
        * [3.5.2.3.1.字节常量](#CharacterLiterals)
        * [3.5.2.2.2.字符串常量](#StringLiterals)
        * [3.5.2.2.3.字符转义](#CharacterEscapes)
        * [3.5.2.2.4.原始字符串常量](#RawStringLiterals)
      * [3.5.2.3.字节和字节字符串常量](#ByteAndByteStringLiterals)
        * [3.5.2.3.1.字节常量](#ByteLiterals)
        * [3.5.2.3.2.字节字符串常量](#ByteStringLiterals)
        * [3.5.2.3.3.原始字节字符串常量](#RawByteStringLiterals)
      * [3.5.2.4.数字常量](#NumberLiterals)
        * [3.5.2.4.1.整形常量](#IntegerLiterals)
        * [3.5.2.4.2.浮点常量](#FloatingPointLiterals)
      * [3.5.2.5.布尔常量](#BooleanLiterals)
  * [3.6.路径](#Paths)
* [4.语法扩展](#SyntaxExtensions)
  * [4.1.宏](#Macros)
    * [4.1.1.示例宏](#MacroByExample)
    * [4.1.2.解析限制](#ParsingLimitations)
  * [4.2.宏中有用的语法扩展](#SyntaxExtensionsUsefulInMacros)
  * [4.3.宏调试的语法扩展](#SyntaxExtensionsForMacroDebugging)
  * [4.4.准引用](#Quasiquoting)
* [5.包装箱和源文件](#CratesAndSourceFiles)
* [6.项和属性](#ItemsAndAttributes)
  * [6.1.项](#Items)
    * [6.1.1.类型参数](#TypeParameters)
    * [6.1.2.模块](#Modules)
      * [6.1.2.0.1.`extern crate`声明](#ExternCrateDeclarations)
      * [6.1.2.0.2.`use`声明](#UseDeclarations)
    * [6.1.3.函数](#Functions)
      * [6.1.3.1.泛型函数](#GenericFunctions)
      * [6.1.3.2.不安全性](#Unsafety)
        * [6.1.3.2.1不安全函数](#UnsafeFunctions)
        * [6.1.3.2.2.不安全块](#UnsafeBlocks)
        * [6.1.3.2.3.被认为是未定义的行为](#BehaviorConsideredUndefined)
        * [6.1.3.2.4.不认为是不安全的行为](#BehaviourNotConsideredUnsafe)
      * [6.1.3.3.发散函数](#DivergingFunctions)
      * [6.1.3.4.外部（语言）函数](#ExternFunctions)
    * [6.1.4.类型别名](#TypeAliases)
    * [6.1.5.结构体](#Structures)
    * [6.1.6.枚举](#Enumerations)
    * [6.1.7.常量项](#ConstantItems)
    * [6.1.8.静态项](#StaticItems)
      * [6.1.8.1.可变静态量](#MutableStatics)
    * [6.1.9.特性](#Traits)
    * [6.1.10.实现](#Implementations)
    * [6.1.11.外部块](#ExternalBlocks)
  * [6.2.可见性和私有性](#VisibilityAndPrivacy)
    * [6.2.1 Re-exporting and Visibility](#ReExportingAndVisibility)
  * [6.3.属性](#Attributes)
    * [6.3.1.包装箱专有属性](#CrateOnlyAttributes)
    * [6.3.2.模块特有属性](#ModuleOnlyAttributes)
    * [6.3.3.函数特有属性](#FunctionOnlyAttributes)
    * [6.3.4.静态量特有属性](#StaticOnlyAttributes)
    * [6.3.5.FFI属性](#FFIAttributes)
    * [6.3.6.宏相关属性](#MacroRelatedAttributes)
    * [6.3.7.各种（其它）属性](#MiscellaneousAttributes)
    * [6.3.8.条件编译](#ConditionalCompilation)
    * [6.3.9.Lint检查属性](#LintCheckAttributes)
    * [6.3.10.语言项](#LanguageItems)
    * [6.3.11.内联属性](#InlineAttributes)
    * [6.3.12.`derive`属性](#Derive)
    * [6.3.13.编译器功能](#CompilerFeatures)
* [7.语句和表达式](#StatementsAndExpressions)
  * [7.1.语句](#Statements)
    * [7.1.1.声明语句](#DeclarationStatements)
      * [7.1.1.1.项声明](#ItemDeclarations)
      * [7.1.1.2.位置声明](#SlotDeclarations)
    * [7.1.2.表达式语句](#ExpressionStatements)
  * [7.2.表达式](#Expressions)
      * [7.2.0.1.左值，右值和临时值](#LvaluesRvaluesAndTemporaries)
      * [7.2.0.2.移动和拷贝类型](#MovedAndCopiedTypes)
    * [7.2.1.常量表达式](#LiteralExpressions)
    * [7.2.2.路径表达式](#PathExpressions)
    * [7.2.3.元组表达式](#TupleExpressions)
    * [7.2.4.单元表达式](#UnitExpressions)
    * [7.2.5.结构体表达式](#StructureExpressions)
    * [7.2.6.块表达式](#BlockExpressions)
    * [7.2.7.方法调用表达式](#MethodCallExpressions)
    * [7.2.8.字段表达式](#FieldExpressions)
    * [7.2.9.数组表达式](#ArrayExpressions)
    * [7.2.10.索引表达式](#IndexExpressions)
    * [7.2.11.单目运算符表达式](#UnaryOperatorExpressions)
    * [7.2.12.双目运算符表达式](#BinaryOperatorExpressions)
      * [7.2.12.1.算术运算符](#ArithmeticOperators)
      * [7.2.12.2.二进制运算符](#BitwiseOperators)
      * [7.2.12.3.惰性布尔运算符](#LazyBooleanOperators)
      * [7.2.12.4.比较运算符](#ComparisonOperators)
      * [7.2.12.5.类型转换表达式](#TypeCastExpressions)
      * [7.2.12.6.赋值表达式](#AssignmentExpressions)
      * [7.2.12.7.复合赋值表达式](#CompoundAssignmentExpressions)
      * [7.2.12.8.运算符优先级](#OperatorPrecedence)
    * [7.2.13.组合表达式](#GroupedExpressions)
    * [7.2.14.调用表达式](#CallExpressions)
    * [7.2.15.Lambda表达式](#7.2.15.Lambda表达式)
    * [7.2.16.While循环](#WhileLoops)

## <a name="Introduction"></a>1.介绍
本文档是Rust编程语言的主要参考。它提供3种类型的材料：

* 非正式的介绍了每种语言结构和其用法的章节
* 非正式的介绍了内存模型，并发模型。运行时服务，链接模型和调试工具的章节
* 提供了影响了Rust设计的基础原理和语言参考的附录章节

本文档并不充当语言的介绍。它假设你熟悉Rust语言的背景基础。一个单独的[book](http://doc.rust-lang.org/book/)可以帮助你熟悉这些背景。

本文档也不充当包含在Rust语言发行版中的[标准库](http://doc.rust-lang.org/std/)的参考。这些库的文档来源于从源码中抽取的文档属性。很多你期望是语言功能的部分可能是Rust库的功能，所以你应该在库文档中寻找，而不是在这。

你可能也对[语法](http://doc.rust-lang.org/grammar.html)感兴趣。

## <a name="Notation"></a>2.标记法

### <a name="UnicodeProductions"></a>2.1.Unicode组合
一些Rust语法中的组合允许Unicode代码点超越ASCII范围。我们根据Unicode标准指定的字符属性定义这些组合，而不是采用ASCII范围的代码点。语法文档中有一个[特殊Unicode组合](https://doc.rust-lang.org/stable/grammar.html#special-unicode-productions)部分列出了这些组合。

### <a name="StringTableProductions"></a>2.2.字符串表组合
语法中的一些规则 - 特别的像[单目运算符](#UnaryOperatorExpressions)，[双目运算符](BinaryOperatorExpressions)和[关键字](https://doc.rust-lang.org/stable/grammar.html#keywords) - 表现为一种简单的形式：作为一串非引用，可打印的空白分隔的字符串表。这些例子是[记号](#Tokens)规则的子集，并假设为编译器的词法分析的结果，由一个DFA（确定有限状态自动机）驱动，通过分离所有这些字符串表入口执行。

当语法中出现一个在双引号（`"`）中的字符串时，它是字符串表组合中的一个单独成员的隐式引用。查看[记号](#Tokens)以获取更多信息。

## <a name="LexicalStructure"></a>3.词法结构

### <a name="InputFormat"></a>3.1.输入格式
Rust的输入被解释为一串UTF-8编码的Unicode代码点。大部分Rust语法规则根据ASCII范围的代码点定义，不过一小部分根据Unicode属性或显示代码点列表定义。<sup>[1](#Fn1)</sup>

### <a name="Identifiers"></a>3.2.标识符
一个标识符是如下形式的任何非空Unicode字符串<sup>[2](#Fn2)</sup>：

* 第一个字符拥有`XID_start`属性
* 其它字符拥有`XID_continue`属性

以上组合*不能*出现在[关键字](#Keywords)中。

> **注意**：`XID_start`和`XID_continue`作为一个字符属性包含了用来组成更有名的C和JavaScript家族的标识符的字符集。

### <a name="Comments"></a>3.3.注释
Rust代码中的注释大体上遵循C++风格的行（`//`）和块（`/* ... */`）注释形式。嵌套块注释也被支持。

以正好*3*个斜杠开头（`///`）的行注释，和以在块开始处正好一个斜杠和两个星号开头（`/**`）的块注释，它们被作为`doc`[属性](#Attributes)的一个特殊语法解析。这就是说，它们等同于在注释体周围写上`#[doc="..."]`（这包含了注释字符自身，也就是说`/// Foo`等同于`#[doc="/// Foo"]`）。

`//!`注释适用于注释对象的父对象，而不是它之后的对象本身。`//!`注释通常用于在包装箱索引页显示信息。

非文档注释被当作空白的形式解析。

### <a name="Whitespace"></a>3.4.空白

空白任何只包含下列字符的非空字符串：

* `U+0020`（空格，`' '`）
* `U+0009`（制表符，`'\t'`）
* `U+000A`（换行符，`'\n'`）
* `U+000D`（回车，`'\r'`）。

Rust是一个“自由形式”语言，这意味着任何形式的空格只用来分隔语法中的*记号*，并无语义意义。

Rust程序中如果每个空白元素被替换任何其它空格元素时它们仍有相同的意义，比如一个单独的空格字符。

### <a name="Tokens"></a>3.5.记号

记号是语法中的主要组合，使用正则（非递归）语言定义。“简单”记号采用[字符串表组合](#StringTableProductions)形式，并作为双引号引用的字符串出现在剩下的语法中。其它记号则有确定的规则。

#### <a name="Literals"></a>3.5.1.常量
常量是一个包含单独记号的表达式，而不是一串记号，它立即而直接的代表了它所赋的值，而不是通过名字引用或其它赋值规则。常量是一种形式的常表达式，所以它（主要）在编译时赋值。

##### <a name="Examples"></a>3.5.1.1.例子

###### <a name="CharactersAndStrings"></a>3.5.1.1.1.字符和字符串

|  | 例子 | `#`集合 | 字符集 | 转义 |
| ------------- | ------------- | ------------- | ------------- | ------------- |
|[字符](#CharacterLiterals)|`'H'`|`N/A`|所有Unicode|`\'`和[字节](#ByteEscapes)和[Unicode](#UnicodeEscapes)|
|[字符串](#StringLiterals)|`"hello"`|`N/A`|所有Unicode|`\“`和[字节](#ByteEscapes)和[Unicode](#UnicodeEscapes)|
|[原始字符串](#RawStringLiterals)|`r#"hello"#`|`0...`|所有Unicode|`N/A`|
|[字节](#ByteLiterals)|`b'H'`|`N/A`|所有ASCII|`\'`和[字节](#ByteEscapes)|
|[字节字符串](#ByteStringLiterals)|`b"hello"`|`N/A`|所有ASCII|`\“`和[字节](#ByteEscapes)|
|[原始字节字符串](#RawByteStringLiterals)|`br#"hello"#`|`0...`|所有ASCII|`N/A`|

####### <a name="ByteEscapes"></a>3.5.1.1.2.字节转义

|| 名称 |
| -------- | ---------------------- |
|`\x7F`|8位字符编码（正好2个数字）|
|`\n`|换行符|
|`\r`|回车|
|`\t`|制表符|
|`\\`|反斜线|

###### <a name="UnicodeEscapes"></a>3.5.1.1.3.Unicode转义

|| 名称 |
| -------- | ---------------------- |
|`\u{7FFF}`|24位Unicode字符编码（最多6个数字）|

###### <a name="Numbers"></a>3.5.1.1.4.数字

| [数字常量](#NumberLiterals)`*` | 例子 | 幂 | 后缀 |
| ------------- | ------------- | ------------- | ------------- |
|十进制整数|`98_222`|`N/A`|整数后缀|
|十六进制整数|`0xff`|`N/A`|整数后缀|
|八进制整数|`0o77`|`N/A`|整数后缀|
|二进制整数|`0b1111_0000`|`N/A`|整数后缀|
|浮点数|`123.0E+77`|`Optional`|浮点后缀|

`*`所有数字常量允许`_`作为可视分隔符：`1_234.0E+18f64`

###### <a name="Suffixes"></a>3.5.1.1.5.后缀

| 整数 | 浮点数 |
| ---------------- | ------------- |
|`u8`，`i8`，`u16`，`i16`，`u32`，`i32`，`u64`，`i64`，`is`（`isize`），`us`（`usize`）|`f32`，`f64`|

##### <a name="CharacterAndStringLiterals"></a>3.5.1.2.字符和字符串常量

```
char_lit : '\x27' char_body '\x27' ;
string_lit : '"' string_body * '"' | 'r' raw_string ;

char_body : non_single_quote
          | '\x5c' [ '\x27' | common_escape | unicode_escape ] ;

string_body : non_double_quote
            | '\x5c' [ '\x22' | common_escape | unicode_escape ] ;
raw_string : '"' raw_string_body '"' | '#' raw_string '#' ;

common_escape : '\x5c'
              | 'n' | 'r' | 't' | '0'
              | 'x' hex_digit 2

unicode_escape : 'u' '{' hex_digit+ 6 '}';

hex_digit : 'a' | 'b' | 'c' | 'd' | 'e' | 'f'
          | 'A' | 'B' | 'C' | 'D' | 'E' | 'F'
          | dec_digit ;
oct_digit : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' ;
dec_digit : '0' | nonzero_dec ;
nonzero_dec: '1' | '2' | '3' | '4'
           | '5' | '6' | '7' | '8' | '9' ;
```

###### <a name="CharacterLiterals"></a>3.5.2.2.1.字符常量
一个*字符常量*是一个在两个`U+0027`（单引号）字符中的单个Unicode字符，当它是`U+0027`自身的时候，它必须前跟一个`U+005C`字符（`\`）*转义*。

###### <a name="StringLiterals"></a>3.5.2.2.2.字符串常量
一个*字符串常量*是一串在两个`U+0022`（双引号）字符中的任意Unicode字符，当字符是`U+0022`自身的时候，它必须前跟一个`U+005C`字符（`\`）*转义*，或者使用*原始字符串常量*。

多行字符串常量可以通过在换行之前前跟一个`U+005C`字符（`\`）来结束每一行来定义。这回导致`U+005C`字符，换行符和所有下一行开头的空白都被忽略。

```rust
let a = "foobar";
let b = "foo\
         bar";

assert_eq!(a,b);
```

###### <a name="CharacterEscapes"></a>3.5.2.2.3.字符转义
不管字符还是非原始字符串常量都提供了一些额外的*转义*。一个转义以一个`U+005C`（`\`）开始并后跟如下一种形式：

* 一个*8位代码点转义*以`U+0078`（`x`）开头后跟正好两位*十六进制数*。它代表等于它提供的十六进制值的Unicode代码点
* 一个*24位代码点转义*以`U+0075`（`u`）开头后跟位于大括号`U+007B`（`{`）和`U+007D`（`}`）之间最多6位*十六进制值*。它代表等于它提供的十六进制值的Unicode代码点
* 一个*空白转义*是字符`U+006E`（`n`），`U+0072`（`r`）或`U+0074`（`t`）中的一个，分别代表Unicode值`U+000A`（`LF`），`U+000D`（`CR`）或`U+0009`（`HT`）
* *反斜杠转义*是字符`U+005C`（`\`），它必须转义才能代表*自己*。

###### <a name="RawStringLiterals"></a>3.5.2.2.4.原始字符串常量
原始字符串常量并不进行任何转义。它以字符`U+0072`（`r`）后跟0个或多个字符`U+0023`（`#`）和一个`U+0022`（双引号）字符开头。*原始字符串体*并不由上面的EBNF语法定义：它可以包含任意Unicode字符序列并以另一个`U+0022`（双引号）字符结尾，后跟与开始`U+0022`（双引号）字符前跟的相同数量的`U+0023`（`#`）字符。

原始字符串体包含的所有Unicode字符代表它自己，`U+0022`（双引号）（当后跟0个或多个`U+0023`（`#`）字符用来开始原始字符串常量时除外）或`U+005C`（`\`）并没有特殊含义。

字符串常量的例子：

```rust
"foo"; r"foo";                     // foo
"\"foo\""; r#""foo""#;             // "foo"

"foo #\"# bar";
r##"foo #"# bar"##;                // foo #"# bar

"\x52"; "R"; r"R";                 // R
"\\x52"; r"\x52";                  // \x52
```

##### <a name="ByteAndByteStringLiterals"></a>3.5.2.3.字节和字节字符串常量

```
byte_lit : "b\x27" byte_body '\x27' ;
byte_string_lit : "b\x22" string_body * '\x22' | "br" raw_byte_string ;

byte_body : ascii_non_single_quote
          | '\x5c' [ '\x27' | common_escape ] ;

byte_string_body : ascii_non_double_quote
            | '\x5c' [ '\x22' | common_escape ] ;
raw_byte_string : '"' raw_byte_string_body '"' | '#' raw_byte_string '#' ;
```

###### <a name="ByteLiterals"></a>3.5.2.3.1.字节常量
一个*字节常量*是一个位于两个`U+0027`（单引号）字符中的一个单独ASCII字符（在`U+0000`到`U+007F`范围中），除了`U+0027`自己，它需要前跟`U+005C`（`\`）*转义*，或者一个单独的*转义字符*。它等于一个`u8`8位无符号整形*数字常量*。

###### <a name="ByteStringLiterals"></a>3.5.2.3.2.字节字符串常量
一个非原始*字节字符串常量*是一串ASCII字符和*转义符*，前跟`U+0062`（`b`）和`U+0022`（双引号），然后以`U+0022`结尾。如果`U+0022`出现常量中，它前跟`U+005C`（`\`）转义。又或者，一个字节字符串常量可以是一个*原始字节字符串常量*，如下定义。一个字节字符串常量等同于一个`&'static [u8]0`借用的8位无符号整形数组。

一些额外的*转义*可以在字节或非原始字节字符串常量中使用。一个转义以`U+005C`（`\`）开始并后跟如下一种形式：

* 一个*字节转义*以`U+0078`开头后跟正好两位*十六进制数*。它代表十六进制值代表的字节
* 一个*空白转义*是字符`U+006E`（`n`），`U+0072`（`r`）或`U+0074`（`t`）中的一个，分别代表Unicode值`U+000A`（`LF`），`U+000D`（`CR`）或`U+0009`（`HT`）
* 一个*反斜杠转义*是`U+005C`（`\`）字符，它必须被转义以代表ASCII编码的`0x5C`

###### <a name="RawByteStringLiterals"></a>3.5.2.3.3.原始字节字符串常量

原始字节字符串常量并不进行任何转义。它们以`U+0062`（`b`）开头，后跟`U+0072`（`r`）,后跟0个或多个`U+0023`（`#`），和一个`U+0022`（双引号）。*原始字符串体*并不由上述EBNF语法定义：它可以包含任何ASCII字符序列并以另一个`U+0022`（双引用）结束，后跟与开头`U+0022`（双引号）之前相同数量的`U+0023`（`#`）。一个原始字节字符串常量不能包含非ASCII字节。

原始字节字符串体中的所有字符代表它们的ASCII编码，`U+0022`（双引号）（当后跟0个或多个`U+0023`（`#`）字符用来开始原始字符串常量时除外）或`U+005C`（`\`）并没有特殊含义。

字节字符串常量的例子：

```rust
b"foo"; br"foo";                     // foo
b"\"foo\""; br#""foo""#;             // "foo"

b"foo #\"# bar";
br##"foo #"# bar"##;                 // foo #"# bar

b"\x52"; b"R"; br"R";                // R
b"\\x52"; br"\x52";                  // \x52
```

##### <a name="NumberLiterals"></a>3.5.2.4.数字常量

```
num_lit : nonzero_dec [ dec_digit | '_' ] * float_suffix ?
        | '0' [       [ dec_digit | '_' ] * float_suffix ?
              | 'b'   [ '1' | '0' | '_' ] +
              | 'o'   [ oct_digit | '_' ] +
              | 'x'   [ hex_digit | '_' ] +  ] ;

float_suffix : [ exponent | '.' dec_lit exponent ? ] ? ;

exponent : ['E' | 'e'] ['-' | '+' ] ? dec_lit ;
dec_lit : [ dec_digit | '_' ] + ;
```

一个*数字常量*是一个*整形常量*或者一个*浮点数常量*。识别这两种常量的语法是混合的。

###### <a name="IntegerLiterals"></a>3.5.2.4.1.整形常量

一个*整形常量*有下述4种形式中的一种：

* 一个*十进制常量*以一个*十进制数*后跟任意*十进制数*的组合和*下划线*
* 一个*十六进制常量*以`U+0030 U+0078`（`0x`）字符序列开头后跟任意十六进制数组合和下划线
* 一个*八进制常量*以`U+0030 U+006F`（`0o`）字符序列开头后跟任意八进制数组合和下划线
* 一个*二进制常量*以`U+0030 U+0062`（`0b`）字符序列开头后跟任意二进制数组合和下划线

像任何常量一样，一个整形常量可后跟（直接后跟，不带任何空白）一个*整形后缀*，它强制设置了常量的类型。整形后缀必须是如下整形类型的名字之一：`u8`，`i8`，`u16`，`i16`，`u32`，`i32`，`u64`，`i64`，`isize`或`usize`。

*无后缀整形常量*的类型通过类型推断确定。如果一个整形类型可以通过程序上下文唯一确定，无后缀整形就是这个类型。如果程序上下文未限定类型，它默认是32位有符号整形；如果程序上下文过度限制了类型，它被认为是一个静态类型错误。

各种形式的整形常量例子：

```rust
123i32;                            // type i32
123u32;                            // type u32
123_u32;                           // type u32
0xff_u8;                           // type u8
0o70_i16;                          // type i16
0b1111_1111_1001_0000_i32;         // type i32
0usize;                            // type usize
```

###### <a name="FloatingPointLiterals"></a>3.5.2.4.2.浮点常量
一个浮点常量有如下两种形式的一种：

* 一个*十进制常量*后跟一个句号字符`U+002E`（`.`）。它可以后跟另一个十进制常量，和一个可选的*指数*
* 一个单独的*十进制常量*后跟一个*指数*

默认，一个浮点常量是泛型类型，也就是说，类似整形常量，它的类型必须在上下文中唯一确定。这里有两种有效的*浮点后缀*，`f32`和`f64`（32位和64位浮点类型），它们显式的确定了常量的类型。

各种形式的浮点常量的例子：

```rust
123.0f64;        // type f64
0.1f64;          // type f64
0.1f32;          // type f32
12E+99_f64;      // type f64
let x: f64 = 2.; // type f64
```

最后一个例子稍显不同因为不可能对一个以句号结尾的浮点常量使用后缀语法。`2.f64`会尝试在`2`上调用一个叫`f64`的方法。

浮点数所代表的语义在[机器类型](#MachineTypes)中描述。

##### <a name="BooleanLiterals"></a>3.5.2.5.布尔常量
不二类型的两个值写做`true`和`false`。

#### <a name="Symbols"></a>3.5.3.符号

```
symbol : "::" | "->"
       | '#' | '[' | ']' | '(' | ')' | '{' | '}'
       | ',' | ';' ;
```

符号是一种通用的可打印[记号](#Tokens)，它在各种语法组合中起到了结构作用。在这里列出它们是为了除了出现在[单目运算符](#UnaryOperatorExpressions)，[双目运算法](#BinaryOperatorExpressions)或[关键字](#Keywords)中的以外的各种各样的可打印记号集合的完整性。

### <a name="Paths"></a>3.6.路径

```
expr_path : [ "::" ] ident [ "::" expr_path_tail ] + ;
expr_path_tail : '<' type_expr [ ',' type_expr ] + '>'
               | expr_path ;

type_path : ident [ type_path_tail ] + ;
type_path_tail : '<' type_expr [ ',' type_expr ] + '>'
               | "::" type_path ;
```

一个*路径*是一个或多个被一个命名空间限定符*逻辑*分隔的路径部分的序列。如果一个路径只包含一个部分，它可能代表一个[项](#Items)或一个位于本地控制域内的[位置](#MemorySlots)。如果一个路径包含多个部分，它代表一个[项](#Items)。

每个项在它的包装箱中都有一个“严格的路径”，不过命名这个项的路径只在这个给定的包装箱内才有意义。在包装箱之间并无全局命名空间可言；一个项的严格路径仅仅在包装箱内才能识别。

两个只包含标识符部分的简单路径的例子：

```rust
x;
x::y::z;
```

路径部分通常是[标识符](#Identifiers)，不过路径的结尾部分可以是一个尖括号中的一系列参数。在[表达式](#Expressions)上下文中，在命名空间标识符中最后一个（`::`）后给出的参数列表是为了消除与另一个相关的使用小于号（`<`）的表达式的歧义。在类型表达式中，最后的命名空间标识符可以省略。

两个带有类型参数的路径的例子：

```rust
type T = HashMap<i32,String>; // Type arguments used in a type expression
let x  = id::<i32>(10);       // Type arguments used in a call expression
```

路径可以通过指明多种前置标识符来改变它解析的方式：

* 以`::`开头的路径被认为是全局路径，路径的部分从包装箱根开始解析。路径中的每个标识符都必须被解析为一个项。

```rust
mod a {
    pub fn foo() {}
}
mod b {
    pub fn foo() {
        ::a::foo(); // call a's foo function
    }
}
```

* 以`super`关键字开头的路径相对其父模块开始解析。其后的每个标识符都必须解析为一个项。

```rsut
mod a {
    pub fn foo() {}
}
mod b {
    pub fn foo() {
        super::a::foo(); // call a's foo function
    }
}
```

以`self`关键字开头的路径相对当前模块开始解析。其后的每个标识符都必须解析为一个项。

```rust
fn foo() {}
fn bar() {
    self::foo();
}
```

## <a name="SyntaxExtensions"></a>4.语法扩展
一些Rust的小功能并未核心到足以拥有自己的语法，甚至没有被实现为函数，它们确实有名字，并用过一个一致的语法调用：`name!(...)`。例子包括：

* `format!`：格式化数据为一个字符串
* `env!`：在编译时寻找一个环境变量的值
* `file!`：返回正在编译的文件的路径
* `stringify!`：机智的打印作为参数的Rust表达式
* `include!`：引用给定文件中的Rust表达式
* `include_str!`：引用给定文件中的内容作为一个字符串
* `include_bytes!`：引用给定文件中的内容作为一个二进制对象
* `error!`，`warn!`，`info!`，`debug!`：提供诊断信息

上述所有的扩展均是带有值的表达式。

`rustc`的使用者可以通过两种方法定义新的语法：

* [编译器插件](http://doc.rust-lang.org/book/plugins.html)可以引入任意的Rust代码在编译时
* [宏](http://doc.rust-lang.org/book/macros.html)可以通过一个高级的，声明式的方式定义新语法

### <a name="Macros"></a>4.1.宏

```
expr_macro_rules : "macro_rules" '!' ident '(' macro_rule * ')' ;
macro_rule : '(' matcher * ')' "=>" '(' transcriber * ')' ';' ;
matcher : '(' matcher * ')' | '[' matcher * ']'
        | '{' matcher * '}' | '$' ident ':' ident
        | '$' '(' matcher * ')' sep_token? [ '*' | '+' ]
        | non_special_token ;
transcriber : '(' transcriber * ')' | '[' transcriber * ']'
            | '{' transcriber * '}' | '$' ident
            | '$' '(' transcriber * ')' sep_token? [ '*' | '+' ]
            | non_special_token ;
```

`macro_rules`允许你以声明的方式定义语法扩展。我们叫这种扩展“示例宏”或者简单的“宏” -- 来与[编译器插件](http://doc.rust-lang.org/book/plugins.html)中的“宏过程”相区别。

目前，宏可以扩展为表达式，语句，项，或者模式。

（一个`sep_token`是任何不是`*`和`+`的记号。一个`non_special_token`是任何不是分隔符或`$`的记号。）

宏扩展器通过名字寻找宏调用，并轮流尝试每一种宏规则。它改写第一个成功的匹配。匹配和改写非常相关，所以我们会一起描述它们。

#### <a name="MacroByExample"></a>4.1.1.示例宏
宏扩展器匹配和改写每一个不以`$`开头的记号，包括分隔符。为了解析的原因，分隔符必须是一对，不过它们并不特殊。

在匹配器中，`$`*名字*`:`*指示符*匹配Rust语法中由*指示符*命名的非终结符。有效的指示符是`item`，`block`，`stmt`，`pat`，`expr``ty`（类型），`ident`，`path`，`tt `（在Rust语法中的`=>`的任意一边）。在改写器中，指示符已经被知道，所以只有指示符的名字出现在美元符号后。

在匹配和改写中，类似克莱尼星号的运算符代表重复。克莱尼星号运算符包含`$`和括号，可选后跟一个分隔记号，后跟`*`或`+`。`*`意味着0次或多次重复，`+`代表至少1次重复。括号并不被匹配和改写。在匹配的时候，一个名字被绑定为*所有*它匹配到的名字，在一个模仿每一次成功的匹配中出现的重复结构的结构中。改写的工作就是整理出这些结构。

改写这些重复的规则叫做“示例宏”。本质上，循环的每一“层”都被执行一次，然后它们必须在一个名字被改写的时候被执行一次。因此`( $( $i:ident ),* ) => ( $i )`是一个无效的宏，不过`( $( $i:ident ),* ) => ( $( $i:ident ),* )`是可以接受的（如果是普通的话）。

当示例宏遇到一个重复时，它会检查其中出现的所有`$`名字。在“当前层”，它们必须被重复相同的次数，所以`( $( $i:ident ),* ; $( $j:ident ),* ) => ( $( ($i,$j) ),* )`在你给出`(a,b,c ; d,e,f)`参数时是有效的，而`(a,b,c ; d,e)`则不行。重复会同步的遍历一层中的选择，所以之前的输入会被改写为`( (a,d), (b,e), (c,f) )`。

嵌套重复是被允许的。

#### <a name="ParsingLimitations"></a>4.1.2.解析限制
宏系统使用的解析器是相当的强大，不过Rust语法的解析被限制为如下两种方式：

1. 解析器会尽可能多的解析。如果它尝试将`$i:expr [ , ]`和`8 [ , ]`匹配，它会尝试解析`i`为一个数组索引操作并失败。增加一个分隔符可以解决这个问题。
2. 解析器消除所有的歧义当它到达一个`$`*名字*`:`*指示符*时。这个要求大多影响出现在一个`$(...)*`的开头或者之后的名字-指示符对；在之前增加一个不同的记号可以解决这个问题。

### <a name="SyntaxExtensionsUsefulInMacros"></a>4.2.宏中有用的语法扩展

* `stringify!`：将标识符参数转换为字符串常量
* `concat!`：连接一个逗号分隔的常量列表

### <a name="SyntaxExtensionsForMacroDebugging"></a>4.3.宏调试的语法扩展

* `log_syntax!`：在编译时打印出参数
* `trace_macros!`：传递`true`或`false`来启用和禁用宏扩展日志

### <a name="Quasiquoting"></a>4.4.准引用
如下的宏扩展被用来准引用Rust语法树，通常用在[宏过程](http://doc.rust-lang.org/book/plugins.html#syntax-extensions)中：

* `quote_expr!`
* `quote_item!`
* `quote_pat!`
* `quote_stmt!`
* `quote_tokens!`
* `quote_matcher!`
* `quote_ty!`
* `quote_attr!`

记住当`$name : ident`出现在`quote_tokens!`的输入中，结果包含后跟两个记号的解引用的`name`。然而，向`quote_matcher!`传递相同形式的参数则会变成准引用的非终结符的MBE匹配器。并没有发生解引用。否则`quote_matcher!`和`quote_tokens!`的结果是相似的。

目前阶段文档非常有限。

## <a name="CratesAndSourceFiles"></a>5.包装箱和源文件
Rust是一个*编译型*语言。它的语义遵循一个在编译时和运行时之间的*阶段区别*（*phase distinction*）。这些使用*静态解释*的语义规则控制编译的成功与失败。叫做“动态语义”的语义规则控制程序运行时的行为。一个由于违反了编译时规则而编译失败的程序没有定义的动态语义；编译器应该中断并报告错误，并不产生可执行文件。

编译模型以一个叫*包装箱*（*crate*）的结构为中心。每次编译以源文件形式处理一个单独的包装箱，并且如果成功，产生一个二进制形式的单独的包装箱：一个二进制文件或库文件。

一个包装箱是一个编译和链接，同时也是版本控制，发布和运行时加载的单位。一个包装箱包含一个嵌套[模块](#Modules)作用域的树。树的顶层是一个匿名（从模块内路径的角度来看）模块并且包装箱内的每一个项都有一个代表它在包装箱模块树中位置的严格的[模块路径](Paths)。

Rust编译器总是接收一个单独的源文件作为输入，并总是产生一个单独的包装箱输出。对源文件的处理可能导致其它源文件被作为模块载入。源文件的后缀是`.rs`。

一个Rust源文件描述了一个模块，它的名字和它在当前包装箱中模块树的位置则在源文件之外定义：要么通过一个在引用的源文件中的`mod_item`显式定义，或者使用包装箱自己的名字。

每个源文件包括0个或多个`item`定义，并可选任定义意数量适用于它包含模块的`attribute`。匿名包装箱定义的属性对影响编译器的行为提供了重要的元数据。

```rust
// Crate name
#![crate_name = "projx"]

// Specify the output type
#![crate_type = "lib"]

// Turn on a warning
#![warn(non_camel_case_types)]
```

一个包含`main`函数的包装箱可以被编译为一个可执行文件。如果`main`出现，它的返回值必须是`unit`并且不接收任何参数。

## <a name="ItemsAndAttributes"></a>6.项和属性
包装箱包含[项](#Items)，它们每一个都可能附加一些[属性](#Attributes)。

### <a name="Items"></a>6.1.项

```
item : extern_crate_decl | use_decl | mod_item | fn_item | type_item
     | struct_item | enum_item | static_item | trait_item | impl_item
     | extern_block ;
```

一个`项`是包装箱的一部分。项在包装箱中被组织为一个嵌套的[模块](#Modules)集。每个包装箱都一个一个单独的“最外层”的匿名模块；所有其它包装箱中的项都有在包装箱中的模块树中的[路径](#Paths)。

项全部在编译时被确定，大部分在执行期间保持不变，并被储存在只读内存中。

这有几种类型的项：

* [`extern crate`声明](#ExternCrateDeclarations)
* [`use`声明](#UseDeclarations)
* [模块](#Modules)
* [函数](#Functions)
* [类型定义](#TypeDefinitions)
* [结构体](#Structures)
* [枚举](#Enumerations)
* [静态项](#StaticItems)
* [特性](#Traits)
* [实现](#Implementations)

一些项组成了一个隐式的定义子项的作用域。换句话说，在一个函数或模块中，项的定义可以（在多种情况下）与语句，控制块，和组成了项体的类似结构。这些范围内的项与定义在作用域外声明的项是一样的--它仍是一个静态项--除了那些位于模块命名空间中的*路径名称*由包含其中的项的名字定义的项，或者是其包含的私有项（对于函数来说）。

#### <a name="TypeParameters"></a>6.1.1.类型参数
除了模块外的所有项都能被类型参数化。类型参数通过一个逗号分隔的包含在尖括号（`<...>`）中的标识符列表给出，位于项定义中项名称之后，并不作为项类型的一部分。在实践中，类型推断系统通常可以从上下文汇总推断出参数类型。这是因为，Rust没有类型抽象的概念：这里并没有一等的“代表所有”的类型。

#### <a name="Modules"></a>6.1.2.模块

```
mod_item : "mod" ident ( ';' | '{' mod '}' );
mod : item * ;
```

一个模块是一个0个或多个[项](#Items)的容器。

一个*模块项*是一个模块，包含在一个大括号中，被命名的，前缀`mod`关键字。一个模块项引入了一个新的，命名了的模块到组成包装箱的模块树中。模块可以任意嵌套。

一个模块的例子：

```rust
mod math {
    type Complex = (f64, f64);
    fn sin(f: f64) -> f64 {
        /* ... */
    }
    fn cos(f: f64) -> f64 {
        /* ... */
    }
    fn tan(f: f64) -> f64 {
        /* ... */
    }
}
```

模块和类型分享相同的命名空间。在同一作用域中定义一个与模块相同名字的类型是不允许的：这就是说，一个类型定义，特性，结构体，或者类型参数不能覆盖作用域中的模块的名字，反之也不行。

一个没有体的模块从一个外部文件加载，默认它与模块有相同的名字，加上`.rs`扩展名。当一个嵌套的子模块从一个外部文件加载时，它从镜像于模块层次的子目录中加载。

```rust
// Load the `vec` module from `vec.rs`
mod vec;

mod thread {
    // Load the `local_data` module from `thread/local_data.rs`
    mod local_data;
}
```

用于加载外部文件模块的目录和文件可被`path`属相影响。

```rust
#[path = "thread_files"]
mod thread {
    // Load the `local_data` module from `thread_files/tls.rs`
    #[path = "tls.rs"]
    mod local_data;
}
```

###### <a name="ExternCrateDeclarations"></a>6.1.2.0.1.`extern crate`声明

```
extern_crate_decl : "extern" "crate" crate_name
crate_name: ident | ( string_lit "as" ident )
```

一个*`extern crate`声明*指定了一个外部包装箱的依赖。接着外部包装箱被绑定在定义为由`extern_crate_decl`提供的`ident`的作用域中。

外部包装箱在编译时被解析为一个特定的`soname`，而一个运行时链接要求`soname`在运行时被传递给连接器用于加载。`soname`在运行时解析，通过扫描编译器的库路径和匹配由外部包装箱在编译时的`crateid`属性中的字符串常量提供的可选的`crateid`，假设存在一个默认的等于`extern_crate_decl`中提供的`ident`的`name`属性。

3个`extern crate`声明的例子：

```
extern crate pcre;

extern crate std; // equivalent to: extern crate std as std;

extern crate "std" as ruststd; // linking to 'std' under another name
```

###### <a name="UseDeclarations"></a>6.1.2.0.2.`use`声明

```
use_decl : "pub" ? "use" [ path "as" ident
                          | path_glob ] ;

path_glob : ident [ "::" [ path_glob
                          | '*' ] ] ?
          | '{' path_item [ ',' path_item ] * '}' ;

path_item : ident | "self" ;
```

一个*use声明*创建了1个或多个与一些其它[路径](#Paths)同义的本地名字。通常`use`声明被用来缩短用来引用一个模块项的路径。这些定义可以出现在[模块](#Modules)或[代码块](#Blocks)的顶部。

> **注意**：不像许多语言，Rust中的`use`声明*并不*声明外部包装箱的链接依赖。相反[`extern crate`声明](#ExternCrateDeclarations)声明链接依赖。

`use`声明支持一系列的方便的简写：

* 重绑定目标名称为一个新的本地名称，使用`use p::q::r as x;`语法
* 同时绑定一系列的只有最后一个元素不同的路径，使用大括号语法`use a::b::{c,d,e,f};`
* 绑定所有匹配一个给定前缀的所有路径，使用星号通配符语法`use a::b::*;`
* 同时绑定一系列的只有最后一个元素不同的路径和它们的直系父模块，使用`self`关键字，例如`use a::b::{self, c, d};`

一个`use`声明的例子：

```rust
use std::option::Option::{Some, None};
use std::collections::hash_map::{self, HashMap};

fn foo<T>(_: T){}
fn bar(map1: HashMap<String, usize>, map2: hash_map::HashMap<String, usize>){}

fn main() {
    // Equivalent to 'foo(vec![std::option::Option::Some(1.0f64),
    // std::option::Option::None]);'
    foo(vec![Some(1.0f64), None]);

    // Both `hash_map` and `HashMap` are in scope.
    let map1 = HashMap::new();
    let map2 = hash_map::HashMap::new();
    bar(map1, map2);
}
```

就像项一样，`use`声明对包含它的模块默认是私有的。同时也像项一样，`use`声明可以是公有的，它使用`pub`关键字。这样的一个`use`声明用来*重导出*名称。因此一公有的`use`声明可以*重定向*一些公有名称到一个不同的目标定义中：甚至是一个位于不同模块中的严格私有的路径。如果一系列这样的重定向组成了一个环或者不能无二义的解析，它们会展示出一个编译时错误。

一个重导出的例子：

```rust
mod quux {
    pub use quux::foo::{bar, baz};

    pub mod foo {
        pub fn bar() { }
        pub fn baz() { }
    }
}
```

在这个例子中，模块`quux`重导出了两个定义在`foo`中的公有名称。

另外注意`use`项中的路径是相对与包装箱根的。所以，在之前的例子中，`use`指的是`quux::foo::{bar, baz}`，而不是简单的`foo::{bar, baz}`。这也意味着顶级模块定义需要位于包装箱根，如果你想要直接使用`use`项中定义的模块。也可以在`use`项的开头使用`self`和`super`来分别引用当前和直接父模块。所有在`use`声明中访问声明模块的规则同样适用于模块定义和`extern crate`声明。

一个什么可以和什么不可以使用`use`项的例子：

```rust
use foo::core::iter;  // good: foo is at the root of the crate
use foo::baz::foobaz;    // good: foo is at the root of the crate

mod foo {
    extern crate core;

    use foo::core::iter; // good: foo is at crate root
//  use core::iter;      // bad:  core is not at the crate root
    use self::baz::foobaz;  // good: self refers to module 'foo'
    use foo::bar::foobar;   // good: foo is at crate root

    pub mod bar {
        pub fn foobar() { }
    }

    pub mod baz {
        use super::bar::foobar; // good: super refers to module 'foo'
        pub fn foobaz() { }
    }
}

fn main() {}
```

#### <a name="Functions"></a>6.1.3.函数
一个*函数项*定义了一系列[语句](#Statements)和一个可选的位于末尾的[表达式](#Expressions)，和一个名称和一个参数集合。函数使用`fn`关键字定义。函数定义了一个输入[位置](#MemorySlots)的集合作为参数，通过调用者像函数传递参数，和一个由函数传回给调用者的输出[位置](#MemorySlots)。

一个函数也可以被拷贝到一个第一等级类型值中，这时它拥有响应的[函数类型](#FunctionTypes)，也可以被作为一个函数项使用（带有间接调用函数的额外开销）。

每一个函数的控制路径逻辑上以一个`return`表达式或一个发散表达式结尾。如果函数最外层块的最后有一个产生值得表达式，这个表达式被翻译为一个隐式的`return`表达式最为最后的表达式。

一个函数的例子：

```rust
fn add(x: i32, y: i32) -> i32 {
    return x + y;
}
```

就像`let`绑定一样，函数参数是确定模式的，所以任何在`let`绑定中有效的模式在参数中也有效。

```rust
fn first((value, _): (i32, i32)) -> i32 { value }
```

##### <a name="GenericFunctions"></a>6.1.3.1.泛型函数
一个*泛型函数*允许1个或多个*参数化类型*出现在它的签名中。每个类型参数必须被显式声明，包含在尖括号内，在函数名后由逗号分隔。

```rust
fn iter<T>(seq: &[T], f: |T|) {
    for elt in seq.iter() { f(elt); }
}
fn map<T, U>(seq: &[T], f: |T| -> U) -> Vec<U> {
    let mut acc = vec![];
    for elt in seq.iter() { acc.push(f(elt)); }
    acc
}
```

在函数签名或函数体中，类型参数的名称可以用作类型的名称。

当一个泛型函数被引用时，被实例化的类型基于引用的上下文。例如，在`[1, 2]`上调用上面定义的`iter`函数时会实例化类型参数`T`为`i32`，并且会要求闭包参数拥有`fn(i32)`类型。

类型参数也可以通过在函数名后加上一个尾部的[路径](#Paths)部分来显式指定。这在没有充足的上下文来确定类型参数时也许有用。例如，`mem::size_of::<u32>() == 4`。

因为泛型函数的参数类型是模糊的，可以执行的操作是有限的。参数类型的值只能被移动，不能复制。

```rust
fn id<T>(x: T) -> T { x }
```

相同的，[特性](#Traits)限制可以针对特定类型参数来允许拥有特性的方法在那个类型值上被调用。

##### <a name="Unsafety"></a>6.1.3.2.不安全性
不安全操作是指那些潜在违反了Rust静态语义的内存安全保证的操作。

如下的语言级功能不能用在Rust的安全子集中：

* 解引用一个[裸指针](#PointerTypes)
* 读写一个[可变静态变量](#MutableStatics)
* 调用一个不安全函数（包括一个固有功能或外部语言函数）

###### <a name="UnsafeFunctions"></a>6.1.3.2.1不安全函数
不安全函数是指那些不是在所有上下文中或对所有可能的输入都安全的函数。这个函数必须带上`unsafe`前缀并且只能被一个`unsafe`块或另一个`unsafe`函数调用。

###### <a name="UnsafeBlocks"></a>6.1.3.2.2.不安全块
一个代码块可以带上`unsafe`关键字前缀，来允许调用`unsafe`函数或解引用一个在安全函数中的裸指针。

当一个程序猿有足够的信心认为一系列潜在的不安全操作实际上是安全的，它们将这些（作为一个整体）代码包含在一个`unsafe`块中。编译器会考虑安全的使用这些代码，在当前的上下文中。

不安全块常被用来封装外部语言库，直接使用硬件或实现并不直接出现在语言中的功能。例如，Rust提供了实现内存安全并发所需要的语言功能不过线程和消息传递的实现位于标准库中。

Rust的类型系统是动态安全要求的一个保守估计，所以在一些情况下使用安全代码会带来性能开销。例如，一个双向链表不是一个树结构并且只能在安全代码中表现引用计数的指针。通过使用`unsafe`块来使用裸指针来表现可逆链表，它可以只使用装箱实现。

###### <a name="BehaviorConsideredUndefined"></a>6.1.3.2.3.被认为是未定义的行为
下面是一系列在所有Rust代码中都被禁止的行为，包括在`unsafe`块和`unsafe`函数中。类型检查提供了这些问题绝不会出现在安全代码中的保证。

* 数据竞争
* 解引用一个空/悬垂指针
* 读取[undef](http://llvm.org/docs/LangRef.html#undefined-values)（未初始化）内存
* 使用裸指针时打破[指针混淆规则](http://llvm.org/docs/LangRef.html#pointer-aliasing-rules)（C使用的规则的子集）
* `&mut`和`&`遵循LLVM的范围[noalias](http://llvm.org/docs/LangRef.html#noalias)模型，除非如果`&T`包含一个`UnsafeCell<U>`的话。不安全代码不能违反这些别名保证
* 不使用`UnsafeCell<U>`改变一个不可变值/引用
* 通过编译器固有功能调用未定义行为：
  * 使用`std::ptr::offset`（`offset`功能）来索引超过一个对象边界的值，除了结尾后一个字节的例外，它是被允许的
  * 在重叠的缓冲区上使用`std::ptr::copy_nonoverlapping_memory`（`memcpy32/memcpy64`功能）
* 基本类型的无效值，即便是在私有字段/本地变量中：
  * 悬垂/空引用或装箱
  * 在`bool`中一个不是`false`（`0`）或`true`（`1`）的值
  * 一个`enum`不包含类型定义的判别式
  * 在`char`中一个等于或大于`char::MAX`的值
  * 在`str`中的非UTF-8字节序列
* 在Rust中展开外部语言代码或者在其它语言中展开Rust代码。Rust的异常系统与其它语言中的异常处理并不兼容。展开必须在FFI内被捕获和处理

###### <a name="BehaviourNotConsideredUnsafe"></a>6.1.3.2.4.不认为是不安全的行为
下面是一系列在Rust中不认为是*不安全*的行为，不过它们可能不是你想要的。

* 死锁
* 从私有字段（`std::repr`）读取数据
* 由于引用计数循环引起的泄漏，即便在全局堆中
* 退出但未执行析构函数
* 发送信号
* 访问/修改文件系统
* 无符号数溢出（明确定义为包裹）
* 有符号数溢出（明确定义为二进制补码包裹）

##### <a name="DivergingFunctions"></a>6.1.3.3.发散函数
可以在一般输出值的位置写一个`!`字符来定义一种特殊类型的函数。例如：

```rust
fn my_err(s: &str) -> ! {
    println!("{}", s);
    panic!();
}
```

我们称这类函数是“发散”的因为它们永远也不会向调用者返回一个值。发散函数中的每一个控制路径都必须以`panic!()`结尾或调用另一个发散函数。`!`标记并*不*代表一个类型。

正如上面提到的定义一个函数可以是有用的，因为类型检查器会检查每一个控制路径是否以一个`return`或发散函数结尾。所以，如果`my_err`没有使用`!`定义，下面的代码将不能通过类型检查：

```rust
fn f(i: i32) -> i32 {
   if i == 42 {
     return 42;
   }
   else {
     my_err("Bad number!");
   }
}
```

如果`my_err`没有使用`!`标记上面的代码将不会编译，因为条件选择的`else`分支不会返回一个`i32`，这是`f`签名所要求的。`my_err`添加的`!`标记告诉类型检查器，对于任何进入`my_err`的控制流，`f`不应该进行进一步的类型判断，因为依赖这个判断的上下文的控制流将不会继续。因此`f`的返回值只需要考虑`if`分支的情况。

##### <a name="ExternFunctions"></a>6.1.3.4.外部（语言）函数
外部函数是Rust FFI（外部语言接口）的一部分，提供与[外部块](#ExternalBlocks)相对的功能。外部块允许Rust代码调用其它语言代码，Rust代码中定义的带有函数体的外部函数*可以被其它语言调用*。它们与任何其它Rust函数一样的方式被定义，除了它们带有`extern`修饰符。

```rust
// Declares an extern fn, the ABI defaults to "C"
extern fn new_i32() -> i32 { 0 }

// Declares an extern fn with "stdcall" ABI
extern "stdcall" fn new_i32_stdcall() -> i32 { 0 }
```

不像正常的函数，外部函数是`extern "ABI" fn()`类型的。这个与上面的定义在外部块中函数有相同的类型。

```rust
let fptr: extern "C" fn() -> i32 = new_i32;
```

外部函数可以直接在Rust代码中被调用因为Rust使用像C语言一样的大而相邻的栈段。

#### <a name="TypeAliases"></a>6.1.4.类型别名
一个*类型别名*为一个已知[类型](#Types)定义了一个新名字。类型别名使用`type`关键字声明。每一个值有一个单一的，特定的类型；一个值的类型特定方面包括：

* 一个值是否包含子值或者是不可分的
* 一个值是否代表文字的或数字的信息
* 一个值是否代表整形或浮点型信息
* 访问值所需的内存操作序列
* 类型的[种类](#TypeKinds)

例如，`(u8, u8)`定义了一对不可变值的集合，它们每一个都包含可通过模式匹配访问的两个8位无符号整形，并且在内存中`x`部分位于`y`部分之前：

```rust
type Point = (u8, u8);
let p: Point = (41, 68);
```

#### <a name="Structures"></a>6.1.5.结构体
一个*结构体*是一个使用`struct`关键字定义的[结构体类型](#StructureTypes)。

一个`struct`项和它的应用的例子：

```rust
struct Point {x: i32, y: i32}
let p = Point {x: 10, y: 11};
let px: i32 = p.x;
```

一个*元组结构体*是一个名义上的[元组类型](#TupleTypes)，它也使用`struct`关键字定义。例如：

```rust
struct Point(i32, i32);
let p = Point(10, 11);
let px: i32 = match p { Point(x, _) => x };
```

一个*类单元结构体*是一个没有任何字段的结构体，通过整个忽略字段列表来定义。这样的类型会有一个单独的值，就像单元类型的`()`一样。例如：

```rust
struct Cookie;
let c = [Cookie, Cookie, Cookie, Cookie];
```

一个结构体准确的内存布局并未指定。你可以使用[`repr`属性](#FFIAttributes)来指定一个特定的布局。

#### <a name="Enumerations"></a>6.1.6.枚举
一个*枚举*既是一个名义上[枚举类型](#EnumeratedTypes)的定义也是一个*构造函数*的集合，它可以被用来创建和模式匹配相应的枚举类型值。

枚举使用`enum`关键字定义。

一个`enum`和它应用的例子：

```rust
enum Animal {
  Dog,
  Cat
}

let mut a: Animal = Animal::Dog;
a = Animal::Cat;
```

枚举构造函数可以有命名了的和未命名的字段：

```rust
enum Animal {
    Dog (String, f64),
    Cat { name: String, weight: f64 }
}

let mut a: Animal = Animal::Dog("Cocoa".to_string(), 37.2);
a = Animal::Cat { name: "Spotty".to_string(), weight: 2.7 };
```

在这个例子中，`Cat`是一个*类结构体枚举变量*，而`Dog`是被简单的叫做一个枚举变量。

枚举有一个判别式，你可以显示的指定它：

```rust
enum Foo {
    Bar = 123,
}
```

如果你未赋值，它们会从`0`开始，并按顺序每一个变量加一。

你可以强转一个枚举来获得它的值：

```rust
let x = Foo::Bar as u32; // x is now 123u32
```

这只在没有任何变量附加了一个值的情况下有效。如果是`Bar(i32)`的话，这是不被允许的。

#### <a name="ConstantItems"></a>6.1.7.常量项

```
const_item : "const" ident ':' type '=' expr ';' ;
```

一个*常量项*是一个*命名了的常量值*，它并不代表程序中一个特定的内存位置。常量本质上会在被使用时内联，这意味着它们在被使用时会被直接拷贝到相对的上下文中。对同一常量的引用不一定能确保引用了相同的内存地址。

常量值必须没有析构函数，并且因此接受大部分形式的数据。常量可能引用了其它常量的地址，这种情况下这个地址将拥有`static`生命周期。编译器，然而，仍有多次转换常量的自由，所以引用地址可能并不稳定。

常量必须被显示指定类型。类型可以是`bool`，`char`，一个数字，或者由这些基本类型衍生出的类型。衍生类型是`static`生命周期的引用，定长数组，元组，枚举变量，和结构体。

```rust
const BIT1: u32 = 1 << 0;
const BIT2: u32 = 1 << 1;

const BITS: [u32; 2] = [BIT1, BIT2];
const STRING: &'static str = "bitstring";

struct BitsNStrings<'a> {
    mybits: [u32; 2],
    mystring: &'a str
}

const BITS_N_STRINGS: BitsNStrings<'static> = BitsNStrings {
    mybits: BITS,
    mystring: STRING
};
```

#### <a name="StaticItems"></a>6.1.8.静态项

```
static_item : "static" ident ':' type '=' expr ';' ;
```

*静态项*与*常量*类似，除了它在程序中代表一个确定的内存位置。一个静态量在使用时从不“内联”，并且所有的引用都指向相同的内存位置。静态项有`static`声明周期，它比Rust程序中其它任何声明周期都长。静态项可以存放在只读内存中如果它们不含任何内在的可变性。

可能含有内部可变性的静态项可以通过`UnsafeCell`语言项获得。对静态项的所有访问都是安全的，不过它拥有一系列的限制：

* 静态量不能包含任何析构函数
* 静态值的类型必须是`Sync`以允许线程安全的访问
* 静态量不能通过值引用其它静态量，只能通过引用
* 常量不能引用静态量

常量大体上是比静态量更合适的，除非储存了大量的数据，或者要求单地址和可变性。

##### <a name="MutableStatics"></a>6.1.8.1.可变静态量
如果静态项是用`mut`关键字声明的，那么它就允许被程序修改。Rust的目标之一就是使并发bug难以出现，而这明显是一个巨大的数据竞争和其他bug的根源。为此，需要一个`unsafe`块来读写可变静态变量。需要额外注意以确保对于同一进程中的其它线程来说对可变静态量的修改时安全的。

然而，可变静态量也可以是很有用的。它们可以用来与C语言库交互和被C语言库使用（在`extern`块中）。

```rust
static mut LEVELS: u32 = 0;

// This violates the idea of no shared state, and this doesn't internally
// protect against races, so this function is `unsafe`
unsafe fn bump_levels_unsafe1() -> u32 {
    let ret = LEVELS;
    LEVELS += 1;
    return ret;
}

// Assuming that we have an atomic_add function which returns the old value,
// this function is "safe" but the meaning of the return value may not be what
// callers expect, so it's still marked as `unsafe`
unsafe fn bump_levels_unsafe2() -> u32 {
    return atomic_add(&mut LEVELS, 1);
}
```

可变静态量与正常静态量有相同的限制，除了值的类型并不要求实现了`Sync`。

#### <a name="Traits"></a>6.1.9.特性
*特性*描述了一个方法类型的集合。

特性可以包含方法的默认实现，按照一些未知的[`self`类型](#SelfTypes)编写；`self`类型要么完全不确定，要么被一些其它的特性所限制。

特性通过对特定类型分别[实现](#Implementations)。

```rust
trait Shape {
    fn draw(&self, Surface);
    fn bounding_box(&self) -> BoundingBox;
}
```

这里定义了一个带有两个方法的特性。所有在这个作用域中[实现](#Implementations)了这个特性的值都可以调用`draw`和`bounding_box`两个方法，使用`value.bounding_box()`[语法](#MethodCallExpressions)。

可以为特性指定一个类型参数来使之泛型化。它出现在特性名称的后面，使用[泛型函数](#Generic functions)相同的语法。

```rust
trait Seq<T> {
   fn len(&self) -> u32;
   fn elt_at(&self, n: u32) -> T;
   fn iter<F>(&self, F) where F: Fn(T);
}
```

泛型函数可以使用特性作为它们类型参数的*界限*。这将产生两个影响：只有实现了这个特性的类型可以实例化这个参数，并且在泛型函数内，特性的方法可以在这个参数类型的值上调用。例如：

```rust
fn draw_twice<T: Shape>(surface: Surface, sh: T) {
    sh.draw(surface);
    sh.draw(surface);
}
```

特性也可以定义跟自己相同名称的[对象类型](#ObjectTypes)。这个类型的值通过强转指针值（指向作用域中一个实现了给定特性的类型）为特性名称的指针来创建，作为一个类型来使用。

```rust
let myshape: Box<Shape> = Box::new(mycircle) as Box<Shape>;
```

结果的值是一个包含我们强转的值的装箱，连同我们使用的实现的方法标识符信息。带有特性类型的值可以在其之上[调用方法](#MethodCallExpressions)，对于特性的任何方法，并且可以用来实例化特性限制的类型参数。

特性方法可以是静态的，这意味着它缺少`self`参数。这意味着它们只能使用方法调用语法（`f(x)`）而不能使用方法调用语法（`obj.f()`）调用。使用特性静态方法的方式是使用特性名称限制它，将特性名称作为模块来对待。例如：

```rust
trait Num {
    fn from_i32(n: i32) -> Self;
}
impl Num for f64 {
    fn from_i32(n: i32) -> f64 { n as f64 }
}
let x: f64 = Num::from_i32(42);
```

特性可以从其它特性继承。例如：

```rust
trait Shape { fn area(&self) -> f64; }
trait Circle : Shape { fn radius(&self) -> f64; }
```

`Circle : Shape`语法意味着实现了`Circle`的类型也必须实现`Shape`。多个父特性使用`+`分隔，`trait Circle : Shape + PartialEq { }`。在一个给定的实现了`Circle`的`T`类型，可以涉及到`Shape`的方法，因为类型检查器检查任何实现了`Circle`是否也实现了`Shape`。

在类型参数化函数中，父特性的方法可以在子特性限制的值上调用。参考前面`trait Circle : Shape`的例子：

```rust
fn radius_times_area<T: Circle>(c: T) -> f64 {
    // `c` is both a Circle and a Shape
    c.radius() * c.area()
}
```

同样的，父特性的方法可以在特性对象上调用：

```rust
let mycircle = Box::new(mycircle) as Box<Circle>;
let nonsense = mycircle.radius() * mycircle.area();
```

#### <a name="Implementations"></a>6.1.10.实现
*实现*是一个为一个特定类型实现了一个[特性](#Traits)的项。

实现使用`impl`关键字实现。

```rust
struct Circle {
    radius: f64,
    center: Point,
}

impl Copy for Circle {}

impl Clone for Circle {
    fn clone(&self) -> Circle { *self }
}

impl Shape for Circle {
    fn draw(&self, s: Surface) { do_draw_circle(s, *self); }
    fn bounding_box(&self) -> BoundingBox {
        let r = self.radius;
        BoundingBox{x: self.center.x - r, y: self.center.y - r,
         width: 2.0 * r, height: 2.0 * r}
    }
}
```

也可以并不涉及到特性来定义一个实现。这样的实现的方法只能在实现对象类型的值上直接调用。在这样的实现中，特性类型和`impl`后面的`for`被省略。这样的实现被限制在名义上的类型（枚举，结构体），并且因为`self`类型它只能出现在当前模块或子模块中：

```rust
struct Point {x: i32, y: i32}

impl Point {
    fn log(&self) {
        println!("Point is at ({}, {})", self.x, self.y);
    }
}

let my_point = Point {x: 10, y:11};
my_point.log();
```

当在`impl`中*指定了*一个特性，所有声明为特性一部分的的方法都必须被实现，带有匹配的类型和类型参数数量。

实现可以带有类型参数，它可以与特性的类型参数不同。实现的类型参数写在`impl`关键字之后。

```rust
impl<T> Seq<T> for Vec<T> {
   /* ... */
}
impl Seq<bool> for u32 {
   /* Treat the integer as a sequence of bits */
}
```

#### <a name="ExternalBlocks"></a>6.1.11.外部块

```
extern_block_item : "extern" '{' extern_block '}' ;
extern_block : [ foreign_fn ] * ;
```

外部块组成了Rust外部语言接口的基础。外部块中的定义描述了外部的，非Rust库的符号。

外部块中的函数与其它Rust函数一样被定义，除了它们可能没有函数体并以一个分号结尾。

```rsut
extern crate libc;
use libc::{c_char, FILE};

extern {
    fn fopen(filename: *const c_char, mode: *const c_char) -> *mut FILE;
}
```

外部块中的函数可以被Rust代码调用，就像Rust中定义的函数一样。Rust编译器会自动在Rust ABI和外部语言ABI之间转换。

一系列的[属性](#Attributes)可以控制外部块的行为。

外部块默认假设它调用的库使用标准C“cdecl” ABI。其它的ABI可以用`abi`字符串属性指定，如下所示：

```rust
// Interface to the Windows API
extern "stdcall" { }
```

`link`属性允许指定库的名称。指定后编译器会尝试链接给定名称的原生库。

```rust
#[link(name = "crypto")]
extern { }
```

外部块中定义的函数的类型是`extern "abi" fn(A1, ..., An) -> R`，其中`A1...An`声明了参数类型列表而`R`则是声明的返回值类型。

### <a name="VisibilityAndPrivacy"></a>6.2.可见性和私有性
这里有两个术语经常被交替使用，而它们尝试传达的是一个问题的答案“这个项可以在这个地方使用吗？”

Rust的名称解析工作在一个全局的命名空间分层结构上。层次中的每一层可以被认为是一些项。这项项是上面提到的这些，也包括外部包装箱。声明或定义一个新模块可以被认为是在其定义的地方向层次中插入一个新的树。

为了控制接口是否可以跨模块使用，Rust检查每一个项的使用来确定这是否是允许的。这是私有警告产生的地方，或者“你使用了一个另一个模块的私有项而这是不允许的。”

Rust的一切默认是*私有的*，除了一个例外。一个`pub`枚举中的枚举变量也默认是公有的。你被允许使用`priv`关键字来修改这个默认的可见性。当一个项被定义为`pub`，它可以被认为能够被外部世界访问。例如：

```rust
// Declare a private struct
struct Foo;

// Declare a public struct with a private field
pub struct Bar {
    field: i32
}

// Declare a public enum with two public variants
pub enum State {
    PubliclyAccessibleState,
    PubliclyAccessibleState2,
}
```

根据标记一个项可以是公有或私有的，Rust允许一个项通过两种方式访问：

* 如果一个项是公有的，那么它可以从外部被任何它的公有父项使用
* 如果一个项是私有的，它可以被当前模块和子项访问

这两种方式非常强大，可以用来创建暴露公有API而隐藏内部实现细节的模块层次结构。为了帮助解释，这里有一些用例和它们如何继承的：

* 一个库开发者想要向链接它的库的包装箱暴露一些功能。作为第一个情况的结果，这意味着任何可以在外部使用的功能必须从根到目标项都声明为`pub`。其中的任何私有项都会不允许外部访问。
* 一个包装箱需要一个全局可用的“帮助模块”，不过它并不像暴露帮助模块作为公有API。为了做到这点，包装箱层次的根可以有一个在内部作为“公有API”的私有模块。因为整个包装箱是根的子项，那么根据第二条在整个本地包装箱中可以访问这个私有模块。
* 当为一个模块编写单元测试时，通常习惯上定义一个用于测试的叫做`mod test`的直接子模块。根据第二条它可以访问父模块中的任何项，这意味着内部实现细节也可以在子模块中无间隙的被测试。

在第二条中，它提到了一个私有项“可以被”当前模块和子项访问，不过访问一个项的实际意义依赖于项是什么。访问一个模块，例如，意味着查看它的内部（来导入更多项）。另一方面，访问一个函数意味着调用它。另外，路径表达式和导入语句被认为是访问一个项，就导入/表达式只在目标项在当前可见作用域中时才有效这点而言。

下面是一个演示了上述3点的例子：

```rust
// This module is private, meaning that no external crate can access this
// module. Because it is private at the root of this current crate, however, any
// module in the crate may access any publicly visible item in this module.
mod crate_helper_module {

    // This function can be used by anything in the current crate
    pub fn crate_helper() {}

    // This function *cannot* be used by anything else in the crate. It is not
    // publicly visible outside of the `crate_helper_module`, so only this
    // current module and its descendants may access it.
    fn implementation_detail() {}
}

// This function is "public to the root" meaning that it's available to external
// crates linking against this one.
pub fn public_api() {}

// Similarly to 'public_api', this module is public so external crates may look
// inside of it.
pub mod submodule {
    use crate_helper_module;

    pub fn my_method() {
        // Any item in the local crate may invoke the helper module's public
        // interface through a combination of the two rules above.
        crate_helper_module::crate_helper();
    }

    // This function is hidden to any module which is not a descendant of
    // `submodule`
    fn my_implementation() {}

    #[cfg(test)]
    mod test {

        #[test]
        fn test_my_implementation() {
            // Because this module is a descendant of `submodule`, it's allowed
            // to access private items inside of `submodule` without a privacy
            // violation.
            super::my_implementation();
        }
    }
}
```

对于一个Rust程序为了通过私有性检查，所有路径必须根据上面给出的两种情况访问。这包括所有的`use`语句，表达式，类型，等。。。

#### <a name="ReExportingAndVisibility"></a>6.2.1.重导出和可见性
Rust允许公有的重导出项通过`pub use`指令。因为这是一个公有指令，更具上面的规则它允许在当前模块中使用。它本质上允许对重导出项的公有访问。例如，下面的程序是有效的：

```rust
pub use self::implementation::api;

mod implementation {
    pub mod api {
        pub fn f() {}
    }
}
```

这意味着任何引用`implementation::api::f`的外部包装箱将会受到一个私有性违反，而`api::f`路径则是被允许的。

当重导出一个私有项，它可以被认为是允许缩短“私有链”，而不是按正常情况传递命名空间层次。

### <a name="Attributes"></a>6.3.属性

```
attribute : '#' '!' ? '[' meta_item ']' ;
meta_item : ident [ '=' literal
                  | '(' meta_seq ')' ] ? ;
meta_seq : meta_item [ ',' meta_seq ] ? ;
```

任何项声明都可以有*属性*适用于它。Rust的属性采用ECMA-335中的属性模型，使用来自ECMA-334（C#）的语法。属性是一个通用的，自由形式的元数据，它根据名称，使用公约，语言和编译器版本来翻译。属性可能表现为任何如下：

* 一个单独的标识符，属性名称
* 一个标识符后跟一个等号“`=`”和一个常量，提供一个键/值对
* 一个标识符后跟一个参数化的子属性参数列表

井号（`#`）后带有感叹号（`!`）的属性适用于定义于其中的项。井号后没有感叹号的属性适用于它之后的项。

一个属性的例子：

```rust
// General metadata applied to the enclosing module or crate.
#![crate_type = "lib"]

// A function marked as a unit test
#[test]
fn test_foo() {
  /* ... */
}

// A conditionally-compiled module
#[cfg(target_os="linux")]
mod bar {
  /* ... */
}

// A lint attribute used to suppress a warning/error
#[allow(non_camel_case_types)]
type int8_t = i8;
```

> **注意**：在将来的某个时候，编译器将会区分语言保留的和用户可用的属性。在这之前，在被加载的语法扩展和编译器自带的属性之间并没有有效的区别。

#### <a name="CrateOnlyAttributes"></a>6.3.1.包装箱专有属性

* `crate_name` - 指定包装箱的名称
* `crate_type ` - 详见[链接](#Linkage)
* `feature` - 详见[编译器功能](#CompilerFeatures)
* `no_builtins` - 禁用可能存在的调用库函数时的特定代码模式优化
* `no_main` - 禁用生成`main`符号。在被链接的其它对象定义了`main`是有用
* `no_start` - 禁用链接`native`包装箱，它指定了“start”语言项。
* `no_std` - 禁用链接`std`包装箱
* `plugin` - 加载一系列命名的包装箱作为编译器插件，例如，`#![plugin(foo, bar)]`。可选的每个插件的参数，也就是说，`#![plugin(foo(... args ...))]`，被提供给插件的注册函数。使用这个属性需要`plugin`功能通道。

#### <a name="ModuleOnlyAttributes"></a>6.3.2.模块特有属性

* `no_implicit_prelude` - 禁用在模块中插入`use std::prelude::*`
* `path` -  指定加载模块的文件。`#[path="foo.rs"] mod bar;`等同于`mod bar { /* contents of foo.rs */ }`。路径相对于当前模块的目录。

#### <a name="FunctionOnlyAttributes"></a>6.3.3.函数特有属性

* `main` - 表明这个函数应给被传递给入口点，而不是包装箱根名为`main`的函数
* `plugin_registrar` - 标记这个函数作为[编译器插件](http://doc.rust-lang.org/1.0.0-beta/book/plugins.html)的注册点，例如可加载的语法扩展
* `start` - 表明这个函数应该作为入口点，覆盖“`start`”语言项。详见“`start`”[语言项](#LanguageItems)
* `test` - 表明这个函数是一个测试函数，只在`--test`时被编译
* `should_panic` - 表明这个测试函数应该恐慌，反转成功条件
* `cold` - 这个函数不太可能被执行，所以用不同的方式优化它（和调用它）

#### <a name="StaticOnlyAttributes"></a>6.3.4.静态量特有属性

* `thread_local` - 对于一个`static mut`，这个信号表明这个静态量的值可能会因当前线程而改变。具体的结果由实现定义

#### <a name="FFIAttributes"></a>6.3.5.FFI属性
在一个`extern`块上，如下属性会被翻译：

* `link_args` - 指定传递给链接器的参数，而不仅仅是库名称和类型。这个需要功能通道并且具体行为由实现定义（根据不同的连接器调用语法）
* `link` - 表明一个原生库应该被链接以便这个块中的声明能够被正确链接。`link`支持一个可选的`kind`键值，他有3个可能的值：`dylib`，`static`和`framework`。查看[外部块](#ExternalBlocks)以了解更多。两个例子：`#[link(name = "readline")]`和`#[link(name = "CoreFoundation", kind = "framework")]`

对于`extern`块内的声明，如下属性会被翻译：

* `link_name` - 应该被导入的函数或静态量记号的名字
* `linkage` - 对于静态量，它指定了[链接类型](http://llvm.org/docs/LangRef.html#linkage-types)

对于`enum`：

* `repr` - 在类C语言的枚举中，这设定的用于表现的内部类型。它接受一个参数，它代表这个枚举应该表现为哪种基本类型，或者`C`，它指定应该使用当前平台C ABI默认的`enum`大小。注意C中的枚举表现是未定义的，所以当C语言代码在特定标记下编译时这可能不正确。

对于`struct`：

* `repr` - 指定用于结构体的表现。它接受一系列选项。目前可接受的值是`C`和`packed`，它们也可以组合使用。`C`将会使用C ABI兼容的结构布局，而`packed`会移除字段间的填充（注意这可能是非常脆弱的并且在需要对齐的平台可能会出错）

#### <a name="MacroRelatedAttributes"></a>6.3.6.宏相关属性

* 在`mod`上使用`macro_use` - 这个模块中定义的宏对于父模块可见，在这个模块被引入后
* 在`extern crate`上`macro_use` - 从这个包装箱载入宏。一个可选的名称列表`#[macro_use(foo, bar)]`限制只引入这些名字的宏。`extern crate`必须出现在包装箱根，而不是在`mod`中，这确保了[`$crate`宏变量](http://doc.rust-lang.org/1.0.0-beta/book/macros.html#the-variable-$crate)的功能正常
* 在`extern crate`上使用`macro_reexport` - 重导出命名了的宏
* `macro_export` - 导出一个宏用于跨包装箱使用
* 在`extern crate`上使用`no_link` - 即使我们为这个包装箱载入了宏，也不链接到输出中

查看[book中宏部分](http://doc.rust-lang.org/1.0.0-beta/book/macros.html#scoping-and-macro-import/export)来了解宏的更多信息

#### <a name="MiscellaneousAttributes"></a>6.3.7.各种（其它）属性

* `export_name` - 对于静态量和函数，这确定了导出记号的名称
* `link_section` - 对于静态量和函数，这确定了这个项的内容会出现在目标文件中的哪个部分
* `no_mangle` - 对于任何项，不使用标准的名字改编。设定了对于标识符这个项的记号
* `packed` - 对于结构体或枚举，消除任何用于对齐字段的填充
* `simd` - 对与特定元组结构体，对于算术操作，它使用目标的SIMD指令，如果有的话；需要`simd`功能通道来使用这个属性
* `static_assert` - 对于类型是`bool`的静态量，如果它不初始化为`true`就报错并终止编译
* `unsafe_destructor` - 允许实现“`drop`”语言项当类型并未实现“`send`”语言项时；需要`unsafe_destructor`功能通道来使用这个属性
* `unsafe_no_drop_flag` - 对于结构体，去掉阻止析构函数被运行两次的标记。使用这个属性时同一对象可能会运行多次析构函数。
* `doc` - 像`/// foo`的文档注释等同于`#[doc = "foo"]`
* `rustc_on_unimplemented` - 编写一个自定义的记录作为当某个类型被发现未实现特性的错误信息。你可以使用格式化参数，像`{T}`，`{A}`来代表使用相应特性的类型参数类型相同名称的类型。`{Self}`用来代替应该却未实现特性的类型的名称。为了使用它，需要启用`on_unimplemented`功能通道。

#### <a name="ConditionalCompilation"></a>6.3.8.条件编译
有时你希望对于相同的代码有不同的编译输出，依赖编译目标，例如目标操作系统，或启用发行构建。

这里有两种配置选项，一个是是否定义（`#[cfg(foo)]`），另一个是包含一个字符串用于检查（`#[cfg(bar = "baz")]`）（目前只用编译器定义的配置可以使用后一种形式）。

```rust
// The function is only included in the build when compiling for OSX
#[cfg(target_os = "macos")]
fn macos_only() {
  // ...
}

// This function is only included when either foo or bar is defined
#[cfg(any(foo, bar))]
fn needs_foo_or_bar() {
  // ...
}

// This function is only included when compiling for a unixish OS with a 32-bit
// architecture
#[cfg(all(unix, target_pointer_width = "32"))]
fn on_32bit_unix() {
  // ...
}

// This function is only included when foo is not defined
#[cfg(not(foo))]
fn needs_not_foo() {
  // ...
}
```

这演示了一些使用`#[cfg(...)]`属性可以达成的条件编译。`any`，`all`和`not`可以用来嵌套组成任意复杂的配置。

如下配置必须由实现定义：

* `target_arch = "..."`。目标CPU构架，例如`x86`，`x86_64`，`mips`，`powerpc`，`arm`或`aarch64`
* `target_endian = "..."`。目标CPU的字节序，要么是`little`要么是`big`
* `target_family = "..."`。目标操作系统家族，例如`"unix"`或`"windows"`。这些配置选项自身被定义为配置，像`unix`或`windows`
* `target_os = "..."`。目标操作系统，例如包括`"win32"`，`"macos"`，`"linux"`，`"android"`，`"freebsd"`，`"dragonfly"`，`"bitrig"`或`"openbsd"`
* `target_pointer_width = "..."`。目标指针的位长度。`32`针对32位指针，同理`64`针对64位指针
* `unix`。见`target_family`
* `windows`。见`target_family`

#### <a name="LintCheckAttributes"></a>6.3.9.Lint检查属性
Lint检查命名了一个潜在的不合适的代码模式，例如不可达代码或被省略的文档，对于那些指定了属性的静态实体。

对于任何`C`的lint检查：

* `allow(C)`覆盖了`C`的检查所以违规将不会被报告
* `deny(C)`表明在遇到`C`的违规后产生一个错误
* `forbid(C)`与`deny(C)`相同，不过也禁止了改变之后的lint级别
* `warn(C)`对于`C`的违规提出警告并继续编译

编译器支持的lint检查可以通过`rustc -W help`找到，以及它们的默认设定。[编译器插件](http://doc.rust-lang.org/1.0.0-beta/book/plugins.html#lint-plugins)可以提供额外的lint检查。

```rust
mod m1 {
    // Missing documentation is ignored here
    #[allow(missing_docs)]
    pub fn undocumented_one() -> i32 { 1 }

    // Missing documentation signals a warning here
    #[warn(missing_docs)]
    pub fn undocumented_too() -> i32 { 2 }

    // Missing documentation signals an error here
    #[deny(missing_docs)]
    pub fn undocumented_end() -> i32 { 3 }
}
```

这个例子展示了如何使用`allow`与`warn`来切换一个特定检查的开关：

```rust
#[warn(missing_docs)]
mod m2{
    #[allow(missing_docs)]
    mod nested {
        // Missing documentation is ignored here
        pub fn undocumented_one() -> i32 { 1 }

        // Missing documentation signals a warning here,
        // despite the allow above.
        #[warn(missing_docs)]
        pub fn undocumented_two() -> i32 { 2 }
    }

    // Missing documentation signals a warning here
    pub fn undocumented_too() -> i32 { 3 }
}
```

这个例子展示了如何使用`forbid`在lint检查中禁止`allow`的使用：

```rsut
#[forbid(missing_docs)]
mod m3 {
    // Attempting to toggle warning signals an error here
    #[allow(missing_docs)]
    /// Returns 2.
    pub fn undocumented_too() -> i32 { 2 }
}
```

#### <a name="LanguageItems"></a>6.3.10.语言项
Rust代码中定义了一些基本Rust操作，而不是直接用C或汇编语言实现。这样定义的操作便于编译器查找。`lang`语言项使这样的定义变得可能。例如，Rust标准库中的`str`模块定义字符串相等函数：

```rust
#[lang="str_eq"]
pub fn eq_slice(a: &str, b: &str) -> bool {
    // details elided
}
```

`str_eq`对于Rust编译器有特殊的意义，并且这个属性的出现意味着将会使用这个定义生成字符串相等函数的调用。

一个完整的内建语言项的列表将在未来被添加。

#### <a name="InlineAttributes"></a>6.3.11.内联属性
内联属性用来建议编译器进行一个内联扩展并在调用者中放置函数或静态量的拷贝而不是不是生成函数调用和对静态量定义的位置的访问。

编译器基于内部启发来自动的内联函数。不正确的内联函数确实会使程序变慢，所以应该谨慎使用。

不可变静态量总是被认为是可内联的除非被标记为`#[inline(never)]`。当两个不同的可内联静态量拥有相同内存地址时的行为是未定义的。换句话说，编译器可以自由的将这这两个重复的静态量折叠在一起。

`#[inline]`和`#[inline(always)]`总是使得函数被序列化进包装箱的元数据中以允许跨包装箱内联。

这有3种不同类型的内联属性：

* `#[inline]`提示编译器进行一个内联展开
* `#[inline(always)]`请求编译器总是进行一个内联展开
* `#[inline(never)]`请求编译器从不进行一个内联展开

#### <a name="Derive"></a>6.3.12.`derive`属性
`derive`属性允许指定特性自动为数据结构实现。例如，下面会为`Foo`创建一个`PartialEq`和`Clone`特性的`impl`，类型参数`T`将会得到带有`PartialEq`和`Clone`限制的合适的`impl`：

```rust
#[derive(PartialEq, Clone)]
struct Foo<T> {
    a: i32,
    b: T
}
```

生成的`PartialEq`的`impl`等同于

```rsut
impl<T: PartialEq> PartialEq for Foo<T> {
    fn eq(&self, other: &Foo<T>) -> bool {
        self.a == other.a && self.b == other.b
    }

    fn ne(&self, other: &Foo<T>) -> bool {
        self.a != other.a || self.b != other.b
    }
}
```

#### <a name="CompilerFeatures"></a>6.3.13.编译器功能
Rust的特定部分可能被实现在编译器中，不过它们不一定能够被日常使用。这些功能经常是“原型质量”或“基本上能用在生产环境”，不过也可能不够稳定到能被认为是一个成熟的语言功能。

为此，Rust可以识别一个特殊的包装箱级别的属性形式：

```rust
#![feature(feature1, feature2, feature3)]
```

这个指令通知编译器这个功能列表：`feature1`，`feature2`和`feature3`应该都被启用。这个只在包装箱级别被识别，不在模块级别。没有这个指令，所有的功能被认为是关闭的，同时使用这些功能会导致编译错误。

目前实现了的编译器功能有：

* `advanced_slice_patterns` - 详见[匹配表达式](#MatchExpressions)；具体的片段模式语义倾向于改变，所以一些类型还不稳定
* `slice_patterns` - 可用，不过实际上，片段模式是灰常糟糕并完全不稳定的
* `asm` - `asm!`宏提供了一个内联汇编的手段。这常常是有用的，不过具体语法和它的语义可能会改变，所以请选择性的使用这个宏
* `associated_types` - 允许特性中的类型别名。实验性功能
* `box_patterns` - 允许`box`模式，具体的语义倾向于改变
* `box_syntax` - 允许使用`box`表达式，具体的语义倾向于改变
* `concat_idents` - 允许使用`concat_idents`宏，对于连接标识符它在很多方面都是不足的，所以可能直接完全移除是更合理的
* `custom_attribute` - 允许使用编译器未知的属性这样新属性可以以后向兼容的方式添加进来（RFC 572）
* `custom_derive` - 允许使用`#[derive(Foo,Bar)]`作为`#[derive_Foo] #[derive_Bar]`的语法糖，它可以作为用户定义的语法扩展
* `intrinsics` - 允许使用“rust-intrinsics”ABI。编译器固有功能是天生不稳定的并且不能保证它们干了啥
* `lang_items` - 允许使用`#[lang]`属性。就像`intrinsics`，语言项是天生不稳定的并且不能保证它们干了啥
* `link_args` - 这个属性用于指定传递给连接器的自定义标记，不过强烈不建议使用。编译器对于系统连接器的使用并不保证在将来持续，并且如果不使用系统连接器传递自定义标记也没有多少意义
* `link_llvm_intrinsics` - 允许通过`#[link_name="llvm.*"]`链接LLVM的固有功能
* `linkage` - 允许使用`linkage`属性，它是不可移植的
* `log_syntax` - 允许使用`log_syntax`宏属性，它有点很强的hack味道所以确定会被移出
* `main` - 允许`#[main]`属性的使用，它改变Rust程序的入口点。这个功能倾向于改变
* `macro_reexport` - 允许宏在被一个包装箱导入后从另一个包装箱中重导出。记住这个功能开始仅仅被设计为在Rust标准库中使用，并且倾向于改变
* `non_ascii_idents` - 使编译器支持使用非ASCII标识符，不过它的实现还十分粗糙，所以它可以看作是一个实验性功能直到标识符的规格完全出炉为止
* `no_std` - 允许使用`#![no_std]`包装箱属性，它禁用隐式的`extern crate std`。它特别的要求使用`libstd`“外观”下的不稳定API例如`libcore`和`libcollections`。当使用语法扩展时可能会产生问题，包括`#[derive]`
* `on_unimplemented` - 允许使用`#[rustc_on_unimplemented]`属性，它允许为特性定义添加特定的记录作为期望但未找到实现时的错误信息
* `optin_builtin_traits` - 允许定义默认和负（negative）的特性实现。实验性功能
* `plugin` - 使用[编译器插件](http://doc.rust-lang.org/1.0.0-beta/book/plugins.html)来自定义lint检查或语法扩展。这依赖编译器内部细节并倾向于改变
* `plugin_registrar` - 表明一个包装箱提供了[编译器插件](http://doc.rust-lang.org/1.0.0-beta/book/plugins.html)
* `quote` - 允许使用`quote_*!`系列宏，它们的实现非常差并会被修改为一个更合适的实现
* `rustc_attrs` - 启用内部的`#[rustc_*]`属性，它只供内部使用或可能在将来增加意义
* `rustc_diagnostic_macros` - 一个神秘的功能，用在`rustc`的实现中，对一般用户没有意义
* `simd` - 允许使用`#[simd]`属性，它过度的简单并不是我们想长期维护的SIMD接口
* `simd_ffi` - 允许在外部函数的签名中使用SIMD向量。SIMD接口倾向于改变
* `staged_api` - 允许使用稳定性标记和包装箱的`#![staged_api]`属性
* `static_assert` - `#[static_assert]`的功能是实验性和不稳定的。这个属性可以附加到`static`的`bool`类型上并且编译器会在`bool`在编译时为`false`时报错。这个功能的这个版本是不直观和不合意的
* `start` - 允许使用`#[start]`属性，它改变Rust程序的入口点。这个功能，特别是被标记函数的签名，倾向于改变
* `struct_inherit` - 允许使用结构体继承，它勉强被实现并可能被移除。不要使用它
* `struct_variant` - 机构化枚举变量（带有命名字段的）。目前还不知道这个枚举变量样式是否能作为元组形式被完全支持，并且也不确定这样的变量是否应该在语言中保留。到目前为止这个变量形式被隐藏在功能标记之后
* `thread_local` - `#[thread_local]`属性的使用时实验性的并被认为是不稳定的。这个属性用来定义一个`static`作为独特的各线程平衡的LLVM实现，它在内核加载器和动态链接器中一致工作。这并不一定在所有平台有效，并且使用也是不受鼓励的
* `trace_macros` - 允许`trace_macros`宏的使用，它有很重的hack倾向并确定会被移除
* `unboxed_closures` - Rust新的闭包设计，它目前正在实现中并带有很多已知的bug
* `unsafe_destructor` - 允许使用`#[unsafe_destructor]`属性，它被广泛的认为不安全并在语言的改进中将会过时
* `unsafe_no_drop_flag` - 允许使用`#[unsafe_no_drop_flag]`属性，它移除了实现了`Drop`特性的类型的隐藏标记。`Drop`标记的设计倾向于改变，并且这个功能将来可能会被移除
* `unmarked_api` - 允许使用`#![staged_api]`包装箱中的那些没有稳定标记的项。这些项不被编译器允许存在，所以如果你需要它很有可能是一个编译器bug
* `visible_private_types` - 允许公有API暴露私有类型，例如，作为公有函数的返回值。这个功能将来可能会被移除
* `allow_internal_unstable` - 允许`macro_rules!`宏被`#[allow_internal_unstable]`属性标记，设计为允许`std`宏在内部调用`#[unstable]`/功能通道后的功能而不影响调用者（也就是说，就封装而言让它们表现起来更像函数调用）

如果一个功能被提升为语言功能，那么所有现存的程序将会开始收到关于被启用的新功能的`#[feature]`指令的编译警告（因为这个指令已不再需要）。然而，如果一个功能被决定应该从语言中移除，将会产生一个错误（如果之前没有一个解析错误的话）。这种情况下指令不再必须，并且如果这些功能不被移除的话现存代码可能会出错。

如果在指令中发现了未知功能，将导致一个编译错误。一个未知功能是指还未被编译器识别的功能。

## <a name="StatementsAndExpressions"></a>7.语句和表达式
Rust*主要*是一个表达式语言。这意味着大部分形式的产生值或产生作用的计算都直接被统一的*表达式*语法分类管理。每种表达式通常都可以*嵌套*在其它类型的表达式之中，并且评估表达式涉及到指定表达式产生的值和自表达式的顺序的规则是自我评估的。

相反的，Rust中的语句*大多*用于包含和显式的排序表达式计算。

### <a name="Statements"></a>7.1.语句
一个*语句*是一个块的一部分，块则是外部[表达式](#Expressions)或[函数](#Functions)的一部分。

Rust有两种类型的语句：[声明语句](#DeclarationStatements)和[表达式语句](#ExpressionStatements)。

#### <a name="DeclarationStatements"></a>7.1.1.声明语句
一个*声明语句*在内部的语句块中引入了1个或多个*名称*。声明的名称可能代表新的位置或新的项。

##### <a name="ItemDeclarations"></a>7.1.1.1.项声明
一个*项声明语句*是一个与模块中[项](#Items)声明句法上相同的形式。声明一个项 - 函数，枚举，结构体，类型，静态量，特性，实现或模块 - 在一个语句中是一个简单的将它的作用域限制在一个包含它所有使用的狭小空间里的方法；否则它在意义上等同于在语句块外声明项。

> **注意**：这并不隐式的捕获函数的动态环境当声明一个函数本地的项时。

##### <a name="SlotDeclarations"></a>7.1.1.2.位置声明

```
let_decl : "let" pat [':' type ] ? [ init ] ? ';' ;
init : [ '=' ] expr ;
```

一个*位置声明*引入一个新的位置的集合，由一个模式提供。这个模式可以后跟一个类型标记，和/或一个初始化表达式。当没有类型标记时，编译器会推断它的类型，或发出一个错误如果用于明确推断的类型信息是不足时。任何通过位置声明引入的位置从声明的点到包含它的块区域末尾都是可见的。

#### <a name="ExpressionStatements"></a>7.1.2.表达式语句
一个*表达式语句*评估一个[表达式](#Expressions)但忽略它的结果。一个表达式语句`e;`的类型总是`()`,不管`e`的类型是什么。作为一个规则，一个表达式语句的目的是切换评估表达式的作用。

### <a name="Expressions"></a>7.2.表达式
一个表达式可以有两个作用：它总是产生一个*值*，它可能产生*作用*（或者被认为是“副作用”）。一个表达式*计算*出一个值，并在*计算过程*中产生作用。很多表达式包含子表达式（操作数）。每种类型的表达式的意义表明了如下内容：

* 当评估表达式时是否评估子表达式
* 评估子表达式时的顺序
* 如何合并子表达式的值来得到表达式的值

这样，表达式的结构明确了执行的结构。块只是另一种形式的表达式，所以块，语句，表达式和块可以在任意深度递归嵌套进它们之中。

##### <a name="LvaluesRvaluesAndTemporaries"></a>7.2.0.1.左值，右值和临时值
表达式可以分为两个主要的分类：*左值*和*右值*。同理在每个表达式中，子表达式是也会出现在*左值上下文*或*右值上下文*中。表达式的计算依赖它自己的分类和它出现的上下文。

一个左值是一个代表了内存位置的表达式。这个表达式是[路径](#PathExpressions)（引用本地变量，函数和方法参数，或静态变量），解引用（`*expr`），[索引表达式](#IndexExpressions)（`expr[expr]`），和[字段引用](#FieldExpressions)（`expr.f`）。所有其它的表达式是右值。

一个[赋值](#AssignmentExpressions)或[复合赋值](#CompoundAssignmentExpressions)表达式的左操作数是一个左值上下文，就像一个单独的单目[借用](#UnaryOperatorExpressions)运算符。所有其它的表达式上下文是右值上下文。

当一个左值在*左值上下文*被计算时，它代表一个内存位置，当在*右值上下文*被计算时，它代表*储存在*那个内存位置的值。

当一个右值用在左值上下文中时，会产生一个临时的未命名的左值并被使用。这个历时量的生命周期等于任何指向它的引用的最长的一个。

##### <a name="MovedAndCopiedTypes"></a>7.2.0.2.移动和拷贝类型
当一个[局部变量](#MemorySlots)被作为一个[右值](#LvaluesRvaluesAndTemporaries)使用时它要么被移动要么被拷贝，根据它的类型。所有实现了`Copy`的类型被拷贝，所有其它的被移动。

#### <a name="LiteralExpressions"></a>7.2.1.常量表达式
一个*常量表达式*包含一个我们之前描述过的[常量](#Literals)形式。它直接描述了一个数字，字符，字符串，布尔值，或者单元值。

```rust
();        // unit type
"hello";   // string type
'5';       // character type
5;         // integer type
```

#### <a name="PathExpressions"></a>7.2.2.路径表达式
一个[路径](#Paths)用在表达式上下文中代表一个局部变量或一个项。路径表达式是[左值](#LvaluesRvaluesAndTemporaries)。

#### <a name="TupleExpressions"></a>7.2.3.元组表达式
元组写作用括号包含的逗号分隔的0个或多个表达式。它们用来创建[元组类型](#TupleTypes)值。

```rust
(0,);
(0.0, 4.5);
("a", 4us, true);
```

#### <a name="UnitExpressions"></a>7.2.4.单元表达式
`()`表达式代表一个*单元类型值*，唯一一个类型和值相同名字的类型。

#### <a name="StructureExpressions"></a>7.2.5.结构体表达式

```rust
struct_expr : expr_path '{' ident ':' expr
                      [ ',' ident ':' expr ] *
                      [ ".." expr ] '}' |
              expr_path '(' expr
                      [ ',' expr ] * ')' |
              expr_path ;
```

这有多种形式的结构体表达式。一个*结构体表达式*包含一个[结构体项](#Structures)的[路径](#Paths)，后跟一个大括号包围的逗号分隔的0个或多个键值对的列表，提供了结构体新实例的字段值。一个字段名字可以是任何标识符，并被一个分号与它的值的表达式分开。有且只有结构体是可变的时结构体字段代表的位置才可以改变。

一个*元组结构体表达式*包含一个[结构体项](#Structures)的[路径](#Paths)，后跟一个括号包围的口号分隔的0个或多个表达式的列表（换句话说，结构体表项的路径后跟一个元组表达式）。结构体项必须是一个元组结构体项。

一个*类单元结构体表达式*只包含[结构体项](#Structures)的[路径](#Paths)。

下面是结构体表达式的例子：

```rust
Point {x: 10.0, y: 20.0};
TuplePoint(10.0, 20.0);
let u = game::User {name: "Joe", age: 35, score: 100_000};
some_fn::<Cookie>(Cookie);
```

一个结构体表达式组成了一个新的指定名称的结构体类型的值。注意对于一个给定的*类单元*结构体类型，它们总是相同的类型。

可以结构体表达式可以以`..`后跟代表函数更新的表达式的语法来结尾。`..`后面的表达式（`base`）必须与这个新生成的结构体类型相同。整个表达式代表的结果是创建了一个新的结构体（与`base`表达式一样的类型），其中被给出的字段值被显示指定而其它字段的值则由`base`表达式中的值提供。

```rust
let base = Point3d {x: 1, y: 2, z: 3};
Point3d {y: 0, z: 10, .. base};
```

#### <a name="BlockExpressions"></a>7.2.6.块表达式

```
block_expr : '{' [ stmt ';' | item ] *
                 [ expr ] '}' ;
```

一个*块表达式*类似于模块如果这个声明是可能的话。每个块在概念上引入了一个新的命名空间域。`use`项可以向项中引入新的名称而声明的项只在块的作用域之内。

一个块会按顺序执行每一条语句，然后再执行表达式（如果有的话）。如果块以语句结束，它的值是`()`：

```rust
let x: () = { println!("Hello."); };
```

如果它以表达式结束，它的值和类型都是表达式的：

```rust
let x: i32 = { println!("Hello."); 5 };

assert_eq!(5, x);
```

#### <a name="MethodCallExpressions"></a>7.2.7.方法调用表达式

```
method_call_expr : expr '.' ident paren_expr_list ;
```

一个*方法调用*包含应给表达式后跟一个单独的点，一个标识符，和括号包围的表达式列表。方法调用被解析为特定特性上的方法，要么静态分发给一个函数，如果左边的`self`的类型是明确知道的；要么动态分发，如果左侧表达式是一个间接的[对象类型](#ObjectTypes)。

#### <a name="FieldExpressions"></a>7.2.8.字段表达式

```
field_expr : expr '.' ident ;
```

一个*字段表达式*包含一个表达式后跟一个单独的点和一个标识符，不过不直接后跟一个括号包围的表达式列表（后者是[方法调用表达式](#MethodCallExpressions)）。一个字段表达式代表[结构体](#StructureTypes)的一个字段。

```
mystruct.myfield;
foo().x;
(Struct {a: 10, b: 20}).a;
```

一个字段访问是一个引用字段值的[左值](#LvaluesRvaluesAndTemporaries)。当字段的类型是可变的，它可以被[赋值](#AssignmentExpressions)。

另外，如果表达式点左边的类型是一个指针的话，它会自动解引用来使访问字段变得可能。

#### <a name="ArrayExpressions"></a>7.2.9.数组表达式

```
array_expr : '[' "mut" ? array_elems? ']' ;

array_elems : [expr [',' expr]*] | [expr ';' expr] ;
```

一个[数组](#ArrayAndSliceTypes)表达式写作方括号包围的逗号分隔的0个或多个相同类型的表达式。

在`[expr ';' expr]`的形式中，`';'`之后的表达式必须是可以在编译时评估的常量表达式，例如[常量](#Literals)或[静态项](#StaticItems)。

```rust
[1, 2, 3, 4];
["a", "b", "c", "d"];
[0; 128];              // array with 128 zeros
[0u8, 0u8, 0u8, 0u8];
```

#### <a name="IndexExpressions"></a>7.2.10.索引表达式

```
idx_expr : expr '[' expr ']' ;
```

[数组](#ArrayAndSliceTypes)类型表达式可以在后面写一个方括号包围的表达式来进行索引。当数组是可变的，结果的[左值](#LvaluesRvaluesAndTemporaries)可以被赋值。

索引是从0开始的，并且可以是任何整数类型。向量的访问在运行时进行边界检查。当这个检查失败时，它会将这个线程设置为*恐慌状态*。

```rust
([1, 2, 3, 4])[0];
(["a", "b"])[10]; // panics
```

#### <a name="UnaryOperatorExpressions"></a>7.2.11.单目运算符表达式
Rust定义了3个单目运算符。它们都写作前缀运算符，写在它们适用的表达式之后。

* `-`：取反。只能用于数字类型
* `*`：解引用。当作用于一个[指针](#PointerTypes)时它代表指针指向的位置。对于可变位置的指针，结果的[左值](#LvaluesRvaluesAndTemporaries)可以被赋值。对于非指针类型，它调用`std::ops::Deref`特性的`deref`方法，或者`std::ops::DerefMut`特性的`deref_mut`方法（如果它被类型实现了并且要求外层表达式将会或可能使解引用可变），并且产生一个从重载方法返回的解引用的`&`或`&mut`借用的指针作为结果
* `!`：逻辑取反。对于布尔类型，它在`true`和`false`之间切换。对于整形类型，它反转值的补码表示的每个单独的位

#### <a name="BinaryOperatorExpressions"></a>7.2.12.双目运算符表达式

```
binop_expr : expr binop expr ;
```

双目运算符按照[运算符优先级](#OperatorPrecedence)给出。

##### <a name="ArithmeticOperators"></a>7.2.12.1.算术运算符
双目运算符是调用内部特性的语法糖，定义在`std`库的`std::ops`模块中。这意味着算术运算符可以被用户定义类型重写。对于标准类型的运算符的默认意义提供如下。

* `+`：加法和数组/字符串连接。调用`std::ops::Add`特性的`add`方法
* `-`：减法。调用`std::ops::Sub`特性的`sub`方法
* `*`：乘法。调用`std::ops::Mul`特性的`mul`方法
* `/`：除法。调用`std::ops::Div`特性的`div`方法
* `%`：取余。调用`std::ops::Rem`特性的`rem`方法

##### <a name="BitwiseOperators"></a>7.2.12.2.二进制运算符
就像[算术运算符](#ArithmeticOperators)，二进制运算符是调用内部特性的语法糖。这意味着二进制运算符可以被用户定义类型重写。对于标准类型的运算符的默认意义提供如下。

* `&`：和。调用`std::ops::BitAnd`特性的`bitand`方法
* `|`：或。调用`std::ops::BitOr`特性的`bitor`方法
* `^`：异或。调用`std::ops::BitXor`特性的`bitxor`方法
* `<<`：左移。调用`std::ops::Shl`特性的`shl`方法
* `>>`：右移。调用`std::ops::Shr`特性的`shr`方法

##### <a name="LazyBooleanOperators"></a>7.2.12.3.惰性布尔运算符
`||`和`&&`可以作用于布尔类型的操作数。`||`操作符代表逻辑“或”，而`&&`操作符代表逻辑“和”。它们与`|`和`&`不同的地方在于右侧的操作数只在左侧操作数不能确定表达式结果时才被计算。这就是说，`||`只在左操作数计算为`false`时计算右操作数，而`&&`只在计算为`true`的时候。

##### <a name="ComparisonOperators"></a>7.2.12.4.比较运算符
比较运算符，就像[算术运算符](#ArithmeticOperators)，和[二进制运算符](#BitwiseOperators)，是调用内部特性的语法糖。这意味着比较运算符可以被用户定义类型重写。对于标准类型的运算符的默认意义提供如下。

* `==`：等于。调用`std::ops::Shr`特性的`shr`方法
* `!=`：不等于。调用`std::ops::Shr`特性的`shr`方法
* `<`：小于。调用`std::ops::Shr`特性的`shr`方法
* `>`：大于。调用`std::ops::Shr`特性的`shr`方法
* `<=`：小于等于。调用`std::ops::Shr`特性的`shr`方法
* `>=`：大于等于。调用`std::ops::Shr`特性的`shr`方法

##### <a name="TypeCastExpressions"></a>7.2.12.5.类型转换表达式

一个类型转换表达式表现为双目运算符`as`。

执行`as`表达式将左侧的值转换为右侧的类型。

一个`as`表达式的例子：

```rsut
fn avg(v: &[f64]) -> f64 {
  let sum: f64 = sum(v);
  let sz: f64 = len(v) as f64;
  return sum / sz;
}
```

##### <a name="AssignmentExpressions"></a>7.2.12.6.赋值表达式
一个*赋值表达式*包含一个[左值](#LvaluesRvaluesAndTemporaries)表达式后跟一个等号（`=`）和一个[右值](#LvaluesRvaluesAndTemporaries)表达式。

计算一个赋值表达式[拷贝或移动](#MovedAndCopiedTypes)右侧操作数到左侧操作数。

```rust
x = y;
```

##### <a name="CompoundAssignmentExpressions"></a>7.2.12.7.复合赋值表达式
`+`，`-`，`*`，`/`，`%`，`|`，`^`，`<<`和`>>`运算符可以与`=`运算符复合。`lval OP= val`表达式等同于`lval = lval OP val`运算符。例如`x = x + 1`可以写成`x += 1`。

任何这样的表达式总是[单元](#PrimitiveTypes)类型

##### <a name="OperatorPrecedence"></a>7.2.12.8.运算符优先级
Rust双目运算符优先级顺序如下，从强到弱降序：

```rust
as
* / %
+ -
<< >>
&
^
|
== != < > <= >=
&&
||
= ..
```

相同优先级的运算符从左到右顺序计算。同级的[单目运算符](#UnaryOperatorExpressions)强于任何双目运算符。

#### <a name="GroupedExpressions"></a>7.2.13.组合表达式
一个包含在括号中的表达式代表它它内部表达式的结果。括号可以用来显示的指定表达式内的计算顺序。

```
paren_expr : '(' expr ')' ;
```

一个括号表达式的例子：

```rust
let x: i32 = (2 + 3) * 4;
```

#### <a name="CallExpressions"></a>7.2.14.调用表达式

```
expr_list : [ expr [ ',' expr ]* ] ? ;
paren_expr_list : '(' expr_list ')' ;
call_expr : expr paren_expr_list ;
```

一个*调用表达式*调用一个函数，提供0个或多个输入位置和一个可选的引用位置作为函数的输出，绑定为调用右侧的`lval`。如果函数最终返回，那么表达式完成。

一些调用表达式的例子：

```rust
let x: i32 = add(1i32, 2i32);
let pi: Result<f32, _> = "3.14".parse();
```

#### <a name="CallExpressions"></a>7.2.15.Lambda表达式

```
ident_list : [ ident [ ',' ident ]* ] ? ;
lambda_expr : '|' ident_list '|' expr ;
```

一个*lambda表达式*（有时叫做“匿名函数表达式”）定义了一个函数并代表一个值，在一个单独的表达式里。一个lambda表达式是一个管道符号分隔（`|`）的标识符列表后跟一个表达式。

一个lambda表达式代表了一个映射了一系列参数（`ident_list`）在后跟`ident_list`的表达式之上的函数。`ident_list`之中标识符是函数的参数。参数类型并不需要指定，因为编译器可以从上下文中推断。

lambda表达式在向其它函数传递函数作为参数时最有用，作为一个定义和捕获函数的简写。

特别的，lambda表达式*捕获它所处的环境*，而正规的[函数定义](#Functions)则不行。捕捉到的类型依赖lambda表达式推断的[函数类型](#FunctionTypes)。对于最简单和最廉价形式（类似于一个`|| { }`表达式），lambda表达式通过引用捕获它的环境，有效的借用所有在函数中提到的外部变量。另外，编译器可能会推断一个lambda表达式应该从它的环境拷贝或移动值到它捕获的环境。

在这个例子中，我们定义了一个`ten_times`函数，它获取一个高层次的函数作为参数，并用一个作为参数的lambda表达式来调用它：

```rust
fn ten_times<F>(f: F) where F: Fn(i32) {
    let mut i = 0i32;
    while i < 10 {
        f(i);
        i += 1;
    }
}

ten_times(|j| println!("hello, {}", j));
```

#### <a name="WhileLoops"></a>7.2.16.While循环

```
while_expr : [ lifetime ':' ] "while" no_struct_literal_expr '{' block '}' ;
```

一个`while`循环以计算布尔循环条件表达式开始。如果循环条件表达式值为`true`，循环体执行并将控制返回给循环条件表达式。如果循环条件表达式值为`false`，`while`表达式结束。

一个栗子：

```rust
let mut i = 0;

while i < 10 {
    println!("hello");
    i = i + 1;
}
```
