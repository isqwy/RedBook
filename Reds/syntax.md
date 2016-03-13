# 语法

语法与[REBOL](http://www.rebol.com)相似。在目前的自举阶段，词法分析器是由`REBOL`提供的。但不幸**REBOL也没有正式的语法定义**，仅一些粗浅描述：

*  http://www.rebol.com/docs/core23/rebolcore-3.html
*  http://www.rebol.com/r3/docs/guide/code-syntax.html

当前Red/System仍使用8位的ASCII字符编码。以后会切换到 `UTF-8`

## 分隔符

[字符串](../datatypes/string.md)分隔符：`"双引号"`或`{花括号}`

```R
"this is a string"
{This is
 a multiline
 string.
}
```

[代码块](../datatypes/block.md)分隔符：`[方括号]`

```R
if a > 0 [print "TRUE"]
either a > 0 [print "TRUE"][print "FALSE"]
while [a > 0][print "loop" a: a - 1]
```

[路径](../datatypes/path.md)分隔符：`/`

```R
s: declare struct! [
    i [integer!] 
    b [byte!]
]
s/i: 123
s/b: #"A"
```

## 语法形式自由

唯一的限制是：**标识符以空白分隔&正确配对分隔符**。下例都是合法代码：

```R
while [a > 0][print "loop" a: a - 1]

while [a > 0]
    [print "loop" a: a - 1]

while [
   a > 0
][
   print "loop"
   a: a - 1
]
```

## 注释

单行注释：

```
;this is a commented line
print "hello world"    ; this is another comment
```

多行注释：

```
comment {
    This is a
    multiline
    comment
}
```

使用规则：

* 单行注释允许出现在源码中任意位置。
* 多行注释禁止出现在表达式中。

例如：

```R
a: 1 + comment {5} 4   ; 造成编译错误
```