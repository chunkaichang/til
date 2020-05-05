# Runtime polymorphism with std::variant and std::visit (C++17)

In addition to virtual functions, runtime polymorphism can be done with `std::variant` and `std::visit`.
Instead of storing base pointers and invoking methods with virtual functions, this method stores variant objects and invokes methods using `std::visit`, which takes a callable object and the variant object to visit. See this [post] for syntax and examples.  

## Pros
- No vptr chasing overhead
- No need to modify the entire inheritance hierarchy when adding/modifying virtual functions
- Classes can be unrelated

## Cons
- Need to know all the types at compile time
- May use more memory (size of variant depends on the max of type size)
