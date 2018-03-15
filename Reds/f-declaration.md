# 声明

```
<name>: func | function [
   [<attributes>]                      ;-- 属性，可选
   "<function purpose>"                ;-- 函数文档字串，可选
   <argument> [<datatype>]
    "<argument description>"           ;-- 参数文档字串，可选
   ...
   return: [<datatype>]                ;-- 返回值类型，可选
   "<returned value description>"      ;-- 返回值描述，可选
   /local                              ;-- 内部变量，可选
   <local> [<datatype>]
   ...
][
   <body>
]

<name>       : 函数名
<attributes> : 特殊属性
<argument>   : 参数名
<datatype>   : integer! | byte! | logic! | pointer! [integer! | byte!] |
               float! | float32! | c-string! | struct! [<members>]
<local>      : 内部变量
<body>       : 函数体
```

> 将来，由`function`定义的函数会不同于`func`定义的，因此现阶段最好仅使用`func`

## 例子

```
hello: func [][print "hello"]          ;-- no arguments, no locals, no return value

why?: func [return: [integer!]][42]    ;-- minimal function returning an integer

inc: func [                            ;-- increment an integer
   a [integer!]
   return: [integer!]
][
   a + 1                               ;-- last value is returned
]

percent?: func [                       ;-- return relative percentage of a / b
   a [integer!]
   b [integer!]
   return: [integer!]
   /local c                            ;-- declare local variables
][
   c: 100
   a * c / b
]
```

**关于参数传递**

> 当前的实现中，`integer!,float!,float32!,byte!,logic!,pointer!`这6种变量是传值，而`c-string!, struct!`这两种变量是传址。



