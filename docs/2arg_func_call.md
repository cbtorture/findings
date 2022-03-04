**L1**: List initializer evaluation ordering
============================================

<br>

## Description

The order in which the elements in a list initializer are executed is unspeccified. Here we
will be looking at a relatively simple example (akin to the same level as that of [F1](TODO LINK)) in
terms of possible ordering outcomes.

```c
int result[2] = {counter(), counter()};
```

The execution can change which value we have at `result[0]` and `result[1]`; this is a simple
example as there are only two permutations.

## Implicit version

The implicit version of what we will be analysing is related to the following:

```c
#include<assert.h>
#include<stdio.h>

/* Global data */
int count = 0;

/* Counting function */
int counter()
{
        count++;
        return count;
}

int main()
{
        /**
        * Calculate the array
        *
        * Ex1: {1, 2}
        * Ex2: {2, 1}
        */
        int result[2] = {counter(), counter()};

        printf("[%d, %d]\n", result[0], result[1]);
}        
```

<br>

## Results of behaviour

Here we document the behaviour of various C compiler implementations on the above code
and which arguments (further variations) were used and lastly the outcome. You can
reproduce the below by checking out the [L1 tests](https://github.com/cbtorture/tests/tree/master/lists/l1).

Running: `program.c`

| Compiler | Options| Standard | `result` | Observation |
|----|----|----|---|---|
| gcc | -O0 | c99 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | c9x | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | c89 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | c90 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | c2x | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | c17 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | c18 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | c11 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | c1x | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | gnu11 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | iso9899:1990 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | iso9899:199409 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | gnu89 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | gnu90 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | iso9899:1999 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | gnu99 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | iso9899:2011 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | iso9899:2017 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O0 | gnu17 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | c99 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | c9x | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | c89 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | c90 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | c2x | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | c17 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | c18 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | c11 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | c1x | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | gnu11 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | iso9899:1990 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | iso9899:199409 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | gnu89 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | gnu90 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | iso9899:1999 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | gnu99 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | iso9899:2011 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | iso9899:2017 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O1 | gnu17 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | c99 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | c9x | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | c89 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | c90 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | c2x | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | c17 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | c18 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | c11 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | c1x | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | gnu11 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | iso9899:1990 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | iso9899:199409 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | gnu89 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | gnu90 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | iso9899:1999 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | gnu99 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | iso9899:2011 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | iso9899:2017 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O2 | gnu17 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | c99 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | c9x | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | c89 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | c90 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | c2x | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | c17 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | c18 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | c11 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | c1x | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | gnu11 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | iso9899:1990 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | iso9899:199409 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | gnu89 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | gnu90 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | iso9899:1999 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | gnu99 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | iso9899:2011 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | iso9899:2017 | `[1, 2]` |left-to-right execution ordering |
| gcc | -O3 | gnu17 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | c99 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | c9x | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | c89 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | c90 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | c2x | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | c17 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | c18 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | c11 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | c1x | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | gnu11 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | iso9899:1990 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | iso9899:199409 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | gnu89 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | gnu90 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | iso9899:1999 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | gnu99 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | iso9899:2011 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | iso9899:2017 | `[1, 2]` |left-to-right execution ordering |
| clang | -O0 | gnu17 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | c99 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | c9x | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | c89 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | c90 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | c2x | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | c17 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | c18 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | c11 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | c1x | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | gnu11 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | iso9899:1990 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | iso9899:199409 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | gnu89 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | gnu90 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | iso9899:1999 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | gnu99 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | iso9899:2011 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | iso9899:2017 | `[1, 2]` |left-to-right execution ordering |
| clang | -O1 | gnu17 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | c99 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | c9x | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | c89 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | c90 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | c2x | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | c17 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | c18 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | c11 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | c1x | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | gnu11 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | iso9899:1990 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | iso9899:199409 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | gnu89 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | gnu90 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | iso9899:1999 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | gnu99 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | iso9899:2011 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | iso9899:2017 | `[1, 2]` |left-to-right execution ordering |
| clang | -O2 | gnu17 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | c99 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | c9x | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | c89 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | c90 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | c2x | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | c17 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | c18 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | c11 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | c1x | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | gnu11 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | iso9899:1990 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | iso9899:199409 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | gnu89 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | gnu90 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | iso9899:1999 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | gnu99 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | iso9899:2011 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | iso9899:2017 | `[1, 2]` |left-to-right execution ordering |
| clang | -O3 | gnu17 | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O0 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O1 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O2 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| ccomp | -O3 | Default | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | c99 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | c9x | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | c89 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | c90 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | c2x | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | c17 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | c18 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | c11 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | c1x | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | gnu11 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | iso9899:1990 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | iso9899:199409 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | gnu89 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | gnu90 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | iso9899:1999 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | gnu99 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | iso9899:2011 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | iso9899:2017 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O0 | gnu17 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | c99 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | c9x | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | c89 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | c90 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | c2x | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | c17 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | c18 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | c11 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | c1x | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | gnu11 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | iso9899:1990 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | iso9899:199409 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | gnu89 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | gnu90 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | iso9899:1999 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | gnu99 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | iso9899:2011 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | iso9899:2017 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O1 | gnu17 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | c99 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | c9x | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | c89 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | c90 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | c2x | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | c17 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | c18 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | c11 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | c1x | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | gnu11 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | iso9899:1990 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | iso9899:199409 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | gnu89 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | gnu90 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | iso9899:1999 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | gnu99 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | iso9899:2011 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | iso9899:2017 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O2 | gnu17 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | c99 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | c9x | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | c89 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | c90 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | c2x | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | c17 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | c18 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | c11 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | c1x | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | gnu11 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | iso9899:1990 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | iso9899:199409 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | gnu89 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | gnu90 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | iso9899:1999 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | gnu99 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | iso9899:2011 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | iso9899:2017 | `[1, 2]` |left-to-right execution ordering |
| tcc | -O3 | gnu17 | `[1, 2]` |left-to-right execution ordering |

**TODO:** Behaviur of several software verifiers on these too

<br>

## Explicit versions

Unwound versions that can be used for benchmarking. Every compiler and options
combination that has a certain obersavtion is matched below:

### left-to-right execution ordering

```c
#include<assert.h>
#include<stdio.h>

/* Global data */
int count = 0;

/* Counting function */
int counter()
{
        count++;
        return count;
}

int main()
{
        int res1_val = counter();
        int res2_val = counter();
        int result[2] = {res1_val, res2_val};

        assert(result[0] == 1);
        assert(result[1] == 2);
}
```