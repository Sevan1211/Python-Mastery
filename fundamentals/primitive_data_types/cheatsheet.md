# Primitive Data Types — Cheatsheet

## Types at a glance

| Type | Example | Mutable? | Falsy value |
|------|---------|----------|-------------|
| `int` | `42` | No | `0` |
| `float` | `3.14` | No | `0.0` |
| `str` | `"hello"` | No | `""` |
| `bool` | `True` | No | `False` |
| `NoneType` | `None` | No | `None` |

## Type checking

```python
type(x)                # returns <class 'type'>
isinstance(x, int)     # True/False — preferred in production
```

## Type conversion

```python
int("5")         # 5
float("3.14")    # 3.14
str(42)          # "42"
bool(0)          # False
int(float("3.9")) # 3  (two-step for string floats)
```

## Division operators

```python
10 / 3    # 3.333...  (true division, always float)
10 // 3   # 3         (floor division)
10 % 3    # 1         (modulo/remainder)
```

## Truthy / Falsy quick check

Falsy: `0`, `0.0`, `""`, `None`, `False`, `[]`, `{}`, `()`
Everything else is truthy (including `"0"` and `-1`).

## None idiom

```python
if x is None:       # ✓ correct
if x is not None:   # ✓ correct
if x == None:       # ✗ avoid
```

## Float gotcha

```python
0.1 + 0.2 == 0.3              # False!
abs(0.1 + 0.2 - 0.3) < 1e-9  # True ✓
```

## Bool is int

```python
True == 1    # True
False == 0   # True
True + True  # 2
```
