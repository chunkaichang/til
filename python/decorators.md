# Decorators

The well-known `@` symbol is just a syntactic sugar for decorators (e.g., the built-in special class methods: `@classmethod`, `@staticmethod`, and `@property`.) 

Decorators are based on the following concepts:
- Functions are first-class objects
- Returning a reference of an inner function

The followings are notes of writing and using decorators and the application of decorators.

## On writing decorators
0. Search for existing implementations before implementing your own decorators.
1. For supporting generic functions, the inner function should take `*args, **kwargs` and return the result of the wrapped function
2. For preserving the decorated functions' name and docstring, the inner functions should be decorated with `functools.wraps`.
3. For decorators with arguments, another level of nesting is neccessary. The first level takes the arguments of the decorator and returns an inner function that can be used as a decorator.
4. See [here][flexible_decorators] for writing decorators that can be used both with and without arguments. The trick is to enforce keyword-only arguments.
5. For implementing a class as a decorator, its `__init__()` method must take a function and its `__call__()` method must be implemented to decorate the function.

## On using decorators
1. When multiple decorators are nested, think of it as the first decorator calls the second and so on.

## Applications
- Profiling
- Debugging (e.g., printing signatures, [logging](logging.md))
- Caching (e.g., `functools.lru_cache`)
- Rate-limiting
- Plugin registration (e.g., registering custom accessors in pandas)
- Test fixtures (e.g., fixtures in pytest)

---
## References
[Primer on Python Decorators](https://realpython.com/primer-on-python-decorators/)

[flexible_decorators]: https://realpython.com/primer-on-python-decorators/#both-please-but-never-mind-the-bread 
