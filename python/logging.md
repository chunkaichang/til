# Logging

The `logging` modules provides an API for logging events with different severity along with contextual information to multiple destinations (e.g., console, file, HTTP, etc.)

## Logger objects
- Loggers are the interface to the application. They can be accessed via `logging.getLogger(name)`. Each logger of the same name is a singleton.
- Logging methods: `debug()`, `info()`, `warning()`, `error()`, `critical()`
- `exception()` logs an error message along with the exception info
- Each logger can have multiple handlers to handle events at different levels. Handlers can be added to a logger via `addHandler()`.

## Handler objects
- Some useful handler classes are:
        - `logging.StreamHandler()`: for logging to console
        - `logging.FileHandler()`: for logging to a file 
- Each handler handles events higher than its level (set by `setLevel()`)
- Handlers can also format its output via setting a formatter

## Formatter objects
- Formatters customizes the output message from the application along with built-in contextual info (e.g., time, process id, etc.)
- App-specific contextual info can be injected via the filter objects

## Filter objects
- Filter provides sophisticated control of event filtering (beyond log levels, e.g., by logger names). It can also be used to inject app-specific data as log record attributes, which can then be accessed by formatters.


---
## References
[Python3 logging doc](https://docs.python.org/3/library/logging.html)
