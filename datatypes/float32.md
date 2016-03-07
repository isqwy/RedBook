# float32!

32位单精度浮点数。用于某些流行库的接口，纯Red/System程序默认使用[float!](float.md)。

`float32!`没有字面形式，而由`float!`转换：

```
pi32: as float32! 3.1415927
```

`float32!`也能再转换到`float!`。同时，针对位移操作，也能将`float32!`转换成`integer!`形式。

```
s: as float32! 3.1415927
print [
   as float! s lf
   as integer! s
]
will output
3.14159270000000
1518260631
```
数学运算同于`float!`。
