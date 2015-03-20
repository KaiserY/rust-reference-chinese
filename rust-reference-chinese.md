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
语法中的一些规则 - 特别的像[单目运算符](http://doc.rust-lang.org/reference.html#unary-operator-expressions)，[二进制运算符](http://doc.rust-lang.org/reference.html#binary-operator-expressions)和[关键字](http://doc.rust-lang.org/reference.html#keywords) - 表现为一种简单的形式：作为一串非引用，可打印的空白分隔的字符串表。
