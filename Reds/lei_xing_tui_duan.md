# 类型推断

函数针对内部变量提供有限的类型推断。即，若变量正确初始化，可忽略其类型声明。

**例子**

```
foo: func [
   a [integer!]
   return: [integer!]
   /local c                            ;-- omitted local variable type
][
   c: 10                               ;-- variable type is integer!
   a + c
]
```

