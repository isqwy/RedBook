# pointer!

指针`pointer!`持有某个值的内存地址。由被指值地址及其值型定义。因`c-string!,struct!`已隐含指针，而`logic!`无需指针，故需显式定义指针的值型仅`integer!,float!,float32!,byte!`四种。

`byte!`指针相当于`c-string!`引用。区别仅在于对指向值的解释。`byte!`指针指向的是无约束字节流。而`c-string!`指针指向的是以`null`为终止的字节数组。

>在32位系统，指针尺寸为4字节；在64位系统为8字节。

## 指针格式

```
declare pointer! [<datatype>]

<datatype>: integer! | byte! | float! | float32!
```
示例：

```
foo: declare pointer! [integer!]       ;-- equivalent to C's: int *foo;
bar: declare pointer! [byte!]          ;-- equivalent to C's: char *bar;
baz: declare pointer! [float!]         ;-- equivalent to C's: double *baz;
```
>当前，在初始化之前，全局指针默认为空，而局部指针不确定，将来或许出于调试考虑，默认为`0BADBAD0h`之类的值。

指针声明仅在函数规范块的参数中需要，局部指针变量声明可跳过值型部分，而由类型推断系统猜测。

**全局指针声明示例**

```
p: declare pointer! [integer!]         ;-- int *p;
p: declare pointer! [byte!]            ;-- char *p;
p: declare pointer! [float!]           ;-- double *p;
```

**函数内部变量声明示例**

```
func [/local p [pointer! [integer!]]   ;-- int *p;
func [/local p [pointer! [byte!]]      ;-- char *p;
func [/local p [pointer! [float!]]     ;-- double *p;
```

**指针变量类型推断示例：**

```
foo: func [
   a [struct! [count [integer!]]]
   /local
       p [pointer! [integer!]]         ;-- explicit declaration
][
   foobar p                            ;-- foobar modifies p
   a/count: p/value
]

bar: func [
   a [struct! [count [integer!]]]
   /local p                            ;-- p datatype inferred
][
   p: declare pointer! [integer!]      ;-- p initialized (implicit declaration)
   foobar p
   a/count: p/value
]

bar2: func [
   a [struct! [count [integer!]]]
   /local p                            ;-- p datatype inferred
][
   p: GetPointer a                     ;-- datatype is guessed from return value
   foobar p
   a/count: p/value
]
```

## 访问

指针的修饰字`/value`可访问指针指向的值：

```
<pointer>/value                        ;-- read access
<pointer>/value: <value>               ;-- write access

<pointer> : pointer variable
<value>   : a value of same type as in pointer's definition
```

例如：

```
p:   declare pointer! [integer!]       ;-- declare a pointer on an integer
bar: declare pointer! [integer!]       ;-- declare another pointer on an integer

p: as [pointer! [integer!]] 40000000h  ;-- type cast an integer! to a pointer!
p/value: 1234                          ;-- write 1234 at address 40000000h
foo: p/value                           ;-- read pointed value back
bar: p                                 ;-- assign pointer address to 'bar
```

## 算术

指针可加减运算以偏移地址。偏移大小取决于所指向值型的尺寸。如：

```
p: declare pointer! [integer!]         ;-- pointed value memory size is 4 bytes

p: as [pointer! [integer!]] 40000000h
p: p + 1                               ;-- p points now to 40000004h
p: p + 1                               ;-- p points now to 40000008h
q: declare pointer! [byte!]            ;-- pointed value memory size is 1 byte
q: as [pointer! [byte!]] 40000000h
q: q + 1                               ;-- p points now to 40000001h
q: q + 1                               ;-- p points now to 40000002h
```

两个指针加减则求出左边指针的偏移量：

```
offset: p - q                          ;-- would store 6 in offset
                                       ;-- type of offset is pointer! [integer!]
```

## 路径

可用路径语法模拟数组索引访问。索引起始于1。

```
<pointer>/<integer>                    ;-- literal integer index provided
<pointer>/<index>                      ;-- index provided by a variable

<pointer>  : a pointer variable
<integer>  : an integer literal value
<index>    : an integer variable
```

例如：

```
p: declare pointer! [integer!]

p: as [pointer! [integer!]] 40000000h
a: p/1                                 ;-- reads an integer! from 40000000h
p/2: a                                 ;-- writes the integer! to 40000004h
```

整数变量也可用作索引：

```
p: declare pointer! [integer!]

p: as [pointer! [integer!]] 40000000h
c: 2
p/c: 1234                              ;-- writes 1234 (4 bytes) at 40000004h
```

>修饰字`/value`相当于路径`/1`

## 数组

可用指针访问一维字面值数组。

```
<variable>: [<items>]

<variable> : a pointer of same type as the array items.
<items>    : is a non-empty list of integer!, byte!, float! or float32! values.
```

数组元素静态分配，在数组之前，以一个32位的字存放数组大小，可用`/0`访问。

```
list: [123 456 789]
probe list/0                           ;-- outputs 3
probe list/2                           ;-- outputs 456

s: [#"h" #"e" #"l" #"l" #"o"]
sz: as int-ptr! s
probe sz/0                             ;-- outputs 5
probe s/5                              ;-- outputs o
```

## NULL

`null`没有确定的类型，但可替代任意指针类值，它不能用于非显式声明的变量值初始化。

```
a: declare pointer! [integer!]
a: null                                ;-- valid assignment, 'a type is defined
b: null                                ;-- invalid assignment, type of b cannot
                                       ;--  be deduced by the compiler

foo: func [s [c-string!] return: [c-string!]][
   if s = null [
       print "error"
       return null
   ]
   return uppercase s
]

b: foo "test"                          ;-- will set b to "TEST"
b: foo null                            ;-- will print "error" and set b to null
```

##C语言的空指针

Red/System没有对C语言空指针的显式支持，而用字节指针表现。

```
p-buffer!: alias struct! [buffer [c-string!]]

set-hello: function [
   s [p-buffer!]
][
   s/buffer: "hello"
   s                                   ;-- equivalent to C's char **
]

foo: func [
   /local
       c [p-buffer!]
][
   c: declare p-buffer!
   set-hello c
   print c/buffer
]

foo                                    ;-- call foo function
;;would print hello
```

>运行时宏：**byte-ptr!**
>>用于原生内存访问，等价于 `pointer! [byte!]`

## 变量指针

可用变量的取字形式，访问它的内存地址：

```
:<variable>

<variable> : a variable name of allowed type.
```
该语法的返回值是变量对应值型的指针：

|variable	|:variable|
|----:|:----|
|integer!	|pointer! [integer!]|
|byte!	|pointer! [byte!]|
|float!	|pointer! [float!]|
|float32!	|pointer! [float32!]|

例如：

```
s: declare pointer! [integer!]
a: 123
s: :a
print s/value       ;-- will output 123
```
