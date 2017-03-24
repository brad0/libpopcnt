libpopcnt
=========
[![Build Status](https://travis-ci.org/kimwalisch/libpopcnt.svg)](https://travis-ci.org/kimwalisch/libpopcnt)
[![Build Status](https://ci.appveyor.com/api/projects/status/github/kimwalisch/libpopcnt?branch=master&svg=true)](https://ci.appveyor.com/project/kimwalisch/libpopcnt)
[![GitHub license](https://img.shields.io/badge/license-BSD%202-blue.svg)](https://github.com/kimwalisch/libpopcnt/blob/master/LICENSE)

```libpopcnt.h``` is a header only C/C++ library for counting the
number of 1 bits (bit population count) in an array as quickly as
possible using specialized CPU instructions e.g.
[POPCNT](https://en.wikipedia.org/wiki/SSE4#POPCNT_and_LZCNT),
[AVX2](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions).
```libpopcnt.h``` has been tested successfully using the GCC,
Clang and MSVC compilers.

The algorithms used in ```libpopcnt.h``` are described in the paper
[Faster Population Counts using AVX2 Instructions](https://arxiv.org/abs/1611.07612)
by Daniel Lemire, Nathan Kurz and Wojciech Mula (23 Nov 2016).

How it works
============
```libpopcnt.h``` uses a combination of 3 different bit population
count algorithms based on the CPU architecture and the input array
size:

* For array sizes < 1 kilobyte an unrolled ```POPCNT``` algorithm
is used.
* For array sizes ≥ 1 kilobyte an ```AVX2``` algorithm is used.
* For CPUs without ```POPCNT``` instruction a portable 
integer algorithm is used.

The GitHub repository
[WojciechMula/sse-popcount](https://github.com/WojciechMula/sse-popcount/tree/master/results)
contains extensive benchmarks for the 3 algorithms used in
```libpopcnt.h```. The algorithm are named
```builtin-popcnt-unrolled```, ```avx2-harley-seal```, ```harley-seal```.

C/C++ API
=========
```C++
#include "libpopcnt.h"

/// Count the number of 1 bits in the data array.
/// @param data  An array
/// @param size  Size of data in bytes
uint64_t popcnt(const void* data, uint64_t size);
```

How to compile
==============
Compilation does not require any special compiler flags (like
```-mpopcnt```, ```-mavx2```)! Also on x86 CPUs ```libpopcnt.h```
checks using ```cpuid``` if the CPU supports POPCNT/AVX2
before using it.


```bash
# How to compile using GCC
gcc -O2 program.c

# How to compile using Clang
clang -O2 program.c

# How to compile using Microsoft Visual C++
cl /O2 program.cpp
```
