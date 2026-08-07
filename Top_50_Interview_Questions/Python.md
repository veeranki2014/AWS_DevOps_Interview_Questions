```markdown
# Python Interview Questions and Answers

## 1. What is Python?

Python is a high-level, general-purpose programming language known for readable syntax, a large standard library, and a broad package ecosystem. It is widely used for automation, APIs, data processing, testing, and DevOps tooling.

## 2. Is Python compiled or interpreted?

Python source is typically compiled into bytecode and then executed by a Python virtual machine. The implementation details vary between runtimes such as CPython and PyPy.

## 3. What is dynamic typing?

In a dynamically typed language, variable names are not permanently bound to one data type. Types belong to objects and are checked at runtime.

```python
value = 10
value = "ten"
```

## 4. What are Python’s common built-in data types?

Common types include:

- `int`, `float`, `complex`
- `bool`
- `str`
- `list`, `tuple`, `range`
- `dict`
- `set`, `frozenset`
- `bytes`, `bytearray`
- `NoneType`

## 5. Compare lists and tuples.

Lists are mutable, while tuples are immutable. Tuples can be used as dictionary keys when all their elements are hashable.

## 6. Compare sets and dictionaries.

A set stores unique hashable values. A dictionary stores key-value pairs with unique hashable keys. Both generally provide efficient membership lookup.

## 7. What does mutable mean?

A mutable object can be changed after creation. Lists, dictionaries, and sets are mutable, while strings, integers, and tuples are immutable.

## 8. Why are mutable default arguments dangerous?

Default arguments are evaluated once when the function is defined, so the same mutable object is reused:

```python
def add_item(item, items=[]):
    items.append(item)
    return items
```

Use `None` instead:

```python
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items
```

## 9. What is a list comprehension?

A list comprehension creates a list from an iterable using concise transformation and filtering syntax:

```python
squares = [number * number for number in range(10)]
```

## 10. What is a generator?

A generator produces values lazily instead of storing the complete sequence in memory. Generator functions use `yield`.

```python
def numbers():
    for number in range(3):
        yield number
```

## 11. What is the difference between `return` and `yield`?

`return` terminates a function and produces one result. `yield` suspends a generator while preserving its execution state so it can produce additional values later.

## 12. What is an iterator?

An iterator provides values one at a time through `__next__()` and returns itself from `__iter__()`. It raises `StopIteration` when exhausted.

## 13. What is the difference between an iterable and an iterator?

An iterable can produce an iterator using `iter()`. An iterator tracks iteration state and produces the next value using `next()`.

## 14. What do `*args` and `**kwargs` mean?

`*args` collects additional positional arguments into a tuple. `**kwargs` collects additional keyword arguments into a dictionary.

```python
def deploy(*targets, **options):
    pass
```

## 15. What is argument unpacking?

An iterable can be unpacked into positional arguments with `*`, while a mapping can be unpacked into keyword arguments with `**`.

```python
function(*arguments, **options)
```

## 16. What is a lambda function?

A lambda is a small anonymous function containing one expression:

```python
sorted(items, key=lambda item: item["name"])
```

Named functions are often clearer for nontrivial behavior.

## 17. What is a decorator?

A decorator wraps or transforms a function or class. It is applied using `@decorator` syntax and is commonly used for logging, authorization, retries, caching, and instrumentation.

## 18. What is a context manager?

A context manager defines setup and cleanup around a block:

```python
with open("config.txt", encoding="utf-8") as file:
    content = file.read()
```

Cleanup occurs even if an exception is raised.

## 19. How do you create a custom context manager?

Implement `__enter__` and `__exit__`, or use `contextlib.contextmanager`:

```python
from contextlib import contextmanager

@contextmanager
def managed_resource():
    resource = acquire()
    try:
        yield resource
    finally:
        release(resource)
```

## 20. How does exception handling work?

Use `try`, `except`, `else`, and `finally`:

```python
try:
    result = perform_operation()
except TimeoutError as error:
    handle_timeout(error)
else:
    process(result)
finally:
    cleanup()
```

## 21. Why should `except Exception` be used carefully?

It can hide unexpected failures if exceptions are silently ignored. Catch the narrowest expected exception, record useful context, and re-raise when the caller should handle the failure.

Avoid catching `BaseException` in normal application code because it includes termination-related exceptions.

## 22. How do you define a custom exception?

Subclass an appropriate exception type:

```python
class DeploymentError(RuntimeError):
    pass
```

Custom exceptions let callers distinguish domain failures from generic errors.

## 23. What is a module?

A module is a Python source file that can contain variables, functions, and classes and can be imported by other Python code.

## 24. What is a package?

A package organizes related modules under a common namespace. Modern Python supports regular packages and namespace packages.

## 25. What does `if __name__ == "__main__"` do?

Code inside this condition runs when the file is executed directly, but not when it is imported as a module.

```python
if __name__ == "__main__":
    main()
