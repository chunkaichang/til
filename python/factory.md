# Factory Patterns

The factory design pattern provides a common interface to create objects that share the same interface. Some benefits of using factories include:
- The object creation details are agnostic to applications
- Object creation can be based on runtime information
- Code reusing

## Naive implementation
The naive way to implement a factory is to have a class method that instantiates different objects based on some predefined arguments. An `if-elif-else` chain is usually observed, which makes the code hard to read as more object types are added.

## Remove `if-elif-else` in library code using registration
A solution to avoid `if-elif-else` is to add an `register()` method to the factory class. The `register()` method takes a key and a class that implement object creation for a particular object type. Internally, it maintains a map from each key to the corresponding builder class. By doing so, adding new types can be done by simply calling `register()` in the library code.
Applications get an object by calling `get_obj()`.

```python
class Factory:
    def __init__(self):
        self._builders = {}

    def register(self, key, builder):
        self._builders[key] = builder

    def get_obj(self, key):
        builder = self._builders.get(key)
        if not builder:
            raise ValueError(key)
        return builder()

factory = Factory()
factory.register('A', ImplClassA)
factory.register('B', ImplClassB)
```

## Generic object factory
Sometimes we might need multiple factories in our applications. Since the functionality of factories are the same, instead of implementing multiple factories, we can create a generic object factory that can be inherited to reuse code.

```python
class ObjectFactory:
    def __init__(self):
        self._builders = {}

    def register_builder(self, key, builder):
        self._builders[key] = builder

    def create(self, key, **kwargs):
        builder = self._builders.get(key)
        if not builder:
            raise ValueError(key)
        return builder(**kwargs) ## invokes __call__() of the builder

class MusicServiceProvider(object_factory.ObjectFactory):
    def get(self, service_id, **kwargs):
        return self.create(service_id, **kwargs)

class SpotifyServiceBuilder:
    def __init__(self):
        self._instance = None

    def __call__(self, spotify_client_key, spotify_client_secret, **_ignored):
        if not self._instance:
            access_code = self.authorize(
                spotify_client_key, spotify_client_secret)
            self._instance = SpotifyService(access_code)
        return self._instance

services = MusicServiceProvider()
services.register_builder('SPOTIFY', SpotifyServiceBuilder())
services.register_builder('APPLE', AppleServiceBuilder())
```
Here a specialized factory `MusicServiceProvider` inherits the generic object factory and provides an interface `get()` to applications. As a result, the implementation detail (factory pattern) is decoupled from applications.

Note that the generic object factory's `create()` also takes `kwargs`, which allows passing builder-specific information to builders without changing the interface.

---
## References
[Factory method in Python](https://realpython.com/factory-method-python/)
