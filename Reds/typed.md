# typed

针对原生函数开关带类型信息的变参模式。
与`variadic`属性相似。
原生函数使用此属性需在规范块中提供两个参数：
* 一个整数变量用于参数统计；
* 一个参数记录的指针(使用typed-value!别名)；

`typed-value!`别名如此定义：

```
typed-value!: alias struct! [
   value   [integer!]                  ;-- argument value or pointer
   type    [integer!]                  ;-- argument type   
]
```

类型由运行时定义：

```
#define type-logic!        1
#define type-integer!      2
#define type-byte!         3
#define type-float32!      4
#define type-float64!      5
#define type-float!        5
#define type-c-string!     6
#define type-byte-ptr!     7
#define type-int-ptr!      8
#define type-function!     9
#define type-struct!       1000
#define any-struct?        [1000 <=]
#define alias?             [1001 <=]
```

NULL值的类型ID设置为 type-int-ptr! ID(pointer! [integer!])

**类型化引入函数**
>能对引入函数使用`typed`属性，但此属性需要匹配对应的栈布局，故可能只保留用于以`red/system`写的引用函数。

**例子**

```
vprint: func [
  [typed]
  count [integer!]
  list  [typed-value!]
][
  print ["count: " count lf]
  until [
      print [list/value " : "]
      print [form-type list/type lf]  ;-- form-type converts a type ID to a c-string
      list: list + 1
      count: count - 1
      zero? count
  ]
]
```

传递参数给带有类型信息的变参函数，使用方括号打包：

```
vprint ["hello" 123 "world"]
```

输出结果：

```
count: 3
00402043 : c-string!                   ;-- pointer to "hello" c-string
0000007B : integer!                    ;-- 123 in hexadecimal
0040203E : c-string!                   ;-- pointer to "world" c-string
```

>**`typed`属性仅能单独或与`cdecl`属性配合使用。**

