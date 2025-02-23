
## Summary
| Feature               | `x86_64`   | `aarch64`  | `riscv64` |
|-----------------------|------------|------------|-----------|
| Basic instruction set | OK         | OK         | No        |
| Atomics               | OK         | OK         | No        |
| Vector extensions     | AVX2       | No         | No        |
| Overflow builtins     | OK         | OK         | No        |
| Builtin assembler     | OK         | No         | No        |
| TLS                   | OK         | OK         | No        |
| PIC                   | Partial    | No         | No        |
| `long double`         | OKish      | Incomplete | No        |
| Addressing modes      | OK         | Partial    | No        |
| Optimizations         | Incomplete | Incomplete | No        |
| Direct compilation    | Partial    | No         | No        |
| Test coverage         | 96.361%    | 94.969%    | 0%        |
| ABI                   | OK         | OK         | No        |
| Debug build           | OK         | Partial    | No        |
| Inline assembly       | Partial    | No         | No        |
| Sanitizers            | Partial    | Partial    | No        |


## Categories' description
- Basic instruction set
  
  Whether the basic instruction set (integer/float ops, cmps, branching,
  basically the essential stuff needed to write a program) is implemented.
- Atomics
  
  Whether the `__builtin_sync*`, `__builtin_atomic_*` intrinsics and `_Atomic` are
  implemented and tested.
- Vector extension
  
  Whether the vector instructions (SIMD) can be used.
- Overflow builtins
  
  Whether `__builtin_*_overflow` are implemented and tested.
- Builtin assembler
  
  Whether the internal assembler support the given architecture
- TLS
  
  Whether C11 Thread Local Storage support is available.
- PIC
  
  Whether Position Independent Code generation is available.
- `long double`
  
  Whether `long double` codegen is available.
- Addressing modes
  
  Whether addressing modes are used effectively.
- Optimizations
  
  Whether compiling with `-O1` is as reliable as `-O0`.
- Direct compilation
  
  Whether the direct compilation pipeline have been implemented for the target.
  
  `C -> IR -> OBJ` as opposed to `C -> IR -> ASM -> OBJ`.
- Test coverage
  
  The ratio of successful codegen tests.
- ABI
  
  Whether the ABI have been implemented.
- Debug build
  
  Whether compiling with `-O0 -g` is reliable.
- Inline assembly
  
  Whether inline assembly support is present.


TODO: Expand each category for all archs
## x86_64
### Inline assembly
- No input operands
- No constraints
- No AT&T syntax (the weird one)

### Assembler
Missing features:
- Error checking for invalid operands (the compiler will just assert and crash)
- Legacy instructions

## aarch64


## riscv64
You can write programs like hello world. What else do you even need?


## Compiler status
| Host\Target | `x86_64` | `aarch64` | `riscv64` |
|-------------|----------|-----------|-----------|
| `x86_64`    | hw       | qemu      | qemu      |
| `aarch64`   | no       | no        | no        |
| `riscv64`   | no       | no        | no        |
