# 语法

The syntax is almost the same as the one used by REBOL language, as the lexer (LOAD) is currently provided by REBOL during the bootstrapping phase. The REBOL syntax does not have a formal specification nor an exhaustive documentation, just a superficial description, but it is enough to work with. See:

*  http://www.rebol.com/docs/core23/rebolcore-3.html (There is a typo in the table, all literals are missing a caret (^) character after the first quote)
*  http://www.rebol.com/r3/docs/guide/code-syntax.html
	A complete syntax specification for both Red and Red/System will be provided during the implementation of the Red language layer.

For now, Red/System uses 8-bit character encoding (ASCII). Once proper Unicode support will be provided by the Red language layer, Red/System will switch to UTF-8 source encoding.
	Here are a few practical aspects of the language syntax:
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