**F1**: Function Call Evaluation ordering
=========================================

<br>

## Description

The order in which the arguments to a function is evaluated is unspeccified. Here
we will be looking at a piece of code which results in different outcomes depending
on the order in which the outer function's arguments are evaluated with an outer
function that is sensitive to which argument is which value and with an inner function
which depends on global state such that each successive call results in a different result
but a determinable one:

```c
int result = f(counter(), counter());
```

**F1** is a very basic test with the same inner-function, `counter()`, being used as
the outer function's, `f(__arg1__, __arg2__)`, arguments (only 2 arguments).

## Implicit version

The implicit version of the program will be the following:

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

        //assert(result == 4);
        //assert(result == 5);
}
```

We will be testing a two versions of this same program to account for the two possible
outcomes (and also what they _mean_ semantically):

1. `result == 4`
	* This implies left-to-right evaluation
2. `result == 5`
	* This implies right-to-left evaluation

<br>

---

<br>

## Results of behaviour

Here we test several different compilers and software verifiers and several flags and C
standards per each. We will be testing two versions in this test:

### Program 1 (`result == 4`)

Running program: `functions/f1/program_analysis.c`

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

	assert(result == 4);

	return 0;
}
```


Expected assertion: `result == 4`

| Verifier | Arguments | Assertion expected | Assertion result | Job hash |
| --- | --- | --- | --- | --- |
|CBMC| ['cbmc']| `result == 4` | ✅️| `5ba82bd6b31c6f683a6898e4c1e89fb7` |
|ESBMC| ['esbmc']| `result == 4` | ✅️| `bacc94d48de173aa1de67a4d03c2e799` |
|GCC| ['gcc', '-std=c99', '-O0', '-o', '3a9ba9292af33bad43be0df6c8ea389e']| `result == 4` | ❌️| `3a9ba9292af33bad43be0df6c8ea389e` |
|GCC| ['gcc', '-std=c9x', '-O0', '-o', '20d5fe108c7fb6a8407524452312f31b']| `result == 4` | ❌️| `20d5fe108c7fb6a8407524452312f31b` |
|GCC| ['gcc', '-std=c89', '-O0', '-o', 'a6b4de34f8a8d3a2b1988c3843215f32']| `result == 4` | ❌️| `a6b4de34f8a8d3a2b1988c3843215f32` |
|GCC| ['gcc', '-std=c90', '-O0', '-o', '0107ff703d7eef96ab3789af0c9603e6']| `result == 4` | ❌️| `0107ff703d7eef96ab3789af0c9603e6` |
|GCC| ['gcc', '-std=c2x', '-O0', '-o', 'd6dba0f0e2a520ff209e0a1de3f40d1c']| `result == 4` | ❌️| `d6dba0f0e2a520ff209e0a1de3f40d1c` |
|GCC| ['gcc', '-std=c17', '-O0', '-o', '0944285764068a76ecc6fbd53f44a1ec']| `result == 4` | ❌️| `0944285764068a76ecc6fbd53f44a1ec` |
|GCC| ['gcc', '-std=c18', '-O0', '-o', '923e7d926d54da3ec9b3ab52af0dc2e8']| `result == 4` | ❌️| `923e7d926d54da3ec9b3ab52af0dc2e8` |
|GCC| ['gcc', '-std=c11', '-O0', '-o', '9383a9ced9f13f45a746db8f312ec53c']| `result == 4` | ❌️| `9383a9ced9f13f45a746db8f312ec53c` |
|GCC| ['gcc', '-std=c1x', '-O0', '-o', '43083cad79238ec5e7d2f038c4ca7b05']| `result == 4` | ❌️| `43083cad79238ec5e7d2f038c4ca7b05` |
|GCC| ['gcc', '-std=gnu11', '-O0', '-o', '593cc6a1e936396e2dfe44e73f747935']| `result == 4` | ❌️| `593cc6a1e936396e2dfe44e73f747935` |
|GCC| ['gcc', '-std=iso9899:1990', '-O0', '-o', '35b541612d58aeb32c3c35f3b7c0d32e']| `result == 4` | ❌️| `35b541612d58aeb32c3c35f3b7c0d32e` |
|GCC| ['gcc', '-std=iso9899:199409', '-O0', '-o', 'cb502324e6ca21edf23197a033b199f9']| `result == 4` | ❌️| `cb502324e6ca21edf23197a033b199f9` |
|GCC| ['gcc', '-std=gnu89', '-O0', '-o', '1388a6d1ee3a606e51909d7670650200']| `result == 4` | ❌️| `1388a6d1ee3a606e51909d7670650200` |
|GCC| ['gcc', '-std=gnu90', '-O0', '-o', '20699d4f48d67f8bbd2c088358dda93c']| `result == 4` | ❌️| `20699d4f48d67f8bbd2c088358dda93c` |
|GCC| ['gcc', '-std=iso9899:1999', '-O0', '-o', '7086bf496ee0bd00b38d1deb1457a6ed']| `result == 4` | ❌️| `7086bf496ee0bd00b38d1deb1457a6ed` |
|GCC| ['gcc', '-std=gnu99', '-O0', '-o', 'a49d775b9c92e880177902185cd9c66f']| `result == 4` | ❌️| `a49d775b9c92e880177902185cd9c66f` |
|GCC| ['gcc', '-std=iso9899:2011', '-O0', '-o', 'cfcdef135180100186eebe3c34c4d2fb']| `result == 4` | ❌️| `cfcdef135180100186eebe3c34c4d2fb` |
|GCC| ['gcc', '-std=iso9899:2017', '-O0', '-o', 'dae39efc35cfac623901798dd4a7e73c']| `result == 4` | ❌️| `dae39efc35cfac623901798dd4a7e73c` |
|GCC| ['gcc', '-std=gnu11', '-O0', '-o', '593cc6a1e936396e2dfe44e73f747935']| `result == 4` | ❌️| `593cc6a1e936396e2dfe44e73f747935` |
|GCC| ['gcc', '-std=c99', '-O1', '-o', '0522980fe4c84bc84ed7bfdfe4a054c4']| `result == 4` | ❌️| `0522980fe4c84bc84ed7bfdfe4a054c4` |
|GCC| ['gcc', '-std=c9x', '-O1', '-o', '2e353ca5cddbcb785710b1cf68ba5235']| `result == 4` | ❌️| `2e353ca5cddbcb785710b1cf68ba5235` |
|GCC| ['gcc', '-std=c89', '-O1', '-o', '9facf514ce56f87440f01c062a884801']| `result == 4` | ❌️| `9facf514ce56f87440f01c062a884801` |
|GCC| ['gcc', '-std=c90', '-O1', '-o', 'b819fb34bb00376e6191b929bad540d5']| `result == 4` | ❌️| `b819fb34bb00376e6191b929bad540d5` |
|GCC| ['gcc', '-std=c2x', '-O1', '-o', '668814c85e205159dc0d8ec39e14f8e5']| `result == 4` | ❌️| `668814c85e205159dc0d8ec39e14f8e5` |
|GCC| ['gcc', '-std=c17', '-O1', '-o', 'bc7b4762206ad5d80e56169273c9fea2']| `result == 4` | ❌️| `bc7b4762206ad5d80e56169273c9fea2` |
|GCC| ['gcc', '-std=c18', '-O1', '-o', '038b8074010aa1dce694cbfa33040c51']| `result == 4` | ❌️| `038b8074010aa1dce694cbfa33040c51` |
|GCC| ['gcc', '-std=c11', '-O1', '-o', '4a8bdb61c07d15278614327757e4c6a2']| `result == 4` | ❌️| `4a8bdb61c07d15278614327757e4c6a2` |
|GCC| ['gcc', '-std=c1x', '-O1', '-o', '2b2cd7dcc45f21100004d6649dd06377']| `result == 4` | ❌️| `2b2cd7dcc45f21100004d6649dd06377` |
|GCC| ['gcc', '-std=gnu11', '-O1', '-o', '1682b527a412623859e6bdaabd5eb376']| `result == 4` | ❌️| `1682b527a412623859e6bdaabd5eb376` |
|GCC| ['gcc', '-std=iso9899:1990', '-O1', '-o', 'ca6b750745b9a746bc096fa8bcd70242']| `result == 4` | ❌️| `ca6b750745b9a746bc096fa8bcd70242` |
|GCC| ['gcc', '-std=iso9899:199409', '-O1', '-o', 'f2d4dc96081ab16dacd3924bd4153d2e']| `result == 4` | ❌️| `f2d4dc96081ab16dacd3924bd4153d2e` |
|GCC| ['gcc', '-std=gnu89', '-O1', '-o', '28850b35a500565148a593c73258e346']| `result == 4` | ❌️| `28850b35a500565148a593c73258e346` |
|GCC| ['gcc', '-std=gnu90', '-O1', '-o', '9781d81615381039f6cad7fce06705b7']| `result == 4` | ❌️| `9781d81615381039f6cad7fce06705b7` |
|GCC| ['gcc', '-std=iso9899:1999', '-O1', '-o', '17aaae8e7c084f1ed5383f6afafea78c']| `result == 4` | ❌️| `17aaae8e7c084f1ed5383f6afafea78c` |
|GCC| ['gcc', '-std=gnu99', '-O1', '-o', '3238892999d9a6e2ee7dec9832065ab9']| `result == 4` | ❌️| `3238892999d9a6e2ee7dec9832065ab9` |
|GCC| ['gcc', '-std=iso9899:2011', '-O1', '-o', 'a5a437f03c20b0f1ad5a1347b56ba88f']| `result == 4` | ❌️| `a5a437f03c20b0f1ad5a1347b56ba88f` |
|GCC| ['gcc', '-std=iso9899:2017', '-O1', '-o', '770dc625931489b1dd1bf93b4ce80dff']| `result == 4` | ❌️| `770dc625931489b1dd1bf93b4ce80dff` |
|GCC| ['gcc', '-std=gnu11', '-O1', '-o', '1682b527a412623859e6bdaabd5eb376']| `result == 4` | ❌️| `1682b527a412623859e6bdaabd5eb376` |
|GCC| ['gcc', '-std=c99', '-O2', '-o', 'dea3347ed488c798fec9605466efa3fa']| `result == 4` | ❌️| `dea3347ed488c798fec9605466efa3fa` |
|GCC| ['gcc', '-std=c9x', '-O2', '-o', '67f35a887502c1e2f5e8c230071fa296']| `result == 4` | ❌️| `67f35a887502c1e2f5e8c230071fa296` |
|GCC| ['gcc', '-std=c89', '-O2', '-o', '1cc08ee1baf9aa299ac677c67947ce65']| `result == 4` | ❌️| `1cc08ee1baf9aa299ac677c67947ce65` |
|GCC| ['gcc', '-std=c90', '-O2', '-o', 'f68fdee1a046572d713ef0509bcab4a2']| `result == 4` | ❌️| `f68fdee1a046572d713ef0509bcab4a2` |
|GCC| ['gcc', '-std=c2x', '-O2', '-o', 'aa50956d47d39942282e616ff99b8ca1']| `result == 4` | ❌️| `aa50956d47d39942282e616ff99b8ca1` |
|GCC| ['gcc', '-std=c17', '-O2', '-o', '954a82a7ea3120a79305ad6161c68f0a']| `result == 4` | ❌️| `954a82a7ea3120a79305ad6161c68f0a` |
|GCC| ['gcc', '-std=c18', '-O2', '-o', '9dbe0d03f214c988d0ead9f7eb6a4148']| `result == 4` | ❌️| `9dbe0d03f214c988d0ead9f7eb6a4148` |
|GCC| ['gcc', '-std=c11', '-O2', '-o', 'ac1763de234179b1650de8c57136798b']| `result == 4` | ❌️| `ac1763de234179b1650de8c57136798b` |
|GCC| ['gcc', '-std=c1x', '-O2', '-o', 'f5971c4987a1d40b638eddcb2659fd2e']| `result == 4` | ❌️| `f5971c4987a1d40b638eddcb2659fd2e` |
|GCC| ['gcc', '-std=gnu11', '-O2', '-o', 'ab60203886833ea85d81a90fbb822148']| `result == 4` | ❌️| `ab60203886833ea85d81a90fbb822148` |
|GCC| ['gcc', '-std=iso9899:1990', '-O2', '-o', 'ca1d119164d783b2345f13c13c0f56be']| `result == 4` | ❌️| `ca1d119164d783b2345f13c13c0f56be` |
|GCC| ['gcc', '-std=iso9899:199409', '-O2', '-o', '8716bec93e87526b379b99f09f02305b']| `result == 4` | ❌️| `8716bec93e87526b379b99f09f02305b` |
|GCC| ['gcc', '-std=gnu89', '-O2', '-o', '7c2807f38a5932978595484bf4565943']| `result == 4` | ❌️| `7c2807f38a5932978595484bf4565943` |
|GCC| ['gcc', '-std=gnu90', '-O2', '-o', 'c224fa91692cfe283df222d1b03551dd']| `result == 4` | ❌️| `c224fa91692cfe283df222d1b03551dd` |
|GCC| ['gcc', '-std=iso9899:1999', '-O2', '-o', '71ae5a04c64b31ee4e8a42c5e7cd2d8b']| `result == 4` | ❌️| `71ae5a04c64b31ee4e8a42c5e7cd2d8b` |
|GCC| ['gcc', '-std=gnu99', '-O2', '-o', 'c799255fc682fbf82f057ed7846362bc']| `result == 4` | ❌️| `c799255fc682fbf82f057ed7846362bc` |
|GCC| ['gcc', '-std=iso9899:2011', '-O2', '-o', 'fa58c126202c95c0d6f40b31121b649b']| `result == 4` | ❌️| `fa58c126202c95c0d6f40b31121b649b` |
|GCC| ['gcc', '-std=iso9899:2017', '-O2', '-o', '01de5ac3b90a0758318285be249d8762']| `result == 4` | ❌️| `01de5ac3b90a0758318285be249d8762` |
|GCC| ['gcc', '-std=gnu11', '-O2', '-o', 'ab60203886833ea85d81a90fbb822148']| `result == 4` | ❌️| `ab60203886833ea85d81a90fbb822148` |
|GCC| ['gcc', '-std=c99', '-O3', '-o', '26324eaa0c61adfd5f2a19aeecdf3ab4']| `result == 4` | ❌️| `26324eaa0c61adfd5f2a19aeecdf3ab4` |
|GCC| ['gcc', '-std=c9x', '-O3', '-o', 'bb3bd9223cb5db9d4188496d83e80fcc']| `result == 4` | ❌️| `bb3bd9223cb5db9d4188496d83e80fcc` |
|GCC| ['gcc', '-std=c89', '-O3', '-o', '0ea699c7866c9ce1e13c845b87f9707e']| `result == 4` | ❌️| `0ea699c7866c9ce1e13c845b87f9707e` |
|GCC| ['gcc', '-std=c90', '-O3', '-o', '5e3341cdd75770a344a57fe9d3b7daf6']| `result == 4` | ❌️| `5e3341cdd75770a344a57fe9d3b7daf6` |
|GCC| ['gcc', '-std=c2x', '-O3', '-o', '57c9fb207322afc14608829bc9bee0ad']| `result == 4` | ❌️| `57c9fb207322afc14608829bc9bee0ad` |
|GCC| ['gcc', '-std=c17', '-O3', '-o', 'bf268c209183ccb8ca227a73f044171a']| `result == 4` | ❌️| `bf268c209183ccb8ca227a73f044171a` |
|GCC| ['gcc', '-std=c18', '-O3', '-o', '59cf4a8b39260727466b283bc64ffb02']| `result == 4` | ❌️| `59cf4a8b39260727466b283bc64ffb02` |
|GCC| ['gcc', '-std=c11', '-O3', '-o', '2fa69796d838548048d0ab8c5a9b2b72']| `result == 4` | ❌️| `2fa69796d838548048d0ab8c5a9b2b72` |
|GCC| ['gcc', '-std=c1x', '-O3', '-o', 'ffa3d671311bf0f0d7b6d9e4a7082875']| `result == 4` | ❌️| `ffa3d671311bf0f0d7b6d9e4a7082875` |
|GCC| ['gcc', '-std=gnu11', '-O3', '-o', 'a9dcbe762777e884bb8884f1d9dd6b7f']| `result == 4` | ❌️| `a9dcbe762777e884bb8884f1d9dd6b7f` |
|GCC| ['gcc', '-std=iso9899:1990', '-O3', '-o', 'dc0855c1eab3514070e7a92d1230a980']| `result == 4` | ❌️| `dc0855c1eab3514070e7a92d1230a980` |
|GCC| ['gcc', '-std=iso9899:199409', '-O3', '-o', 'ef1f10f6123162285beee69252f9053c']| `result == 4` | ❌️| `ef1f10f6123162285beee69252f9053c` |
|GCC| ['gcc', '-std=gnu89', '-O3', '-o', '2d987d7a115c8c5130dfc612750c17cd']| `result == 4` | ❌️| `2d987d7a115c8c5130dfc612750c17cd` |
|GCC| ['gcc', '-std=gnu90', '-O3', '-o', 'b816a3ec8f857b6ea8bddc12141e050e']| `result == 4` | ❌️| `b816a3ec8f857b6ea8bddc12141e050e` |
|GCC| ['gcc', '-std=iso9899:1999', '-O3', '-o', '514f8264053d134073d91b582ba1e0b8']| `result == 4` | ❌️| `514f8264053d134073d91b582ba1e0b8` |
|GCC| ['gcc', '-std=gnu99', '-O3', '-o', '0877207327cd458872d42d11d9b86573']| `result == 4` | ❌️| `0877207327cd458872d42d11d9b86573` |
|GCC| ['gcc', '-std=iso9899:2011', '-O3', '-o', '4c783600575d287d58bf330a35b69121']| `result == 4` | ❌️| `4c783600575d287d58bf330a35b69121` |
|GCC| ['gcc', '-std=iso9899:2017', '-O3', '-o', '816ec2e7e576582688d9f2475b32329a']| `result == 4` | ❌️| `816ec2e7e576582688d9f2475b32329a` |
|GCC| ['gcc', '-std=gnu11', '-O3', '-o', 'a9dcbe762777e884bb8884f1d9dd6b7f']| `result == 4` | ❌️| `a9dcbe762777e884bb8884f1d9dd6b7f` |
|Clang| ['clang', '-std=c99', '-O0', '-o', 'ba54afffa89b9bff05bdefe33aed634c']| `result == 4` | ✅️| `ba54afffa89b9bff05bdefe33aed634c` |
|Clang| ['clang', '-std=c9x', '-O0', '-o', '592abffe031da4921169787eeccfa63f']| `result == 4` | ✅️| `592abffe031da4921169787eeccfa63f` |
|Clang| ['clang', '-std=c89', '-O0', '-o', '11011998ad4703e11cc59f205f494e39']| `result == 4` | ✅️| `11011998ad4703e11cc59f205f494e39` |
|Clang| ['clang', '-std=c90', '-O0', '-o', '28811206fe79e3348d88327cf348924d']| `result == 4` | ✅️| `28811206fe79e3348d88327cf348924d` |
|Clang| ['clang', '-std=c2x', '-O0', '-o', 'be3b8eeb1968733db62ea66bc97d98c1']| `result == 4` | ✅️| `be3b8eeb1968733db62ea66bc97d98c1` |
|Clang| ['clang', '-std=c17', '-O0', '-o', 'b9d578ef4ce70ab8a86923c40dd8b3fa']| `result == 4` | ✅️| `b9d578ef4ce70ab8a86923c40dd8b3fa` |
|Clang| ['clang', '-std=c18', '-O0', '-o', '99ebc95ccc5464f97ee707c7e9b8f06b']| `result == 4` | ✅️| `99ebc95ccc5464f97ee707c7e9b8f06b` |
|Clang| ['clang', '-std=c11', '-O0', '-o', 'd875719582d7a9833d578c5383763cf0']| `result == 4` | ✅️| `d875719582d7a9833d578c5383763cf0` |
|Clang| ['clang', '-std=c1x', '-O0', '-o', '278f7e3d473d0209e4e9914ea59df3ce']| `result == 4` | ✅️| `278f7e3d473d0209e4e9914ea59df3ce` |
|Clang| ['clang', '-std=gnu11', '-O0', '-o', 'e7c93bb2652339db4386981a63e1ea21']| `result == 4` | ✅️| `e7c93bb2652339db4386981a63e1ea21` |
|Clang| ['clang', '-std=iso9899:1990', '-O0', '-o', '0404327b5decfeba24e626f82802d0cf']| `result == 4` | ✅️| `0404327b5decfeba24e626f82802d0cf` |
|Clang| ['clang', '-std=iso9899:199409', '-O0', '-o', '333f389b2f9f7a8e676ba0fb0bacee2c']| `result == 4` | ✅️| `333f389b2f9f7a8e676ba0fb0bacee2c` |
|Clang| ['clang', '-std=gnu89', '-O0', '-o', '3630fee57a26625777ee17f5dc6304b7']| `result == 4` | ✅️| `3630fee57a26625777ee17f5dc6304b7` |
|Clang| ['clang', '-std=gnu90', '-O0', '-o', '4dd5e9411a4d73868fd5bfeadaf8ba9d']| `result == 4` | ✅️| `4dd5e9411a4d73868fd5bfeadaf8ba9d` |
|Clang| ['clang', '-std=iso9899:1999', '-O0', '-o', 'a5cb43e4ac5b9809995a0f48decaba43']| `result == 4` | ✅️| `a5cb43e4ac5b9809995a0f48decaba43` |
|Clang| ['clang', '-std=gnu99', '-O0', '-o', '7a0735583f3658c41998f7e76ef6cdad']| `result == 4` | ✅️| `7a0735583f3658c41998f7e76ef6cdad` |
|Clang| ['clang', '-std=iso9899:2011', '-O0', '-o', '86395550dd7ce327e90f0c68b0e86706']| `result == 4` | ✅️| `86395550dd7ce327e90f0c68b0e86706` |
|Clang| ['clang', '-std=iso9899:2017', '-O0', '-o', '5cae3bc471ce41e280599d8f084a2383']| `result == 4` | ✅️| `5cae3bc471ce41e280599d8f084a2383` |
|Clang| ['clang', '-std=gnu11', '-O0', '-o', 'e7c93bb2652339db4386981a63e1ea21']| `result == 4` | ✅️| `e7c93bb2652339db4386981a63e1ea21` |
|Clang| ['clang', '-std=c99', '-O1', '-o', '064a8e3b0d975e2fc02d26ce32e7f6cc']| `result == 4` | ✅️| `064a8e3b0d975e2fc02d26ce32e7f6cc` |
|Clang| ['clang', '-std=c9x', '-O1', '-o', '5dc4721df2006c20491e6b1573c57b9c']| `result == 4` | ✅️| `5dc4721df2006c20491e6b1573c57b9c` |
|Clang| ['clang', '-std=c89', '-O1', '-o', '9c93d6f0e27921003f2e89e40dffd366']| `result == 4` | ✅️| `9c93d6f0e27921003f2e89e40dffd366` |
|Clang| ['clang', '-std=c90', '-O1', '-o', '3a217e5e0ac86b867f8707758487373e']| `result == 4` | ✅️| `3a217e5e0ac86b867f8707758487373e` |
|Clang| ['clang', '-std=c2x', '-O1', '-o', 'aecb00d6d6df38b95828e1ca4bc37dbc']| `result == 4` | ✅️| `aecb00d6d6df38b95828e1ca4bc37dbc` |
|Clang| ['clang', '-std=c17', '-O1', '-o', '5eef581a73fc4a6cb8df4d7f0b587018']| `result == 4` | ✅️| `5eef581a73fc4a6cb8df4d7f0b587018` |
|Clang| ['clang', '-std=c18', '-O1', '-o', '77763f9cfdb9123df4c9012f17caede1']| `result == 4` | ✅️| `77763f9cfdb9123df4c9012f17caede1` |
|Clang| ['clang', '-std=c11', '-O1', '-o', '53d198213e47a66aeab93651377bf56a']| `result == 4` | ✅️| `53d198213e47a66aeab93651377bf56a` |
|Clang| ['clang', '-std=c1x', '-O1', '-o', 'de0cf458d1c62aacb7799df83d7354a8']| `result == 4` | ✅️| `de0cf458d1c62aacb7799df83d7354a8` |
|Clang| ['clang', '-std=gnu11', '-O1', '-o', '531746939f0e74ee7208ce2bb4faf594']| `result == 4` | ✅️| `531746939f0e74ee7208ce2bb4faf594` |
|Clang| ['clang', '-std=iso9899:1990', '-O1', '-o', 'd1018923583abd6465a25634c4bd87f7']| `result == 4` | ✅️| `d1018923583abd6465a25634c4bd87f7` |
|Clang| ['clang', '-std=iso9899:199409', '-O1', '-o', '4b3031b48d6392ea4b1af2292d45bf18']| `result == 4` | ✅️| `4b3031b48d6392ea4b1af2292d45bf18` |
|Clang| ['clang', '-std=gnu89', '-O1', '-o', 'da640bed4470bad728f970156502287e']| `result == 4` | ✅️| `da640bed4470bad728f970156502287e` |
|Clang| ['clang', '-std=gnu90', '-O1', '-o', '433273c167f5de0b1106c5933c98bdaf']| `result == 4` | ✅️| `433273c167f5de0b1106c5933c98bdaf` |
|Clang| ['clang', '-std=iso9899:1999', '-O1', '-o', 'f964413b8b32c72b805d625ddb43c6a6']| `result == 4` | ✅️| `f964413b8b32c72b805d625ddb43c6a6` |
|Clang| ['clang', '-std=gnu99', '-O1', '-o', '9249e1a023eb868bf6f2ace955f4e1d1']| `result == 4` | ✅️| `9249e1a023eb868bf6f2ace955f4e1d1` |
|Clang| ['clang', '-std=iso9899:2011', '-O1', '-o', 'd6e0d721404f457e4d98b3cc60ec262a']| `result == 4` | ✅️| `d6e0d721404f457e4d98b3cc60ec262a` |
|Clang| ['clang', '-std=iso9899:2017', '-O1', '-o', 'dbad79b5fde34363ab7d33a7d5d75ca9']| `result == 4` | ✅️| `dbad79b5fde34363ab7d33a7d5d75ca9` |
|Clang| ['clang', '-std=gnu11', '-O1', '-o', '531746939f0e74ee7208ce2bb4faf594']| `result == 4` | ✅️| `531746939f0e74ee7208ce2bb4faf594` |
|Clang| ['clang', '-std=c99', '-O2', '-o', '58ebcdbe2d10d35cd4716317ffee8369']| `result == 4` | ✅️| `58ebcdbe2d10d35cd4716317ffee8369` |
|Clang| ['clang', '-std=c9x', '-O2', '-o', '36e539cca58daf6f1f11b992eef631ce']| `result == 4` | ✅️| `36e539cca58daf6f1f11b992eef631ce` |
|Clang| ['clang', '-std=c89', '-O2', '-o', '560346e8ae8919c1390b6f0ee4282f9b']| `result == 4` | ✅️| `560346e8ae8919c1390b6f0ee4282f9b` |
|Clang| ['clang', '-std=c90', '-O2', '-o', '4ad3ea7bf97f70c9069813ea1ef2ee5f']| `result == 4` | ✅️| `4ad3ea7bf97f70c9069813ea1ef2ee5f` |
|Clang| ['clang', '-std=c2x', '-O2', '-o', 'f9468dff63fdf54d3285ba481b9f633b']| `result == 4` | ✅️| `f9468dff63fdf54d3285ba481b9f633b` |
|Clang| ['clang', '-std=c17', '-O2', '-o', 'e9dd24eed06e61531ef5b1b8e20e4390']| `result == 4` | ✅️| `e9dd24eed06e61531ef5b1b8e20e4390` |
|Clang| ['clang', '-std=c18', '-O2', '-o', '334df205ebcf2b2c749404276f159e67']| `result == 4` | ✅️| `334df205ebcf2b2c749404276f159e67` |
|Clang| ['clang', '-std=c11', '-O2', '-o', '04db8dda69e4c9a9533d9755a3ef9dc1']| `result == 4` | ✅️| `04db8dda69e4c9a9533d9755a3ef9dc1` |
|Clang| ['clang', '-std=c1x', '-O2', '-o', '830d01dd218bb3a5673d51361a1d74dc']| `result == 4` | ✅️| `830d01dd218bb3a5673d51361a1d74dc` |
|Clang| ['clang', '-std=gnu11', '-O2', '-o', '3932283c2a189c1ee0f6472e8058d742']| `result == 4` | ✅️| `3932283c2a189c1ee0f6472e8058d742` |
|Clang| ['clang', '-std=iso9899:1990', '-O2', '-o', 'e705f79e841919378e44802b5443eb40']| `result == 4` | ✅️| `e705f79e841919378e44802b5443eb40` |
|Clang| ['clang', '-std=iso9899:199409', '-O2', '-o', '684536d774088c5252e09348bc0bb790']| `result == 4` | ✅️| `684536d774088c5252e09348bc0bb790` |
|Clang| ['clang', '-std=gnu89', '-O2', '-o', '4e2e0fae6b04434a376698e858e87861']| `result == 4` | ✅️| `4e2e0fae6b04434a376698e858e87861` |
|Clang| ['clang', '-std=gnu90', '-O2', '-o', 'c2155b243784ce512f18a9954db79b69']| `result == 4` | ✅️| `c2155b243784ce512f18a9954db79b69` |
|Clang| ['clang', '-std=iso9899:1999', '-O2', '-o', '42afabc37a33a9d836da5e90dde3dfb0']| `result == 4` | ✅️| `42afabc37a33a9d836da5e90dde3dfb0` |
|Clang| ['clang', '-std=gnu99', '-O2', '-o', '2d48cebd1642f196b95383aea4696ff8']| `result == 4` | ✅️| `2d48cebd1642f196b95383aea4696ff8` |
|Clang| ['clang', '-std=iso9899:2011', '-O2', '-o', 'e51afd1f725dc10524d888c8c95f8f1b']| `result == 4` | ✅️| `e51afd1f725dc10524d888c8c95f8f1b` |
|Clang| ['clang', '-std=iso9899:2017', '-O2', '-o', '12145536b37b08adc74e7b06f93add14']| `result == 4` | ✅️| `12145536b37b08adc74e7b06f93add14` |
|Clang| ['clang', '-std=gnu11', '-O2', '-o', '3932283c2a189c1ee0f6472e8058d742']| `result == 4` | ✅️| `3932283c2a189c1ee0f6472e8058d742` |
|Clang| ['clang', '-std=c99', '-O3', '-o', '3bfc8381ee1a7a8544b62d3b3b769897']| `result == 4` | ✅️| `3bfc8381ee1a7a8544b62d3b3b769897` |
|Clang| ['clang', '-std=c9x', '-O3', '-o', '8f1350649e8744e9b6e8343fe5cf78f8']| `result == 4` | ✅️| `8f1350649e8744e9b6e8343fe5cf78f8` |
|Clang| ['clang', '-std=c89', '-O3', '-o', '825c9625761a36ed7165121eeb923495']| `result == 4` | ✅️| `825c9625761a36ed7165121eeb923495` |
|Clang| ['clang', '-std=c90', '-O3', '-o', 'df833cac67a0727bc312a2cc1313a914']| `result == 4` | ✅️| `df833cac67a0727bc312a2cc1313a914` |
|Clang| ['clang', '-std=c2x', '-O3', '-o', '852de66e1c52965781f6fce87165ffc3']| `result == 4` | ✅️| `852de66e1c52965781f6fce87165ffc3` |
|Clang| ['clang', '-std=c17', '-O3', '-o', 'bb5ad62119bd5e7dba8f24337e42176e']| `result == 4` | ✅️| `bb5ad62119bd5e7dba8f24337e42176e` |
|Clang| ['clang', '-std=c18', '-O3', '-o', 'a226ce2338b4c6e21e93069c47b363f5']| `result == 4` | ✅️| `a226ce2338b4c6e21e93069c47b363f5` |
|Clang| ['clang', '-std=c11', '-O3', '-o', 'e19343a077e1309bce5baa6550deeb74']| `result == 4` | ✅️| `e19343a077e1309bce5baa6550deeb74` |
|Clang| ['clang', '-std=c1x', '-O3', '-o', 'f6992c3e95a3893ecfa44e5c52b94fec']| `result == 4` | ✅️| `f6992c3e95a3893ecfa44e5c52b94fec` |
|Clang| ['clang', '-std=gnu11', '-O3', '-o', 'b95dc6d2f16ed8c5e79e76b6afdea7d9']| `result == 4` | ✅️| `b95dc6d2f16ed8c5e79e76b6afdea7d9` |
|Clang| ['clang', '-std=iso9899:1990', '-O3', '-o', '2e1ee2005de2cbe880f5b4b56e9f9e6e']| `result == 4` | ✅️| `2e1ee2005de2cbe880f5b4b56e9f9e6e` |
|Clang| ['clang', '-std=iso9899:199409', '-O3', '-o', '1399edbcad9c688ac3f6b92c23498c35']| `result == 4` | ✅️| `1399edbcad9c688ac3f6b92c23498c35` |
|Clang| ['clang', '-std=gnu89', '-O3', '-o', '069342034bfd7b999d28a6868c679492']| `result == 4` | ✅️| `069342034bfd7b999d28a6868c679492` |
|Clang| ['clang', '-std=gnu90', '-O3', '-o', 'f1a8a0624c6267d6a4c0ff453fd73fb5']| `result == 4` | ✅️| `f1a8a0624c6267d6a4c0ff453fd73fb5` |
|Clang| ['clang', '-std=iso9899:1999', '-O3', '-o', '247f6a795c9f7bb47e7ea616945e6d44']| `result == 4` | ✅️| `247f6a795c9f7bb47e7ea616945e6d44` |
|Clang| ['clang', '-std=gnu99', '-O3', '-o', '7e2ee3a51800a1cddf56579256fee5a4']| `result == 4` | ✅️| `7e2ee3a51800a1cddf56579256fee5a4` |
|Clang| ['clang', '-std=iso9899:2011', '-O3', '-o', '53da8eceecd848baccf4198054caf71c']| `result == 4` | ✅️| `53da8eceecd848baccf4198054caf71c` |
|Clang| ['clang', '-std=iso9899:2017', '-O3', '-o', 'a8d35ae2b69f4ca7636675b84c6600f8']| `result == 4` | ✅️| `a8d35ae2b69f4ca7636675b84c6600f8` |
|Clang| ['clang', '-std=gnu11', '-O3', '-o', 'b95dc6d2f16ed8c5e79e76b6afdea7d9']| `result == 4` | ✅️| `b95dc6d2f16ed8c5e79e76b6afdea7d9` |
|TCC| ['tcc', '-std=c99', '-O0', '-o', 'd17044d52513692f117c6ec8fc3fc39a']| `result == 4` | ✅️| `d17044d52513692f117c6ec8fc3fc39a` |
|TCC| ['tcc', '-std=c9x', '-O0', '-o', 'da3232c20af62eafd67808c253921a06']| `result == 4` | ✅️| `da3232c20af62eafd67808c253921a06` |
|TCC| ['tcc', '-std=c89', '-O0', '-o', '2915bc2fd8549ed3a5d85bb4b7ae547c']| `result == 4` | ✅️| `2915bc2fd8549ed3a5d85bb4b7ae547c` |
|TCC| ['tcc', '-std=c90', '-O0', '-o', '0b8777920dc05dd4ad134f2946b34a19']| `result == 4` | ✅️| `0b8777920dc05dd4ad134f2946b34a19` |
|TCC| ['tcc', '-std=c2x', '-O0', '-o', '253782851cc0cb07b3fdc2ce86723fb4']| `result == 4` | ✅️| `253782851cc0cb07b3fdc2ce86723fb4` |
|TCC| ['tcc', '-std=c17', '-O0', '-o', '4c14cb1a8ec6eb627f1050576a9fa9db']| `result == 4` | ✅️| `4c14cb1a8ec6eb627f1050576a9fa9db` |
|TCC| ['tcc', '-std=c18', '-O0', '-o', 'a5116b53af48078faa05585fd62f54ab']| `result == 4` | ✅️| `a5116b53af48078faa05585fd62f54ab` |
|TCC| ['tcc', '-std=c11', '-O0', '-o', '293117ea48d99ab81e90d1cdfaf916c1']| `result == 4` | ✅️| `293117ea48d99ab81e90d1cdfaf916c1` |
|TCC| ['tcc', '-std=c1x', '-O0', '-o', '0d4eb74fc42ac980cf2ca68f1c90a4a4']| `result == 4` | ✅️| `0d4eb74fc42ac980cf2ca68f1c90a4a4` |
|TCC| ['tcc', '-std=gnu11', '-O0', '-o', 'ad111efe8b7814900991beee52c9a796']| `result == 4` | ✅️| `ad111efe8b7814900991beee52c9a796` |
|TCC| ['tcc', '-std=iso9899:1990', '-O0', '-o', 'ac1a15953f51f54e688f3b4727f41a51']| `result == 4` | ✅️| `ac1a15953f51f54e688f3b4727f41a51` |
|TCC| ['tcc', '-std=iso9899:199409', '-O0', '-o', '42840afcf2ae5fd51d2ecc9299e9571f']| `result == 4` | ✅️| `42840afcf2ae5fd51d2ecc9299e9571f` |
|TCC| ['tcc', '-std=gnu89', '-O0', '-o', 'be70667ceb5b42751a0d8c21fa097e4e']| `result == 4` | ✅️| `be70667ceb5b42751a0d8c21fa097e4e` |
|TCC| ['tcc', '-std=gnu90', '-O0', '-o', 'c67b96c357ee414e002924c76daae55a']| `result == 4` | ✅️| `c67b96c357ee414e002924c76daae55a` |
|TCC| ['tcc', '-std=iso9899:1999', '-O0', '-o', '994d2d68eedcb516c20ab63fda3114a8']| `result == 4` | ✅️| `994d2d68eedcb516c20ab63fda3114a8` |
|TCC| ['tcc', '-std=gnu99', '-O0', '-o', '08a7db84264a0cb589719c78ccba8ea2']| `result == 4` | ✅️| `08a7db84264a0cb589719c78ccba8ea2` |
|TCC| ['tcc', '-std=iso9899:2011', '-O0', '-o', '8fc4b3893c923cc70dc3821cd50318ad']| `result == 4` | ✅️| `8fc4b3893c923cc70dc3821cd50318ad` |
|TCC| ['tcc', '-std=iso9899:2017', '-O0', '-o', 'ec8059d3853a19cf20a44d61884ce722']| `result == 4` | ✅️| `ec8059d3853a19cf20a44d61884ce722` |
|TCC| ['tcc', '-std=gnu11', '-O0', '-o', 'ad111efe8b7814900991beee52c9a796']| `result == 4` | ✅️| `ad111efe8b7814900991beee52c9a796` |
|TCC| ['tcc', '-std=c99', '-O1', '-o', 'fe23a12f55a6f0b3b2d536658358ce49']| `result == 4` | ✅️| `fe23a12f55a6f0b3b2d536658358ce49` |
|TCC| ['tcc', '-std=c9x', '-O1', '-o', '1e8e803b29da0468df2253c105a165fc']| `result == 4` | ✅️| `1e8e803b29da0468df2253c105a165fc` |
|TCC| ['tcc', '-std=c89', '-O1', '-o', '007599465b0347736edf728cc46874e0']| `result == 4` | ✅️| `007599465b0347736edf728cc46874e0` |
|TCC| ['tcc', '-std=c90', '-O1', '-o', '26e38dbc9c4ea02be13be3ae6c89d097']| `result == 4` | ✅️| `26e38dbc9c4ea02be13be3ae6c89d097` |
|TCC| ['tcc', '-std=c2x', '-O1', '-o', 'beab8dbd6e7e393d3bc5321a89aba574']| `result == 4` | ✅️| `beab8dbd6e7e393d3bc5321a89aba574` |
|TCC| ['tcc', '-std=c17', '-O1', '-o', 'fc8a86cca3e314aa6d4c81a3d147db11']| `result == 4` | ✅️| `fc8a86cca3e314aa6d4c81a3d147db11` |
|TCC| ['tcc', '-std=c18', '-O1', '-o', '5e6843f4549546ac8cec6e2133912557']| `result == 4` | ✅️| `5e6843f4549546ac8cec6e2133912557` |
|TCC| ['tcc', '-std=c11', '-O1', '-o', 'a7e0392930c04f89ef52c546880858ff']| `result == 4` | ✅️| `a7e0392930c04f89ef52c546880858ff` |
|TCC| ['tcc', '-std=c1x', '-O1', '-o', '1001607136ba67cff44043b1360f70f2']| `result == 4` | ✅️| `1001607136ba67cff44043b1360f70f2` |
|TCC| ['tcc', '-std=gnu11', '-O1', '-o', '633969c7f4175d55d5999fa9140da7c4']| `result == 4` | ✅️| `633969c7f4175d55d5999fa9140da7c4` |
|TCC| ['tcc', '-std=iso9899:1990', '-O1', '-o', '0a669fe8eaccb652a2939a807ca42c35']| `result == 4` | ✅️| `0a669fe8eaccb652a2939a807ca42c35` |
|TCC| ['tcc', '-std=iso9899:199409', '-O1', '-o', '9820f95c8aa0aab233fb3173f76fac66']| `result == 4` | ✅️| `9820f95c8aa0aab233fb3173f76fac66` |
|TCC| ['tcc', '-std=gnu89', '-O1', '-o', 'dc7d3ebc02801b2211721a3654389358']| `result == 4` | ✅️| `dc7d3ebc02801b2211721a3654389358` |
|TCC| ['tcc', '-std=gnu90', '-O1', '-o', '995bfc5013e2e87453ac1cb560be79f0']| `result == 4` | ✅️| `995bfc5013e2e87453ac1cb560be79f0` |
|TCC| ['tcc', '-std=iso9899:1999', '-O1', '-o', '1273d26792011b80feaea70148b6c851']| `result == 4` | ✅️| `1273d26792011b80feaea70148b6c851` |
|TCC| ['tcc', '-std=gnu99', '-O1', '-o', '89b6460113a5540fa17f923630d1aaf3']| `result == 4` | ✅️| `89b6460113a5540fa17f923630d1aaf3` |
|TCC| ['tcc', '-std=iso9899:2011', '-O1', '-o', '75dd26dbee8147189fa7396a1c54aa41']| `result == 4` | ✅️| `75dd26dbee8147189fa7396a1c54aa41` |
|TCC| ['tcc', '-std=iso9899:2017', '-O1', '-o', '5fc8fb820d52b28b0b6047890dc8ee7c']| `result == 4` | ✅️| `5fc8fb820d52b28b0b6047890dc8ee7c` |
|TCC| ['tcc', '-std=gnu11', '-O1', '-o', '633969c7f4175d55d5999fa9140da7c4']| `result == 4` | ✅️| `633969c7f4175d55d5999fa9140da7c4` |
|TCC| ['tcc', '-std=c99', '-O2', '-o', '8a60e9ffc21d2d1f3a9dc38caa79caaf']| `result == 4` | ✅️| `8a60e9ffc21d2d1f3a9dc38caa79caaf` |
|TCC| ['tcc', '-std=c9x', '-O2', '-o', 'd55a8a520a98859d4772eb7d7d2574ae']| `result == 4` | ✅️| `d55a8a520a98859d4772eb7d7d2574ae` |
|TCC| ['tcc', '-std=c89', '-O2', '-o', '716ab53b08c78e9d7543fa5a45ca1501']| `result == 4` | ✅️| `716ab53b08c78e9d7543fa5a45ca1501` |
|TCC| ['tcc', '-std=c90', '-O2', '-o', '0b197e3613c6c69f612320de5d5bf821']| `result == 4` | ✅️| `0b197e3613c6c69f612320de5d5bf821` |
|TCC| ['tcc', '-std=c2x', '-O2', '-o', '35af2900ec5fe7a10627144018a3471c']| `result == 4` | ✅️| `35af2900ec5fe7a10627144018a3471c` |
|TCC| ['tcc', '-std=c17', '-O2', '-o', '8e6671031fa4750c7b606d06ed4d3582']| `result == 4` | ✅️| `8e6671031fa4750c7b606d06ed4d3582` |
|TCC| ['tcc', '-std=c18', '-O2', '-o', 'd1127d5dee9a769b7da76db1cd95835a']| `result == 4` | ✅️| `d1127d5dee9a769b7da76db1cd95835a` |
|TCC| ['tcc', '-std=c11', '-O2', '-o', 'a1ff60d4d2a34e98b46c43c64543e852']| `result == 4` | ✅️| `a1ff60d4d2a34e98b46c43c64543e852` |
|TCC| ['tcc', '-std=c1x', '-O2', '-o', '5ee7e564d96389e2b729ce212177e635']| `result == 4` | ✅️| `5ee7e564d96389e2b729ce212177e635` |
|TCC| ['tcc', '-std=gnu11', '-O2', '-o', '57f3e45c86361e52c4ec94cd0a630156']| `result == 4` | ✅️| `57f3e45c86361e52c4ec94cd0a630156` |
|TCC| ['tcc', '-std=iso9899:1990', '-O2', '-o', '12bbd8d1e0acdd52e1dca5f2a299a904']| `result == 4` | ✅️| `12bbd8d1e0acdd52e1dca5f2a299a904` |
|TCC| ['tcc', '-std=iso9899:199409', '-O2', '-o', 'a1a920b47b2d270f89985320623818cf']| `result == 4` | ✅️| `a1a920b47b2d270f89985320623818cf` |
|TCC| ['tcc', '-std=gnu89', '-O2', '-o', 'af44188a32fb988a2b67dad2261620b2']| `result == 4` | ✅️| `af44188a32fb988a2b67dad2261620b2` |
|TCC| ['tcc', '-std=gnu90', '-O2', '-o', 'f5c921cb6b53eed5001d2b7697a43e86']| `result == 4` | ✅️| `f5c921cb6b53eed5001d2b7697a43e86` |
|TCC| ['tcc', '-std=iso9899:1999', '-O2', '-o', '597cc90143b4e0badc04779a3aba56eb']| `result == 4` | ✅️| `597cc90143b4e0badc04779a3aba56eb` |
|TCC| ['tcc', '-std=gnu99', '-O2', '-o', 'cad534e056741944fa3bf48b5d4a6df4']| `result == 4` | ✅️| `cad534e056741944fa3bf48b5d4a6df4` |
|TCC| ['tcc', '-std=iso9899:2011', '-O2', '-o', '14aa7522b9c68180d8df38abc864b353']| `result == 4` | ✅️| `14aa7522b9c68180d8df38abc864b353` |
|TCC| ['tcc', '-std=iso9899:2017', '-O2', '-o', '24c1c1fea2b3101e2ccb7b9c5fcb40c2']| `result == 4` | ✅️| `24c1c1fea2b3101e2ccb7b9c5fcb40c2` |
|TCC| ['tcc', '-std=gnu11', '-O2', '-o', '57f3e45c86361e52c4ec94cd0a630156']| `result == 4` | ✅️| `57f3e45c86361e52c4ec94cd0a630156` |
|TCC| ['tcc', '-std=c99', '-O3', '-o', '0a9bd561e3d06ab897383f31d5d00f61']| `result == 4` | ✅️| `0a9bd561e3d06ab897383f31d5d00f61` |
|TCC| ['tcc', '-std=c9x', '-O3', '-o', '7f91b733116f63b2489d5b71a013e3ce']| `result == 4` | ✅️| `7f91b733116f63b2489d5b71a013e3ce` |
|TCC| ['tcc', '-std=c89', '-O3', '-o', 'a8497a087315e821b6f0b0e2e9c46ba3']| `result == 4` | ✅️| `a8497a087315e821b6f0b0e2e9c46ba3` |
|TCC| ['tcc', '-std=c90', '-O3', '-o', '02649098c4e85b92c108d6655953e687']| `result == 4` | ✅️| `02649098c4e85b92c108d6655953e687` |
|TCC| ['tcc', '-std=c2x', '-O3', '-o', '494826f02c5fd584d1a926f76e8cc649']| `result == 4` | ✅️| `494826f02c5fd584d1a926f76e8cc649` |
|TCC| ['tcc', '-std=c17', '-O3', '-o', '899b09a12cda14b8b260fcb23d59f29f']| `result == 4` | ✅️| `899b09a12cda14b8b260fcb23d59f29f` |
|TCC| ['tcc', '-std=c18', '-O3', '-o', '4331e5eabfd9a08250b871b9c5dc99f5']| `result == 4` | ✅️| `4331e5eabfd9a08250b871b9c5dc99f5` |
|TCC| ['tcc', '-std=c11', '-O3', '-o', '5e594876cb2edc420d8c9db8d5c1f18e']| `result == 4` | ✅️| `5e594876cb2edc420d8c9db8d5c1f18e` |
|TCC| ['tcc', '-std=c1x', '-O3', '-o', '12b3fad14342c1be49ab30a3e5c09ba5']| `result == 4` | ✅️| `12b3fad14342c1be49ab30a3e5c09ba5` |
|TCC| ['tcc', '-std=gnu11', '-O3', '-o', '23534b6f94531ef4c391b5d9fe172686']| `result == 4` | ✅️| `23534b6f94531ef4c391b5d9fe172686` |
|TCC| ['tcc', '-std=iso9899:1990', '-O3', '-o', 'd3d3aef36558475dcdf0769339aeec3b']| `result == 4` | ✅️| `d3d3aef36558475dcdf0769339aeec3b` |
|TCC| ['tcc', '-std=iso9899:199409', '-O3', '-o', 'dec49598644f2ae450eed30bad13c559']| `result == 4` | ✅️| `dec49598644f2ae450eed30bad13c559` |
|TCC| ['tcc', '-std=gnu89', '-O3', '-o', 'eadd44c83053a7cc608e6c41a12f5328']| `result == 4` | ✅️| `eadd44c83053a7cc608e6c41a12f5328` |
|TCC| ['tcc', '-std=gnu90', '-O3', '-o', 'f6e3b2fda06fceb90687a81eef9fc7c2']| `result == 4` | ✅️| `f6e3b2fda06fceb90687a81eef9fc7c2` |
|TCC| ['tcc', '-std=iso9899:1999', '-O3', '-o', 'd738af4c26d1443633d06232d26679a9']| `result == 4` | ✅️| `d738af4c26d1443633d06232d26679a9` |
|TCC| ['tcc', '-std=gnu99', '-O3', '-o', 'dd8adea178c32e39b46dc69567db3e17']| `result == 4` | ✅️| `dd8adea178c32e39b46dc69567db3e17` |
|TCC| ['tcc', '-std=iso9899:2011', '-O3', '-o', 'f77009e35ee9ecee8aaea4f6ecabcdf7']| `result == 4` | ✅️| `f77009e35ee9ecee8aaea4f6ecabcdf7` |
|TCC| ['tcc', '-std=iso9899:2017', '-O3', '-o', 'e8687773c1bd3dc67fa0486f55b61bb0']| `result == 4` | ✅️| `e8687773c1bd3dc67fa0486f55b61bb0` |
|TCC| ['tcc', '-std=gnu11', '-O3', '-o', '23534b6f94531ef4c391b5d9fe172686']| `result == 4` | ✅️| `23534b6f94531ef4c391b5d9fe172686` |

