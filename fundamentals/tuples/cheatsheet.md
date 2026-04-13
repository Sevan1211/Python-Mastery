# Tuples — Cheatsheet

## Creating Tuples

```python
empty = ()
empty = tuple()
single = (42,)                   # COMMA REQUIRED — (42) is just int 42
multi = (1, 2, 3)
no_parens = 1, 2, 3             # Packing — still a tuple
from_iter = tuple([1, 2, 3])    # Convert any iterable
from_range = tuple(range(5))    # (0, 1, 2, 3, 4)
from_str = tuple("abc")         # ('a', 'b', 'c')
nested = ((1, 2), (3, 4))
```

## Indexing & Slicing

```python
t = (10, 20, 30, 40, 50)
t[0]      # 10
t[-1]     # 50
t[1:3]    # (20, 30)        — returns a tuple
t[::-1]   # (50, 40, 30, 20, 10)
t[::2]    # (10, 30, 50)
```

## Immutability

```python
t = (1, 2, 3)
t[0] = 99               # TypeError! Tuples are immutable

# BUT: mutable objects INSIDE a tuple can change
t = (1, [2, 3])
t[1].append(4)           # Works! t → (1, [2, 3, 4])

# "Modifying" a tuple = create a new one
t = (1, 2, 3)
t = t[:1] + (99,) + t[2:]   # (1, 99, 3)
```

## Tuple Methods (only 2!)

| Method | Description | Example |
|--------|-------------|---------|
| `t.count(x)` | Count occurrences of x | `(1,2,1).count(1)` → `2` |
| `t.index(x)` | First index of x (raises `ValueError` if missing) | `(10,20,30).index(20)` → `1` |

## Operators

```python
(1, 2) + (3, 4)         # (1, 2, 3, 4)  — concatenation
(1, 2) * 3              # (1, 2, 1, 2, 1, 2)  — repetition
3 in (1, 2, 3)          # True  — membership
(1, 2) < (1, 3)         # True  — lexicographic comparison
(1, 2) == (1, 2)        # True  — equality
len((1, 2, 3))          # 3
```

## Packing & Unpacking

```python
# Packing
coords = 3, 7           # (3, 7)

# Unpacking
x, y = coords           # x=3, y=7

# Star unpacking
first, *rest = (1, 2, 3, 4)     # first=1, rest=[2, 3, 4]  ← list!
*init, last = (1, 2, 3, 4)      # init=[1, 2, 3], last=4
a, *mid, z = (1, 2, 3, 4, 5)    # a=1, mid=[2, 3, 4], z=5

# Ignore with _
_, y, _ = (1, 2, 3)     # y=2

# Swapping
a, b = b, a
```

## Named Tuples

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])
p = Point(3, 7)

p.x              # 3  — field access
p[0]             # 3  — index access
p._replace(x=10) # Point(x=10, y=7)  — returns NEW tuple
p._asdict()       # {'x': 3, 'y': 7}
Point._fields     # ('x', 'y')
```

## Tuples as Dict Keys / Set Elements

```python
# Tuples are hashable (if all elements are hashable)
grid = {}
grid[(0, 0)] = "start"
grid[(1, 2)] = "treasure"

visited = set()
visited.add((3, 4))

# NOT hashable if it contains mutable objects
bad = (1, [2, 3])       # hash(bad) → TypeError
```

## Built-in Functions with Tuples

```python
t = (3, 1, 4, 1, 5)
len(t)           # 5
min(t)           # 1
max(t)           # 5
sum(t)           # 14
sorted(t)        # [1, 1, 3, 4, 5]  — returns a LIST!
tuple(sorted(t)) # (1, 1, 3, 4, 5)  — re-wrap to tuple
any(t)           # True
all(t)           # True
```

## Return Values

```python
def min_max(nums):
    return min(nums), max(nums)

lo, hi = min_max([3, 1, 4])    # lo=1, hi=4
result = min_max([3, 1, 4])    # result=(1, 4)
```

## Traversal Patterns

```python
t = ("a", "b", "c")

# Direct iteration
for item in t:
    print(item)

# With index
for i, item in enumerate(t):
    print(i, item)

# Iterating pairs
pairs = ((1, "x"), (2, "y"), (3, "z"))
for num, letter in pairs:
    print(num, letter)
```

## Tuples vs Lists — When to Use Which

| Use Tuple When | Use List When |
|---------------|---------------|
| Data shouldn't change | Data needs modification |
| Dict keys or set elements | Need append/remove/sort in-place |
| Function return values | Building up a collection |
| Coordinates, RGB, records | Dynamic sequences |
| Performance matters (slightly faster) | Need mutability |

## Complexity

| Operation | Time |
|-----------|------|
| Index `t[i]` | O(1) |
| Slice `t[a:b]` | O(b-a) |
| `in` | O(n) |
| `count()` | O(n) |
| `index()` | O(n) |
| `len()` | O(1) |
| Concatenation `+` | O(n+m) |
| Repetition `*k` | O(nk) |

## Common Gotchas

1. **Single-element trap**: `(5)` is int, `(5,)` is tuple
2. **Accidental trailing comma**: `x = 42,` creates `(42,)` not `42`
3. **`sorted()` returns a list**, not a tuple — wrap with `tuple()`
4. **Star unpacking `*rest` is a list**, not a tuple
5. **`+=` on mutable in tuple**: `t[0] += [3]` both mutates the list AND raises TypeError
6. **Unhashable contents**: `(1, [2])` can't be used as dict key or in sets
7. **`t.index(x)` crashes** if x isn't found — use `try/except` or `in` check first
