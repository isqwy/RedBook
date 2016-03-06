# 变量

**变量**是表示内存地址的标签（也称为标识符），由非空白可打印字符序列组成。

>* 空白字符：newline,space,tab
>* 可打印字符：20h~7Eh的单字节字符。

首先，下列字符被用于分隔符或某些值型的格式：

```
[ ] { } " ( ) / \ @ # $ % ^ , : ; < >
```

其次，变量首字符不能是下列字符：

    0 1 2 3 4 5 6 7 8 9 '

再次，要避免十六进制整数的形式：由`A~F`开头，且由2,4,8个`A~F`与`0~9`构成，并以`h`结尾。

最后，所有标识符(变量及函数名)都不区分大小写。

**Unicode**
>因目前仍是ascii编码，故暂不支持Unicode作变量名。

## 赋值

变量可持有任意类型的值。即可以是实值(如 `integer!,pointer!`)也可以是对实值的引用(如`struct!,c-string!`)。给变量赋值，以冒号作为变量名后缀：

```R
foo: 123
bar: "hello"
```

		Multiple assignments
		Multiple assignments, like a: b: 123, are not currently supported in Red/System. Such a feature will be added in the future at some point, probably on the rewrite of Red/System in Red.
	3.2 Getting a value
		Just use the variable name without any decoration to get its value or to pass it as a function's argument.
		bar: "hello"
print bar
		will output:
		hello
	3.3 Typing
		Variables do have a type. Variables do not need to be declared before being used, but they require to be initialized anyway. Function local variables require to be declared, but the type specification part can be skipped if the variable is properly initialized. For example:
		foo: 123
bar: "hello"
size: length? bar
id: GetProcessID                       ;-- 'GetProcessID would return an integer!
		compute: func [
   a [integer!]
   return: [integer!]
   /local c                            ;-- 'c is declared without a type
][
   c: 1                                ;-- inferred type is integer!
   a + c
]
		are valid variable usages.
		Initializations have to be done at root level of code, attempt to initialize from a block of code will result in a compilation error.
		foo: 123                               ;-- valid initialization
		if a < b [foo: 123]                    ;-- invalid initialization
		Note: A function body block is considered a root level.
		Allowed types for variables are:
			○ integer!
			○ byte!
			○ float!
			○ float32!
			○ logic!
			○ c-string!
			○ struct!
			○ pointer!
