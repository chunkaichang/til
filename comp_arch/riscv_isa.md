# RISC-V ISA

## Instructions

- NOP is explicitly encoded as `ADDI x0, x0, 0`. This allows microarchitectural optimizations, reads only one register (x0), and can be served by multiple functional units (INT and AGU).

- Conditional branches do not use condition codes (i.e., fused compare-and-branch), leading to similar benefits of macro-up fusion in x86 without hardware support. Also, branches do not support static branch hints.

- No conditional moves or predicated instructions. The spec says both can complicate the implementation of out-of-order execution (e.g., need to copy value in an architectural register to a physical register if predicate is false).

- System calls and debugging are supported by the SYSTEM instructions. 

- HINT instructions allow sending performance hints to the microarchitecture. They are encoded as integer computational instructions with destination register equal `x0` (always zero). However, currently there are no standard hints.

## Memory

- Endianess is platform-dependent. The spec says some domains (e.g., networking devices) use big-endian.

- Misaligned accesses are allowed (depending on the execution environment) and can be handled in software. However, misaligned loads and stores do not guarantee atomicity.

- Weak memory consistency model is the default and seperates out I/O ordering from memory R/W ordering for performance. There is an extension for total store ordering.