```

## 26. What is a virtual environment?

A virtual environment creates an isolated Python runtime context with its own installed packages:

```bash
python -m venv .venv
```

It prevents project dependencies from interfering with system or other project dependencies.

## 27. What is `pip`?

`pip` installs and manages Python packages from indexes or other supported sources. Production builds should use controlled repositories and pinned or locked dependencies.

## 28. Why should dependencies be pinned?

Pinning or locking improves reproducibility and limits unexpected upgrades. Security updates should still be applied through a deliberate dependency-update process.

## 29. What is object-oriented programming?

Object-oriented programming organizes code around objects that combine data and behavior. Important concepts include encapsulation, inheritance, composition, and polymorphism.

## 30. What is `self`?

`self` conventionally refers to the current class instance inside an instance method. Python supplies it when the method is invoked through an instance.

## 31. Compare instance, class, and static methods.

- Instance methods receive `self`.
- Class methods receive `cls` and use `@classmethod`.
- Static methods receive neither automatically and use `@staticmethod`.

## 32. What is inheritance?

Inheritance lets one class derive behavior and attributes from another. Composition is often preferable when the relationship is “has a” rather than “is a.”

## 33. What is method overriding?

Method overriding occurs when a subclass provides its own implementation of a method inherited from a base class.

## 34. What is multiple inheritance?

Multiple inheritance allows a class to inherit from multiple base classes. Python resolves method lookup using the class method-resolution order, or MRO.

## 35. What is a dataclass?

A dataclass generates common methods for data-oriented classes:

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class Deployment:
    name: str
    version: str
```

It can reduce repetitive constructor and representation code.

## 36. What are type hints?

Type hints describe expected types for parameters, return values, and variables:

```python
def add(left: int, right: int) -> int:
    return left + right
```

They improve tooling and static analysis but are not normally enforced automatically at runtime.

## 37. What is the Global Interpreter Lock?

In standard CPython builds, the Global Interpreter Lock historically permits only one thread at a time to execute Python bytecode in a process. Threads can still help with I/O-bound work, while processes or other approaches are commonly used for CPU-bound parallelism.

## 38. Compare threading, multiprocessing, and asyncio.

- Threading is useful for many blocking I/O operations.
- Multiprocessing provides separate processes for CPU parallelism and isolation.
- `asyncio` provides cooperative concurrency for asynchronous I/O.

The correct choice depends on workload and libraries.

## 39. What are `async` and `await`?

`async def` defines a coroutine function. `await` suspends that coroutine until an awaitable completes, allowing the event loop to run other work.

## 40. How do you read a large file efficiently?

Iterate over the file instead of loading it all into memory:

```python
with open("application.log", encoding="utf-8") as file:
    for line in file:
        process(line)
```

## 41. How do you work with JSON?

Use the `json` module:

```python
import json

with open("config.json", encoding="utf-8") as file:
    config = json.load(file)
```

Use `json.loads()` and `json.dumps()` for strings.

## 42. How do you execute an external command safely?

Use `subprocess.run()` with an argument list:

```python
import subprocess

result = subprocess.run(
    ["kubectl", "get", "pods", "-o", "json"],
    check=True,
    capture_output=True,
    text=True,
)
```

Avoid `shell=True` with untrusted input.

## 43. How should environment variables be read?

Use `os.environ`:

```python
import os

environment = os.environ.get("APP_ENV", "development")
```

Validate required values and do not print secrets.

## 44. How should Python applications log messages?

Use the `logging` module rather than scattered `print()` calls. Include severity and operational context, configure handlers centrally, and avoid recording credentials or sensitive payloads.

## 45. What is unit testing?

Unit testing verifies small pieces of behavior in isolation. Python’s standard library includes `unittest`, while `pytest` is a widely used third-party testing framework.

## 46. What is mocking?

Mocking replaces a dependency with controlled test behavior. It is useful for external APIs, time, random behavior, subprocesses, and cloud services, but excessive mocking can create unrealistic tests.

## 47. How would you call a REST API robustly?

Use timeouts, authentication, TLS verification, bounded retries for transient failures, status-code validation, pagination, rate-limit handling, schema checks, and careful logging.

## 48. How do you make a Python automation script idempotent?

Read the current state, calculate the required change, avoid duplicate creation, use stable identifiers, handle partial completion, and verify the final state.

## 49. How should secrets be handled in Python automation?

Retrieve them through environment injection, workload identity, or an approved secret manager. Never hardcode them, commit them, include them in exception messages, or expose them through command-line arguments when avoidable.

## 50. What makes a production-quality Python automation tool?

It should include clear configuration, type hints, structured logging, timeouts, bounded retries, idempotency, explicit exceptions, secret protection, unit and integration tests, dependency locking, linting, formatting, and documentation.

