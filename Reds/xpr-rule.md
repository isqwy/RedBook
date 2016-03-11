# 表达式语法规则


```
<literal>       ::= ... 字面值 ...
<variable>      ::= ... 变量名 ...
<logic-call>    ::= ... 返回 logic! 值型的函数调用 ...
<func-call>     ::= ... 返回其它值型的函数调用 ...
<statement>     ::= ... 语句或无返回值的函数...

<logic-literal> ::= "true" | "false"

<math-op>       ::= "+" | "-" | "*" | "/" | "//" | "%"
<bitwise-op>    ::= "and" | "or" | "xor"
<comparison-op> ::= "=" | "<>" | "<" | "<=" | ">=" | ">"
<op>            ::= <math-op> | <bitwise-op>

<cond-expr>     ::= <value> <comparison-op> <expression>
<condition>     ::= <logic-literal> | <logic-call> | <cond-expr>

<all>           ::= "ALL" "[" <condition>+ "]"
<any>           ::= "ANY" "[" <condition>+ "]" 
<either>        ::= "EITHER" <condition> "[" <expression>+ "]" "[" <expression>+ "]"
<short-circuit> ::= <all> | <any> | <either>

<paren>         ::= "(" <expression> ")"
<value>         ::= <variable> | <literal> | <paren> | "null"

<infix>         ::= <value> <op> <expression>
<prefix>        ::= <func-call> <expression>* | <statement> <expression>* 
<call>          ::= <prefix>+ | <infix> | <cond-expr>

<expression>    ::= <call> | <value> | <short-circuit>
```

## 例子

```
a: 123
foo a + 1
0 < foo a + 1
any [(0 < foo a + 1) a > 0]

if any [(0 < foo a + 1) a > 0][print "ok"]

b: 1 + (2 * a - either zero? a [0][a + 100])
```
