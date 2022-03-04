**F1**: Function Call Evaluation ordering
=========================================

<br>

## Description

The order in which the arguments to a function is evaluated is unspeccified. Here
we will be looking at a piece of code which results in different outcomes depending
on the order in which these funtions are executed by using globally shared state
with funcitons senstiive to that.

```c
int result = f(counter(), counter());
```

**F1** is the most basic of the tests being done, there are only two ways the function
call to `f` could be evaluated (argument-wise) and the function being used to generate
the argument expressions is the same function; `counter()`.

## Implicit version

The implicit version of what we will be analysing is related to the following:

```c
#include<assert.h>
#include<stdio.h>

int globalCounter = 0;

int counter()
{
        globalCounter++;
        return globalCounter;
}

int f(int x1, int x2)
{
        return (x1*2)+x2;
}

int main()
{
        int result = f(counter(), counter());

        printf("%d\n", result);
}
```

<br>

## Results of behaviour

Here we document the behaviour of various C compiler implementations on the above code
and which arguments (further variations) were used and lastly the outcome. You can
reproduce the below by checking out the [F1 tests](https://github.com/cbtorture/tests/tree/master/functions/f1).

### Compiler tests

Running: `program.c`

| Compiler | Options| Standard | `result` | Observation |
|----|----|----|---|---|
| gcc | -O0 | c99 | `5` |right-to-left execution ordering |
| gcc | -O0 | c9x | `5` |right-to-left execution ordering |
| gcc | -O0 | c89 | `5` |right-to-left execution ordering |
| gcc | -O0 | c90 | `5` |right-to-left execution ordering |
| gcc | -O0 | c2x | `5` |right-to-left execution ordering |
| gcc | -O0 | c17 | `5` |right-to-left execution ordering |
| gcc | -O0 | c18 | `5` |right-to-left execution ordering |
| gcc | -O0 | c11 | `5` |right-to-left execution ordering |
| gcc | -O0 | c1x | `5` |right-to-left execution ordering |
| gcc | -O0 | gnu11 | `5` |right-to-left execution ordering |
| gcc | -O0 | iso9899:1990 | `5` |right-to-left execution ordering |
| gcc | -O0 | iso9899:199409 | `5` |right-to-left execution ordering |
| gcc | -O0 | gnu89 | `5` |right-to-left execution ordering |
| gcc | -O0 | gnu90 | `5` |right-to-left execution ordering |
| gcc | -O0 | iso9899:1999 | `5` |right-to-left execution ordering |
| gcc | -O0 | gnu99 | `5` |right-to-left execution ordering |
| gcc | -O0 | iso9899:2011 | `5` |right-to-left execution ordering |
| gcc | -O0 | iso9899:2017 | `5` |right-to-left execution ordering |
| gcc | -O0 | gnu17 | `5` |right-to-left execution ordering |
| gcc | -O1 | c99 | `5` |right-to-left execution ordering |
| gcc | -O1 | c9x | `5` |right-to-left execution ordering |
| gcc | -O1 | c89 | `5` |right-to-left execution ordering |
| gcc | -O1 | c90 | `5` |right-to-left execution ordering |
| gcc | -O1 | c2x | `5` |right-to-left execution ordering |
| gcc | -O1 | c17 | `5` |right-to-left execution ordering |
| gcc | -O1 | c18 | `5` |right-to-left execution ordering |
| gcc | -O1 | c11 | `5` |right-to-left execution ordering |
| gcc | -O1 | c1x | `5` |right-to-left execution ordering |
| gcc | -O1 | gnu11 | `5` |right-to-left execution ordering |
| gcc | -O1 | iso9899:1990 | `5` |right-to-left execution ordering |
| gcc | -O1 | iso9899:199409 | `5` |right-to-left execution ordering |
| gcc | -O1 | gnu89 | `5` |right-to-left execution ordering |
| gcc | -O1 | gnu90 | `5` |right-to-left execution ordering |
| gcc | -O1 | iso9899:1999 | `5` |right-to-left execution ordering |
| gcc | -O1 | gnu99 | `5` |right-to-left execution ordering |
| gcc | -O1 | iso9899:2011 | `5` |right-to-left execution ordering |
| gcc | -O1 | iso9899:2017 | `5` |right-to-left execution ordering |
| gcc | -O1 | gnu17 | `5` |right-to-left execution ordering |
| gcc | -O2 | c99 | `5` |right-to-left execution ordering |
| gcc | -O2 | c9x | `5` |right-to-left execution ordering |
| gcc | -O2 | c89 | `5` |right-to-left execution ordering |
| gcc | -O2 | c90 | `5` |right-to-left execution ordering |
| gcc | -O2 | c2x | `5` |right-to-left execution ordering |
| gcc | -O2 | c17 | `5` |right-to-left execution ordering |
| gcc | -O2 | c18 | `5` |right-to-left execution ordering |
| gcc | -O2 | c11 | `5` |right-to-left execution ordering |
| gcc | -O2 | c1x | `5` |right-to-left execution ordering |
| gcc | -O2 | gnu11 | `5` |right-to-left execution ordering |
| gcc | -O2 | iso9899:1990 | `5` |right-to-left execution ordering |
| gcc | -O2 | iso9899:199409 | `5` |right-to-left execution ordering |
| gcc | -O2 | gnu89 | `5` |right-to-left execution ordering |
| gcc | -O2 | gnu90 | `5` |right-to-left execution ordering |
| gcc | -O2 | iso9899:1999 | `5` |right-to-left execution ordering |
| gcc | -O2 | gnu99 | `5` |right-to-left execution ordering |
| gcc | -O2 | iso9899:2011 | `5` |right-to-left execution ordering |
| gcc | -O2 | iso9899:2017 | `5` |right-to-left execution ordering |
| gcc | -O2 | gnu17 | `5` |right-to-left execution ordering |
| gcc | -O3 | c99 | `5` |right-to-left execution ordering |
| gcc | -O3 | c9x | `5` |right-to-left execution ordering |
| gcc | -O3 | c89 | `5` |right-to-left execution ordering |
| gcc | -O3 | c90 | `5` |right-to-left execution ordering |
| gcc | -O3 | c2x | `5` |right-to-left execution ordering |
| gcc | -O3 | c17 | `5` |right-to-left execution ordering |
| gcc | -O3 | c18 | `5` |right-to-left execution ordering |
| gcc | -O3 | c11 | `5` |right-to-left execution ordering |
| gcc | -O3 | c1x | `5` |right-to-left execution ordering |
| gcc | -O3 | gnu11 | `5` |right-to-left execution ordering |
| gcc | -O3 | iso9899:1990 | `5` |right-to-left execution ordering |
| gcc | -O3 | iso9899:199409 | `5` |right-to-left execution ordering |
| gcc | -O3 | gnu89 | `5` |right-to-left execution ordering |
| gcc | -O3 | gnu90 | `5` |right-to-left execution ordering |
| gcc | -O3 | iso9899:1999 | `5` |right-to-left execution ordering |
| gcc | -O3 | gnu99 | `5` |right-to-left execution ordering |
| gcc | -O3 | iso9899:2011 | `5` |right-to-left execution ordering |
| gcc | -O3 | iso9899:2017 | `5` |right-to-left execution ordering |
| gcc | -O3 | gnu17 | `5` |right-to-left execution ordering |
| clang | -O0 | c99 | `4` |left-to-right execution ordering |
| clang | -O0 | c9x | `4` |left-to-right execution ordering |
| clang | -O0 | c89 | `4` |left-to-right execution ordering |
| clang | -O0 | c90 | `4` |left-to-right execution ordering |
| clang | -O0 | c2x | `4` |left-to-right execution ordering |
| clang | -O0 | c17 | `4` |left-to-right execution ordering |
| clang | -O0 | c18 | `4` |left-to-right execution ordering |
| clang | -O0 | c11 | `4` |left-to-right execution ordering |
| clang | -O0 | c1x | `4` |left-to-right execution ordering |
| clang | -O0 | gnu11 | `4` |left-to-right execution ordering |
| clang | -O0 | iso9899:1990 | `4` |left-to-right execution ordering |
| clang | -O0 | iso9899:199409 | `4` |left-to-right execution ordering |
| clang | -O0 | gnu89 | `4` |left-to-right execution ordering |
| clang | -O0 | gnu90 | `4` |left-to-right execution ordering |
| clang | -O0 | iso9899:1999 | `4` |left-to-right execution ordering |
| clang | -O0 | gnu99 | `4` |left-to-right execution ordering |
| clang | -O0 | iso9899:2011 | `4` |left-to-right execution ordering |
| clang | -O0 | iso9899:2017 | `4` |left-to-right execution ordering |
| clang | -O0 | gnu17 | `4` |left-to-right execution ordering |
| clang | -O1 | c99 | `4` |left-to-right execution ordering |
| clang | -O1 | c9x | `4` |left-to-right execution ordering |
| clang | -O1 | c89 | `4` |left-to-right execution ordering |
| clang | -O1 | c90 | `4` |left-to-right execution ordering |
| clang | -O1 | c2x | `4` |left-to-right execution ordering |
| clang | -O1 | c17 | `4` |left-to-right execution ordering |
| clang | -O1 | c18 | `4` |left-to-right execution ordering |
| clang | -O1 | c11 | `4` |left-to-right execution ordering |
| clang | -O1 | c1x | `4` |left-to-right execution ordering |
| clang | -O1 | gnu11 | `4` |left-to-right execution ordering |
| clang | -O1 | iso9899:1990 | `4` |left-to-right execution ordering |
| clang | -O1 | iso9899:199409 | `4` |left-to-right execution ordering |
| clang | -O1 | gnu89 | `4` |left-to-right execution ordering |
| clang | -O1 | gnu90 | `4` |left-to-right execution ordering |
| clang | -O1 | iso9899:1999 | `4` |left-to-right execution ordering |
| clang | -O1 | gnu99 | `4` |left-to-right execution ordering |
| clang | -O1 | iso9899:2011 | `4` |left-to-right execution ordering |
| clang | -O1 | iso9899:2017 | `4` |left-to-right execution ordering |
| clang | -O1 | gnu17 | `4` |left-to-right execution ordering |
| clang | -O2 | c99 | `4` |left-to-right execution ordering |
| clang | -O2 | c9x | `4` |left-to-right execution ordering |
| clang | -O2 | c89 | `4` |left-to-right execution ordering |
| clang | -O2 | c90 | `4` |left-to-right execution ordering |
| clang | -O2 | c2x | `4` |left-to-right execution ordering |
| clang | -O2 | c17 | `4` |left-to-right execution ordering |
| clang | -O2 | c18 | `4` |left-to-right execution ordering |
| clang | -O2 | c11 | `4` |left-to-right execution ordering |
| clang | -O2 | c1x | `4` |left-to-right execution ordering |
| clang | -O2 | gnu11 | `4` |left-to-right execution ordering |
| clang | -O2 | iso9899:1990 | `4` |left-to-right execution ordering |
| clang | -O2 | iso9899:199409 | `4` |left-to-right execution ordering |
| clang | -O2 | gnu89 | `4` |left-to-right execution ordering |
| clang | -O2 | gnu90 | `4` |left-to-right execution ordering |
| clang | -O2 | iso9899:1999 | `4` |left-to-right execution ordering |
| clang | -O2 | gnu99 | `4` |left-to-right execution ordering |
| clang | -O2 | iso9899:2011 | `4` |left-to-right execution ordering |
| clang | -O2 | iso9899:2017 | `4` |left-to-right execution ordering |
| clang | -O2 | gnu17 | `4` |left-to-right execution ordering |
| clang | -O3 | c99 | `4` |left-to-right execution ordering |
| clang | -O3 | c9x | `4` |left-to-right execution ordering |
| clang | -O3 | c89 | `4` |left-to-right execution ordering |
| clang | -O3 | c90 | `4` |left-to-right execution ordering |
| clang | -O3 | c2x | `4` |left-to-right execution ordering |
| clang | -O3 | c17 | `4` |left-to-right execution ordering |
| clang | -O3 | c18 | `4` |left-to-right execution ordering |
| clang | -O3 | c11 | `4` |left-to-right execution ordering |
| clang | -O3 | c1x | `4` |left-to-right execution ordering |
| clang | -O3 | gnu11 | `4` |left-to-right execution ordering |
| clang | -O3 | iso9899:1990 | `4` |left-to-right execution ordering |
| clang | -O3 | iso9899:199409 | `4` |left-to-right execution ordering |
| clang | -O3 | gnu89 | `4` |left-to-right execution ordering |
| clang | -O3 | gnu90 | `4` |left-to-right execution ordering |
| clang | -O3 | iso9899:1999 | `4` |left-to-right execution ordering |
| clang | -O3 | gnu99 | `4` |left-to-right execution ordering |
| clang | -O3 | iso9899:2011 | `4` |left-to-right execution ordering |
| clang | -O3 | iso9899:2017 | `4` |left-to-right execution ordering |
| clang | -O3 | gnu17 | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O0 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O1 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O2 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| ccomp | -O3 | Default | `4` |left-to-right execution ordering |
| tcc | -O0 | c99 | `4` |left-to-right execution ordering |
| tcc | -O0 | c9x | `4` |left-to-right execution ordering |
| tcc | -O0 | c89 | `4` |left-to-right execution ordering |
| tcc | -O0 | c90 | `4` |left-to-right execution ordering |
| tcc | -O0 | c2x | `4` |left-to-right execution ordering |
| tcc | -O0 | c17 | `4` |left-to-right execution ordering |
| tcc | -O0 | c18 | `4` |left-to-right execution ordering |
| tcc | -O0 | c11 | `4` |left-to-right execution ordering |
| tcc | -O0 | c1x | `4` |left-to-right execution ordering |
| tcc | -O0 | gnu11 | `4` |left-to-right execution ordering |
| tcc | -O0 | iso9899:1990 | `4` |left-to-right execution ordering |
| tcc | -O0 | iso9899:199409 | `4` |left-to-right execution ordering |
| tcc | -O0 | gnu89 | `4` |left-to-right execution ordering |
| tcc | -O0 | gnu90 | `4` |left-to-right execution ordering |
| tcc | -O0 | iso9899:1999 | `4` |left-to-right execution ordering |
| tcc | -O0 | gnu99 | `4` |left-to-right execution ordering |
| tcc | -O0 | iso9899:2011 | `4` |left-to-right execution ordering |
| tcc | -O0 | iso9899:2017 | `4` |left-to-right execution ordering |
| tcc | -O0 | gnu17 | `4` |left-to-right execution ordering |
| tcc | -O1 | c99 | `4` |left-to-right execution ordering |
| tcc | -O1 | c9x | `4` |left-to-right execution ordering |
| tcc | -O1 | c89 | `4` |left-to-right execution ordering |
| tcc | -O1 | c90 | `4` |left-to-right execution ordering |
| tcc | -O1 | c2x | `4` |left-to-right execution ordering |
| tcc | -O1 | c17 | `4` |left-to-right execution ordering |
| tcc | -O1 | c18 | `4` |left-to-right execution ordering |
| tcc | -O1 | c11 | `4` |left-to-right execution ordering |
| tcc | -O1 | c1x | `4` |left-to-right execution ordering |
| tcc | -O1 | gnu11 | `4` |left-to-right execution ordering |
| tcc | -O1 | iso9899:1990 | `4` |left-to-right execution ordering |
| tcc | -O1 | iso9899:199409 | `4` |left-to-right execution ordering |
| tcc | -O1 | gnu89 | `4` |left-to-right execution ordering |
| tcc | -O1 | gnu90 | `4` |left-to-right execution ordering |
| tcc | -O1 | iso9899:1999 | `4` |left-to-right execution ordering |
| tcc | -O1 | gnu99 | `4` |left-to-right execution ordering |
| tcc | -O1 | iso9899:2011 | `4` |left-to-right execution ordering |
| tcc | -O1 | iso9899:2017 | `4` |left-to-right execution ordering |
| tcc | -O1 | gnu17 | `4` |left-to-right execution ordering |
| tcc | -O2 | c99 | `4` |left-to-right execution ordering |
| tcc | -O2 | c9x | `4` |left-to-right execution ordering |
| tcc | -O2 | c89 | `4` |left-to-right execution ordering |
| tcc | -O2 | c90 | `4` |left-to-right execution ordering |
| tcc | -O2 | c2x | `4` |left-to-right execution ordering |
| tcc | -O2 | c17 | `4` |left-to-right execution ordering |
| tcc | -O2 | c18 | `4` |left-to-right execution ordering |
| tcc | -O2 | c11 | `4` |left-to-right execution ordering |
| tcc | -O2 | c1x | `4` |left-to-right execution ordering |
| tcc | -O2 | gnu11 | `4` |left-to-right execution ordering |
| tcc | -O2 | iso9899:1990 | `4` |left-to-right execution ordering |
| tcc | -O2 | iso9899:199409 | `4` |left-to-right execution ordering |
| tcc | -O2 | gnu89 | `4` |left-to-right execution ordering |
| tcc | -O2 | gnu90 | `4` |left-to-right execution ordering |
| tcc | -O2 | iso9899:1999 | `4` |left-to-right execution ordering |
| tcc | -O2 | gnu99 | `4` |left-to-right execution ordering |
| tcc | -O2 | iso9899:2011 | `4` |left-to-right execution ordering |
| tcc | -O2 | iso9899:2017 | `4` |left-to-right execution ordering |
| tcc | -O2 | gnu17 | `4` |left-to-right execution ordering |
| tcc | -O3 | c99 | `4` |left-to-right execution ordering |
| tcc | -O3 | c9x | `4` |left-to-right execution ordering |
| tcc | -O3 | c89 | `4` |left-to-right execution ordering |
| tcc | -O3 | c90 | `4` |left-to-right execution ordering |
| tcc | -O3 | c2x | `4` |left-to-right execution ordering |
| tcc | -O3 | c17 | `4` |left-to-right execution ordering |
| tcc | -O3 | c18 | `4` |left-to-right execution ordering |
| tcc | -O3 | c11 | `4` |left-to-right execution ordering |
| tcc | -O3 | c1x | `4` |left-to-right execution ordering |
| tcc | -O3 | gnu11 | `4` |left-to-right execution ordering |
| tcc | -O3 | iso9899:1990 | `4` |left-to-right execution ordering |
| tcc | -O3 | iso9899:199409 | `4` |left-to-right execution ordering |
| tcc | -O3 | gnu89 | `4` |left-to-right execution ordering |
| tcc | -O3 | gnu90 | `4` |left-to-right execution ordering |
| tcc | -O3 | iso9899:1999 | `4` |left-to-right execution ordering |
| tcc | -O3 | gnu99 | `4` |left-to-right execution ordering |
| tcc | -O3 | iso9899:2011 | `4` |left-to-right execution ordering |
| tcc | -O3 | iso9899:2017 | `4` |left-to-right execution ordering |
| tcc | -O3 | gnu17 | `4` |left-to-right execution ordering |

### Verifier tests

| Verifier | Options| Standard | `exit code` | Observation | Expected assertion|
|----|----|----|---|---|---|
| cbmc | [] |  | `0` | `assert(result == 4);` |✅️ |
| esbmc | [] |  | `0` | `assert(result == 4);` |✅️ |


| Verifier | Options| Standard | `exit code` | Observation | Expected assertion|
|----|----|----|---|---|---|
| cbmc | [] |  | `10` | `assert(result == 5);` |❌️ |
| esbmc | [] |  | `1` | `assert(result == 5);` |❌️ |

<br>

## Explicit versions

Unwound versions that can be used for benchmarking. Every compiler and options
combination that has a certain obersavtion is matched below:

### right-to-left execution ordering

```c
#include<assert.h>
#include<stdio.h>

int globalCounter = 0;

int counter()
{
        globalCounter++;
        return globalCounter;
}

int f(int x1, int x2)
{
        return (x1*2)+x2;
}

int main()
{
        int arg2 = counter();
        int arg1 = counter();
        int result = f(arg1, arg2);

        assert(result == 5);
}
```

<br>

### left-to-right execution ordering

```c
#include<assert.h>
#include<stdio.h>

int globalCounter = 0;

int counter()
{
        globalCounter++;
        return globalCounter;
}

int f(int x1, int x2)
{
        return (x1*2)+x2;
}

int main()
{
        int arg1 = counter();
        int arg2 = counter();
        int result = f(arg1, arg2);

        assert(result == 4);
}
```