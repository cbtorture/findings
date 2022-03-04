Lists
=====

## What is this section about?

This section is aimed at documenting the behaviour of so called _"list initialzers"_ in C. This is a language
feature (**TODO**: Find out when this was added (which specificaiton version)) which allows one to initialize
the elements of an array as part of the array declaration.

An example may be the following:

```c
/**
* Initialize an array to have the values of [0, 1, 2]:
*/
int array[3] = {0, 1, 2};
```

## Rules

The example provided above is a very basic example and the behaviour of it is the same across implementations for
the simple reason that the values used to initialize it, namely `0`, `1` and `2`, require no further evaluation.
Where the focus of this section comes in is when these expressions are more complex, specifically with expressions
that have side-effects and where those side-effects effect the returned value of the evaluation.

We will therefore be looking therefore at code that is more along the lines of the following:

```c
/**
* Initialize the array to have the first element be the returned value of `f()`
* and the second element the returned value of `g()`
*/
int array[2] = {f(), g()};
```

### Unspecified behaviour

One would assume that the order of evaluation would be to call `f()` then `g()` and have those returned values
respectively saved into the positions, `array[0]` and `array[1]`. The latter happens, but the former (the order
of calls) is unspecified and the order of execution is up to the compiler implementer.

---

## Table of behaviour

This section contains several tables relating to how a specific snippet of C code, which makes use of this
undeterminism, behaves.

1. [2-argument function call](2arg_func_call.md)