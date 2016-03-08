# 非官方的开始

目前[^1]，[Red-lang](http://www.red-lang.org)官网还没有正式的语言参考。这里只是个人学习英文资料时翻译与理解的笔记，偿试着按参考手册的结构组织内容。

## 语言简介

`Red`的目标是成为首个全栈编程语言。下面是它的主要特性：

* 范式无关，默认提供函数式/命令式/符号式范式
* 支持基于原型的对象
* 同像语言（Red语言是其自身的元语言）
* 既能以静态方式，也能以JIT方式编译为本地代码
* 强力支持并发和并行（通过Actor和并行聚集）
* 通过内置Red/System DSL提供底层系统编程能力
* 提供高级脚本特性和REPL控制台支持
* 高度可嵌入（像Lua一样，或者更好一些）
* 低内存占用，支持垃圾回收
* 低磁盘空间占用（<1MB）
* 生成单个的命令行可执行文件
* 零安装、零配置
* 独立的跨平台工具链
* 除运行的操作系统外，概无其他依赖

`Red`的设计目标是尽可能提供表达能力，同时保持源码的高可读性。



## 相关资料网址

* 本书GitBook地址：https://qwy.gitbooks.io/redit/content/
* 本书GitHub地址：https://github.com/isqwy/redit
* Github上的Red语言：https://github.com/red/red
* Red官网：http://red-lang.org/
* http://www.red-by-example.org/index.html



----
[1]:{{file.mtime}}