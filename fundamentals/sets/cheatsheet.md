# Sets — Cheatsheet

Fast recall reference for all set concepts, methods, operations, and gotchas.

---

## Creating Sets

```python
# Literal (non-empty)
s = {1, 2, 3}

# Empty set (MUST use set(), NOT {})
s = set()                    # ✓ empty set
d = {}                       # ✗ this is a dict!

# From iterable (duplicates removed)
set([1, 2, 2, 3])           # {1, 2, 3}
set("hello")                 # {'h', 'e', 'l', 'o'}
set(range(5))                # {0, 1, 2, 3, 4}
set((1, 2, 3))               # {1, 2, 3}
set({"a": 1, "b": 2})        # {'a', 'b'} — keys only

# Set comprehension
{x ** 2 for x in range(5)}   # {0, 1, 4, 9, 16}
{x for x in lst if x > 0}    # filtered set
```

---

## Set Properties

| Property | Rule | Consequence |
|----------|------|-------------|
| **Unordered** | No guaranteed order | No indexing `s[0]`, no slicing |
| **Unique** | No duplicate elements | `{1, 1, 2} → {1, 2}` |
| **Mutable** | Can add/remove after creation | Use `frozenset` for immutable |
| **Hashable elements only** | Elements must be immutable | No lists, dicts, or sets inside |

**Hashable types:** `int`, `float`, `str`, `bool`, `None`, `tuple` (of hashables), `frozenset`  
**NOT hashable:** `list`, `dict`, `set`

---

## Adding Elements

```python
s = {1, 2}

s.add(3)              # Add single element → {1, 2, 3}
s.add(2)              # Duplicate ignored → {1, 2, 3}

s.update([4, 5])      # Add from iterable → {1, 2, 3, 4, 5}
s.update("ab")        # Each char added → {..., 'a', 'b'}
s.update([6], {7})    # Multiple iterables at once
```

**GOTCHA: `add("hello")` vs `update("hello")`**

| Method | Input `"hello"` | Result |
|--------|-----------------|--------|
| `s.add("hello")` | Adds whole string | `{'hello'}` |
| `s.update("hello")` | Adds each char | `{'h', 'e', 'l', 'o'}` |

---

## Removing Elements

| Method | Missing Element | Returns | Use When |
|--------|----------------|---------|----------|
| `s.remove(x)` | `KeyError` | `None` | Sure `x` exists |
| `s.discard(x)` | No error | `None` | Not sure if `x` exists |
| `s.pop()` | `KeyError` (empty) | The element | Need any arbitrary element |
| `s.clear()` | N/A | `None` | Empty the entire set |

```python
s = {1, 2, 3}
s.remove(2)     # {1, 3} — KeyError if missing
s.discard(99)   # {1, 3} — no error if missing
s.pop()         # removes & returns arbitrary element
s.clear()       # set()
```

---

## Membership Testing — O(1)

```python
s = {1, 2, 3}
1 in s          # True  — O(1)
4 in s          # False — O(1)
4 not in s      # True  — O(1)
```

**vs list lookup (O(n)):**
```python
# Convert list → set for repeated lookups
lookup = set(big_list)   # O(n) once
x in lookup              # O(1) each time
```

---

## Set Operations

Given `A = {1, 2, 3, 4}` and `B = {3, 4, 5, 6}`:

| Operation | Operator | Method | Result |
|-----------|----------|--------|--------|
| **Union** | `A \| B` | `A.union(B)` | `{1,2,3,4,5,6}` |
| **Intersection** | `A & B` | `A.intersection(B)` | `{3,4}` |
| **Difference** | `A - B` | `A.difference(B)` | `{1,2}` |
| **Symmetric Diff** | `A ^ B` | `A.symmetric_difference(B)` | `{1,2,5,6}` |

**Key difference:** Methods accept any iterable (`s.union([1,2])`). Operators require sets on both sides (`s | {1,2}`).

### In-Place (Mutating) Versions

| Operation | Operator | Method |
|-----------|----------|--------|
| Union update | `A \|= B` | `A.update(B)` |
| Intersection update | `A &= B` | `A.intersection_update(B)` |
| Difference update | `A -= B` | `A.difference_update(B)` |
| Symmetric diff update | `A ^= B` | `A.symmetric_difference_update(B)` |

---

## Subset / Superset / Disjoint

```python
a = {1, 2}
b = {1, 2, 3, 4}

# Subset: every element of a is in b
a <= b              # True (subset)
a.issubset(b)       # True
a < b               # True (proper subset: subset + not equal)
a < a               # False (not proper subset of itself)
a <= a              # True (subset of itself)

# Superset: b contains every element of a
b >= a              # True (superset)
b.issuperset(a)     # True
b > a               # True (proper superset)

# Disjoint: no elements in common
{1, 2}.isdisjoint({3, 4})  # True
{1, 2}.isdisjoint({2, 3})  # False
```

---

## Set Comprehensions

```python
# Basic
{x ** 2 for x in range(5)}          # {0, 1, 4, 9, 16}

# With filter
{x for x in nums if x > 0}          # positive numbers only

# From strings
{c.lower() for c in text if c.isalpha()}  # unique letters

# Unique lengths
{len(w) for w in words}              # unique word lengths
```

