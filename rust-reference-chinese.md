# Rust 参考手册

* [1.介绍](#Introduction)
* [2.标记法](#Notation)
  * [2.1.Unicode组合](#UnicodeProductions)
  * [2.2.字符串表组合](#StringTableProductions)

## <a name="Introduction"></a>1.介绍
本文档是Rust编程语言的主要参考。它提供3种类型的材料：

* 非正式的介绍了每种语言结构和其用法的章节
* 非正式的介绍了内存模型，并发模型。运行时服务，链接模型和调试工具的章节
* 提供了影响了Rust设计的基础原理和语言参考的附录章节

本文档并不充当语言的介绍。它假设你熟悉Rust语言的背景基础。一个单独的[book](http://doc.rust-lang.org/book/)可以帮助你熟悉这些背景。

本文档也不充当包含在Rust语言发行版中的[标准库](http://doc.rust-lang.org/std/)的参考。这些库的文档来源于从源码中抽取的文档属性。很多你期望是语言功能的部分可能是Rust库的功能，所以你应该在库文档中寻找，而不是在这。

你可能也对[语法](http://doc.rust-lang.org/grammar.html)感兴趣。

## <a name="Notation"></a>2.标记法
Rust的语法定义在Unicode代码点之上，每个字符通常记作`U+XXXX`，由4个或更多十六进制整数`X`组成。大部分Rust语法被限制在Unicode的ASCII范围，并且在本文档中被描述为扩展巴科斯范式（EBNF）的一个方言，准确的说应该是能被像`llgen`这样的通用自动LL(k)解析工具支持的EBNF的一个方言。这个方言可以自我参考的定义为如下：

```
grammar : rule + ;
rule    : nonterminal ':' productionrule ';' ;
productionrule : production [ '|' production ] * ;
production : term * ;
term : element repeats ;
element : LITERAL | IDENTIFIER | '[' productionrule ']' ;
repeats : [ '*' | '+' ] NUMBER ? | NUMBER ? | '?' ;
```

其中：

* 语法中的空白将被忽略
* 方括号用于组合规则
* `LITERAL`是一个单独可打印的ASCII字符，或者一个转义的十六进制形式的ASCII码`\xQQ`，在单引号中，指代的对应的Unicode代码点是`U+00QQ`
* `IDENTIFIER`是一个非空的ASCII字母或下划线的字符串。
* `repeat`适用于临近`element`，规则如下：
  * `?`代表0次或1次重复
  * `*`代表0次或多次重复
  * `+`代表1次或多次重复
  * `NUMBER`尾随一个重复符号给出了最大的重复次数
  * `NUMBER`自身给出了一个准确的重复次数
  
我们希望多数读者应该熟悉这个EBNF方言。

### <a name="UnicodeProductions"></a>2.1.Unicode组合
一些Rust语法中的组合允许Unicode代码点超越ASCII范围。我们根据Unicode标准指定的字符属性定义这些组合，而不是采用ASCII范围的代码点。[特殊Unicode组合](#UnicodeProductions)部分列出了这些组合。

### <a name="StringTableProductions"></a>2.2.字符串表组合
语法中的一些规则 - 特别的像[单目运算符](http://doc.rust-lang.org/reference.html#unary-operator-expressions)，[二进制运算符](http://doc.rust-lang.org/reference.html#binary-operator-expressions)和[关键字](http://doc.rust-lang.org/reference.html#keywords) - 表现为一种简单的形式：作为一串非引用，可打印的空白分隔的字符串表。这些例子是[记号](#Tokens)规则的子集，并假设为编译器的词法分析的结果，由一个DFA（确定有限状态自动机）驱动，通过分离所有这些字符串表入口执行。

当语法中出现一个在双引号（`"`）中的字符串时，它是字符串表组合中的一个单独成员的隐式引用。查看[记号](#Tokens)以获取更多信息。

## <a name="LexicalStructure"></a>3.词法结构

### <a name="InputFormat"></a>3.1.输入格式
Rust的输入被解释为一串UTF-8编码的Unicode代码点。大部分Rust语法规则根据ASCII范围的代码点定义，不过一小部分根据Unicode属性或显示代码点列表定义。

### <a name="SpecialUnicodeProduction"></a>3.2.特殊Unicode组合
Rust语法中的如下这些组合根据Unicode属性定义：`ident`，`non_null`，`non_star`，`non_eol`，`non_slash_or_star`，`non_single_quote`和`non_double_quote`。

#### <a name="Identifiers"></a>3.2.1.标识符
`ident`组合是如下形式的任何非空Unicode字符串：

* 第一个字符拥有`XID_start`属性
* 其它字符拥有`XID_continue`属性

以上组合*不能*出现在[关键字](#Keywords)中。

> **注意**：`XID_start`和`XID_continue`作为一个字符属性包含了用来组成更有名的C和JavaScript家族的标识符的字符集。

#### <a name="DelimiterRestrictedProductions"></a>3.2.2.界定限制组合
一些组合通过排除特定Unicode字符定义：

* `non_null`是任何不是`U+0000`（null）的单个Unicode字符
* `non_eol`是`non_null`排除`U+000A`（`\n`）
* `non_star`是`non_null`排除`U+002A`（`*`）
* `non_slash_or_star`是`non_null`排除`U+002F`（`/`）和`U+002A`（`*`）
* `non_single_quote`是`non_null`排除`U+0027`（`'`）
* `non_double_quote`是`non_null`排除`U+0022`（`"`）

### <a name="Comments"></a>3.3.注释

```
comment : block_comment | line_comment ;
block_comment : "/*" block_comment_body * "*/" ;
block_comment_body : [block_comment | character] * ;
line_comment : "//" non_eol * ;
```

Rust代码中的注释大体上遵循C++风格的行和块注释形式。嵌套块注释也被支持。

行注释以正好*3*个斜杠开头，和块注释以在块开始处正好一个斜杠和两个星号开头（`/**`），它们被作为`doc`属性的一个特殊语法解析。这就是说，它们等同于在注释体周围写上`#[doc="..."]`（这包含了注释字符自身，也就是说`/// Foo`等同于`#[doc="/// Foo"]`）。

`//!`注释适用于注释对象的父对象，而不是它之后的对象本身。`//!`注释通常用于在包装箱索引页显示信息。

非文档注释被当作空白的形式解析。

### <a name="Whitespace"></a>3.4.空白

```
whitespace_char : '\x20' | '\x09' | '\x0a' | '\x0d' ;
whitespace : [ whitespace_char | comment ] + ;
```

`whitespace_char`组合是任何非空Unicode字符串包含任何下述Unicode字符：`U+0020`（空格，`' '`），`U+0009`（制表符，`'\t'`），`U+000A`（换行符，`'\n'`）和`U+000D`（回车，`'\r'`）。

Rust是一个“自由形式”语言，这意味着任何形式的空格只用来分隔语法中的*记号*，并无语义意义。

Rust程序中如果每个空白元素被替换任何其它空格元素时它们仍有相同的意义，比如一个单独的空格字符。

### <a name="Tokens"></a>3.5.记号

```
simple_token : keyword | unop | binop ;
token : simple_token | ident | literal | symbol | whitespace token ;
```

记号是语法中的主要组合，使用正则（非递归）语言定义。“简单”记号采用[字符串表组合](#StringTableProductions)形式，并作为双引号引用的字符串出现在剩下的语法中。其它记号则有确定的规则。

#### <a name="Keywords"></a>3.5.1.关键字

|   |  |  |  |  |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| abstract  | alignof  | as  | become  | box  |
| break  | const  | continue  | crate  | do  |
| else  | enum  | extern  | false  | final  |
| fn  | for  | if  | impl  | in  |
| let  | loop  | macro  | match  | mod  |
| move  | mut  | offsetof  | override  | priv  |
| pub  | pure  | ref  | return  | sizeof  |
| static  | self  | struct  | super  | true  |
| trait  | type  | typeof  | unsafe  | unsized  |
| use  | virtual  | where  | while  | yield  |

每个这些关键字在语法中有特殊含义，并且它们都排除`ident`规则之外。

注意有些关键字是保留的，现在并没有意义。

#### <a name="Literals"></a>3.5.2.常量
常量是一个包含单独记号的表达式，而不是一串记号，它立即而直接的代表了它所赋的值，而不是通过名字引用或其它赋值规则。常量是一种形式的常表达式，所以它（主要）在编译时赋值。

```
lit_suffix : ident;
literal : [ string_lit | char_lit | byte_string_lit | byte_lit | num_lit ] lit_suffix ?;
```

可选的后缀只对特定数字常量有效，不过它被未来的扩展保留，也就是说，上述给出了词法语法，不过Rust解析器会拒绝除了下面[数字常量](#NumberLiterals)中提到的12种特殊情况外的一切形式。

##### <a name="Examples"></a>3.5.2.1.例子

###### <a name="CharactersAndStrings"></a>3.5.2.1.1.字符和字符串

|  | 例子 | `#`集合 | 字符集 | 转义 |
| ------------- | ------------- | ------------- | ------------- | ------------- |
|[字符](#CharacterLiterals)|`'H'`|`N/A`|所有Unicode|`\'`和[字节](#ByteEscapes)和[Unicode](#UnicodeEscapes)|
|[字符串](#StringLiterals)|`"hello"`|`N/A`|所有Unicode|`\“`和[字节](#ByteEscapes)和[Unicode](#UnicodeEscapes)|
|[原始字符串](#RawStringLiterals)|`r#"hello"#`|`0...`|所有Unicode|`N/A`|
|[字节](#ByteLiterals)|`b'H'`|`N/A`|所有ASCII|`\'`和[字节](#ByteEscapes)|
|[字节字符串](#ByteStringLiterals)|`b"hello"`|`N/A`|所有ASCII|`\“`和[字节](#ByteEscapes)|
|[原始字节字符串](#RawByteStringLiterals)|`br#"hello"#`|`0...`|所有ASCII|`N/A`|

####### <a name="ByteEscapes"></a>3.5.2.1.2.字节转义

|| 名称 |
| -------- | ---------------------- |
|`\x7F`|8位字符编码（正好2个数字）|
|`\n`|换行符|
|`\r`|回车|
|`\t`|制表符|
|`\\`|反斜线|

###### <a name="UnicodeEscapes"></a>3.5.2.1.3.Unicode转义

|| 名称 |
| -------- | ---------------------- |
|`\u{7FFF}`|24位Unicode字符编码（最多6个数字）|

###### <a name="Numbers"></a>3.5.2.1.4.数字

| [数字常量](#NumberLiterals)`*` | 例子 | 幂 | 后缀 |
| ------------- | ------------- | ------------- | ------------- |
|十进制整数|`98_222`|`N/A`|整数后缀|
|十六进制整数|`0xff`|`N/A`|整数后缀|
|八进制整数|`0o77`|`N/A`|整数后缀|
|二进制整数|`0b1111_0000`|`N/A`|整数后缀|
|浮点数|`123.0E+77`|`Optional`|浮点后缀|

`*`所有数字常量允许`_`作为可视分隔符：`1_234.0E+18f64`

###### <a name="Suffixes"></a>3.5.2.1.5.后缀

| 整数 | 浮点数 |
| ---------------- | ------------- |
|`u8`，`i8`，`u16`，`i16`，`u32`，`i32`，`u64`，`i64`，`is`（`isize`），`us`（`usize`）|`f32`，`f64`|

##### <a name="CharacterAndStringLiterals"></a>3.5.2.2.字符和字符串常量

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

###### <a name="Floating-pointLiterals"></a>3.5.2.4.2.浮点常量
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

