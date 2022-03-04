**F2**: Function Call Evaluation ordering (3 args)
=================================================

<br>

## Description

The order in which the arguments to a function is evaluated is unspeccified. Here
we will be looking at a piece of code which results in different outcomes depending
on the order in which these funtions are executed by using globally shared state
with funcitons senstiive to that.

```c
int result = f(counter(), counter(), counter());
```

**F2** is simply [F1](TODO) but with three arguments.

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

int f(int x1, int x2, int x3)
{
        return (x1*2)+x2+x3*3;
}

int main()
{
        /**
        * 1,2,3 => (1*2)+2+3*3 => 13
        * 3,2,1 => (3*2)+2+1*3 => 11
        */
        int result = f(counter(), counter(), counter());

        printf("%d\n", result);
}
```

<br>

## Results of behaviour

Here we document the behaviour of various C compiler implementations on the above code
and which arguments (further variations) were used and lastly the outcome. You can
reproduce the below by checking out the [F2 tests](https://github.com/cbtorture/tests/tree/master/functions/f2).

Running: `program.c`

| Compiler | Options| Standard | `result` | Observation |
|----|----|----|---|---|
| gcc | -O0 | c99 | `11` |right-to-left execution ordering |
| gcc | -O0 | c9x | `11` |right-to-left execution ordering |
| gcc | -O0 | c89 | `11` |right-to-left execution ordering |
| gcc | -O0 | c90 | `11` |right-to-left execution ordering |
| gcc | -O0 | c2x | `11` |right-to-left execution ordering |
| gcc | -O0 | c17 | `11` |right-to-left execution ordering |
| gcc | -O0 | c18 | `11` |right-to-left execution ordering |
| gcc | -O0 | c11 | `11` |right-to-left execution ordering |
| gcc | -O0 | c1x | `11` |right-to-left execution ordering |
| gcc | -O0 | gnu11 | `11` |right-to-left execution ordering |
| gcc | -O0 | iso9899:1990 | `11` |right-to-left execution ordering |
| gcc | -O0 | iso9899:199409 | `11` |right-to-left execution ordering |
| gcc | -O0 | gnu89 | `11` |right-to-left execution ordering |
| gcc | -O0 | gnu90 | `11` |right-to-left execution ordering |
| gcc | -O0 | iso9899:1999 | `11` |right-to-left execution ordering |
| gcc | -O0 | gnu99 | `11` |right-to-left execution ordering |
| gcc | -O0 | iso9899:2011 | `11` |right-to-left execution ordering |
| gcc | -O0 | iso9899:2017 | `11` |right-to-left execution ordering |
| gcc | -O0 | gnu17 | `11` |right-to-left execution ordering |
| gcc | -O1 | c99 | `11` |right-to-left execution ordering |
| gcc | -O1 | c9x | `11` |right-to-left execution ordering |
| gcc | -O1 | c89 | `11` |right-to-left execution ordering |
| gcc | -O1 | c90 | `11` |right-to-left execution ordering |
| gcc | -O1 | c2x | `11` |right-to-left execution ordering |
| gcc | -O1 | c17 | `11` |right-to-left execution ordering |
| gcc | -O1 | c18 | `11` |right-to-left execution ordering |
| gcc | -O1 | c11 | `11` |right-to-left execution ordering |
| gcc | -O1 | c1x | `11` |right-to-left execution ordering |
| gcc | -O1 | gnu11 | `11` |right-to-left execution ordering |
| gcc | -O1 | iso9899:1990 | `11` |right-to-left execution ordering |
| gcc | -O1 | iso9899:199409 | `11` |right-to-left execution ordering |
| gcc | -O1 | gnu89 | `11` |right-to-left execution ordering |
| gcc | -O1 | gnu90 | `11` |right-to-left execution ordering |
| gcc | -O1 | iso9899:1999 | `11` |right-to-left execution ordering |
| gcc | -O1 | gnu99 | `11` |right-to-left execution ordering |
| gcc | -O1 | iso9899:2011 | `11` |right-to-left execution ordering |
| gcc | -O1 | iso9899:2017 | `11` |right-to-left execution ordering |
| gcc | -O1 | gnu17 | `11` |right-to-left execution ordering |
| gcc | -O2 | c99 | `11` |right-to-left execution ordering |
| gcc | -O2 | c9x | `11` |right-to-left execution ordering |
| gcc | -O2 | c89 | `11` |right-to-left execution ordering |
| gcc | -O2 | c90 | `11` |right-to-left execution ordering |
| gcc | -O2 | c2x | `11` |right-to-left execution ordering |
| gcc | -O2 | c17 | `11` |right-to-left execution ordering |
| gcc | -O2 | c18 | `11` |right-to-left execution ordering |
| gcc | -O2 | c11 | `11` |right-to-left execution ordering |
| gcc | -O2 | c1x | `11` |right-to-left execution ordering |
| gcc | -O2 | gnu11 | `11` |right-to-left execution ordering |
| gcc | -O2 | iso9899:1990 | `11` |right-to-left execution ordering |
| gcc | -O2 | iso9899:199409 | `11` |right-to-left execution ordering |
| gcc | -O2 | gnu89 | `11` |right-to-left execution ordering |
| gcc | -O2 | gnu90 | `11` |right-to-left execution ordering |
| gcc | -O2 | iso9899:1999 | `11` |right-to-left execution ordering |
| gcc | -O2 | gnu99 | `11` |right-to-left execution ordering |
| gcc | -O2 | iso9899:2011 | `11` |right-to-left execution ordering |
| gcc | -O2 | iso9899:2017 | `11` |right-to-left execution ordering |
| gcc | -O2 | gnu17 | `11` |right-to-left execution ordering |
| gcc | -O3 | c99 | `11` |right-to-left execution ordering |
| gcc | -O3 | c9x | `11` |right-to-left execution ordering |
| gcc | -O3 | c89 | `11` |right-to-left execution ordering |
| gcc | -O3 | c90 | `11` |right-to-left execution ordering |
| gcc | -O3 | c2x | `11` |right-to-left execution ordering |
| gcc | -O3 | c17 | `11` |right-to-left execution ordering |
| gcc | -O3 | c18 | `11` |right-to-left execution ordering |
| gcc | -O3 | c11 | `11` |right-to-left execution ordering |
| gcc | -O3 | c1x | `11` |right-to-left execution ordering |
| gcc | -O3 | gnu11 | `11` |right-to-left execution ordering |
| gcc | -O3 | iso9899:1990 | `11` |right-to-left execution ordering |
| gcc | -O3 | iso9899:199409 | `11` |right-to-left execution ordering |
| gcc | -O3 | gnu89 | `11` |right-to-left execution ordering |
| gcc | -O3 | gnu90 | `11` |right-to-left execution ordering |
| gcc | -O3 | iso9899:1999 | `11` |right-to-left execution ordering |
| gcc | -O3 | gnu99 | `11` |right-to-left execution ordering |
| gcc | -O3 | iso9899:2011 | `11` |right-to-left execution ordering |
| gcc | -O3 | iso9899:2017 | `11` |right-to-left execution ordering |
| gcc | -O3 | gnu17 | `11` |right-to-left execution ordering |
| clang | -O0 | c99 | `13` |left-to-right execution ordering |
| clang | -O0 | c9x | `13` |left-to-right execution ordering |
| clang | -O0 | c89 | `13` |left-to-right execution ordering |
| clang | -O0 | c90 | `13` |left-to-right execution ordering |
| clang | -O0 | c2x | `13` |left-to-right execution ordering |
| clang | -O0 | c17 | `13` |left-to-right execution ordering |
| clang | -O0 | c18 | `13` |left-to-right execution ordering |
| clang | -O0 | c11 | `13` |left-to-right execution ordering |
| clang | -O0 | c1x | `13` |left-to-right execution ordering |
| clang | -O0 | gnu11 | `13` |left-to-right execution ordering |
| clang | -O0 | iso9899:1990 | `13` |left-to-right execution ordering |
| clang | -O0 | iso9899:199409 | `13` |left-to-right execution ordering |
| clang | -O0 | gnu89 | `13` |left-to-right execution ordering |
| clang | -O0 | gnu90 | `13` |left-to-right execution ordering |
| clang | -O0 | iso9899:1999 | `13` |left-to-right execution ordering |
| clang | -O0 | gnu99 | `13` |left-to-right execution ordering |
| clang | -O0 | iso9899:2011 | `13` |left-to-right execution ordering |
| clang | -O0 | iso9899:2017 | `13` |left-to-right execution ordering |
| clang | -O0 | gnu17 | `13` |left-to-right execution ordering |
| clang | -O1 | c99 | `13` |left-to-right execution ordering |
| clang | -O1 | c9x | `13` |left-to-right execution ordering |
| clang | -O1 | c89 | `13` |left-to-right execution ordering |
| clang | -O1 | c90 | `13` |left-to-right execution ordering |
| clang | -O1 | c2x | `13` |left-to-right execution ordering |
| clang | -O1 | c17 | `13` |left-to-right execution ordering |
| clang | -O1 | c18 | `13` |left-to-right execution ordering |
| clang | -O1 | c11 | `13` |left-to-right execution ordering |
| clang | -O1 | c1x | `13` |left-to-right execution ordering |
| clang | -O1 | gnu11 | `13` |left-to-right execution ordering |
| clang | -O1 | iso9899:1990 | `13` |left-to-right execution ordering |
| clang | -O1 | iso9899:199409 | `13` |left-to-right execution ordering |
| clang | -O1 | gnu89 | `13` |left-to-right execution ordering |
| clang | -O1 | gnu90 | `13` |left-to-right execution ordering |
| clang | -O1 | iso9899:1999 | `13` |left-to-right execution ordering |
| clang | -O1 | gnu99 | `13` |left-to-right execution ordering |
| clang | -O1 | iso9899:2011 | `13` |left-to-right execution ordering |
| clang | -O1 | iso9899:2017 | `13` |left-to-right execution ordering |
| clang | -O1 | gnu17 | `13` |left-to-right execution ordering |
| clang | -O2 | c99 | `13` |left-to-right execution ordering |
| clang | -O2 | c9x | `13` |left-to-right execution ordering |
| clang | -O2 | c89 | `13` |left-to-right execution ordering |
| clang | -O2 | c90 | `13` |left-to-right execution ordering |
| clang | -O2 | c2x | `13` |left-to-right execution ordering |
| clang | -O2 | c17 | `13` |left-to-right execution ordering |
| clang | -O2 | c18 | `13` |left-to-right execution ordering |
| clang | -O2 | c11 | `13` |left-to-right execution ordering |
| clang | -O2 | c1x | `13` |left-to-right execution ordering |
| clang | -O2 | gnu11 | `13` |left-to-right execution ordering |
| clang | -O2 | iso9899:1990 | `13` |left-to-right execution ordering |
| clang | -O2 | iso9899:199409 | `13` |left-to-right execution ordering |
| clang | -O2 | gnu89 | `13` |left-to-right execution ordering |
| clang | -O2 | gnu90 | `13` |left-to-right execution ordering |
| clang | -O2 | iso9899:1999 | `13` |left-to-right execution ordering |
| clang | -O2 | gnu99 | `13` |left-to-right execution ordering |
| clang | -O2 | iso9899:2011 | `13` |left-to-right execution ordering |
| clang | -O2 | iso9899:2017 | `13` |left-to-right execution ordering |
| clang | -O2 | gnu17 | `13` |left-to-right execution ordering |
| clang | -O3 | c99 | `13` |left-to-right execution ordering |
| clang | -O3 | c9x | `13` |left-to-right execution ordering |
| clang | -O3 | c89 | `13` |left-to-right execution ordering |
| clang | -O3 | c90 | `13` |left-to-right execution ordering |
| clang | -O3 | c2x | `13` |left-to-right execution ordering |
| clang | -O3 | c17 | `13` |left-to-right execution ordering |
| clang | -O3 | c18 | `13` |left-to-right execution ordering |
| clang | -O3 | c11 | `13` |left-to-right execution ordering |
| clang | -O3 | c1x | `13` |left-to-right execution ordering |
| clang | -O3 | gnu11 | `13` |left-to-right execution ordering |
| clang | -O3 | iso9899:1990 | `13` |left-to-right execution ordering |
| clang | -O3 | iso9899:199409 | `13` |left-to-right execution ordering |
| clang | -O3 | gnu89 | `13` |left-to-right execution ordering |
| clang | -O3 | gnu90 | `13` |left-to-right execution ordering |
| clang | -O3 | iso9899:1999 | `13` |left-to-right execution ordering |
| clang | -O3 | gnu99 | `13` |left-to-right execution ordering |
| clang | -O3 | iso9899:2011 | `13` |left-to-right execution ordering |
| clang | -O3 | iso9899:2017 | `13` |left-to-right execution ordering |
| clang | -O3 | gnu17 | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O0 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O1 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O2 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| ccomp | -O3 | Default | `13` |left-to-right execution ordering |
| tcc | -O0 | c99 | `13` |left-to-right execution ordering |
| tcc | -O0 | c9x | `13` |left-to-right execution ordering |
| tcc | -O0 | c89 | `13` |left-to-right execution ordering |
| tcc | -O0 | c90 | `13` |left-to-right execution ordering |
| tcc | -O0 | c2x | `13` |left-to-right execution ordering |
| tcc | -O0 | c17 | `13` |left-to-right execution ordering |
| tcc | -O0 | c18 | `13` |left-to-right execution ordering |
| tcc | -O0 | c11 | `13` |left-to-right execution ordering |
| tcc | -O0 | c1x | `13` |left-to-right execution ordering |
| tcc | -O0 | gnu11 | `13` |left-to-right execution ordering |
| tcc | -O0 | iso9899:1990 | `13` |left-to-right execution ordering |
| tcc | -O0 | iso9899:199409 | `13` |left-to-right execution ordering |
| tcc | -O0 | gnu89 | `13` |left-to-right execution ordering |
| tcc | -O0 | gnu90 | `13` |left-to-right execution ordering |
| tcc | -O0 | iso9899:1999 | `13` |left-to-right execution ordering |
| tcc | -O0 | gnu99 | `13` |left-to-right execution ordering |
| tcc | -O0 | iso9899:2011 | `13` |left-to-right execution ordering |
| tcc | -O0 | iso9899:2017 | `13` |left-to-right execution ordering |
| tcc | -O0 | gnu17 | `13` |left-to-right execution ordering |
| tcc | -O1 | c99 | `13` |left-to-right execution ordering |
| tcc | -O1 | c9x | `13` |left-to-right execution ordering |
| tcc | -O1 | c89 | `13` |left-to-right execution ordering |
| tcc | -O1 | c90 | `13` |left-to-right execution ordering |
| tcc | -O1 | c2x | `13` |left-to-right execution ordering |
| tcc | -O1 | c17 | `13` |left-to-right execution ordering |
| tcc | -O1 | c18 | `13` |left-to-right execution ordering |
| tcc | -O1 | c11 | `13` |left-to-right execution ordering |
| tcc | -O1 | c1x | `13` |left-to-right execution ordering |
| tcc | -O1 | gnu11 | `13` |left-to-right execution ordering |
| tcc | -O1 | iso9899:1990 | `13` |left-to-right execution ordering |
| tcc | -O1 | iso9899:199409 | `13` |left-to-right execution ordering |
| tcc | -O1 | gnu89 | `13` |left-to-right execution ordering |
| tcc | -O1 | gnu90 | `13` |left-to-right execution ordering |
| tcc | -O1 | iso9899:1999 | `13` |left-to-right execution ordering |
| tcc | -O1 | gnu99 | `13` |left-to-right execution ordering |
| tcc | -O1 | iso9899:2011 | `13` |left-to-right execution ordering |
| tcc | -O1 | iso9899:2017 | `13` |left-to-right execution ordering |
| tcc | -O1 | gnu17 | `13` |left-to-right execution ordering |
| tcc | -O2 | c99 | `13` |left-to-right execution ordering |
| tcc | -O2 | c9x | `13` |left-to-right execution ordering |
| tcc | -O2 | c89 | `13` |left-to-right execution ordering |
| tcc | -O2 | c90 | `13` |left-to-right execution ordering |
| tcc | -O2 | c2x | `13` |left-to-right execution ordering |
| tcc | -O2 | c17 | `13` |left-to-right execution ordering |
| tcc | -O2 | c18 | `13` |left-to-right execution ordering |
| tcc | -O2 | c11 | `13` |left-to-right execution ordering |
| tcc | -O2 | c1x | `13` |left-to-right execution ordering |
| tcc | -O2 | gnu11 | `13` |left-to-right execution ordering |
| tcc | -O2 | iso9899:1990 | `13` |left-to-right execution ordering |
| tcc | -O2 | iso9899:199409 | `13` |left-to-right execution ordering |
| tcc | -O2 | gnu89 | `13` |left-to-right execution ordering |
| tcc | -O2 | gnu90 | `13` |left-to-right execution ordering |
| tcc | -O2 | iso9899:1999 | `13` |left-to-right execution ordering |
| tcc | -O2 | gnu99 | `13` |left-to-right execution ordering |
| tcc | -O2 | iso9899:2011 | `13` |left-to-right execution ordering |
| tcc | -O2 | iso9899:2017 | `13` |left-to-right execution ordering |
| tcc | -O2 | gnu17 | `13` |left-to-right execution ordering |
| tcc | -O3 | c99 | `13` |left-to-right execution ordering |
| tcc | -O3 | c9x | `13` |left-to-right execution ordering |
| tcc | -O3 | c89 | `13` |left-to-right execution ordering |
| tcc | -O3 | c90 | `13` |left-to-right execution ordering |
| tcc | -O3 | c2x | `13` |left-to-right execution ordering |
| tcc | -O3 | c17 | `13` |left-to-right execution ordering |
| tcc | -O3 | c18 | `13` |left-to-right execution ordering |
| tcc | -O3 | c11 | `13` |left-to-right execution ordering |
| tcc | -O3 | c1x | `13` |left-to-right execution ordering |
| tcc | -O3 | gnu11 | `13` |left-to-right execution ordering |
| tcc | -O3 | iso9899:1990 | `13` |left-to-right execution ordering |
| tcc | -O3 | iso9899:199409 | `13` |left-to-right execution ordering |
| tcc | -O3 | gnu89 | `13` |left-to-right execution ordering |
| tcc | -O3 | gnu90 | `13` |left-to-right execution ordering |
| tcc | -O3 | iso9899:1999 | `13` |left-to-right execution ordering |
| tcc | -O3 | gnu99 | `13` |left-to-right execution ordering |
| tcc | -O3 | iso9899:2011 | `13` |left-to-right execution ordering |
| tcc | -O3 | iso9899:2017 | `13` |left-to-right execution ordering |
| tcc | -O3 | gnu17 | `13` |left-to-right execution ordering |

**TODO:** Behaviur of several software verifiers on these too

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

int f(int x1, int x2, int x3)
{
	return (x1*2)+x2+x3*3;
}

int main()
{
	/**
	* 1,2,3 => (1*2)+2+3*3 => 13
	* 3,2,1 => (3*2)+2+1*3 => 11
	*/
	int subRes1 = counter();
	int subRes2 = counter();
	int subRes3 = counter();
	int result = f(subRes3, subRes2, subRes1);

	assert(result == 11);
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

int f(int x1, int x2, int x3)
{
	return (x1*2)+x2+x3*3;
}

int main()
{
	/**
	* 1,2,3 => (1*2)+2+3*3 => 13
	* 3,2,1 => (3*2)+2+1*3 => 11
	*/
	int subRes1 = counter();
	int subRes2 = counter();
	int subRes3 = counter();
	int result = f(subRes1, subRes2, subRes3);

	assert(result == 13);
}
```