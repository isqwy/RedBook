# 属性

通过特殊的属性字，可以改变函数在运行时的行为：
* [infix](infix.md);中缀调用。
* [cdecl](cdecl.md);C调用。
* callback;当编译器推断失败时手动强制callback模式。
* [variadic](variadic.md);变参模式。
* [typed](typed.md);参数类型化。
* [custom](custom.md);自定义方式。
* [catch](catch.md);捕获异常。


----
编译器默认为`stdcall`模式，但对引入函数，会推断为callback模式。