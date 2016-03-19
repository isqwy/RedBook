# variadic

触发原生或引入函数的可变参数模式。

使用该模式的原生函数需在规范块中提供两个参数：
* 一个整数变量，于用参数计数；
* 一个指针指向参数列表；

引入函数只需声明此属性即可。

```
my-print: func [                      ;-- native function
  [variadic]
  count [integer!] 
  list  [int-ptr!]
][
  print ["count: " count lf]
  until [
      print [list/value lf]
      list: list + 1
      count: count - 1
      zero? count
  ]
]

#import [                              ;-- imported function
  LIBC-file cdecl [
      printf: "printf" [[variadic]]   ;-- no need to specify any argument here
  ]
]
```

传递给 `variadic`属性的函数的参数，包裹在方括号中：

```
my-print ["hello" 123 "world"]
will output:
count: 3
00402035                               ;-- pointer to "hello" c-string
0000007B                               ;-- 123 in hexadecimal
00402030                               ;-- pointer to "world" c-string

printf ["%s %i %s" "hello" 123 "world"]
will output:

hello 123 world
```

`variadic`属性仅能单独或配合`cdecl`属性使用。