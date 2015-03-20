# Rust 参考手册

* [1.介绍](#Introduction)
* [2.标记法](#Notation)

## <a name="Introduction"></a>1.介绍
本文档是Rust编程语言的主要参考。它提供3种类型的材料：

* 非正式的介绍了每种语言结构和其用法的章节
* 非正式的介绍了内存模型，并发模型。运行时服务，链接模型和调试工具的章节
* 提供了影响了Rust设计的基础原理和语言参考的附录章节

本文档并不充当语言的介绍。它假设你熟悉Rust语言的背景基础。一个单独的[book](http://doc.rust-lang.org/book/)可以帮助你熟悉这些背景。

本文档也不充当包含在Rust语言发行版中的[标准库](http://doc.rust-lang.org/std/)的参考。这些库的文档来源于从源码中抽取的文档属性。很多你期望是语言功能的部分可能是Rust库的功能，所以你应该在库文档中寻找，而不是在这。

你可能也对[语法](http://doc.rust-lang.org/grammar.html)感兴趣。

## <a name="Notation"></a>2.标记法
Rust的语法定义在Unicode代码点之上，每个字符通常记作`U+XXXX`，由4个或更多十六进制整数`X`组成。大部分Rust
