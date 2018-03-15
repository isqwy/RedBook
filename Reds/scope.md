# 作用域

 在`Red/System`中，变量具有**静态作用域**。变量在源代码中声明的位置，决定了它的作用域\(scope\)。

## 全局语境\(global context\)

`全局语境`定义为全局名字空间。所有全局的变量及函数，在其中被定义。该语境是唯一的。一个简单的规则：**所有非函数中声明的变量，都属于全局语境**。

示例：

```
foo: 123                               ;-- global variable

f: func [/local bar [integer!]][
   bar: 123                            ;-- locally scoped variable
]
```

## 函数语境\(Functions Contexts\)

每个定义的函数，都有自己的内部语境。在函数定义块中声明的变量是局部作用域的，不能从函数之外访问。但反过来，却可以在函数中引用与修改全局变量。如果函数中的局部变量与全局变量同名，则优先使用局部变量。且这两个同名变量的值，互不影响。

示例：

```
foo: 1                                 ;-- global variable
var: 2

f: func [
   return: [integer!]
   /local 
       bar [integer!]
][
   bar: 3
   foo + var + bar                     ;-- will return 6
]

f: func [
   return: [integer!]
   /local 
       bar [integer!]
       var [integer!]
][
   bar: 3
   var: 10                             ;-- 'var is a local variable here
   foo + var + bar                     ;-- will return 14
]
```

### 关键字：use





