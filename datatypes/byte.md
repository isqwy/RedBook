# byte!

8个二进制位称为字节(`byte`)，因此，字节可表达`0~255`范围的整数。同时，这也是ASCII码表的范围。因此，byte!值型采用如下格式表示：

```
#"<character>"
#"^<character>"
#"^(hexadecimal)"
#"^(name)"
例如:
#"a"
#"A"
#"5"
#"^A"
#"^(1A)"
#"^(back)"
```

实际上，Red/System的`byte!`值型就相当于REBOL2中的`character!`。

`^`表示转义。

`#"^A"`~`#"^Z"`表示`Ctrl+A,~Ctrl+Z`

