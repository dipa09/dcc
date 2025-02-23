# Changelog

## v0.8 - 09/02/2025

### Added
- C23
  - Add range checking for `enum : bool`.
- x86_64
  - Add `-maes`, `-mfma` command-line options.
  - Add FMA intrinsics.
  - Add `neg` instruction fusion for x64.
  - Add remaining SSE4.2 intrinsics.
  - Handle parameter passing of struct/union by value with `va_arg`.
- arm64
  - Add `constructor/destructor` attributes.
  - Add `__builtin_prefetch`, `__builtin_signbit`, `__builtin_trap`.
  - Add overflows builtins.
  - Add `__sync_*` builtins.
  - Add `__atomic_*` builtins.
  - Add TLS support.
- Assembler
  -  Add `.arch` directive for arm64.
  - `.globl/.global` now accept a list of symbols.
  - `.comm` now accepts an optional third argument for specifying the alignment.
- Add `-fspell-checking` for disabling spell checking in diagnostics.
- Add human readable aliases for some command line options.
- Add `--print-effective-triple` and `--print-multiarch` (they are equivalent).
- Add `-fstack-usage`.
- Add `-finstrument-functions` and `no_instrument_function` attribute.
- Add `-fzero-initialized-in-bss`.
- Add `-s`.
- Add `-nostdlib`, `-nostartfiles` and `-T` linker options.
- Add `-fdollars-in-identifiers`.
- Add `-msse2` for gcc compatiblity.
- Add UB Sanitizer
  - Add `-fsanitize=alignment`.
  - Add `-fsanitize=bool`.
  - Add `-fsanitize=bounds`.
  - Add `-fsanitize=enum`.
  - Add `-fsanitize=float-divide-by-zero` (no `long double`).
  - Add `-fsanitize=integer-divide-by-zero`.
  - Add `-fsanitize=leak`.
  - Add `-fsanitize=nonnull-attribute`.
  - Add `-fsanitize=null`.
  - Add `-fsanitize=pointer-overflow`.
  - Add `-fsanitize=shift-base`.
  - Add `-fsanitize=shift-exponent`.
  - Add `-fsanitize=signed-integer-overflow` (x64 only).
  - Add `-fsanitize=unreachable`.
  - Add `no_sanitize_undefined` attribute.
- Add `-Wuninitilaized`.


