dcc is an optimizing compiler for the C language.

It currently runs on x86_64 Linux and it can cross-compile to aarch64 (arm64).


## Features
- Built-in C preprocessor.
- C11 and partial C23 support.
- Good diagnostic messages.
- Mostly compatible with gcc.
- Several common GCC extensions, plus some new one.
- Intel intrinsics up to AVX2.
- Backends: aarch64, x86-64.
- Integrated assembler.
- Debug info in DWARF4 format.
- ~12 times faster than gcc.
- Plugin API for accessing the AST and extending the compiler.
- UB Sanitizer


## [Status](./backends_status.md)
| Target              | Status         |
|---------------------|----------------|
| `x86_64-linux-gnu`  | alpha-stable   |
| `aarch64-linux-gnu` | alpha-unstable |
| `riscv64-linux-gnu` | incomplete     |


[Demo (06/05/2024)](https://www.youtube.com/watch?v=TPWxtAFwiks)

In the demo I show:
- self-compilation,
- debug info,
- good error messages and warnings,
- preprocessor oddities,
- emulating gcc vector extension by using the operator overloading attribute,
- custom reflection system through the plugin API.


[Changelog](./CHANGELOG.md)


Successful builds:
[8080](https://github.com/superzazu/8080),
[BearSSL](https://github.com/OUIsolutions/BearSslSingle-Unit),
[Meow hash](https://github.com/cmuratori/meow_hash),
[Nuklear](https://github.com/Immediate-Mode-UI/Nuklear),
[TermGL](https://github.com/wojciech-graj/TermGL),
[Verstable](https://github.com/JacksonAllan/Verstable),
[bf-x86](https://github.com/skeeto/bf-x86),
[brainc](https://github.com/detectivekaktus/brainc),
[catimg](https://github.com/posva/catimg),
[cccc](https://github.com/skeeto/cccc),
[chess](https://github.com/weirddan455/chess),
[chibicc](https://github.com/rui314/chibicc),
[clex](https://github.com/h2337/clex?),
[cproc](https://sr.ht/~mcf/cproc),
[fcpp](https://github.com/bagder/fcpp),
[gc](https://github.com/mkirchner/gc),
[genann](https://github.com/codeplea/genann),
[lemon](https://compiler-dept.github.io/lemon),
[libaca](https://github.com/dongyx/libaca),
[libcox](https://github.com/symisc/libcox),
[luigi](https://github.com/nakst/luigi),
[mandel-simd](https://github.com/skeeto/mandel-simd),
[mg](https://github.com/ibara/mg),
[minilua](https://github.com/edubart/minilua),
[minivorbis](https://github.com/edubart/minivorbis),
[miniz-3.0.2](https://github.com/richgel999/miniz),
[oed](https://github.com/ibara/oed),
[optparse](https://github.com/skeeto/optparse),
[parson](https://github.com/kgabis/parson),
[pdjson](https://github.com/skeeto/pdjson),
[pl0c](https://github.com/ibara/pl0c),
[psh](https://github.com/proh14/psh),
[q3vm](https://github.com/jnz/q3vm),
[qbe](https://c9x.me/compile/),
[qoi](https://github.com/phoboslab/qoi),
[quad-tree](https://github.com/leonmavr/quad-tree),
[quadsort](https://github.com/scandum/quadsort),
[race64](https://github.com/skeeto/race64),
[renderer](https://github.com/zauonlok/renderer),
[rpng](https://github.com/raysan5/rpng),
[siod](https://github.com/deriito/siod-v3.0),
[space-shooter.c](https://github.com/tsherif/space-shooter.c),
[sqlite](https://github.com/sqlite/sqlite),
[stb](https://github.com/nothings/stb/),
[tinn](https://github.com/glouw/tinn),
[treecc](https://github.com/rweather/treecc),
[trie](https://github.com/skeeto/trie),
[utf8.h](https://github.com/sheredom/utf8.h),
[vce](https://github.com/ibara/vce),
[z80](https://github.com/superzazu/z80).


## Performance and code quality (02/2025)
Measurements done like the previous (07/2024).

- `dcc-0.8` built with `dcc-0.7`
- `dcc.0` built with `dcc-0.8 -O0`
- `dcc.1` built with `dcc-0.8 -O1 -fno-inline`
- `dcc.2` built with `gcc-13.3.0 -O1 -fwrapv -fno-strict-aliasing -fno-delete-null-pointer-checks -fno-omit-frame-pointer`
- `dcc.3` built with `gcc-13.3.0 -O3`

| Compiler |        Cycles |   Instructions | Time [s] |  Obj Size | Comp. Size | Comp. Speed [LOC/s] |
|----------|--------------:|---------------:|----------|----------:|-----------:|--------------------:|
| dcc.3    |   772,378,385 |  1,108,315,903 | 0.23823  | 1,074,184 |  1,276,768 |          333,253.58 |
| dcc.2    |   875,339,855 |  1,333,849,871 | 0.26986  | 1,074,184 |    949,728 |          294,193.29 |
| dcc.1    | 1,170,765,841 |  1,832,543,712 | 0.36062  | 1,074,184 |  1,002,304 |          220,151.41 |
| dcc.0    | 1,382,189,150 |  2,105,052,535 | 0.41259  | 1,074,184 |  1,035,072 |          192,421.05 |
| gcc-13.3 | 9,541,270,463 | 15,113,325,935 | 2.8327   | 1,372,344 |  1,023,032 |           28,026.62 |


## Performance and code quality (11/2024)
Measurements done like the previous (07/2024).

- `dcc-0.7` built with `dcc-0.6`
- `dcc.0` built with `dcc-0.7 -O0`
- `dcc.1` built with `dcc-0.7 -O1`
- `dcc.2` built with `gcc-13.2.0 -O1 -fwrapv -fno-strict-aliasing -fno-delete-null-pointer-checks -fno-omit-frame-pointer`

| Compiler |         Cycles |   Instructions | Time [s] |  Obj Size | Comp. Size | Comp. Speed [LOC/s] |
|----------|---------------:|---------------:|----------|----------:|-----------:|--------------------:|
| dcc.2    |    879,359,441 |  1,320,932,448 | 0.27442  | 1,074,906 |    878,472 |          289,304.72 |
| dcc.1    |  1,168,743,273 |  1,763,527,317 | 0.36039  | 1,074,906 |    988,456 |          220,291.91 |
| dcc.0    |  1,394,913,162 |  2,120,576,066 | 0.42145  | 1,074,906 |  1,007,832 |          188,375.85 |
| gcc-13.2 | 10,586,317,041 | 16,270,923,387 | 3.19950  | 1,372,336 |  1,756,536 |           24,813.56 |

`dcc.1` is 14.49% faster than `dcc.0`, but 31.33% slower than `dcc.2`.


## Performance and code quality (07/2024)
Measurements done like the one below, except that this time the compilation
target is dcc-0.3 (a single TU of 79391 LOC).

- `dcc-0.6` built with `gcc-13` (this is not tested for performance)
- `dcc.0` built with `dcc-0.6`
- `dcc.1` built with `dcc.0 -O1`
- `dcc.2` built with `gcc -O1 -fwrapv -fno-strict-aliasing -fno-delete-null-pointer-checks -fno-omit-frame-pointer`

| Compiler |        Cycles |   Instructions | Time [s] |  Obj Size | Comp. Size | Comp. Speed [LOC/s] |
|----------|--------------:|---------------:|----------|----------:|-----------:|--------------------:|
| dcc.2    |   852,200,238 |  1,133,624,785 | 0.25804  | 1,078,796 |    765,360 |          307,717.05 |
| dcc.1    | 1,079,790,236 |  1,689,984,564 | 0.33383  | 1,078,796 |    859,560 |          237,697.60 |
| dcc.0    | 1,288,103,423 |  1,894,033,640 | 0.40143  | 1,078,796 |    932,336 |          197,770.47 |
| gcc-13   | 9,282,984,281 | 14,526,056,365 | 2.72147  | 1,372,336 |  1,018,768 |           29,172.10 |


## Performance and code quality (05/2024)
Measurements done on a 10 year-old laptop with an i7-6700HQ CPU compiling the
current version of the compiler (a single translation unit of 83865 LOC) to
object and by disabling all warnings.

Basically the command `cc -w -c dcc.c` has been run by `perf stat` for each compiler.

The compilers that have been considered are:
- `dcc.0` built with `gcc -O0`,
- `dcc.1` built with `dcc.0 -O0`,
- `dcc.2` built with `gcc -O1 -fwrapv -fno-strict-aliasing -fno-delete-null-pointer-checks -fno-omit-frame-pointer`,
- `dcc.3` built with `gcc -O3`,
- `gcc-9.5.0` and `gcc-13.2.0`.

| Compiler |         Cycles |   Instructions | Time [s] | Obj Size | Comp. Size |
|----------|---------------:|---------------:|----------|---------:|-----------:|
| dcc.3    |    847,050,051 |  1,186,707,162 | 0.26324  |  1103536 |    1076664 |
| dcc.2    |    944,942,698 |  1,448,801,591 | 0.28084  |  1103536 |     796408 |
| dcc.1    |  1,552,032,670 |  2,353,993,382 | 0.46962  |  1103536 |     896896 |
| dcc.0    |  1,646,074,155 |  2,499,780,895 | 0.49690  |  1103536 |    1155096 |
| gcc-9    |  8,973,659,906 | 12,843,867,566 | 2,63070  |  1424976 |            |
| gcc-13   | 10,350,651,622 | 16,458,362,668 | 3.01226  |  1425296 |            |

Compilation speed for `dcc.2`: 298621.99 LOC/s

`dcc.2` is 10.95 times faster than `gcc-13.2.0`,

`dcc.1` is 1.06 times faster than `dcc.0` with a size reduction of 258200 bytes.


## Testing
The compiler is tested against an internal testsuite currently composed by ~800 tests (~63K SLOC) and
by building open source projects.

[csmith](https://github.com/csmith-project/csmith) is also used, however at some
point the compiler becomes immune to it.


## Does compilation time matter? (11/10/2024)
Approximately a year ago I started measuring the time spent waiting for the compiler
for the debug build, so no optimizations enabled, and these are the results:

|                                    | gcc         | dcc         |
|------------------------------------|------------:|------------:|
| Total complete timings             | 14271       | 1855        |
| Total incomplete timings           | 8416        | 697         |
| Days between first and last timing | 561         | 74          |
| Slowest build time                 | 11.599s     | 1.423s      |
| Fastest build time                 | 1.679s      | 0.483s      |
| Average build time                 | 2.958s      | 0.546s      |
| Total time spent waiting           | 11h:43m:34s | 00h:16m:53s |

I've been using dcc (unoptimized, with assertions on) for building the next
version of dcc just for a couple of months, in the mean time I haven't used gcc
at all except for double checking, so I am no longer collecting data about gcc
performance. The average build time of dcc can be used to take a guess about the
potential time save.

	(14271*0.546s)/3600 = 2.16h => ~9 hours saved

Assuming that dcc becomes 2x slower

	(2*14271*0.546s)/3600 = 4.33h => ~6 hours saved
