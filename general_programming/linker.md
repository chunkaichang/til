# Understanding symbols and linking process

## Reading
- [Beginner's Guide to Linkers](https://www.lurklurk.org/linkers/linkers.html) by David Drysdale

### Terminology
- Object files: the machine code compiled from a source file (`.o` files in Linux)
- Static libraries: library code bundled statically with an executable (`.a` files in Linux)
- Shared libraries: library code brought in right before `main` is executed (`.so` files in Linux)

Link order matters for static libraries. See [this](https://www.lurklurk.org/linkers/linkers.html#staticlibs) and [this](https://eli.thegreenplace.net/2013/07/09/library-order-in-static-linking).

Shared libraries allow (1) code sharing through paging and (2) updates without recompilation.

## Debugging link-time errors (UNIX)

- `nm`: shows symbols in an object file along with their types. Also useful for demangling C++ functions.
- `ldd`: lists shared libraries that an executable depends on. The dynamic linker will look for these libraries in `LD_LIBRARY_PATH`.
