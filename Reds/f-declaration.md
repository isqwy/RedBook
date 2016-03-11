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
<argument>   : function's argument indentifier
<datatype>   : integer! | byte! | logic! | pointer! [integer! | byte!] |
               float! | float32! | c-string! | struct! [<members>]
<local>      : local variable
<body>       : function's body code
```

