dcc is an optimizing compiler for the C language, targetting x86_64 and aarch64 (arm64).

It currently runs on Linux.


## [Features](https://github.com/dipa09/dcc/blob/main/status.md)
- Built-in C preprocessor.
- C11.
- Good diagnostic messages.
- Mostly compatible with gcc.
- Several common extensions.
- Intel intrinsics up to SSE4.2.
- x86-64 back-end.
- Built-in assembler.
- Debug info in DWARF4 format.
- ~10 times faster than gcc.
- Plugin API for accessing the AST and extending the compiler.


## Status
| Target              | Status     | Note                          |
|---------------------|------------|-------------------------------|
| `x86_64-linux-gnu`  | alpha      | `-O0 -g` OK, `-O1` incomplete |
| `aarch64-linux-gnu` | incomplete |                               |

[Demo (06/05/2024)](https://www.youtube.com/watch?v=TPWxtAFwiks)

[Changelog](https://github.com/dipa09/dcc/blob/main/CHANGELOG.md)


Successful builds:
[Meow hash](https://github.com/cmuratori/meow_hash),
[Nuklear](https://github.com/Immediate-Mode-UI/Nuklear),
[TermGL](https://github.com/wojciech-graj/TermGL),
[chibicc](https://github.com/rui314/chibicc),
[cproc](https://sr.ht/~mcf/cproc),
[gc](https://github.com/mkirchner/gc),
[genann](https://github.com/codeplea/genann),
[lemon](https://compiler-dept.github.io/lemon),
[minilua](https://github.com/edubart/minilua),
[minivorbis](https://github.com/edubart/minivorbis),
[miniz-3.0.2](https://github.com/richgel999/miniz),
[q3vm](https://github.com/jnz/q3vm),
[quadsort](https://github.com/scandum/quadsort),
[renderer](https://github.com/zauonlok/renderer),
[rpng](https://github.com/raysan5/rpng),
[siod](https://github.com/deriito/siod-v3.0),
[sqlite](https://github.com/sqlite/sqlite),
[stb](https://github.com/nothings/stb/),
[tinn](https://github.com/glouw/tinn),
[treecc](https://github.com/rweather/treecc).


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
The compiler is tested against an internal testsuite currently composed by ~700 tests (~48K SLOC) and
by building open source projects.

[csmith](https://github.com/csmith-project/csmith) is also used, however at some point the compiler becomes immune to it.


## Contact
For questions about the project you can ask at davide.dipaolo09@gmail.com
