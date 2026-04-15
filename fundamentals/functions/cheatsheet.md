# Functions — Cheatsheet

## Defining Functions

```python
def function_name(param1, param2):
    """Docstring."""
    return result
```

- `def` introduces a function
- Function body is indented
- `return` sends a value back (defaults to `None` if omitted)
- Functions are objects — can be assigned, passed, returned

---

## Parameters vs Arguments

| Term | Meaning | Example |
|------|---------|---------|
| **Parameter** | Variable in definition | `def f(x):` — `x` is a parameter |
| **Argument** | Value passed in call | `f(5)` — `5` is an argument |
| **Positional** | Matched by position | `f(1, 2)` |
| **Keyword** | Matched by name | `f(a=1, b=2)` |

---

## Parameter Types

```python
def f(pos, /, normal, *, kw_only):
    ...
```

| Syntax | Type | Rule |
|--------|------|------|
| `a` | Regular | Positional or keyword |
| `a=10` | Default | Optional, uses default if omitted |
| `*args` | Variadic positional | Collects extra positional args as tuple |
| `**kwargs` | Variadic keyword | Collects extra keyword args as dict |
| Before `/` | Positional-only | Must be passed by position |
| After `*` | Keyword-only | Must be passed by name |

### Argument Order Rule

```python
def f(pos_only, /, regular, default=val, *args, kw_only, **kwargs):
```

---

## Return Values

```python
return x              # Return single value
return x, y           # Return tuple (multiple values)
return                # Return None explicitly
# (no return)         # Implicit return None
```

### Multiple return unpacking
```python
q, r = divmod(17, 5)
```

### Early return (guard clause)
```python
def f(lst):
    if not lst:
        return None
    # main logic...
```

---

## Default Arguments

```python
def f(x, y=10):       # y defaults to 10
    return x + y
```

### DANGER: Mutable Defaults

```python
# BAD — shared across calls
def f(lst=[]):
    lst.append(1)
    return lst

# GOOD — use None sentinel
def f(lst=None):
    if lst is None:
        lst = []
    lst.append(1)
    return lst
```

Defaults are evaluated **once** at function definition time, not at each call.

---

## *args and **kwargs

```python
def f(*args):         # args is a tuple
    print(args)

def f(**kwargs):      # kwargs is a dict
    print(kwargs)

def f(*args, **kwargs):  # Both
    print(args, kwargs)
```

### Unpacking into calls

```python
args = [1, 2, 3]
f(*args)              # f(1, 2, 3)

kwargs = {"a": 1, "b": 2}
f(**kwargs)           # f(a=1, b=2)
```

---

## Scope — LEGB Rule

| Level | Name | Scope |
|-------|------|-------|
| L | Local | Inside current function |
| E | Enclosing | Inside enclosing function (closures) |
| G | Global | Module level |
| B | Built-in | Python built-in names |

Python searches L → E → G → B when resolving a name.

### Modifying outer scope

```python
global x          # Modify module-level x
nonlocal x        # Modify enclosing function's x
```

---

## Docstrings & Type Hints

```python
def f(name: str, age: int = 0) -> str:
    """Return a greeting string.

    Args:
        name: Person's name.
        age: Person's age.

    Returns:
        Formatted greeting.
    """
    return f"Hi {name}, age {age}"
```

Type hints are **not enforced** at runtime — they're for documentation and tooling.

---

## Lambda Functions

```python
lambda params: expression
```

| Feature | Lambda | def |
|---------|--------|-----|
| Name | Anonymous | Named |
| Body | Single expression | Multiple statements |
| Return | Implicit | Explicit `return` |
| Docstring | No | Yes |

### Common uses

```python
sorted(data, key=lambda x: x[1])
map(lambda x: x * 2, [1, 2, 3])
filter(lambda x: x > 0, [-1, 0, 1, 2])
```

---

## First-Class Functions

Functions can be:

