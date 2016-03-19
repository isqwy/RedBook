# cdecl

函数调用约定为C调用。这样可以把函数作为参数安全地传递给导入的C函数。

```
#import [
    "foo.dll" cdecl [
        foo: "foo" [
            fun     [function! [a [integer!] b [integer!] return: [logic!]]]
            return: [integer!]
        ]
    ]
]

compare: func [
    [cdecl]                            ;-- use C calling convention
    left [integer!] right [integer!]
    return: [logic!]
][
    left <= right
]

foo :compare                           ;-- pass the function pointer
```