# Changelog

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

- Add warnings:
  - `-Wstack-usage`,
  - `-Wcompare-distinct-pointer-types`,
  - `-Winline`,
  - `-Wunused-macros=main`,
  - `-Wunused-macros=nosys`,
  - `-Wunused-macros=all`,
  - `-Wenum-int-mismatch`,
  - `-Wtautological-overlap-compare`,
  - `-Winvalid-memory-model`,
  - `-Wundefined-internal`.

- Add options:
  - `-pthread`,
  - `-foptim-report`,
  - `-finline`,
  - `-msse3`,
  - `-mssse3`,
  - `-msse4`,
  - `-msse4.1`,
	`-msse4.2`.

- Add builtins:
  - `__atomic_add_fetch`
  - `__atomic_and_fetch`,
  - `__atomic_clear`,
  - `__atomic_compare_exchange_n`,
  - `__atomic_compare_exchange`,
  - `__atomic_exchange_n`,
  - `__atomic_exchange`,
  - `__atomic_fetch_add`,
  - `__atomic_fetch_and`,
  - `__atomic_fetch_nand`,
  - `__atomic_fetch_or`,
  - `__atomic_fetch_sub`,
  - `__atomic_fetch_xor`,
  - `__atomic_is_lock_free`,
  - `__atomic_load_n`,
  - `__atomic_load`,
  - `__atomic_nand_fetch`,
  - `__atomic_or_fetch`,
  - `__atomic_signal_fence`,
  - `__atomic_store_n`,
  - `__atomic_store`,
  - `__atomic_sub_fetch`,
  - `__atomic_test_and_set`,
  - `__atomic_thread_fence`,
  - `__atomic_xor_fetch`,
  - `__builtin_frame_address`,
  - `__builtin_return_address`,
  - `__builtin_stack_address`.

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
  - [chibicc](https://github.com/rui314/chibicc),
  - [cproc](https://sr.ht/~mcf/cproc),
  - [gc](https://github.com/mkirchner/gc),
  - [gennan](https://github.com/codeplea/genann),
  - [quadsort](https://github.com/scandum/quadsort),
  - [renderer](https://github.com/zauonlok/renderer),
  - [rpng](https://github.com/raysan5/rpng),
  - [siod](https://github.com/deriito/siod-v3.0),
  - [sqlite3](https://github.com/sqlite/sqlite),
  - [tinn](https://github.com/glouw/tinn).


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
- Fix warnings:
  - `-Wignored-qualifiers`,
  - `-Wmissing-prototypes`,
  - `-Wreturn-local-addr`,
  - `-Wshift-negative-value`,
  - `-Wtype-limits`,
  - `-Wenum-conversion`,
  - `-Wtautological-compare`.

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
