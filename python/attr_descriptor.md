# Object attributes and descriptors

Each Python object stores its attributes in the built-in `__dict__` dictionary.
Objects may also contain special objects called descriptors that can be accessed as attributes. In fact, object properties are actually descriptors under the hood.

Descriptors that implement the `__get__()` method only are called non-data descriptors, while descriptors that implements `__set__()` or `__delete__()` are said to be data descriptors.

## Lookup chain of accessing an attribute
The following is the procedure of accessing an attribute of an object using the `.` operator. If a step fails, the next one is invoked.

1. `__get()__` of the data descriptor
2. the object's `__dict__`
3. `__get()__` of the non-data descriptor
4. the object type's `__dict__`
5. the object parent type's `__dict__`
6. Repeat for all the parent types
7. Throw `AttributeError` if everything above failed

## Applications

### Lazy properties
A lazy property defers initialization of values until the first time it is accessed and the values are cached for later reuse.
A lazy property can be implemented as an **non-data** descriptor. When the property is accessed the first time, the 3rd step in the above lookup chain is reached where values are initialized and stored in the owner's `__dict__`. As a result, later accesses to the properties can then be found in the owner's `__dict__`.

### D.R.Y. code
Suppose a set of properties of an object enforce the same update constraint (e.g., the new value must be non-negative). Instead of implementing the same constraint in each property's setter, we can create a descriptor class that implements the constraint and then instantiate properties from this class.

---
## References
[Python descriptors](https://realpython.com/python-descriptors/