```python
# Assigned to variables
double = lambda x: x * 2

# Passed as arguments
sorted(data, key=len)

# Returned from functions
def make_adder(n):
    return lambda x: x + n

# Stored in data structures
ops = {"add": lambda a, b: a + b, "mul": lambda a, b: a * b}
```

---

## Closures

A function that captures variables from its enclosing scope.

```python
def make_counter():
    count = 0
    def increment():
        nonlocal count
        count += 1
        return count
    return increment

c = make_counter()
c()  # 1
c()  # 2
```

### Late Binding Gotcha

```python
# BUG: all lambdas reference final value of i
funcs = [lambda: i for i in range(3)]
# All return 2

# FIX: capture i as default argument
funcs = [lambda i=i: i for i in range(3)]
# Returns 0, 1, 2
```

---

## Decorators

### Basic decorator

```python
import functools

def my_decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        # before
        result = func(*args, **kwargs)
        # after
        return result
    return wrapper

@my_decorator
def f():
    pass
```

### Decorator with arguments (factory)

```python
def repeat(n):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(n):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def say_hello():
    print("Hello")
```

### Stacking decorators

```python
@decorator_a    # Applied second (outer)
@decorator_b    # Applied first (inner)
def f():
    pass
# Equivalent to: f = decorator_a(decorator_b(f))
```

**Always use `@functools.wraps(func)`** to preserve `__name__`, `__doc__`, etc.

---

## Recursion Basics

```python
def factorial(n):
    if n <= 1:        # Base case
        return 1
    return n * factorial(n - 1)  # Recursive case
```

| Rule | Description |
|------|-------------|
| Base case | Condition that stops recursion |
| Recursive case | Calls itself with smaller input |
| Progress | Each call must move toward base case |
| Limit | Default ~1000 calls (`sys.getrecursionlimit()`) |

---

## Common Patterns

### Predicate function
```python
def is_valid(x):
    return x > 0 and x < 100
```

### Transformation function
```python
def normalize(text):
    return text.strip().lower()
```

### Factory function
```python
def make_multiplier(n):
    return lambda x: x * n
```

### Guard clause
```python
def process(data):
    if not data:
        return []
    # main logic
```

### Pipeline
```python
def pipe(value, *funcs):
    for f in funcs:
        value = f(value)
    return value
```

---

## Built-in Higher-Order Functions

| Function | Purpose | Example |
|----------|---------|---------|
| `map(f, iter)` | Apply f to each | `list(map(str, [1,2,3]))` |
| `filter(f, iter)` | Keep where f is True | `list(filter(bool, [0,1,2]))` |
| `sorted(iter, key=f)` | Sort by key | `sorted(words, key=len)` |
| `min/max(iter, key=f)` | Min/max by key | `max(people, key=lambda p: p['age'])` |

---

## Gotchas Quick Reference

| Gotcha | Problem | Fix |
|--------|---------|-----|
| Mutable default | `def f(lst=[])` shares list | Use `lst=None` + guard |
| No return | Function returns `None` | Add explicit `return` |
| Shadowing builtins | `list = [1,2]` hides `list()` | Use different name |
| Late binding | Lambda in loop captures final var | `lambda i=i: i` |
| Modifying mutable args | Changes affect caller's object | Copy if needed |
| `global` overuse | Makes function impure | Pass values as parameters |
| Default eval timing | `datetime.now()` as default | Use `None` + compute inside |

---

## Function Introspection

```python
f.__name__          # Function name
f.__doc__           # Docstring
f.__defaults__      # Default values tuple
f.__annotations__   # Type hints dict
f.__code__          # Code object
f.__closure__       # Closure variables
```

---

## Interview Quick Tips

- **Name functions clearly** — verbs for actions (`get_`, `is_`, `compute_`)
- **Use guard clauses** for early returns on edge cases
- **Extract helpers** — break complex logic into small named functions
- **Prefer pure functions** — no side effects, same input → same output
- **Know complexity** — closures are O(1) lookup, decorators add O(1) overhead
- **Common decorators**: `@lru_cache`, `@property`, `@staticmethod`, `@classmethod`
