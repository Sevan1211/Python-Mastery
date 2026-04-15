# Dictionaries — Cheatsheet

Quick reference for Python dictionaries. Use this for fast recall, not as a tutorial.

---

## Creating Dictionaries

```python
# Literal (most common)
d = {"key": "value", "name": "Alice", "age": 30}

# Empty dict
d = {}
d = dict()

# From keyword args (keys must be valid identifiers)
d = dict(name="Alice", age=30)

# From list of tuples
d = dict([("a", 1), ("b", 2)])

# From zip (combine two lists)
d = dict(zip(["a", "b"], [1, 2]))

# dict.fromkeys (same value for all keys)
d = dict.fromkeys(["a", "b", "c"], 0)    # {'a': 0, 'b': 0, 'c': 0}
# ⚠️ WARNING: mutable default is shared → use comprehension instead

# Comprehension
d = {x: x**2 for x in range(5)}

# Copy constructor
d = dict(existing_dict)
```

**⚠️ GOTCHA**: `{}` creates an empty **dict**, not an empty set. Use `set()` for empty sets.

---

## Accessing Values

```python
d["key"]              # Returns value. Raises KeyError if missing.
d.get("key")          # Returns value or None if missing.
d.get("key", default) # Returns value or default if missing. Does NOT modify dict.
```

---

## Adding & Updating

```python
d["new_key"] = value         # Add or overwrite
d.update({"a": 1, "b": 2})  # Merge in another dict (in-place)
d.update(a=1, b=2)           # Same with keyword args
d.setdefault("key", default) # Insert default if key missing, return value
```

| Method | Modifies Dict? | Returns |
|--------|---------------|---------|
| `d["key"] = val` | Yes | N/A |
| `d.update(...)` | Yes | `None` |
| `d.setdefault(k, v)` | Yes (if missing) | Existing or new value |

---

## Removing Entries

| Method | Returns | If Key Missing | Use When |
|--------|---------|----------------|----------|
| `del d[key]` | Nothing | `KeyError` | Know key exists |
| `d.pop(key)` | Removed value | `KeyError` | Need the removed value |
| `d.pop(key, default)` | Value or default | Returns default | Key might not exist |
| `d.popitem()` | `(key, value)` tuple | `KeyError` if empty | Remove last entry |
| `d.clear()` | `None` | N/A | Wipe all entries |

---

## All Dictionary Methods

| Method | Description | Returns |
|--------|-------------|---------|
| `d.keys()` | View of all keys | `dict_keys` (live view) |
| `d.values()` | View of all values | `dict_values` (live view) |
| `d.items()` | View of all `(key, value)` pairs | `dict_items` (live view) |
| `d.get(key, default)` | Safe access, returns default if missing | Value or default |
| `d.setdefault(key, val)` | Get value; insert `val` if missing | Value |
| `d.update(other)` | Merge `other` into `d` | `None` |
| `d.pop(key, default)` | Remove and return value | Value or default |
| `d.popitem()` | Remove and return last `(key, value)` | Tuple |
| `d.clear()` | Remove all entries | `None` |
| `d.copy()` | Shallow copy | New dict |
| `dict.fromkeys(keys, val)` | New dict from keys with same value | New dict |

---

## Checking Membership

```python
"key" in d           # True if KEY exists       → O(1)
"key" not in d       # True if KEY doesn't exist → O(1)
"value" in d.values() # True if VALUE exists     → O(n)
```

**⚠️ CRITICAL**: `in` checks **keys**, not values!

---

## Iterating

```python
for k in d:                     # Keys only (default)
for k in d.keys():              # Keys only (explicit)
for v in d.values():            # Values only
for k, v in d.items():          # Key-value pairs (MOST COMMON)
for i, (k, v) in enumerate(d.items()):  # With index
for k in sorted(d):             # Sorted by key
for k in sorted(d, key=d.get):  # Sorted by value (ascending)
for k in sorted(d, key=d.get, reverse=True):  # Sorted by value (descending)
```

---

## Dict Comprehensions

```python
# Basic
{x: x**2 for x in range(5)}

# With filter
{k: v for k, v in d.items() if v > 0}

# Transform values
{k: v.upper() for k, v in d.items()}

# Invert dict (swap keys/values)
{v: k for k, v in d.items()}

# From two lists
{k: v for k, v in zip(keys, values)}

# Conditional values
{k: ("pass" if v >= 70 else "fail") for k, v in scores.items()}
```

---

## Merging Dictionaries

```python
# .update() — modifies in-place
d1.update(d2)             # d2 values win on conflicts

# ** unpacking — new dict (Python 3.5+)
merged = {**d1, **d2}     # d2 wins on conflicts

# | operator — new dict (Python 3.9+)
merged = d1 | d2          # d2 wins

# |= operator — in-place (Python 3.9+)
d1 |= d2                  # d2 wins
```

---

## Copying

