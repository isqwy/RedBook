# custom

有时，想完全控制函数调用的原生栈布局，例如，当函数调用需要动态构造时，
`costom`属性允许手动`push`值入栈，且仍能生成正确的函数调用及相应的栈清理。对于用`cdecl`约定引入的C函数特别有用。它们需要调用者清理栈。跨平台方式下要小心使用这个属性。

**例子**

```
foo: as function! [[custom]] <imported-c-function>
push 123
push 0
foo 2                               ;-- custom call with 2 arguments
```

`costom`调用需要一个整数被编译器使用。此数指示压栈的参数个数。以便调用返回后的栈清理。

**栈的对齐**
>`custom`属性不执行堆栈对齐约束，所以要遵守特定平台的对齐约束。（主要涉及MacOS及ARM平台）。为了跨平台及整洁的方式，请调用`system/stack/align`