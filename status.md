## C Preprocessor
Missing features:
- Named varargs (extension)
- `#line`
- C23 directives
- The ability to preserve comments

Supported pragmas:
- `dependency`
- `error`
- `message`
- `once`
- `pack`
- `poison`
- `system_header`
- `warning`

Supported operators:
- `__has_attribute`
- `__has_builtin`
- `__has_include`

Include guards are recognized when written as usual
```
#ifndef HEADER
#define HEADER
...
#endif
```


## C
Missing features:
- `_Atomic`, `stdatomic.h`
- struct/union definitions inside `sizeof/typeof/alignof`
- Universal character names `\u`, `\U`
- Flexible array member initialization

Features that won't be implemented:
- VLA
- Digraphs/Trigraphs
- `_Complex` and `_Imaginary`
- Old style function prototypes
- Unicode identifiers (unicode is allowed only inside strings and characters)


## C Extensions
- Statement expressions
- `typeof`
- Omitting the middle operand of `?:` expression
- Empty structs
- Variadic macros
- Arithmetic on `void *`
- Case ranges
- Inline assembly
- Zero initialization with `{}` as in C23
- Type inference with `__auto_type` as in C23
- Binary constants as in C23
- Anonymous struct/union members within structs/unions
- Incomplete enums

Extensions are allowed in any C version.

If you want to restrict your program to standard C you can pass `-Werror=pedantic`, or just `-Wpedantic`, with `-std=c89/99/11`.


## Built-in Functions
- `__builtin_FILE`
- `__builtin_FUNCTION`
- `__builtin_LINE`
- `__builtin_add_overflow`
- `__builtin_alloca`
- `__builtin_bswap16`
- `__builtin_bswap32`
- `__builtin_bswap64`
- `__builtin_choose_expr`
- `__builtin_clear_padding`
- `__builtin_clrsb`
- `__builtin_clz`
- `__builtin_constant_p`
- `__builtin_cpu_supports`
- `__builtin_ctz`
- `__builtin_expect`
- `__builtin_ffs`
- `__builtin_has_attribute`
- `__builtin_memcpy`
- `__builtin_memset`
- `__builtin_mul_overflow`
- `__builtin_nan`
- `__builtin_offsetof`
- `__builtin_parity`
- `__builtin_popcount`
- `__builtin_prefetch`
- `__builtin_signbit`
- `__builtin_strlen`
- `__builtin_sub_overflows`
- `__builtin_trap`
- `__builtin_types_compatible_p`
- `__builtin_unreachable`
- `__builtin_va_arg`
- `__builtin_va_copy`
- `__builtin_va_end`
- `__builtin_va_start`
- `__sync_add_and_fetch`
- `__sync_and_and_fetch`
- `__sync_bool_compare_and_swap`
- `__sync_fetch_and_add`
- `__sync_fetch_and_and`
- `__sync_fetch_and_nand`
- `__sync_fetch_and_or`
- `__sync_fetch_and_sub`
- `__sync_fetch_and_xor`
- `__sync_lock_release`
- `__sync_lock_test_and_set`
- `__sync_nand_and_fetch`
- `__sync_synchronize`
- `__sync_val_compare_and_swap`
- `__sync_xor_and_fetch`

New
- `int __builtin_same_type(expression-or-type, expression-or-type)`
  
   Return 1 if the types of the given expressions are exactly the same, 0 otherwise.
- `int __builtin_switch_strategy(compile-time-integer-expression)`

   Tell the compiler to use the specified switch lowering strategy for the following switch
   statements in the current function. Always return 0.


## Intel Intrinsics
Supported intrinsics:
- MMX
- SSE/SSE2/SSE3/SSEE3/SSE4.1/SSE4.2
- AES
- `_rdtsc`

All intrinsics are built-in functions, thus they can be used directly as specified by the
[Intel Intrinsics Guide](https://www.intel.com/content/www/us/en/docs/intrinsics-guide/index.html),
however you may still want to include the various `*intrin.h` headers for using symbolic constants
and for keeping compatibility with other compilers.

`__m64` and `__m128` are built-in types, while `__m128i` and `__m128d` are just typedefs to `__m128`.
The only operator supported is `=`, no implicit conversions occur.


## Attributes
- `aligned`
- `always_inline`
- `cleanup`
- `common`
- `constructor`
- `deprecated`
- `designated_init`
- `destructor`
- `fallthrough`
- `format`
- `nocommon`
- `noinline`
- `nonnull`
- `noreturn`
- `optimize`
- `packed`
- `transparent_union`
- `unavailable`
- `unused`
- `used`
- `visibility`
- `warn_unused_result`
- `weak`

New
- `file_scope`
- `overload`


## Inline Assembly
Available only for the x64 back-end and it currently lacks support for input operands and
constraints, which get ignored. It only accepts the intel syntax.


## Assembler
Missing features:
- AVX and beyond
- Error checking for invalid operands (the compiler will just assert and crash)
- Legacy instructions
