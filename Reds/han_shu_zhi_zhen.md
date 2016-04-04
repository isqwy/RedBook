# 函数指针

函数指针指向该函数的地址。通过此地址可将函数本身作为参数传递。

**语法**

```
:<func-name>
```

**例子**

```
progress: func [[cdecl] count [integer!]][
   print "."                           ;-- make the user see some progress
]

get-file "bigfile.avi" :progress       ;-- blocking job would call 'progress
                                       ;-- periodically
```

函数指针可赋值给变量以非关联化或稍后使用。此种变量不能作为其它函数的参数或返回值，因为函数指针不是一阶值型。

**注意**
>函数地址作为指针伪类型，不能直接用于表达式，但若需要，可转换为`integer!`值型。