---

## Frozenset (Immutable Set)

```python
fs = frozenset([1, 2, 3])

# Supports: in, len, |, &, -, ^, iteration
# Does NOT support: add, remove, discard, pop, clear, update

# Can be a dict key (hashable)
d = {frozenset({1, 2}): "group"}

# Can be inside a set (set of sets)
groups = {frozenset({1, 2}), frozenset({3, 4})}
```

---

## Copying

```python
original = {1, 2, 3}

# Reference (alias) — same object
alias = original           # alias.add(4) changes original!

# Shallow copy — independent
copy1 = original.copy()
copy2 = set(original)
```

---

## Iteration

```python
s = {3, 1, 2}

# Basic (order NOT guaranteed)
for x in s:
    print(x)

# Sorted order
for x in sorted(s):
    print(x)

# With enumerate
for i, x in enumerate(s):
    print(i, x)
```

---

## Built-in Functions

```python
s = {5, 2, 8, 1}

len(s)        # 4
min(s)        # 1
max(s)        # 8
sum(s)        # 16
sorted(s)     # [1, 2, 5, 8] — returns a LIST
list(s)       # [order varies]
tuple(s)      # (order varies)
bool(s)       # True (non-empty)
bool(set())   # False (empty)
any(s)        # True if any element is truthy
all(s)        # True if all elements are truthy
```

---

## Common Patterns

### Deduplication (order-preserving)
```python
def deduplicate(items):
    seen = set()
    result = []
    for item in items:
        if item not in seen:
            seen.add(item)
            result.append(item)
    return result
```

### Contains Duplicate
```python
def has_duplicate(nums):
    return len(nums) != len(set(nums))
```

### Two Sum (set-based)
```python
def has_pair_sum(nums, target):
    seen = set()
    for n in nums:
        if target - n in seen:
            return True
        seen.add(n)
    return False
```

### Find Missing Elements
```python
missing = required_set - provided_set
extra = provided_set - required_set
```

### Longest Consecutive Sequence
```python
def longest_consecutive(nums):
    num_set = set(nums)
    longest = 0
    for n in num_set:
        if n - 1 not in num_set:  # start of sequence
            length = 1
            while n + length in num_set:
                length += 1
            longest = max(longest, length)
    return longest
```

---

## Time Complexity

| Operation | Average | Worst |
|-----------|---------|-------|
| `x in s` | **O(1)** | O(n) |
| `s.add(x)` | **O(1)** | O(n) |
| `s.remove(x)` / `s.discard(x)` | **O(1)** | O(n) |
| `s.pop()` | **O(1)** | O(1) |
| `len(s)` | **O(1)** | O(1) |
| `s \| t` (union) | O(s+t) | |
| `s & t` (intersection) | O(min(s,t)) | |
| `s - t` (difference) | O(s) | |
| `s ^ t` (sym diff) | O(s+t) | |
| `s <= t` (subset) | O(s) | |
| Building `set(iterable)` | O(n) | |

---

## Common Gotchas

| Gotcha | Bug | Fix |
|--------|-----|-----|
| Empty set | `{} → dict` | Use `set()` |
| `True`/`1` collision | `{True, 1} → {True}` | They hash equal |
| No indexing | `s[0] → TypeError` | Use `list(s)[0]` or iterate |
| `add` vs `update` string | `add("hi")` → `{"hi"}`, `update("hi")` → `{"h","i"}` | Choose carefully |
| Mutate during iteration | `for x in s: s.remove(x)` → RuntimeError | Iterate over `s.copy()` |
| Operator needs sets | `s \| [1,2]` → TypeError | Use `s.union([1,2])` or `s \| set([1,2])` |
| Unhashable elements | `{[1,2]}` → TypeError | Use `frozenset` or `tuple` |
| Order not preserved | Insertion order not guaranteed | Use `dict.fromkeys()` or sorted() |
| `==` with `{}` | `set() == {}` → `False` | Compare `set() == set()` |

---

## Complete Methods Reference

| Method | Returns | Mutates? | Description |
|--------|---------|----------|-------------|
| `add(x)` | `None` | Yes | Add element |
| `remove(x)` | `None` | Yes | Remove; KeyError if missing |
| `discard(x)` | `None` | Yes | Remove; silent if missing |
| `pop()` | element | Yes | Remove arbitrary element |
| `clear()` | `None` | Yes | Remove all |
| `copy()` | `set` | No | Shallow copy |
| `update(t)` | `None` | Yes | Add elements from iterable |
| `union(t)` | `set` | No | New set: s ∪ t |
| `intersection(t)` | `set` | No | New set: s ∩ t |
| `difference(t)` | `set` | No | New set: s − t |
| `symmetric_difference(t)` | `set` | No | New set: s △ t |
| `intersection_update(t)` | `None` | Yes | Keep only s ∩ t |
| `difference_update(t)` | `None` | Yes | Remove s ∩ t |
| `symmetric_difference_update(t)` | `None` | Yes | Keep only s △ t |
| `issubset(t)` | `bool` | No | s ⊆ t? |
| `issuperset(t)` | `bool` | No | s ⊇ t? |
| `isdisjoint(t)` | `bool` | No | s ∩ t = ∅? |
