# Decorators

The well-known `@` symbol is just a syntactic sugar for decorators. Decorators are based on the following concepts:
- Functions are first-class objects
- Returning a reference of an inner function

## On writing decorators
1. For supporting generic functions, the inner function should take `*args, **kwargs` and return the result of the wrapped function
2. For preserving the decorated functions' name and docstring, the inner functions should be decorated with `functools.wraps`.

## Applications
- Profiling
- Debugging (e.g., printing signatures)
- Rate-limiting
- Plugin registration (e.g., registering custom accessors in pandas)
- Test fixtures (e.g., fixtures in pytest)

---
## References
[Primer on Python Decorators](https://realpython.com/primer-on-python-decorators/)