```python
# Assignment (NOT a copy — same object)
alias = d               # alias IS d

# Shallow copy (new dict, shared inner objects)
copy1 = d.copy()
copy2 = dict(d)
copy3 = {**d}

# Deep copy (completely independent)
import copy
deep = copy.deepcopy(d)
```

**⚠️ Shallow copy pitfall**: If values are mutable (lists, dicts), both copies share them.

---

## Hashability — What Can Be a Key?

| Type | Hashable? | Can Be Key? |
|------|-----------|-------------|
| `int`, `float`, `str`, `bool`, `None` | ✓ | ✓ |
| `tuple` (of hashables) | ✓ | ✓ |
| `frozenset` | ✓ | ✓ |
| `list` | ✗ | ✗ |
| `dict` | ✗ | ✗ |
| `set` | ✗ | ✗ |

**Rule**: Immutable → hashable → can be key. Mutable → not hashable → cannot be key.

**⚠️ GOTCHA**: `True == 1` and `False == 0`, so they collide as keys.

---

## Common Patterns

### Frequency Counting
```python
freq = {}
for item in items:
    freq[item] = freq.get(item, 0) + 1
```

### Grouping
```python
groups = {}
for item in items:
    key = get_group_key(item)
    groups.setdefault(key, []).append(item)
```

### Inverting (unique values)
```python
inverted = {v: k for k, v in d.items()}
```

### Inverting (duplicate values → lists)
```python
inverted = {}
for k, v in d.items():
    inverted.setdefault(v, []).append(k)
```

### Two Sum
```python
seen = {}  # value → index
for i, num in enumerate(nums):
    complement = target - num
    if complement in seen:
        return [seen[complement], i]
    seen[num] = i
```

### Find Key with Max/Min Value
```python
max_key = max(d, key=d.get)
min_key = min(d, key=d.get)
```

---

## Built-in Functions with Dicts

| Function | Operates On | Example |
|----------|------------|---------|
| `len(d)` | Pairs | `len({"a": 1, "b": 2})` → `2` |
| `min(d)` | Keys | `min({"b": 1, "a": 2})` → `"a"` |
| `max(d)` | Keys | `max({"b": 1, "a": 2})` → `"b"` |
| `sorted(d)` | Keys | `sorted({"b": 1, "a": 2})` → `["a", "b"]` |
| `sum(d.values())` | Values | `sum({"a": 10, "b": 20}.values())` → `30` |
| `any(d)` | Keys | `any({0: "x", 1: "y"})` → `True` |
| `all(d)` | Keys | `all({0: "x", 1: "y"})` → `False` |

---

## Time Complexity

| Operation | Average | Worst |
|-----------|---------|-------|
| `d[key]` (get) | O(1) | O(n) |
| `d[key] = val` (set) | O(1) | O(n) |
| `del d[key]` | O(1) | O(n) |
| `key in d` | O(1) | O(n) |
| `d.get(key)` | O(1) | O(n) |
| `d.pop(key)` | O(1) | O(n) |
| `d.keys() / d.values() / d.items()` | O(1) | O(1) |
| Iterate all items | O(n) | O(n) |
| `d.copy()` | O(n) | O(n) |
| `len(d)` | O(1) | O(1) |
| `d.update(other)` | O(len(other)) | O(len(other)·n) |

Worst case O(n) happens due to hash collisions — exceedingly rare in practice.

---

## Common Gotchas

| # | Gotcha | Fix |
|---|--------|-----|
| 1 | `in` checks keys, not values | Use `val in d.values()` for value check |
| 2 | Modifying dict during iteration → `RuntimeError` | Iterate over `list(d.items())` or build new dict |
| 3 | `dict.fromkeys(keys, [])` shares one list | Use `{k: [] for k in keys}` |
| 4 | `.update()` returns `None` | Don't assign: `result = d.update(...)` |
| 5 | `=` creates alias, not copy | Use `.copy()` or `dict(d)` |
| 6 | Shallow copy shares nested objects | Use `copy.deepcopy()` for nested structures |
| 7 | `True/1` and `False/0` are the same key | Avoid mixing booleans and integers as keys |
| 8 | Mutable default arg `def f(d={})` shares across calls | Use `def f(d=None): d = d or {}` |

---

## Interview Patterns Quick Reference

| Pattern | Key Technique | Examples |
|---------|--------------|----------|
| Frequency counting | `d[x] = d.get(x, 0) + 1` | Most common element, valid anagram |
| Two-sum / complement | Store seen values → check complement | Two sum, pair sum |
| Grouping | `.setdefault(key, []).append(item)` | Group anagrams, categorize |
| Existence / dedup | `key in d` for O(1) check | Contains duplicate, first unique char |
| Index mapping | `d[val] = index` | Two sum (store positions), first unique |
| Inversions | `{v: k for k, v in d.items()}` | Reverse lookup, decode |
| Prefix sum + dict | Store prefix sums → check for complement | Subarray sum equals K |
