# struct!

**结构**`(struct!)`是由多个值构成的记录，而每个值都有自己的值型。**结构变量**持有的是结构值的内存地址。

>`struct!`的成员值会针对不同目标平台在内存中作对位补齐，例如，对于IA32平台，补齐为4字节。函数`size?`所返回的大小就是补齐之后的尺寸。

## 声明

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

## 用法

```
s: declare struct! [
   a   [integer!]
   b   [c-string!]
   c   [struct! [d [integer!]]]
]
```
成员值`c`需要被初始化声明：
```
s/c: declare struct! [d [integer!]]
```

## 访问

用路径语法对`struct!`进行读写访问：
```
<struct>/<member>                      ;-- read access
<struct>/<member>: <value>             ;-- write access

<struct>  : a valid struct variable
<member>  : a valid member identifier in <struct>
<value>   : a value of same datatype as <member>
```

例如，对上节用法示例的访问：

```
foo: s/a                               ;-- reading member 'a in struct 's
s/a: 123                               ;-- writing 123 in member 'a in struct 's
s/b: "hello"
bar: s/c/d                             ;-- deep read/write access is also possible
```
## 运算

可用整数加减运算偏移`struct!`的指针地址：

```
p: declare struct! [                   ;-- let suppose p = 40000000h
   a [integer!]
   b [pointer! [integer!]]
]                                      ;-- struct memory size would be 8 bytes
p: p + 1 
```

## 别名

在复杂的结构嵌套或函数规范中，对子结构作别名化(alias)引用，可简化结构。别名约定以叹号`!`为后缀。
它有自己的名字空间，不与变量相扰。

### 语法

```
<name>: alias struct! [
   <member> [<datatype>]
   ...
]
<name>     : the name to use as alias
<member>   : a valid identifier
<datatype> : integer! | byte! | pointer! [integer! | byte!] | logic! |
             float! | float32! | c-string! | struct! [<members>] |
             function! [<spec>]
```


### 示例

```
book!: alias struct! [                 ;-- defines a new aliased type
   title       [c-string!]
   author      [c-string!]
   year        [integer!]
   last-book   [book!]                 ;-- self-referenced definition
]

gift: declare struct! [
   first  [book!]                      ;-- reference to a book! struct
   second [book!]                      ;-- reference to a book! struct
]

gift/first: declare book!              ;-- book! struct allocation

gift/first/title:  "Contact"
gift/first/author: "Carl Sagan"
gift/first/year:   1985
```


