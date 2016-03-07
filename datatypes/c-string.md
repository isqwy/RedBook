# C-string!

`c-string!`是以字节`#"^(null)"`为终止的非`null`字节序列。而一个**c-string变量**所持有的，是指向第一个字节的内存地址。因此可看作是`byte!`变量的隐式指针。

若一个`c-string!`的首字节是`null`，就是一个**空的c-string!**

## 格式

**c-string!**的字面格式，与`string!`一样，只是元素被限定为单字节的`char!`，而终止符`null`在编译时自动添加。

```
foo: "I am a c-string"
bar: {I am
  a multiline
  c-string
}
```
## c-string!的长度与大小

在运行时，可以用`length?`获取c-string!的长度，用`size?`获取它的大小：

```
a: length? "Hello"    ;返回5
a: size?   "Hello"    ;返回6
```

区别在于，`size?`包含了作为终止符的`null`字节。

## 操作c-string!

由于`c-string!`变量是对字节的指针，因此可用简单的整数加减法移动指针位置。

```
s: "hello"                             ;-- 假设指针地址在40000000h

s: s + 1                               ;-- 现在的地址： 40000001h
print s                                ;-- "ello"

s: s + 1                               ;-- 现在的地址： 40000002h   
print s                                ;-- "llo"
```

由于`c-string!`是字节序列，因此可以用`path!`访问单个字节。

```
foo: "I am a c-string"
foo/1  =>  #"I"                        ;-- byte! value (73)
foo/2  =>  #" "                        ;-- byte! value (32)
...
foo/15 => #"g"                         ;-- byte! value (103)
foo/16 => #"^(00)"                     ;-- byte! value (0) (end marker)

c: 4
foo/c  => #"m"                         ;-- byte! value (109)
```

同样，也可以用`path!`修改其中的字节：

```R
foo: "I am a c-string"
foo/3: #"-"
c: 4
foo/c: #"-"
print foo    ;I -- a c-string
```

>**注意**
>* 字节索引从`1`开始。
>* 索引超出边界的行为还未定义，应避免。

遍历`c-string!`：

```
foo: "I am a c-string"
bar: foo

until [
     print bar/1                   
     bar: bar + 1
     bar/1 = null-byte
]
```
