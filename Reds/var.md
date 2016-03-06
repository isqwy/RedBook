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

变量可持有任意类型的值。即可以是实值(如 `integer!,pointer!`)也可以是对实值的引用(如`struct!,c-string!`)。给变量赋值，以冒号作为变量名后缀*{这被称作**设字**(`set-word!`)类型}*：

```R
foo: 123
bar: "hello"
```

**多重赋值**

例如`a: b: 123`这样的多重赋值形式目前还不支持。

## 用值

没有修饰地使用变量名，将取得它的值，或被作为函数参数传递。*{相应的字型称为**用字**(`word!`)}*

```R
bar: "hello"
print bar    ;打印hello
```

## 类型

变量有类型。用值前不必声明，但需初始化。
函数的内部变量需要声明，但若正确初始化，则可跳过声明的类型规范部分。例如：

```
foo: 123
bar: "hello"
size: length? bar
id: GetProcessID        ;-- 'GetProcessID would return an integer!
compute: func [
   a [integer!]
   return: [integer!]
   /local c             ;-- 'c 的声明没有类型
][
   c: 1                 ;-- 推断其类型为 integer!
   a + c
]
```

初始化只能在代码的根层级中进行，因此，试图在代码块(`block!`)中初始化将造成编译错误：

```R
foo: 123                               ;-- 有效的初始化
if a < b [foo: 123]                    ;-- 无效的初始化
```

**注意**：函数体被看作是根层级代码。

变量有下列类型：

* integer!
* byte!
* float!
* float32!
* logic!
* c-string!
* struct!
* pointer!