### Program 2 (`result == 5`)

Running program: `functions/f1/program_analysis2.c`

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

	assert(result == 5);

	return 0;
}
```


Expected assertion: `result == 5`

| Verifier | Arguments | Assertion expected | Assertion result | Job hash |
| --- | --- | --- | --- | --- |
|CBMC| ['cbmc']| `result == 5` | ❌️| `5ba82bd6b31c6f683a6898e4c1e89fb7` |
|ESBMC| ['esbmc']| `result == 5` | ❌️| `bacc94d48de173aa1de67a4d03c2e799` |
|GCC| ['gcc', '-std=c99', '-O0', '-o', '3a9ba9292af33bad43be0df6c8ea389e']| `result == 5` | ✅️| `3a9ba9292af33bad43be0df6c8ea389e` |
|GCC| ['gcc', '-std=c9x', '-O0', '-o', '20d5fe108c7fb6a8407524452312f31b']| `result == 5` | ✅️| `20d5fe108c7fb6a8407524452312f31b` |
|GCC| ['gcc', '-std=c89', '-O0', '-o', 'a6b4de34f8a8d3a2b1988c3843215f32']| `result == 5` | ✅️| `a6b4de34f8a8d3a2b1988c3843215f32` |
|GCC| ['gcc', '-std=c90', '-O0', '-o', '0107ff703d7eef96ab3789af0c9603e6']| `result == 5` | ✅️| `0107ff703d7eef96ab3789af0c9603e6` |
|GCC| ['gcc', '-std=c2x', '-O0', '-o', 'd6dba0f0e2a520ff209e0a1de3f40d1c']| `result == 5` | ✅️| `d6dba0f0e2a520ff209e0a1de3f40d1c` |
|GCC| ['gcc', '-std=c17', '-O0', '-o', '0944285764068a76ecc6fbd53f44a1ec']| `result == 5` | ✅️| `0944285764068a76ecc6fbd53f44a1ec` |
|GCC| ['gcc', '-std=c18', '-O0', '-o', '923e7d926d54da3ec9b3ab52af0dc2e8']| `result == 5` | ✅️| `923e7d926d54da3ec9b3ab52af0dc2e8` |
|GCC| ['gcc', '-std=c11', '-O0', '-o', '9383a9ced9f13f45a746db8f312ec53c']| `result == 5` | ✅️| `9383a9ced9f13f45a746db8f312ec53c` |
|GCC| ['gcc', '-std=c1x', '-O0', '-o', '43083cad79238ec5e7d2f038c4ca7b05']| `result == 5` | ✅️| `43083cad79238ec5e7d2f038c4ca7b05` |
|GCC| ['gcc', '-std=gnu11', '-O0', '-o', '593cc6a1e936396e2dfe44e73f747935']| `result == 5` | ✅️| `593cc6a1e936396e2dfe44e73f747935` |
|GCC| ['gcc', '-std=iso9899:1990', '-O0', '-o', '35b541612d58aeb32c3c35f3b7c0d32e']| `result == 5` | ✅️| `35b541612d58aeb32c3c35f3b7c0d32e` |
|GCC| ['gcc', '-std=iso9899:199409', '-O0', '-o', 'cb502324e6ca21edf23197a033b199f9']| `result == 5` | ✅️| `cb502324e6ca21edf23197a033b199f9` |
|GCC| ['gcc', '-std=gnu89', '-O0', '-o', '1388a6d1ee3a606e51909d7670650200']| `result == 5` | ✅️| `1388a6d1ee3a606e51909d7670650200` |
|GCC| ['gcc', '-std=gnu90', '-O0', '-o', '20699d4f48d67f8bbd2c088358dda93c']| `result == 5` | ✅️| `20699d4f48d67f8bbd2c088358dda93c` |
|GCC| ['gcc', '-std=iso9899:1999', '-O0', '-o', '7086bf496ee0bd00b38d1deb1457a6ed']| `result == 5` | ✅️| `7086bf496ee0bd00b38d1deb1457a6ed` |
|GCC| ['gcc', '-std=gnu99', '-O0', '-o', 'a49d775b9c92e880177902185cd9c66f']| `result == 5` | ✅️| `a49d775b9c92e880177902185cd9c66f` |
|GCC| ['gcc', '-std=iso9899:2011', '-O0', '-o', 'cfcdef135180100186eebe3c34c4d2fb']| `result == 5` | ✅️| `cfcdef135180100186eebe3c34c4d2fb` |
|GCC| ['gcc', '-std=iso9899:2017', '-O0', '-o', 'dae39efc35cfac623901798dd4a7e73c']| `result == 5` | ✅️| `dae39efc35cfac623901798dd4a7e73c` |
|GCC| ['gcc', '-std=gnu11', '-O0', '-o', '593cc6a1e936396e2dfe44e73f747935']| `result == 5` | ✅️| `593cc6a1e936396e2dfe44e73f747935` |
|GCC| ['gcc', '-std=c99', '-O1', '-o', '0522980fe4c84bc84ed7bfdfe4a054c4']| `result == 5` | ✅️| `0522980fe4c84bc84ed7bfdfe4a054c4` |
|GCC| ['gcc', '-std=c9x', '-O1', '-o', '2e353ca5cddbcb785710b1cf68ba5235']| `result == 5` | ✅️| `2e353ca5cddbcb785710b1cf68ba5235` |
|GCC| ['gcc', '-std=c89', '-O1', '-o', '9facf514ce56f87440f01c062a884801']| `result == 5` | ✅️| `9facf514ce56f87440f01c062a884801` |
|GCC| ['gcc', '-std=c90', '-O1', '-o', 'b819fb34bb00376e6191b929bad540d5']| `result == 5` | ✅️| `b819fb34bb00376e6191b929bad540d5` |
|GCC| ['gcc', '-std=c2x', '-O1', '-o', '668814c85e205159dc0d8ec39e14f8e5']| `result == 5` | ✅️| `668814c85e205159dc0d8ec39e14f8e5` |
|GCC| ['gcc', '-std=c17', '-O1', '-o', 'bc7b4762206ad5d80e56169273c9fea2']| `result == 5` | ✅️| `bc7b4762206ad5d80e56169273c9fea2` |
|GCC| ['gcc', '-std=c18', '-O1', '-o', '038b8074010aa1dce694cbfa33040c51']| `result == 5` | ✅️| `038b8074010aa1dce694cbfa33040c51` |
|GCC| ['gcc', '-std=c11', '-O1', '-o', '4a8bdb61c07d15278614327757e4c6a2']| `result == 5` | ✅️| `4a8bdb61c07d15278614327757e4c6a2` |
|GCC| ['gcc', '-std=c1x', '-O1', '-o', '2b2cd7dcc45f21100004d6649dd06377']| `result == 5` | ✅️| `2b2cd7dcc45f21100004d6649dd06377` |
|GCC| ['gcc', '-std=gnu11', '-O1', '-o', '1682b527a412623859e6bdaabd5eb376']| `result == 5` | ✅️| `1682b527a412623859e6bdaabd5eb376` |
|GCC| ['gcc', '-std=iso9899:1990', '-O1', '-o', 'ca6b750745b9a746bc096fa8bcd70242']| `result == 5` | ✅️| `ca6b750745b9a746bc096fa8bcd70242` |
|GCC| ['gcc', '-std=iso9899:199409', '-O1', '-o', 'f2d4dc96081ab16dacd3924bd4153d2e']| `result == 5` | ✅️| `f2d4dc96081ab16dacd3924bd4153d2e` |
|GCC| ['gcc', '-std=gnu89', '-O1', '-o', '28850b35a500565148a593c73258e346']| `result == 5` | ✅️| `28850b35a500565148a593c73258e346` |
|GCC| ['gcc', '-std=gnu90', '-O1', '-o', '9781d81615381039f6cad7fce06705b7']| `result == 5` | ✅️| `9781d81615381039f6cad7fce06705b7` |
|GCC| ['gcc', '-std=iso9899:1999', '-O1', '-o', '17aaae8e7c084f1ed5383f6afafea78c']| `result == 5` | ✅️| `17aaae8e7c084f1ed5383f6afafea78c` |
|GCC| ['gcc', '-std=gnu99', '-O1', '-o', '3238892999d9a6e2ee7dec9832065ab9']| `result == 5` | ✅️| `3238892999d9a6e2ee7dec9832065ab9` |
|GCC| ['gcc', '-std=iso9899:2011', '-O1', '-o', 'a5a437f03c20b0f1ad5a1347b56ba88f']| `result == 5` | ✅️| `a5a437f03c20b0f1ad5a1347b56ba88f` |
|GCC| ['gcc', '-std=iso9899:2017', '-O1', '-o', '770dc625931489b1dd1bf93b4ce80dff']| `result == 5` | ✅️| `770dc625931489b1dd1bf93b4ce80dff` |
|GCC| ['gcc', '-std=gnu11', '-O1', '-o', '1682b527a412623859e6bdaabd5eb376']| `result == 5` | ✅️| `1682b527a412623859e6bdaabd5eb376` |
|GCC| ['gcc', '-std=c99', '-O2', '-o', 'dea3347ed488c798fec9605466efa3fa']| `result == 5` | ✅️| `dea3347ed488c798fec9605466efa3fa` |
|GCC| ['gcc', '-std=c9x', '-O2', '-o', '67f35a887502c1e2f5e8c230071fa296']| `result == 5` | ✅️| `67f35a887502c1e2f5e8c230071fa296` |
|GCC| ['gcc', '-std=c89', '-O2', '-o', '1cc08ee1baf9aa299ac677c67947ce65']| `result == 5` | ✅️| `1cc08ee1baf9aa299ac677c67947ce65` |
|GCC| ['gcc', '-std=c90', '-O2', '-o', 'f68fdee1a046572d713ef0509bcab4a2']| `result == 5` | ✅️| `f68fdee1a046572d713ef0509bcab4a2` |
|GCC| ['gcc', '-std=c2x', '-O2', '-o', 'aa50956d47d39942282e616ff99b8ca1']| `result == 5` | ✅️| `aa50956d47d39942282e616ff99b8ca1` |
|GCC| ['gcc', '-std=c17', '-O2', '-o', '954a82a7ea3120a79305ad6161c68f0a']| `result == 5` | ✅️| `954a82a7ea3120a79305ad6161c68f0a` |
|GCC| ['gcc', '-std=c18', '-O2', '-o', '9dbe0d03f214c988d0ead9f7eb6a4148']| `result == 5` | ✅️| `9dbe0d03f214c988d0ead9f7eb6a4148` |
|GCC| ['gcc', '-std=c11', '-O2', '-o', 'ac1763de234179b1650de8c57136798b']| `result == 5` | ✅️| `ac1763de234179b1650de8c57136798b` |
|GCC| ['gcc', '-std=c1x', '-O2', '-o', 'f5971c4987a1d40b638eddcb2659fd2e']| `result == 5` | ✅️| `f5971c4987a1d40b638eddcb2659fd2e` |
|GCC| ['gcc', '-std=gnu11', '-O2', '-o', 'ab60203886833ea85d81a90fbb822148']| `result == 5` | ✅️| `ab60203886833ea85d81a90fbb822148` |
|GCC| ['gcc', '-std=iso9899:1990', '-O2', '-o', 'ca1d119164d783b2345f13c13c0f56be']| `result == 5` | ✅️| `ca1d119164d783b2345f13c13c0f56be` |
|GCC| ['gcc', '-std=iso9899:199409', '-O2', '-o', '8716bec93e87526b379b99f09f02305b']| `result == 5` | ✅️| `8716bec93e87526b379b99f09f02305b` |
|GCC| ['gcc', '-std=gnu89', '-O2', '-o', '7c2807f38a5932978595484bf4565943']| `result == 5` | ✅️| `7c2807f38a5932978595484bf4565943` |
|GCC| ['gcc', '-std=gnu90', '-O2', '-o', 'c224fa91692cfe283df222d1b03551dd']| `result == 5` | ✅️| `c224fa91692cfe283df222d1b03551dd` |
|GCC| ['gcc', '-std=iso9899:1999', '-O2', '-o', '71ae5a04c64b31ee4e8a42c5e7cd2d8b']| `result == 5` | ✅️| `71ae5a04c64b31ee4e8a42c5e7cd2d8b` |
|GCC| ['gcc', '-std=gnu99', '-O2', '-o', 'c799255fc682fbf82f057ed7846362bc']| `result == 5` | ✅️| `c799255fc682fbf82f057ed7846362bc` |
|GCC| ['gcc', '-std=iso9899:2011', '-O2', '-o', 'fa58c126202c95c0d6f40b31121b649b']| `result == 5` | ✅️| `fa58c126202c95c0d6f40b31121b649b` |
|GCC| ['gcc', '-std=iso9899:2017', '-O2', '-o', '01de5ac3b90a0758318285be249d8762']| `result == 5` | ✅️| `01de5ac3b90a0758318285be249d8762` |
|GCC| ['gcc', '-std=gnu11', '-O2', '-o', 'ab60203886833ea85d81a90fbb822148']| `result == 5` | ✅️| `ab60203886833ea85d81a90fbb822148` |
|GCC| ['gcc', '-std=c99', '-O3', '-o', '26324eaa0c61adfd5f2a19aeecdf3ab4']| `result == 5` | ✅️| `26324eaa0c61adfd5f2a19aeecdf3ab4` |
|GCC| ['gcc', '-std=c9x', '-O3', '-o', 'bb3bd9223cb5db9d4188496d83e80fcc']| `result == 5` | ✅️| `bb3bd9223cb5db9d4188496d83e80fcc` |
|GCC| ['gcc', '-std=c89', '-O3', '-o', '0ea699c7866c9ce1e13c845b87f9707e']| `result == 5` | ✅️| `0ea699c7866c9ce1e13c845b87f9707e` |
|GCC| ['gcc', '-std=c90', '-O3', '-o', '5e3341cdd75770a344a57fe9d3b7daf6']| `result == 5` | ✅️| `5e3341cdd75770a344a57fe9d3b7daf6` |
|GCC| ['gcc', '-std=c2x', '-O3', '-o', '57c9fb207322afc14608829bc9bee0ad']| `result == 5` | ✅️| `57c9fb207322afc14608829bc9bee0ad` |
|GCC| ['gcc', '-std=c17', '-O3', '-o', 'bf268c209183ccb8ca227a73f044171a']| `result == 5` | ✅️| `bf268c209183ccb8ca227a73f044171a` |
|GCC| ['gcc', '-std=c18', '-O3', '-o', '59cf4a8b39260727466b283bc64ffb02']| `result == 5` | ✅️| `59cf4a8b39260727466b283bc64ffb02` |
|GCC| ['gcc', '-std=c11', '-O3', '-o', '2fa69796d838548048d0ab8c5a9b2b72']| `result == 5` | ✅️| `2fa69796d838548048d0ab8c5a9b2b72` |
|GCC| ['gcc', '-std=c1x', '-O3', '-o', 'ffa3d671311bf0f0d7b6d9e4a7082875']| `result == 5` | ✅️| `ffa3d671311bf0f0d7b6d9e4a7082875` |
|GCC| ['gcc', '-std=gnu11', '-O3', '-o', 'a9dcbe762777e884bb8884f1d9dd6b7f']| `result == 5` | ✅️| `a9dcbe762777e884bb8884f1d9dd6b7f` |
|GCC| ['gcc', '-std=iso9899:1990', '-O3', '-o', 'dc0855c1eab3514070e7a92d1230a980']| `result == 5` | ✅️| `dc0855c1eab3514070e7a92d1230a980` |
|GCC| ['gcc', '-std=iso9899:199409', '-O3', '-o', 'ef1f10f6123162285beee69252f9053c']| `result == 5` | ✅️| `ef1f10f6123162285beee69252f9053c` |
|GCC| ['gcc', '-std=gnu89', '-O3', '-o', '2d987d7a115c8c5130dfc612750c17cd']| `result == 5` | ✅️| `2d987d7a115c8c5130dfc612750c17cd` |
|GCC| ['gcc', '-std=gnu90', '-O3', '-o', 'b816a3ec8f857b6ea8bddc12141e050e']| `result == 5` | ✅️| `b816a3ec8f857b6ea8bddc12141e050e` |
|GCC| ['gcc', '-std=iso9899:1999', '-O3', '-o', '514f8264053d134073d91b582ba1e0b8']| `result == 5` | ✅️| `514f8264053d134073d91b582ba1e0b8` |
|GCC| ['gcc', '-std=gnu99', '-O3', '-o', '0877207327cd458872d42d11d9b86573']| `result == 5` | ✅️| `0877207327cd458872d42d11d9b86573` |
|GCC| ['gcc', '-std=iso9899:2011', '-O3', '-o', '4c783600575d287d58bf330a35b69121']| `result == 5` | ✅️| `4c783600575d287d58bf330a35b69121` |
|GCC| ['gcc', '-std=iso9899:2017', '-O3', '-o', '816ec2e7e576582688d9f2475b32329a']| `result == 5` | ✅️| `816ec2e7e576582688d9f2475b32329a` |
|GCC| ['gcc', '-std=gnu11', '-O3', '-o', 'a9dcbe762777e884bb8884f1d9dd6b7f']| `result == 5` | ✅️| `a9dcbe762777e884bb8884f1d9dd6b7f` |
|Clang| ['clang', '-std=c99', '-O0', '-o', 'ba54afffa89b9bff05bdefe33aed634c']| `result == 5` | ❌️| `ba54afffa89b9bff05bdefe33aed634c` |
|Clang| ['clang', '-std=c9x', '-O0', '-o', '592abffe031da4921169787eeccfa63f']| `result == 5` | ❌️| `592abffe031da4921169787eeccfa63f` |
|Clang| ['clang', '-std=c89', '-O0', '-o', '11011998ad4703e11cc59f205f494e39']| `result == 5` | ❌️| `11011998ad4703e11cc59f205f494e39` |
|Clang| ['clang', '-std=c90', '-O0', '-o', '28811206fe79e3348d88327cf348924d']| `result == 5` | ❌️| `28811206fe79e3348d88327cf348924d` |
|Clang| ['clang', '-std=c2x', '-O0', '-o', 'be3b8eeb1968733db62ea66bc97d98c1']| `result == 5` | ❌️| `be3b8eeb1968733db62ea66bc97d98c1` |
|Clang| ['clang', '-std=c17', '-O0', '-o', 'b9d578ef4ce70ab8a86923c40dd8b3fa']| `result == 5` | ❌️| `b9d578ef4ce70ab8a86923c40dd8b3fa` |
|Clang| ['clang', '-std=c18', '-O0', '-o', '99ebc95ccc5464f97ee707c7e9b8f06b']| `result == 5` | ❌️| `99ebc95ccc5464f97ee707c7e9b8f06b` |
|Clang| ['clang', '-std=c11', '-O0', '-o', 'd875719582d7a9833d578c5383763cf0']| `result == 5` | ❌️| `d875719582d7a9833d578c5383763cf0` |
|Clang| ['clang', '-std=c1x', '-O0', '-o', '278f7e3d473d0209e4e9914ea59df3ce']| `result == 5` | ❌️| `278f7e3d473d0209e4e9914ea59df3ce` |
|Clang| ['clang', '-std=gnu11', '-O0', '-o', 'e7c93bb2652339db4386981a63e1ea21']| `result == 5` | ❌️| `e7c93bb2652339db4386981a63e1ea21` |
|Clang| ['clang', '-std=iso9899:1990', '-O0', '-o', '0404327b5decfeba24e626f82802d0cf']| `result == 5` | ❌️| `0404327b5decfeba24e626f82802d0cf` |
|Clang| ['clang', '-std=iso9899:199409', '-O0', '-o', '333f389b2f9f7a8e676ba0fb0bacee2c']| `result == 5` | ❌️| `333f389b2f9f7a8e676ba0fb0bacee2c` |
|Clang| ['clang', '-std=gnu89', '-O0', '-o', '3630fee57a26625777ee17f5dc6304b7']| `result == 5` | ❌️| `3630fee57a26625777ee17f5dc6304b7` |
|Clang| ['clang', '-std=gnu90', '-O0', '-o', '4dd5e9411a4d73868fd5bfeadaf8ba9d']| `result == 5` | ❌️| `4dd5e9411a4d73868fd5bfeadaf8ba9d` |
|Clang| ['clang', '-std=iso9899:1999', '-O0', '-o', 'a5cb43e4ac5b9809995a0f48decaba43']| `result == 5` | ❌️| `a5cb43e4ac5b9809995a0f48decaba43` |
|Clang| ['clang', '-std=gnu99', '-O0', '-o', '7a0735583f3658c41998f7e76ef6cdad']| `result == 5` | ❌️| `7a0735583f3658c41998f7e76ef6cdad` |
|Clang| ['clang', '-std=iso9899:2011', '-O0', '-o', '86395550dd7ce327e90f0c68b0e86706']| `result == 5` | ❌️| `86395550dd7ce327e90f0c68b0e86706` |
|Clang| ['clang', '-std=iso9899:2017', '-O0', '-o', '5cae3bc471ce41e280599d8f084a2383']| `result == 5` | ❌️| `5cae3bc471ce41e280599d8f084a2383` |
|Clang| ['clang', '-std=gnu11', '-O0', '-o', 'e7c93bb2652339db4386981a63e1ea21']| `result == 5` | ❌️| `e7c93bb2652339db4386981a63e1ea21` |
|Clang| ['clang', '-std=c99', '-O1', '-o', '064a8e3b0d975e2fc02d26ce32e7f6cc']| `result == 5` | ❌️| `064a8e3b0d975e2fc02d26ce32e7f6cc` |
|Clang| ['clang', '-std=c9x', '-O1', '-o', '5dc4721df2006c20491e6b1573c57b9c']| `result == 5` | ❌️| `5dc4721df2006c20491e6b1573c57b9c` |
|Clang| ['clang', '-std=c89', '-O1', '-o', '9c93d6f0e27921003f2e89e40dffd366']| `result == 5` | ❌️| `9c93d6f0e27921003f2e89e40dffd366` |
|Clang| ['clang', '-std=c90', '-O1', '-o', '3a217e5e0ac86b867f8707758487373e']| `result == 5` | ❌️| `3a217e5e0ac86b867f8707758487373e` |
|Clang| ['clang', '-std=c2x', '-O1', '-o', 'aecb00d6d6df38b95828e1ca4bc37dbc']| `result == 5` | ❌️| `aecb00d6d6df38b95828e1ca4bc37dbc` |
|Clang| ['clang', '-std=c17', '-O1', '-o', '5eef581a73fc4a6cb8df4d7f0b587018']| `result == 5` | ❌️| `5eef581a73fc4a6cb8df4d7f0b587018` |
|Clang| ['clang', '-std=c18', '-O1', '-o', '77763f9cfdb9123df4c9012f17caede1']| `result == 5` | ❌️| `77763f9cfdb9123df4c9012f17caede1` |
|Clang| ['clang', '-std=c11', '-O1', '-o', '53d198213e47a66aeab93651377bf56a']| `result == 5` | ❌️| `53d198213e47a66aeab93651377bf56a` |
|Clang| ['clang', '-std=c1x', '-O1', '-o', 'de0cf458d1c62aacb7799df83d7354a8']| `result == 5` | ❌️| `de0cf458d1c62aacb7799df83d7354a8` |
|Clang| ['clang', '-std=gnu11', '-O1', '-o', '531746939f0e74ee7208ce2bb4faf594']| `result == 5` | ❌️| `531746939f0e74ee7208ce2bb4faf594` |
|Clang| ['clang', '-std=iso9899:1990', '-O1', '-o', 'd1018923583abd6465a25634c4bd87f7']| `result == 5` | ❌️| `d1018923583abd6465a25634c4bd87f7` |
|Clang| ['clang', '-std=iso9899:199409', '-O1', '-o', '4b3031b48d6392ea4b1af2292d45bf18']| `result == 5` | ❌️| `4b3031b48d6392ea4b1af2292d45bf18` |
|Clang| ['clang', '-std=gnu89', '-O1', '-o', 'da640bed4470bad728f970156502287e']| `result == 5` | ❌️| `da640bed4470bad728f970156502287e` |
|Clang| ['clang', '-std=gnu90', '-O1', '-o', '433273c167f5de0b1106c5933c98bdaf']| `result == 5` | ❌️| `433273c167f5de0b1106c5933c98bdaf` |
|Clang| ['clang', '-std=iso9899:1999', '-O1', '-o', 'f964413b8b32c72b805d625ddb43c6a6']| `result == 5` | ❌️| `f964413b8b32c72b805d625ddb43c6a6` |
|Clang| ['clang', '-std=gnu99', '-O1', '-o', '9249e1a023eb868bf6f2ace955f4e1d1']| `result == 5` | ❌️| `9249e1a023eb868bf6f2ace955f4e1d1` |
|Clang| ['clang', '-std=iso9899:2011', '-O1', '-o', 'd6e0d721404f457e4d98b3cc60ec262a']| `result == 5` | ❌️| `d6e0d721404f457e4d98b3cc60ec262a` |
|Clang| ['clang', '-std=iso9899:2017', '-O1', '-o', 'dbad79b5fde34363ab7d33a7d5d75ca9']| `result == 5` | ❌️| `dbad79b5fde34363ab7d33a7d5d75ca9` |
|Clang| ['clang', '-std=gnu11', '-O1', '-o', '531746939f0e74ee7208ce2bb4faf594']| `result == 5` | ❌️| `531746939f0e74ee7208ce2bb4faf594` |
|Clang| ['clang', '-std=c99', '-O2', '-o', '58ebcdbe2d10d35cd4716317ffee8369']| `result == 5` | ❌️| `58ebcdbe2d10d35cd4716317ffee8369` |
|Clang| ['clang', '-std=c9x', '-O2', '-o', '36e539cca58daf6f1f11b992eef631ce']| `result == 5` | ❌️| `36e539cca58daf6f1f11b992eef631ce` |
|Clang| ['clang', '-std=c89', '-O2', '-o', '560346e8ae8919c1390b6f0ee4282f9b']| `result == 5` | ❌️| `560346e8ae8919c1390b6f0ee4282f9b` |
|Clang| ['clang', '-std=c90', '-O2', '-o', '4ad3ea7bf97f70c9069813ea1ef2ee5f']| `result == 5` | ❌️| `4ad3ea7bf97f70c9069813ea1ef2ee5f` |
|Clang| ['clang', '-std=c2x', '-O2', '-o', 'f9468dff63fdf54d3285ba481b9f633b']| `result == 5` | ❌️| `f9468dff63fdf54d3285ba481b9f633b` |
|Clang| ['clang', '-std=c17', '-O2', '-o', 'e9dd24eed06e61531ef5b1b8e20e4390']| `result == 5` | ❌️| `e9dd24eed06e61531ef5b1b8e20e4390` |
|Clang| ['clang', '-std=c18', '-O2', '-o', '334df205ebcf2b2c749404276f159e67']| `result == 5` | ❌️| `334df205ebcf2b2c749404276f159e67` |
|Clang| ['clang', '-std=c11', '-O2', '-o', '04db8dda69e4c9a9533d9755a3ef9dc1']| `result == 5` | ❌️| `04db8dda69e4c9a9533d9755a3ef9dc1` |
|Clang| ['clang', '-std=c1x', '-O2', '-o', '830d01dd218bb3a5673d51361a1d74dc']| `result == 5` | ❌️| `830d01dd218bb3a5673d51361a1d74dc` |
|Clang| ['clang', '-std=gnu11', '-O2', '-o', '3932283c2a189c1ee0f6472e8058d742']| `result == 5` | ❌️| `3932283c2a189c1ee0f6472e8058d742` |
|Clang| ['clang', '-std=iso9899:1990', '-O2', '-o', 'e705f79e841919378e44802b5443eb40']| `result == 5` | ❌️| `e705f79e841919378e44802b5443eb40` |
|Clang| ['clang', '-std=iso9899:199409', '-O2', '-o', '684536d774088c5252e09348bc0bb790']| `result == 5` | ❌️| `684536d774088c5252e09348bc0bb790` |
|Clang| ['clang', '-std=gnu89', '-O2', '-o', '4e2e0fae6b04434a376698e858e87861']| `result == 5` | ❌️| `4e2e0fae6b04434a376698e858e87861` |
|Clang| ['clang', '-std=gnu90', '-O2', '-o', 'c2155b243784ce512f18a9954db79b69']| `result == 5` | ❌️| `c2155b243784ce512f18a9954db79b69` |
|Clang| ['clang', '-std=iso9899:1999', '-O2', '-o', '42afabc37a33a9d836da5e90dde3dfb0']| `result == 5` | ❌️| `42afabc37a33a9d836da5e90dde3dfb0` |
|Clang| ['clang', '-std=gnu99', '-O2', '-o', '2d48cebd1642f196b95383aea4696ff8']| `result == 5` | ❌️| `2d48cebd1642f196b95383aea4696ff8` |
|Clang| ['clang', '-std=iso9899:2011', '-O2', '-o', 'e51afd1f725dc10524d888c8c95f8f1b']| `result == 5` | ❌️| `e51afd1f725dc10524d888c8c95f8f1b` |
|Clang| ['clang', '-std=iso9899:2017', '-O2', '-o', '12145536b37b08adc74e7b06f93add14']| `result == 5` | ❌️| `12145536b37b08adc74e7b06f93add14` |
|Clang| ['clang', '-std=gnu11', '-O2', '-o', '3932283c2a189c1ee0f6472e8058d742']| `result == 5` | ❌️| `3932283c2a189c1ee0f6472e8058d742` |
|Clang| ['clang', '-std=c99', '-O3', '-o', '3bfc8381ee1a7a8544b62d3b3b769897']| `result == 5` | ❌️| `3bfc8381ee1a7a8544b62d3b3b769897` |
|Clang| ['clang', '-std=c9x', '-O3', '-o', '8f1350649e8744e9b6e8343fe5cf78f8']| `result == 5` | ❌️| `8f1350649e8744e9b6e8343fe5cf78f8` |
|Clang| ['clang', '-std=c89', '-O3', '-o', '825c9625761a36ed7165121eeb923495']| `result == 5` | ❌️| `825c9625761a36ed7165121eeb923495` |
|Clang| ['clang', '-std=c90', '-O3', '-o', 'df833cac67a0727bc312a2cc1313a914']| `result == 5` | ❌️| `df833cac67a0727bc312a2cc1313a914` |
|Clang| ['clang', '-std=c2x', '-O3', '-o', '852de66e1c52965781f6fce87165ffc3']| `result == 5` | ❌️| `852de66e1c52965781f6fce87165ffc3` |
|Clang| ['clang', '-std=c17', '-O3', '-o', 'bb5ad62119bd5e7dba8f24337e42176e']| `result == 5` | ❌️| `bb5ad62119bd5e7dba8f24337e42176e` |
|Clang| ['clang', '-std=c18', '-O3', '-o', 'a226ce2338b4c6e21e93069c47b363f5']| `result == 5` | ❌️| `a226ce2338b4c6e21e93069c47b363f5` |
|Clang| ['clang', '-std=c11', '-O3', '-o', 'e19343a077e1309bce5baa6550deeb74']| `result == 5` | ❌️| `e19343a077e1309bce5baa6550deeb74` |
|Clang| ['clang', '-std=c1x', '-O3', '-o', 'f6992c3e95a3893ecfa44e5c52b94fec']| `result == 5` | ❌️| `f6992c3e95a3893ecfa44e5c52b94fec` |
|Clang| ['clang', '-std=gnu11', '-O3', '-o', 'b95dc6d2f16ed8c5e79e76b6afdea7d9']| `result == 5` | ❌️| `b95dc6d2f16ed8c5e79e76b6afdea7d9` |
|Clang| ['clang', '-std=iso9899:1990', '-O3', '-o', '2e1ee2005de2cbe880f5b4b56e9f9e6e']| `result == 5` | ❌️| `2e1ee2005de2cbe880f5b4b56e9f9e6e` |
|Clang| ['clang', '-std=iso9899:199409', '-O3', '-o', '1399edbcad9c688ac3f6b92c23498c35']| `result == 5` | ❌️| `1399edbcad9c688ac3f6b92c23498c35` |
|Clang| ['clang', '-std=gnu89', '-O3', '-o', '069342034bfd7b999d28a6868c679492']| `result == 5` | ❌️| `069342034bfd7b999d28a6868c679492` |
|Clang| ['clang', '-std=gnu90', '-O3', '-o', 'f1a8a0624c6267d6a4c0ff453fd73fb5']| `result == 5` | ❌️| `f1a8a0624c6267d6a4c0ff453fd73fb5` |
|Clang| ['clang', '-std=iso9899:1999', '-O3', '-o', '247f6a795c9f7bb47e7ea616945e6d44']| `result == 5` | ❌️| `247f6a795c9f7bb47e7ea616945e6d44` |
|Clang| ['clang', '-std=gnu99', '-O3', '-o', '7e2ee3a51800a1cddf56579256fee5a4']| `result == 5` | ❌️| `7e2ee3a51800a1cddf56579256fee5a4` |
|Clang| ['clang', '-std=iso9899:2011', '-O3', '-o', '53da8eceecd848baccf4198054caf71c']| `result == 5` | ❌️| `53da8eceecd848baccf4198054caf71c` |
|Clang| ['clang', '-std=iso9899:2017', '-O3', '-o', 'a8d35ae2b69f4ca7636675b84c6600f8']| `result == 5` | ❌️| `a8d35ae2b69f4ca7636675b84c6600f8` |
|Clang| ['clang', '-std=gnu11', '-O3', '-o', 'b95dc6d2f16ed8c5e79e76b6afdea7d9']| `result == 5` | ❌️| `b95dc6d2f16ed8c5e79e76b6afdea7d9` |
|TCC| ['tcc', '-std=c99', '-O0', '-o', 'd17044d52513692f117c6ec8fc3fc39a']| `result == 5` | ❌️| `d17044d52513692f117c6ec8fc3fc39a` |
|TCC| ['tcc', '-std=c9x', '-O0', '-o', 'da3232c20af62eafd67808c253921a06']| `result == 5` | ❌️| `da3232c20af62eafd67808c253921a06` |
|TCC| ['tcc', '-std=c89', '-O0', '-o', '2915bc2fd8549ed3a5d85bb4b7ae547c']| `result == 5` | ❌️| `2915bc2fd8549ed3a5d85bb4b7ae547c` |
|TCC| ['tcc', '-std=c90', '-O0', '-o', '0b8777920dc05dd4ad134f2946b34a19']| `result == 5` | ❌️| `0b8777920dc05dd4ad134f2946b34a19` |
|TCC| ['tcc', '-std=c2x', '-O0', '-o', '253782851cc0cb07b3fdc2ce86723fb4']| `result == 5` | ❌️| `253782851cc0cb07b3fdc2ce86723fb4` |
|TCC| ['tcc', '-std=c17', '-O0', '-o', '4c14cb1a8ec6eb627f1050576a9fa9db']| `result == 5` | ❌️| `4c14cb1a8ec6eb627f1050576a9fa9db` |
|TCC| ['tcc', '-std=c18', '-O0', '-o', 'a5116b53af48078faa05585fd62f54ab']| `result == 5` | ❌️| `a5116b53af48078faa05585fd62f54ab` |
|TCC| ['tcc', '-std=c11', '-O0', '-o', '293117ea48d99ab81e90d1cdfaf916c1']| `result == 5` | ❌️| `293117ea48d99ab81e90d1cdfaf916c1` |
|TCC| ['tcc', '-std=c1x', '-O0', '-o', '0d4eb74fc42ac980cf2ca68f1c90a4a4']| `result == 5` | ❌️| `0d4eb74fc42ac980cf2ca68f1c90a4a4` |
|TCC| ['tcc', '-std=gnu11', '-O0', '-o', 'ad111efe8b7814900991beee52c9a796']| `result == 5` | ❌️| `ad111efe8b7814900991beee52c9a796` |
|TCC| ['tcc', '-std=iso9899:1990', '-O0', '-o', 'ac1a15953f51f54e688f3b4727f41a51']| `result == 5` | ❌️| `ac1a15953f51f54e688f3b4727f41a51` |
|TCC| ['tcc', '-std=iso9899:199409', '-O0', '-o', '42840afcf2ae5fd51d2ecc9299e9571f']| `result == 5` | ❌️| `42840afcf2ae5fd51d2ecc9299e9571f` |
|TCC| ['tcc', '-std=gnu89', '-O0', '-o', 'be70667ceb5b42751a0d8c21fa097e4e']| `result == 5` | ❌️| `be70667ceb5b42751a0d8c21fa097e4e` |
|TCC| ['tcc', '-std=gnu90', '-O0', '-o', 'c67b96c357ee414e002924c76daae55a']| `result == 5` | ❌️| `c67b96c357ee414e002924c76daae55a` |
|TCC| ['tcc', '-std=iso9899:1999', '-O0', '-o', '994d2d68eedcb516c20ab63fda3114a8']| `result == 5` | ❌️| `994d2d68eedcb516c20ab63fda3114a8` |
|TCC| ['tcc', '-std=gnu99', '-O0', '-o', '08a7db84264a0cb589719c78ccba8ea2']| `result == 5` | ❌️| `08a7db84264a0cb589719c78ccba8ea2` |
|TCC| ['tcc', '-std=iso9899:2011', '-O0', '-o', '8fc4b3893c923cc70dc3821cd50318ad']| `result == 5` | ❌️| `8fc4b3893c923cc70dc3821cd50318ad` |
|TCC| ['tcc', '-std=iso9899:2017', '-O0', '-o', 'ec8059d3853a19cf20a44d61884ce722']| `result == 5` | ❌️| `ec8059d3853a19cf20a44d61884ce722` |
|TCC| ['tcc', '-std=gnu11', '-O0', '-o', 'ad111efe8b7814900991beee52c9a796']| `result == 5` | ❌️| `ad111efe8b7814900991beee52c9a796` |
|TCC| ['tcc', '-std=c99', '-O1', '-o', 'fe23a12f55a6f0b3b2d536658358ce49']| `result == 5` | ❌️| `fe23a12f55a6f0b3b2d536658358ce49` |
|TCC| ['tcc', '-std=c9x', '-O1', '-o', '1e8e803b29da0468df2253c105a165fc']| `result == 5` | ❌️| `1e8e803b29da0468df2253c105a165fc` |
|TCC| ['tcc', '-std=c89', '-O1', '-o', '007599465b0347736edf728cc46874e0']| `result == 5` | ❌️| `007599465b0347736edf728cc46874e0` |
|TCC| ['tcc', '-std=c90', '-O1', '-o', '26e38dbc9c4ea02be13be3ae6c89d097']| `result == 5` | ❌️| `26e38dbc9c4ea02be13be3ae6c89d097` |
|TCC| ['tcc', '-std=c2x', '-O1', '-o', 'beab8dbd6e7e393d3bc5321a89aba574']| `result == 5` | ❌️| `beab8dbd6e7e393d3bc5321a89aba574` |
|TCC| ['tcc', '-std=c17', '-O1', '-o', 'fc8a86cca3e314aa6d4c81a3d147db11']| `result == 5` | ❌️| `fc8a86cca3e314aa6d4c81a3d147db11` |
|TCC| ['tcc', '-std=c18', '-O1', '-o', '5e6843f4549546ac8cec6e2133912557']| `result == 5` | ❌️| `5e6843f4549546ac8cec6e2133912557` |
|TCC| ['tcc', '-std=c11', '-O1', '-o', 'a7e0392930c04f89ef52c546880858ff']| `result == 5` | ❌️| `a7e0392930c04f89ef52c546880858ff` |
|TCC| ['tcc', '-std=c1x', '-O1', '-o', '1001607136ba67cff44043b1360f70f2']| `result == 5` | ❌️| `1001607136ba67cff44043b1360f70f2` |
|TCC| ['tcc', '-std=gnu11', '-O1', '-o', '633969c7f4175d55d5999fa9140da7c4']| `result == 5` | ❌️| `633969c7f4175d55d5999fa9140da7c4` |
|TCC| ['tcc', '-std=iso9899:1990', '-O1', '-o', '0a669fe8eaccb652a2939a807ca42c35']| `result == 5` | ❌️| `0a669fe8eaccb652a2939a807ca42c35` |
|TCC| ['tcc', '-std=iso9899:199409', '-O1', '-o', '9820f95c8aa0aab233fb3173f76fac66']| `result == 5` | ❌️| `9820f95c8aa0aab233fb3173f76fac66` |
|TCC| ['tcc', '-std=gnu89', '-O1', '-o', 'dc7d3ebc02801b2211721a3654389358']| `result == 5` | ❌️| `dc7d3ebc02801b2211721a3654389358` |
|TCC| ['tcc', '-std=gnu90', '-O1', '-o', '995bfc5013e2e87453ac1cb560be79f0']| `result == 5` | ❌️| `995bfc5013e2e87453ac1cb560be79f0` |
|TCC| ['tcc', '-std=iso9899:1999', '-O1', '-o', '1273d26792011b80feaea70148b6c851']| `result == 5` | ❌️| `1273d26792011b80feaea70148b6c851` |
|TCC| ['tcc', '-std=gnu99', '-O1', '-o', '89b6460113a5540fa17f923630d1aaf3']| `result == 5` | ❌️| `89b6460113a5540fa17f923630d1aaf3` |
|TCC| ['tcc', '-std=iso9899:2011', '-O1', '-o', '75dd26dbee8147189fa7396a1c54aa41']| `result == 5` | ❌️| `75dd26dbee8147189fa7396a1c54aa41` |
|TCC| ['tcc', '-std=iso9899:2017', '-O1', '-o', '5fc8fb820d52b28b0b6047890dc8ee7c']| `result == 5` | ❌️| `5fc8fb820d52b28b0b6047890dc8ee7c` |
|TCC| ['tcc', '-std=gnu11', '-O1', '-o', '633969c7f4175d55d5999fa9140da7c4']| `result == 5` | ❌️| `633969c7f4175d55d5999fa9140da7c4` |
|TCC| ['tcc', '-std=c99', '-O2', '-o', '8a60e9ffc21d2d1f3a9dc38caa79caaf']| `result == 5` | ❌️| `8a60e9ffc21d2d1f3a9dc38caa79caaf` |
|TCC| ['tcc', '-std=c9x', '-O2', '-o', 'd55a8a520a98859d4772eb7d7d2574ae']| `result == 5` | ❌️| `d55a8a520a98859d4772eb7d7d2574ae` |
|TCC| ['tcc', '-std=c89', '-O2', '-o', '716ab53b08c78e9d7543fa5a45ca1501']| `result == 5` | ❌️| `716ab53b08c78e9d7543fa5a45ca1501` |
|TCC| ['tcc', '-std=c90', '-O2', '-o', '0b197e3613c6c69f612320de5d5bf821']| `result == 5` | ❌️| `0b197e3613c6c69f612320de5d5bf821` |
|TCC| ['tcc', '-std=c2x', '-O2', '-o', '35af2900ec5fe7a10627144018a3471c']| `result == 5` | ❌️| `35af2900ec5fe7a10627144018a3471c` |
|TCC| ['tcc', '-std=c17', '-O2', '-o', '8e6671031fa4750c7b606d06ed4d3582']| `result == 5` | ❌️| `8e6671031fa4750c7b606d06ed4d3582` |
|TCC| ['tcc', '-std=c18', '-O2', '-o', 'd1127d5dee9a769b7da76db1cd95835a']| `result == 5` | ❌️| `d1127d5dee9a769b7da76db1cd95835a` |
|TCC| ['tcc', '-std=c11', '-O2', '-o', 'a1ff60d4d2a34e98b46c43c64543e852']| `result == 5` | ❌️| `a1ff60d4d2a34e98b46c43c64543e852` |
|TCC| ['tcc', '-std=c1x', '-O2', '-o', '5ee7e564d96389e2b729ce212177e635']| `result == 5` | ❌️| `5ee7e564d96389e2b729ce212177e635` |
|TCC| ['tcc', '-std=gnu11', '-O2', '-o', '57f3e45c86361e52c4ec94cd0a630156']| `result == 5` | ❌️| `57f3e45c86361e52c4ec94cd0a630156` |
|TCC| ['tcc', '-std=iso9899:1990', '-O2', '-o', '12bbd8d1e0acdd52e1dca5f2a299a904']| `result == 5` | ❌️| `12bbd8d1e0acdd52e1dca5f2a299a904` |
|TCC| ['tcc', '-std=iso9899:199409', '-O2', '-o', 'a1a920b47b2d270f89985320623818cf']| `result == 5` | ❌️| `a1a920b47b2d270f89985320623818cf` |
|TCC| ['tcc', '-std=gnu89', '-O2', '-o', 'af44188a32fb988a2b67dad2261620b2']| `result == 5` | ❌️| `af44188a32fb988a2b67dad2261620b2` |
|TCC| ['tcc', '-std=gnu90', '-O2', '-o', 'f5c921cb6b53eed5001d2b7697a43e86']| `result == 5` | ❌️| `f5c921cb6b53eed5001d2b7697a43e86` |
|TCC| ['tcc', '-std=iso9899:1999', '-O2', '-o', '597cc90143b4e0badc04779a3aba56eb']| `result == 5` | ❌️| `597cc90143b4e0badc04779a3aba56eb` |
|TCC| ['tcc', '-std=gnu99', '-O2', '-o', 'cad534e056741944fa3bf48b5d4a6df4']| `result == 5` | ❌️| `cad534e056741944fa3bf48b5d4a6df4` |
|TCC| ['tcc', '-std=iso9899:2011', '-O2', '-o', '14aa7522b9c68180d8df38abc864b353']| `result == 5` | ❌️| `14aa7522b9c68180d8df38abc864b353` |
|TCC| ['tcc', '-std=iso9899:2017', '-O2', '-o', '24c1c1fea2b3101e2ccb7b9c5fcb40c2']| `result == 5` | ❌️| `24c1c1fea2b3101e2ccb7b9c5fcb40c2` |
|TCC| ['tcc', '-std=gnu11', '-O2', '-o', '57f3e45c86361e52c4ec94cd0a630156']| `result == 5` | ❌️| `57f3e45c86361e52c4ec94cd0a630156` |
|TCC| ['tcc', '-std=c99', '-O3', '-o', '0a9bd561e3d06ab897383f31d5d00f61']| `result == 5` | ❌️| `0a9bd561e3d06ab897383f31d5d00f61` |
|TCC| ['tcc', '-std=c9x', '-O3', '-o', '7f91b733116f63b2489d5b71a013e3ce']| `result == 5` | ❌️| `7f91b733116f63b2489d5b71a013e3ce` |
|TCC| ['tcc', '-std=c89', '-O3', '-o', 'a8497a087315e821b6f0b0e2e9c46ba3']| `result == 5` | ❌️| `a8497a087315e821b6f0b0e2e9c46ba3` |
|TCC| ['tcc', '-std=c90', '-O3', '-o', '02649098c4e85b92c108d6655953e687']| `result == 5` | ❌️| `02649098c4e85b92c108d6655953e687` |
|TCC| ['tcc', '-std=c2x', '-O3', '-o', '494826f02c5fd584d1a926f76e8cc649']| `result == 5` | ❌️| `494826f02c5fd584d1a926f76e8cc649` |
|TCC| ['tcc', '-std=c17', '-O3', '-o', '899b09a12cda14b8b260fcb23d59f29f']| `result == 5` | ❌️| `899b09a12cda14b8b260fcb23d59f29f` |
|TCC| ['tcc', '-std=c18', '-O3', '-o', '4331e5eabfd9a08250b871b9c5dc99f5']| `result == 5` | ❌️| `4331e5eabfd9a08250b871b9c5dc99f5` |
|TCC| ['tcc', '-std=c11', '-O3', '-o', '5e594876cb2edc420d8c9db8d5c1f18e']| `result == 5` | ❌️| `5e594876cb2edc420d8c9db8d5c1f18e` |
|TCC| ['tcc', '-std=c1x', '-O3', '-o', '12b3fad14342c1be49ab30a3e5c09ba5']| `result == 5` | ❌️| `12b3fad14342c1be49ab30a3e5c09ba5` |
|TCC| ['tcc', '-std=gnu11', '-O3', '-o', '23534b6f94531ef4c391b5d9fe172686']| `result == 5` | ❌️| `23534b6f94531ef4c391b5d9fe172686` |
|TCC| ['tcc', '-std=iso9899:1990', '-O3', '-o', 'd3d3aef36558475dcdf0769339aeec3b']| `result == 5` | ❌️| `d3d3aef36558475dcdf0769339aeec3b` |
|TCC| ['tcc', '-std=iso9899:199409', '-O3', '-o', 'dec49598644f2ae450eed30bad13c559']| `result == 5` | ❌️| `dec49598644f2ae450eed30bad13c559` |
|TCC| ['tcc', '-std=gnu89', '-O3', '-o', 'eadd44c83053a7cc608e6c41a12f5328']| `result == 5` | ❌️| `eadd44c83053a7cc608e6c41a12f5328` |
|TCC| ['tcc', '-std=gnu90', '-O3', '-o', 'f6e3b2fda06fceb90687a81eef9fc7c2']| `result == 5` | ❌️| `f6e3b2fda06fceb90687a81eef9fc7c2` |
|TCC| ['tcc', '-std=iso9899:1999', '-O3', '-o', 'd738af4c26d1443633d06232d26679a9']| `result == 5` | ❌️| `d738af4c26d1443633d06232d26679a9` |
|TCC| ['tcc', '-std=gnu99', '-O3', '-o', 'dd8adea178c32e39b46dc69567db3e17']| `result == 5` | ❌️| `dd8adea178c32e39b46dc69567db3e17` |
|TCC| ['tcc', '-std=iso9899:2011', '-O3', '-o', 'f77009e35ee9ecee8aaea4f6ecabcdf7']| `result == 5` | ❌️| `f77009e35ee9ecee8aaea4f6ecabcdf7` |
|TCC| ['tcc', '-std=iso9899:2017', '-O3', '-o', 'e8687773c1bd3dc67fa0486f55b61bb0']| `result == 5` | ❌️| `e8687773c1bd3dc67fa0486f55b61bb0` |
|TCC| ['tcc', '-std=gnu11', '-O3', '-o', '23534b6f94531ef4c391b5d9fe172686']| `result == 5` | ❌️| `23534b6f94531ef4c391b5d9fe172686` |

<br>

---

<br>

## Explicit versions

To improve CSeq per-compiler behaviour we will need explicit versions that provide the given
behaviour, in this case there will be only two explicit versions.

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