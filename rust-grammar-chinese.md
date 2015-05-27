# 语法

* [1.介绍](#Introduction)

## <a name="Introduction"></a>1.介绍
本文档是Rust编程语言语法的主要参考。它只提供一种类型的材料：

* 正式定义了语言语法的章节

本文档并不充当语言的介绍。它假设你熟悉Rust语言的背景基础。一个单独的[book](http://doc.rust-lang.org/book/)可以帮助你熟悉这些背景。

本文档也不充当包含在Rust语言发行版中的[标准库](http://doc.rust-lang.org/std/)的参考。这些库的文档来源于从源码中抽取的文档属性。很多你期望是语言功能的部分可能是Rust库的功能，所以你应该在库文档中寻找，而不是在这。

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
语法中的一些规则 - 特别的像[单目运算符](#UnaryOperatorExpressions)，[双目运算符](BinaryOperatorExpressions)和[关键字](#Keywords) - 表现为一种简单的形式：作为一串非引用，可打印的空白分隔的字符串表。这些例子是[记号](https://doc.rust-lang.org/stable/grammar.html#tokens)规则的子集，并假设为编译器的词法分析的结果，由一个DFA（确定有限状态自动机）驱动，通过分离所有这些字符串表入口执行。

当语法中出现一个在双引号（`"`）中的字符串时，它是字符串表组合中的一个单独成员的隐式引用。查看[记号](#Tokens)以获取更多信息。

## <a name="LexicalStructure"></a>3.词法结构

### <a name="InputFormat"></a>3.1.输入格式
Rust的输入被解释为一串UTF-8编码的Unicode代码点。大部分Rust语法规则根据ASCII范围的代码点定义，不过一小部分根据Unicode属性或显示代码点列表定义。<sup>[1](#Fn1)</sup>

### <a name="SpecialUnicodeProduction"></a>3.2.特殊Unicode组合
Rust语法中的如下这些组合根据Unicode属性定义：`ident`，`non_null`，`non_star`，`non_eol`，`non_slash_or_star`，`non_single_quote`和`non_double_quote`。

#### <a name="Identifiers"></a>3.2.1.标识符
一个标识符是如下形式的任何非空Unicode字符串<sup>[2](#Fn2)</sup>：

* 第一个字符拥有`XID_start`属性
* 其它字符拥有`XID_continue`属性

以上组合*不能*出现在[关键字](#Keywords)中。

> **注意**：`XID_start`和`XID_continue`作为一个字符属性包含了用来组成更有名的C和JavaScript家族的标识符的字符集。

#### <a name="DelimiterRestrictedProductions"></a>3.2.2.分隔符限制组合
一些组合通过排除特定Unicode字符定义：

* `non_null`是任何除了`U+0000`（null）之外的单个Unicode字符
* `non_eol`被限制为除了`U+000A`（`'\n'`）之外的任何`non_null`
* `non_single_quote`被限制为除了`U+0027`（`'`）之外的任何`non_null`
* `non_double_quote`被限制为除了`U+0022`（`"`）之外的任何`non_null`

### <a name="Comments"></a>3.3.注释

```
comment : block_comment | line_comment ;
block_comment : "/*" block_comment_body * "*/" ;
block_comment_body : [block_comment | character] * ;
line_comment : "//" non_eol * ;
```

### <a name="Whitespace"></a>3.4.空白

```
whitespace_char : '\x20' | '\x09' | '\x0a' | '\x0d' ;
whitespace : [ whitespace_char | comment ] + ;
```

### <a name="Tokens"></a>3.5.记号

```
simple_token : keyword | unop | binop ;
token : simple_token | ident | literal | symbol | whitespace token ;
```

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

#### <a name="Literals"></a>3.5.2.常量

```
lit_suffix : ident;
literal : [ string_lit | char_lit | byte_string_lit | byte_lit | num_lit ] lit_suffix ?;
```

可选的后缀只对特定数字常量有效，不过它被未来的扩展保留，也就是说，上述给出了词法语法，不过Rust解析器会拒绝除了下面[数字常量](#NumberLiterals)中提到的12种特殊情况外的一切形式。
