# infix

允许函数被中缀调用。此类函数必须明确具有两个参数，否则导致编译错误。

## 例子

```
avg: func [
  [infix] 
  a [integer!] 
  b [integer!]
  return: [integer!]
][
   (a + b) / 2
]

10 avg 6
```

**注意**
>当指定`infix`之后，前缀调用仍可使用，但仅当函数左侧无值时。

```
func [return: [integer!]][
   avg 10 6                            ;-- will return 8 as well               
]                      

print "ok" avg 10 6                    ;-- will produce a compilation error
```