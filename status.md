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


## Options
Command line options are organized by category.

```
$ dcc -help
Usage: dcc [options] file...
  -help=all                   Show everything
  -help=common                Show all common options
  -help=diagnostic            Show diagnostics and UI options
  -help=warning               Show warning options
  -help=optimization          Show optimization options
  -help=language              Show options related to the C language specifications
  -help=preprocess            Show preprocessor's options
  -help=developer             Show developer options
  -help=codegen               Show code generation options
  -help=instrument            Show instrumentation options
  -help=machine               Show target machine options
  -help=debug                 Show debugging options
  -help=link                  Show linker options
  -help=directory             Show directory options

Note:
  You can specify or disable several option categories
    -help=warning,developer     will display warnings and developer options, while
    -help=^warning              will display all non-warning options
```

List of options excluding warnings and developer options.
```
$ dcc -help=^warning,^developer
Usage: dcc [options] file...
Options:
  -D<name=definition>           Define <macro> to <value> (or 1 if <value> omitted)
  -E                            Only run the preprocessor
  -H                            Show header includes and nesting depth
  -I <dir>                      Add directory to SYSTEM and QUOTE include search path
  -M                            Output dependencies as rules suitable for 'make'
  -MM                           The same as -M, but ignore system headers
  -O<level>                     Set optimization level
  -S                            Stop after compilation stage, do not run the assembler
  -U<name>                      Undefine macro <macro>
  -X                            Pass the following options directly to the linker until --
  -ansi                         Equivalent to -std=c90
  -c                            Stop after assembling stage, do not run the linker
  -g                            Generate debug information in the default format
  -help=[category],...          Display this message on stdout
  -imacros <file>               Include macros from file before parsing
  -include <file>               Include <file> before parsing
  -iquote <directory>           Add directory to QUOTE include search path
  -isystem <directory>          Add directory to SYSTEM include search path
  -l<library>                   Search 'library' when linking
  -nostdinc                     Do not search the standard system directories for header files
  -o <file>                     Specify the primary output destination file
  -pthread                      Link with POSIX threads library
  -std=<version>                Select the language standard (C11 with extensions by default)
  -undef                        Undefine all system defines
  -v                            Print driver's commands
  -version                      Print the version and quit
  -w                            Inhibit all warning messages
  -x <language>                 Specify the input source language
  -fasm                         Recognize 'asm', 'inline' and 'typeof' as keywords (enabled by default)
  -fcheck-recover=[recovery]    Select the recovery mode for run-time checks (default is abort)
  -fcheck-signed-overflow       Enable run-time signed integer overflow checking
  -fcommon                      Put uninitialized globals in the common section
  -fdiagnostics-color           Colorize diagnostics
  -fdiagnostics-lines=<number>  Set how many source's lines print after the diagnostic (3 by default)
  -fdiagnostics-show-option     Amend appropriate diagnostic messages with the command line option that controls them
  -fdirectives-only             Do not expand macros during preprocessing, except __FILE__ and __LINE__
  -fhosted                      Assume hosted environment (enabled by default)
  -fmax-errors=<number>         Maximum number of errors to report
  -fplugin=<name.so>            Load plugin code
  -fplugin-arg                  Pass the followin options directly to the last plugin until -- is encountered
  -fpreprocessed                Consider the input files already preprocessed
  -fsyntax-only                 Check the code for syntax errors
  -ftabstop=<width>             Select how long a tab is (8 by default)
  -ftrack-macro-expansion       Show macro definition in diagnostics related to macros
  -ftrivial-auto-var-init       Automatically initialize non-initialized variables to zero
  -fuse-ld=<name>               Specify the linker to use
  -march=<cpu-type>             Generate code for the specified machine type
  -mred-zone                    Use the so-called red zone for x86-64 code
  -gcolumn-info                 Emit location column info into DWARF debugging information
  -gdwarf                       Generate debug information in default version of DWARF format
  -gstrict-dwarf                Disallow extensions of the selcted DWARF format
```

