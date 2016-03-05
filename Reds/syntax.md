# 语法

语法与[REBOL](http://www.rebol.com)相似。在目前的自举阶段，词法分析器是由`REBOL`提供的。但不幸REBOL也没有正式的语法定义，仅一些粗浅描述：

*  http://www.rebol.com/docs/core23/rebolcore-3.html
*  http://www.rebol.com/r3/docs/guide/code-syntax.html

当前Red/System仍使用8位的ASCII字符编码。以后会切换到 `UTF-8`

## Delimiters
		String delimiters: double quotes
		"this is a string"
		{This is
 a multiline
 string.
}
		Block of code delimiters: square brackets
		if a > 0 [print "TRUE"]
		either a > 0 [print "TRUE"][print "FALSE"]
		while [a > 0][print "loop" a: a - 1]
		Path separator: slash (denotes a hierarchical relation)
		s: declare struct! [i [integer!] b [byte!]]
s/i: 123
s/b: #"A"
## Free-form syntax
		Red/System (and Red) inherits the free-form syntax of the REBOL language. The only syntactic constraints are putting a whitespace (in the large sense) between tokens and correctly pairing delimiters.
		Examples of valid code:
		while [a > 0][print "loop" a: a - 1]
		while [a > 0]
    [print "loop" a: a - 1]
		while [
   a > 0
][
   print "loop"
   a: a - 1
]
		Code guidelines are not yet available. They will follow standard REBOL practices.

## Comments
		Inline comment:
		;this is a commented line
		print "hello world"    ; this is another comment
		Multiline comment:
		comment {
    This is a
    multiline
    comment
}
		Usage rules:
			○ Inline comments are allowed anywhere in the source code
			○ Multi-line comments are allowed anywhere in the source code, except in expressions. Example:
a: 1 + comment {5} 4   ; this will produce a compilation error