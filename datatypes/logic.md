# logic!

`logic!`只有两个值：`TRUE`,`FALSE`。logic变量的初始化，或直接使用这两个值，或使用条件表达式的结果。

```
foo: true
either foo [print "true"][print "false"]

bar: 2 > 5
either bar [print "true"][print "false"]
```