- Build stuff:
  [Verstable](https://github.com/JacksonAllan/Verstable),
  [bf-x86](https://github.com/skeeto/bf-x86),
  [brainc](https://github.com/detectivekaktus/brainc),
  [catimg](https://github.com/posva/catimg),
  [cccc](https://github.com/skeeto/cccc),
  [chess](https://github.com/weirddan455/chess),
  [clex](https://github.com/h2337/clex),
  [fcpp](https://github.com/bagder/fcpp),
  [mandel-simd](https://github.com/skeeto/mandel-simd),
  [optparse](https://github.com/skeeto/optparse),
  [pdjson](https://github.com/skeeto/pdjson),
  [psh](https://github.com/proh14/psh),
  [qoi](https://github.com/phoboslab/qoi),
  [quad-tree](https://github.com/leonmavr/quad-tree),
  [race64](https://github.com/skeeto/race64),
  [space-shooter.c](https://github.com/tsherif/space-shooter.c),
  [trie](https://github.com/skeeto/trie),


### Changed
- Align stack allocations bigger than 16 bytes to 16. Match gcc/clang behavior since it also makes sense.
- Register allocator improvements:
  - Select the right argument register for function pointers and forward-declared functions.
  - Add heuristic for intrinsics and fused instructions.
- Update `__builtin_memcpy/memset` codegen to operate on 32 bytes chunks when AVX is enabled.
- Limit TUs size to 4GB, to reduce compiler's memory footprint.


### Fixed
- Fix bogus warning with `-Wformat` and `+` modifier.
- Fix SysV ABI classification bug with odd struct layout, discovered through fuzzing.
- Apply C99 inline semantic.
- Fix preprocessor bug: correctly ignore unknown pragmas.
- Fix preprocessor bug: bogus replacement when `__VA_ARGS__` is omitted.
- Define missing constants in `<immintrin.h>`.
- Fix missing `-Wunused-function` for infinitely recursive functions.
- Fix bogus `-Wimplicit-fallthrough`.
- Fix `-Wparentheses` bug.
- Fix `-Wundefined-internal`: warn about all undefined static functions.


### Removed
- Remove `-fcheck-recover` and `-fcheck-signed-overflow`, in favor of the well-known `-fsanitize`.


## v0.7 - 10/11/2024

### Changed
- Disable `-Wunused-function` with `-std=c89/90`, since it would cause bogus warnings
  with `math.h`, because libc vendors use `__inline` in _standard_ headers.
- Parse qualifiers and storage class after tag definition.
  e.g.
  ```c
	  struct s0 { int x; } static;           // OK, but deprecated since C23
	  struct s1 { int y; } const;            // 'const' is ignored, emit warning
	  struct s2 { int z; } const typedef s2; // Error
  ```
  Other compilers accept the third one, but they seem to be confused about it as well.
- Collapse `-Wattributes` and `-Wignore-attributes` into one option.
- Enable `-Wtautological-overlap-compare` with `-Wall`.
- More precise diagnostic message for mismatching argument count with variadic functions.
- Use SSE4.1 instructions for `_mm_set*` when `-msse4.1` is set.
- Disable `-gcolumn-info` by default, since gdb ignores it anyway.
- Enable SSE version of `neg` IR instruction by default.
- Mitigate UB on x64 for `__builtin_ctz(0)`, by encoding `bsf` with the `rep` prefix.
- Remove functions called only by unused functions (enabled by default).


### Added
- Add universal charater names for strings and characters.

- Add partial C23 support with the `-std=c23` switch:
  - Add keywords `alignas`, `alingof`, `static_assert`, `thread_local`, `typeof_unqual`, `constexpr`.
  - Add `__has_c_attribute`.
  - Parse the new attribute syntax `[[]]`.
  - Predefine standard macros: `__STDC_UTF_16__`, `__STDC_UTF_32__`.
  - Add digit separators.
  - Add predefined constants: `true`, `false` and `nullptr`.
  - `foo()` becomes equivalent to `foo(void)`.
  - Add type inference with `auto`.
	Missing bit-fields initializers and traditional mode e.g. `auto int x` will be (incorrectly)
	treated as an error.
  - Parse declarations after labels.
  - Add standard attributes: `unsequenced`, `reproducible`, `nodiscard`, `deprecated`, `maybe_unused`,
	`noreturn`, `fallthrough`.
  - Add enum type specifier.

- Add `-Wextern-initializer`, enabled by default.
- Add `-Wimplicit-fallthrough`.
- Warn about assignments to read-only locations. e.g. `"foo"[x] = 'a';`.

- Add attribute `nodebug`.
- Add inline codegen for `__builtin_memset` also for non zero initialization values.
- Add debug-info for bitfields.
- Add `__builtin_memcmp` with inline simd codegen, when the size is known at compile time.
- Add argument checking for intrinsics requiring a constant integer argument within a certain range.

- Start riscv64 backend.
  - Add `-march=rv64g` option.
  - Predefine macros: `__riscv`, ... to complete

- arm64 backend
  - Add codegen for `__builtin_bswap/ctz/clz/clrsb/parity/ffs/popcnt`.
  - Handle non-zero clear byte for `__builtin_memset`.
  - Encode bitmask (logical) immediates.

- x64 backned:
  - Add all AVX/AVX2 intrinsics, except for: `_mm_broadcastb_epi*`, `_mm256_broadcastb_epi*` which
	require EVEX encoding.
  - Add BMI1, MOVBE, `_rdtscp`, RDRAND, RDSEED, RDPID intrinsics.
  - Predefine `__BMI__`, `__BMI2__`, `__MOVBE__`, `__RDSEED__`, `__RDPID__`, `__RDRND__`, `__AVX__`, `__AVX2__`.
  - Add `-mbmi`, `-mbmi2`, `-mrdrnd`, `-mrdseed`, `-mrdpid`, `-mmovbe`, `-mavx`, `-mavx2` command-line options.


- Build more stuff:
  [8080](https://github.com/superzazu/8080),
  [BearSSL](https://github.com/OUIsolutions/BearSslSingle-Unit),
  [libcox](https://github.com/symisc/libcox),
  [luigi](https://github.com/nakst/luigi),
  [mg](https://github.com/ibara/mg),
  [oed](https://github.com/ibara/oed),
  [parson](https://github.com/kgabis/parson),
  [pl0c](https://github.com/ibara/pl0c),
  [qbe](https://c9x.me/compile/),
  [utf8.h](https://github.com/sheredom/utf8.h),
  [vce](https://github.com/ibara/vce),
  [z80](https://github.com/superzazu/z80).


### Removed
- Remove `-Wcast-function-type`.


### Fixed
- Fix segfault caused by recursive enum valuation in compile-time `__builtin_overflow_*`,
  caused by `typeof`.
  Now it's possible to write:
  ```c
  #define __builtin_add_overflow_p(a,b) __builtin_add_overflow(a, b, (typeof((a) + (b))*)0)
  enum {
	  A = 0x7FFFFFFF,
	  B = 3,
	  C = __builtin_add_overflow_p(A, B) ? 0 : A + B,
  };
  ```
- Fix incorrect `sizeof` parsing with un-parenthesized member expressions, like:
  `sizeof ((struct foo *)0)->bar`.
- Fix multi-byte escape sequences for UTF16/32 strings/characters.
- Fix undiagnosed `-Wdangling-else`.
- Fix bug with `-Wtautological-overlap-compare`.
- Fix debug-info issues
  - Inspect `static` local variables.
  - Step on multi-line conditional expressions.
  - Inspect anonymous enums.
- Fix bitfield related bugs.
- Fix a bug with `-Wpointer-to-int-cast`.


## v0.6 - 28/07/2024

### Changed
- Do not emit unused `static` functions by default.
  This reduces compile-time and if the user want to run a function in the
  debugger it can always mark it with the `used` attribute.
- Always reduce unsigned divides by a power of two.
- Rename `-march=host` into `-march=native` for compatiblity with GCC.
- Enable `-Wreturn-type` by default.
- Temporarily disable `-Wsequence-point`, since it's giving false positive on real software.
- Incorporate `-Woverlength-strings` into `-Wpedantic`.
- Merge `-m` and `-march` into a single option.
- Emit `movdqa` instead of `movaps` for memory ops, when the vector subtype is an integer type.
- Improve diagnostic for argument count mismatch errors.
  Use the first argument position having incompatible type when there is one, otherwise use
  the function call '(' position like before.
- Accept storage class after tag type specifiers in declarations/definitions.
- Disable `-Wmisleading-indentation`, since it sometimes gives false positives.


### Added
- Add attributes: `noinline`, `always_inline`, `hot`, `cold` and `noipa`.
- Parse attributes after pointer types. e.g. `void *__attribute__((noinline)) foo();`
- Add warnings: `-Wstack-usage`, `-Wcompare-distinct-pointer-types`, `-Winline`,
  `-Wunused-macros=main`, `-Wunused-macros=nosys`, `-Wunused-macros=all`,
  `-Wenum-int-mismatch`, `-Wtautological-overlap-compare`, `-Winvalid-memory-model`,
  `-Wundefined-internal`.
- Add options: `-pthread`, `-foptim-report`, `-finline`, `-msse3`, `-mssse3`, `-msse4`,
  `-msse4.1`, `-msse4.2`.
- Add builtins: `__atomic_*`, `__builtin_frame_address`, `__builtin_return_address`, `__builtin_stack_address`.
- Implement `__builtin_unreachable` for `-O1` and `-O0`.
- Add optimization passes:
  - Register promotion.
  - Dead code elimination.
  - Constant propagation.
  - Common sub-expression elimination.
  - Strength reduction.
  - Inlining.
  - Dead argument elimination.
  - Various form of CFG reductions.
- Omit frame prolog/epilog for leaf functions with -O1, when possible.
- Restore SWITCH reduction to range change.
- Add SEL reduction for power of 2.
  e.g. `if (cond) flags |= The_Flag` => `flags |= (cond << bit(The_Flag))`
- Build more open-source projects:
  [chibicc](https://github.com/rui314/chibicc),
  [cproc](https://sr.ht/~mcf/cproc),
  [gc](https://github.com/mkirchner/gc),
  [gennan](https://github.com/codeplea/genann),
  [quadsort](https://github.com/scandum/quadsort),
  [renderer](https://github.com/zauonlok/renderer),
  [rpng](https://github.com/raysan5/rpng),
  [siod](https://github.com/deriito/siod-v3.0),
  [sqlite3](https://github.com/sqlite/sqlite),
  [tinn](https://github.com/glouw/tinn).


### Removed
- Remove `-save-temps`.
- Remove `-Wformat-security`.
- Remove old debug-info code for recording type qualifiers.
  Pros:
  - they are not needed to actually debug an application,
  - they make the executable bigger,
  - implementation-wise they are akward,
  - most people probably don't care.
  Cons:
  - One might want to inspect debug-info for getting AST info, but again we
	provide better options for that.
- Remove `file_scope` attribute. Now it's a custom attribute.


### Fixed
- Fix warnings: `-Wignored-qualifiers`, `-Wmissing-prototypes`, `-Wreturn-local-addr`,
  `-Wshift-negative-value`, `-Wtype-limits`, `-Wenum-conversion`, `-Wtautological-compare`.
- Fix linking bug caused by missing some CRT objects.
- Fix self-hosting regressions.
- Fix edge case bugs in initializers.


## v0.5 - 01/04/2024

### Changed
- Show function definition on calls with mismatch argument count.
- `-Wpedantic` with `-std=c89` is best effort, it won't catch all later features uses.
  Most people have move on to later standards, so it doesn't make sense to add
  complexity for this.
- Display helper message for invalid multi-dimensional arrays only once,
  since it's always the same.
- Change `__FUNCTION__` behavior like MSVC.
- Compile as C11 by default.
- Change fatal exit code from 130 to 1.
- Make operator overloading ignore typedefs.


### Added
- Add spellchecking for undeclared members.
- Add `-Wunused-but-set-variable`.
- Add basic support for inline assembly, just templates, outputs and clobbers.
- Add basic cross-compilation support for arm64.
- Add `-Wunused-result` and `warn_unused_result` attribute.
- Add `-Wunused-parameter`.
- Add warning for extern variables with initializer.
- Add UTF8/16 string and character literals.
- Add `#pragma pack`.
- Add `#pragma message`.
- Add `#pragma warning`.
- Add `#pragma error`.
- Add `#pragma dependency`.
- Add `#elifdef/elifndef`.
- Add `-Wunused-macros`.
- Show macro expansion in error messages.
- Generate code for run-time `__builtin_offsetof`.
- Spellcheck undeclared identifiers.
- Spellcheck invalid cpp directives.


### Removed
- Do not support VLA, always treat them as errors.
- Remove `-Wmultichar`. Treat it as an error, since it is implementation defined.
- Remove [cast to union](https://gcc.gnu.org/onlinedocs/gcc/Cast-to-Union.html) extension.
- Remove `-fsigned-char`.
- Remove `-fauto-typedef`. Implement it as a plugin.


### Fixed
- Restore `-Wbad-function-cast`.
- Properly treat `_Generic` expression as lvalue when the selected expression is indeed an lvalue.
- Fix bugs related to lvalue-to-rvalue conversion.


## v0.4 - 30/12/2023

### Changed
- Rename `-fzii` into `-ftrivial-auto-var-init` to be compatible with GCC and
  add support for pattern initialization.
- Fuse load followed by arithmetic instruction. Enable this by default, since
  the compiler's performance didn't resent much.
- Do not allow `extern void` variables. For some reason GCC and CLANG allow them, while MSVC doesn't.
- Set max array size to 4GB. MSVC limit is 2GB, CLANG limit is much bigger
  (thousand of TB) and GCC even larger (`__SIZE_MAX__/2` or something).
  In practice the max stack size is around 10-30MB and allocating arrays that big is unusual.
  Maybe we should add a flag (-fstack-limit maybe?) to allow huge arrays, since they might be
  used for debugging purposes.
- Disable `__label__` parsing.
- Do not allow unicode identifiers, since it adds complexity for an arguably useless
  feature (i.e. code is written in english).

  It's also a source of security [vulnerabilities](http://www.unicode.org/reports/tr55/).


### Added
- Add `-Wreturn-type`
- Add `-Wconversion`
- Emit debug info for SIMD types.
- Add `__builtin_unreachable`
- Predefine conditional feature macros:
  - `__STDC_NO_ATOMICS__`,
  - `__STDC_NO_COMPLEX__`,
  - `__STDC_NO_THREADS__`,
  - `__STDC_NO_VLA__`.
- Add consecutive immediate stores fusion optimization.
- Add `-x` option to select the language for input files.
- Add `-fswitch-strategy` and `__builtin_switch_strategy` for selecting the switch
  lowering method.
- Add `__builtin_memset_strategy` and `__builtin_memcpy_strategy`.
- Plugin API for extending the compiler:
  - AST inspection,
  - Custom attributes and builtins,
  - Callbacks
- Add `optimize` attribute.
- Support consencutive label definitions in the assembler.
- Cleanup operator overloading attribute: more operands, symmetric property,
  better error checking.
- Encode `clflush`, `clflushopt`, `cldemote`, `clrssbsy`, `bt`, `btc`, `btr` and `bts`.
- Add signed integer overflow run-time checking with `-fcheck-signed-overflow`.
- Improve code generation for `_mm_set_*` intrinsics with homogeneous arguments,
  using SSE3/SSSE3 intrinsics when available.
- `__builtin_memset/memcpy` are now considered intrinsics.
- Add `__builtin_trap`.
- Add overflow builtins. Do not add the _p variants since they are redundant.
- Add uninitialized attribute for `-fzii`.
- Add `-Wvarargs`.
- Allow the user to specify built-in functions through `-fplugin`.
- Read string literals and character constants in utf8 and convert them in the
  requested encoding. Wide string literals and character constants are encoded in utf32.


### Removed
- Remove the additional helper message for unterminated condition, since it cause more confusion.
- Remove RTTI. The major drawback is additional work in the compiler, plus you can't
  debug the generated code. In contrast the same feature could be implemented through
  a meta-programming preprocessing pass using the pluing API.
- Remove `-Wlong-long`, since it's also enabled by `-Wpedantic` and nobody care about `long long` anyway.


### Fixed
- Fix `-Wsign-compare`
- Fix `-Waddress`
- Restore `-Wnonnull`, `-Wnonnull-compare`, `-Wunused-function`, `-Wunused-label`, `-Wunused-variable`,
  `-Wsizeof-pointer-memaccess`.
- Restore `__builtin_nan`
- Restore `__builtin_cpu_supports`.
- Restore `-Woverflow`, `-Wbool-operation`, `-Wchar-subscripts`, `-Wint-in-bool-context`, `-Wcomment`,
  `-Wdouble-promotion`, `-Wtautological-compare`, `-Wsizeof-array-argument`.
- Restore type inference with `__auto_type` or with `auto` in C23.


## v0.3 - 01/10/2023

### Added
- Internal assembler for x86_64
- Bit-fields.
- Thread-local storage.


### Fixed
- Fix x87 register allocation.
