# Type Checking in Python3

Type checking is done offline using tools such `mypy`. It has no impact at runtime albeit some overhead at startup time.

## The `typing` module
- The `typing` module defines types for annotating composite types (e.g, `List[Tuple[str, str]]`). 
- For variable-length tuples of the same type T, use `Tuple[T, ...]`
- For code readability, type aliases should be used for long types (e.g., `Deck = List[Tuple[str, str]]`)

### Annotating functions
- `Sequence` represents lists, tuples, and other classes with `__len__()` and `__getitem__()` implemented
- Use `TypeVar` to restrict the set of acceptable types
- Use `Optional` for optional arguments and optional function return
- For `*args` and `**kwargs`, just annotate the underlying possible types (e.g., for `*args` of strings, annotate with `str` instead of `Tuple[str]`)

### Annotating Classes
- There is no need to annotate the `self` and `cls` arguments
- Use string literals for self reference or future reference to other classes.
> Python3.7 and later can directly use class names with `from __future__ import annotations`
- For methods that returns self or cls, use `TypeVar` to specify the base class

## Integration with third-party packages in Mypy
- Type hints for third-party packages can be added via stub files (`*.pyi`). 
- Stub files should be stored in a directory referenced by the `MYPYPATH` env variable

---
## References
- [Python type checking](https://realpython.com/python-type-checking)
- [Awesome python typing](https://github.com/typeddjango/awesome-python-typing)
