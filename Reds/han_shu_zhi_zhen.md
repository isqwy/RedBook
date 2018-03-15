# 函数指针

函数指针指向该函数的地址。通过此地址可将函数本身作为参数传递。

**语法**

```
:<func-name>
```

**例子**

```
progress: func [
   [cdecl] 
   count [integer!]
][
   print "."                           ;-- make the user see some progress
]

get-file "bigfile.avi" :progress       ;-- blocking job would call 'progress
                                       ;-- periodically
```

函数指针可赋值给变量以非关联化或稍后使用。此种变量不能作为其它函数的参数或返回值，因为函数指针不是一阶值型。

**注意**

> 函数地址作为指针伪类型，不能直接用于表达式，但若需要，可转换为`integer!`值型。

## 函数定义的别名

为避免重复函数定义块，可使用关键字`alias`进行别名化：

```
<name>: alias function! [<spec>]

<name> : 别名标识 (约定以 ! 为后缀)
<spec> : 合法的函数定义块
```

例如：

```
foo!: alias function! [n [integer!] return: [integer!]]
bar: func [f [foo!]][...]
```

## 函数的间接引用

```
foo!: alias function! [n [integer!] return: [integer!]] 
inc: func [n [integer!] return: [integer!]][ n + 1 ]
bar: as foo! :inc
print bar 2 ;-- will output 3
```

可以访问存储在结构中的函数指针：

```
s: declare struct! [
    inc [function! [n [integer!] return: [integer!]]]]
s/inc: :inc
probe s/inc 3 ;-- will output 4
```

## 提前退出

 有时，函数需要被提前退出，这由关键字`exit`和`return`来实现。

### exit

仅立即从函数中退出。

```
test: func [a [integer!]][
   if zero? a [exit]                   ;-- 这里如果 a = 0 就退出函数
   ...                                 ;-- 如果 a <> 0, 则继续处理...
]
```

### return

立即从函数中退出并有返回值。

```
test: func [
   a [integer!]
   return: [c-string!]
][
   if zero? a [
       return "Not allowed"            ;-- exit the function here if a = 0
   ]
   "ok"                                ;-- return "ok" if a <> 0
]
```



