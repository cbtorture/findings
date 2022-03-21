Comma operator
==============

The comma-operator is meant to enforce the sequencing of expressions,
from left-to-right and then retruning the result of the last executed
expression - which would be the right-most one.

An example of this would be:

```c
int result = (f1(), f2(), f3());
```

This code, by specification, should execute the argument list
in the order of `f1()`, `f2()` and then lastly `f3()` and return
the result of `f3()` as the value of the expression `(.., .., ..)`.

## Why test it if it is not unspecified?

We may as well, you never know how some compilers can behave, we
may as well make sure. Also, we don't know what the verifiers
are accounting for possibly, so let's test it anyways.