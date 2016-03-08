# struct!

`struct!`是由多个值构成的记录，而每个值都有自己的值型。**struct变量**持有的是`struct!`值的内存地址。

>`struct!`的成员值会针对不同目标平台在内存中作对位补齐，例如，对于IA32平台，补齐为4字节。函数`size?`所返回的大小就是补齐之后的尺寸。

## 声明`struct!`

```
declare struct! [
   <member> [<datatype>]
   ...
]
<member>   : a valid identifier
<datatype> : integer! | byte! | pointer! [integer! | byte!] | logic! |
             float! | float32! | c-string! | struct! [<members>] |
             function! [<spec>]
```

`declare struct!`返回所声明的`struct!`值的内存地址，而这个值的全部成员被初始化为`0`。



