# Primitive Data Types in Python

## 1. Intuition

Every piece of data in Python has a **type**. Before you can work with strings, lists, or dictionaries, you need to understand the basic building blocks — the **primitive data types**.

Think of types like containers: each container holds a specific kind of thing, and Python decides what operations you can do based on what's inside.

Python's core primitive types:

| Type | What it holds | Example |
|------|--------------|---------|
| `int` | Whole numbers | `42`, `-7`, `0` |
| `float` | Decimal numbers | `3.14`, `-0.5`, `1.0` |
| `str` | Text (sequences of characters) | `"hello"`, `'a'` |
| `bool` | True or False | `True`, `False` |
| `NoneType` | Absence of a value | `None` |

---

## 2. Syntax

### Creating values

```python
# Integers
age = 25
negative = -10
zero = 0

# Floats
price = 19.99
temperature = -3.5
whole_as_float = 7.0

# Strings
name = "Sevan"
letter = 'a'
empty_string = ""

# Booleans
is_active = True
is_done = False

# None
result = None
```

### Checking types

```python
type(42)        # <class 'int'>
type(3.14)      # <class 'float'>
type("hello")   # <class 'str'>
type(True)      # <class 'bool'>
type(None)      # <class 'NoneType'>
```

### Type conversion (casting)

```python
int("5")        # 5        (string → int)
float("3.14")   # 3.14     (string → float)
str(42)         # "42"     (int → string)
bool(0)         # False    (0 is falsy)
bool(1)         # True     (nonzero is truthy)
bool("")        # False    (empty string is falsy)
bool("hello")   # True     (non-empty string is truthy)
```

---

## 3. Small Examples

### Example 1: Variable assignment and type

```python
x = 10
print(type(x))  # <class 'int'>

x = 10.5
print(type(x))  # <class 'float'>
```

Python variables don't have fixed types — the same name can point to different types. This is called **dynamic typing**.

### Example 2: Integer vs float division

```python
print(10 / 3)    # 3.3333... (always returns float)
print(10 // 3)   # 3         (floor division, returns int)
print(10 % 3)    # 1         (remainder/modulo)
```

### Example 3: Boolean as numbers

```python
print(True + True)   # 2  (True is 1)
print(False + 10)    # 10 (False is 0)
print(True * 5)      # 5
```

Booleans are a subclass of `int` in Python. `True == 1` and `False == 0`.

### Example 4: None checks

```python
result = None
print(result is None)      # True  (correct way)
print(result == None)      # True  (works but not preferred)
```

Always use `is None` instead of `== None`. This is a Python convention.

---

## 4. Key Concepts

### Truthy and Falsy values

Every Python value has a boolean interpretation:

**Falsy** (evaluates to `False`):
- `0`, `0.0`
- `""` (empty string)
- `None`
- `False`
- `[]`, `{}`, `()` (empty collections — we'll cover these later)

**Truthy** (evaluates to `True`):
- Any nonzero number
- Any non-empty string
- `True`

```python
if 0:
    print("won't print")

if 42:
    print("will print")  # ← this runs

if "":
    print("won't print")

if "hello":
    print("will print")  # ← this runs
```

### Immutability

All primitive types in Python are **immutable** — you can't change the value itself, only reassign the variable.

```python
x = 5
x = x + 1  # This creates a NEW int object (6), not modifying 5
```

### Operator behavior depends on type

```python
# + with numbers: addition
print(3 + 4)        # 7

# + with strings: concatenation
print("hello" + " " + "world")  # "hello world"

# * with string and int: repetition
print("ha" * 3)     # "hahaha"
```

---

## 5. Edge Cases

```python
# Float precision
print(0.1 + 0.2)           # 0.30000000000000004 (NOT 0.3!)
print(0.1 + 0.2 == 0.3)    # False (!)

# Large integers — Python handles them natively
print(10 ** 100)            # works fine, no overflow

# Int/string conversion edge cases
int("10")     # 10  ✓
# int("10.5") # ValueError! Can't go string-float → int directly
int(float("10.5"))  # 10 ✓ (string → float → int)

# Bool is int
isinstance(True, int)   # True
```

---

## 6. Interview Relevance

- **Type checking** comes up in debugging and edge-case handling
- **Truthy/falsy** is used constantly in conditionals and default values
- **Float precision** is a classic gotcha in interviews
- **None checks** (`is None`) appear in linked list, tree, and graph problems
- Understanding `//` vs `/` vs `%` is essential for index math in arrays

---

## 7. Production Relevance

- **Type hints** in production Python: `def process(value: int) -> str:`
- **None as default**: APIs often return `None` for missing data — you must handle it
- **Float precision**: Financial systems use `Decimal` instead of `float`
- **Boolean flags**: Feature flags, config toggles, database status fields
