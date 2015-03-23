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
