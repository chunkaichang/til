# x86-64 assembly

> All assembly code uses AT&T syntax (i.e., the last argument is the destination).

## Opcode suffixes

- `movslq %eax, %rdx`: move `%eax` to `%rdx` with sign extension (note the `s` suffix)
- `movzbl %al, %edx`: move `%al` to `%edx` with zero extension

## Vector instructions

### SSE
Uses XMMs (128-bit) (low 128 bits of the corresponding YMM).

- `addss ...`: add two single-precision floating-point scalars
- `addsd ...`: add two double-precision floating-point scalars
- `addps ...`: add two single-precision floating-point vectors (packed)
- `addpd ...`: add two double-precision floating-point vectors (packed)
- `paddq ...`: add two 64-bit (quad-word) integers in parallel

### AVX
Uses YMMs (256-bit)

Instructions are similar to SSE but prefixed with `v`.

## Idioms

```
# Jump if %rcx is zero
# test does bitwise AND and discard the result (but RFLAGS is updated)
test %rcx, %rcx
je 400c0a <mm+0xda>
```

```
# Conditional move if %rax is non-zero
test %rax, %rax
cmovne %rax, %r8
```

```
# no-ops (for alignment)
data16 data16 data16 nopw %cs:0x0(%rax, %rax, 1)
```

## Linux calling convention

- Callee-saved registers: `rbx`, `rbp, `r12`-`r15`
- Caller-saved registers: all other registers 
- Return value: `rax`
- Function arguments (in order): `rdi`, `rsi`, `rdx`, `rcx`, `r8`, `r9`

---
## References
[MIT 6.172](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-172-performance-engineering-of-software-systems-fall-2018/index.htm)