## Warnings
List of warnings currently implemented.
```
  -Wabsolute-value
  -Waddress
  -Waddress-of-packed-member
  -Waggregate-return
  -Wall
  -Warray-bounds
  -Wattributes
  -Wbad-function-cast
  -Wbool-compare
  -Wbool-operation
  -Wbuiltin-declaration-mismatch
  -Wbuiltin-macro-redefined
  -Wcast-align
  -Wcast-function-type
  -Wcast-qual
  -Wchar-subscripts
  -Wcomment
  -Wconversion
  -Wdangling-else
  -Wdeclaration-after-statement
  -Wdeprecated-declarations
  -Wdesignated-init
  -Wdiscard-qualifiers
  -Wdivision-by-zero
  -Wdo-while
  -Wdouble-promotion
  -Wduplicate-decl-specifier
  -Wduplicated-branches
  -Wduplicated-cond
  -Wempty-body
  -Wenum-compare
  -Wenum-conversion
  -Werror=<warning>
  -Weverything
  -Wextra
  -Wextra-tokens
  -Wflexible-array-array
  -Wflexible-array-nested
  -Wflexible-array-sizeof
  -Wfloat-equal
  -Wformat
  -Wformat-contains-nul
  -Wformat-extra-args
  -Wformat-nonliteral
  -Wformat-signedness
  -Wformat-zero-length
  -Wfree-nonheap-object
  -Wignored-attributes
  -Wignored-qualifiers
  -Wimplicit
  -Wimplicit-function-declaration
  -Wimplicit-int
  -Wincompatible-pointer-types
  -Wint-conversion
  -Wint-in-bool-context
  -Wint-to-pointer-cast
  -Winvalid-memory-model
  -Wlarger-than=<bytes>
  -Wlogical-not-parentheses
  -Wlogical-op
  -Wmacro-redefined
  -Wmain
  -Wmemset-elt-size
  -Wmemset-transposed-args
  -Wmisleading-indentation
  -Wmismatched-dealloc
  -Wmissing-declaration
  -Wmissing-field-initializers
  -Wmissing-prototypes
  -Wnested-externs
  -Wnon-pointer-null
  -Wnonnull
  -Wnonnull-compare
  -Wold-style-declaration
  -Woverflow
  -Woverride-init
  -Wpacked-not-aligned
  -Wpadded
  -Wparentheses
  -Wpedantic
  -Wpointer-arith
  -Wpointer-compare
  -Wpointer-sign
  -Wpointer-to-int-cast
  -Wpointer-type-mismatch
  -Wpragmas
  -Wprio-ctor-dtor
  -Wptr-subtraction-blows
  -Wredundant-decls
  -Wreturn-local-addr
  -Wreturn-type
  -Wsequence-point
  -Wshadow
  -Wshift-count-negative
  -Wshift-count-overflow
  -Wshift-negative-value
  -Wshift-overflow
  -Wsign-compare
  -Wsizeof-array-argument
  -Wsizeof-array-div
  -Wsizeof-bool
  -Wsizeof-pointer-div
  -Wsizeof-pointer-memaccess
  -Wstack-usage=bytes
  -Wstrict-prototypes
  -Wswitch
  -Wswitch-bool
  -Wswitch-default
  -Wswitch-enum
  -Wswitch-outside-range
  -Wswitch-unreachable
  -Wtautological-compare
  -Wtype-limits
  -Wtypedef-redefinition
  -Wundef
  -Wuninitialized
  -Wunknown-pragmas
  -Wunreachable-code
  -Wunused
  -Wunused-but-set-variable
  -Wunused-const-variable
  -Wunused-function
  -Wunused-label
  -Wunused-local-typedefs
  -Wunused-macros
  -Wunused-parameter
  -Wunused-result
  -Wunused-value
  -Wunused-variable
  -Wvarargs
  -Wvolatile-register-var
  -Wwrite-strings
  -Wzero-length-bounds
```
