# Loops & Conditionals — Cheatsheet

## Conditionals

### if / elif / else

```python
if condition:
    ...
elif other_condition:
    ...
else:
    ...
```

- Evaluated **top-down** — first `True` branch wins
- `elif` is optional, can have many
- `else` is optional, catches everything remaining
- Only **one** branch executes

### Ternary (Conditional Expression)

```python
value = x if condition else y
```

- One-line `if`/`else` that **returns a value**
- Nests: `a if c1 else b if c2 else c` (avoid deep nesting)

### match / case (Python 3.10+)

```python
match value:
    case pattern1:
        ...
    case pattern2 if guard:
        ...
    case _:
        ...  # default
```

- `_` is wildcard (default)
- `|` for OR patterns: `case "a" | "b":`
- Can destructure: `case [x, y]:`
- Guards: `case x if x > 0:`

---

## Truthiness

| Falsy | Truthy |
|-------|--------|
| `False`, `None` | `True` |
| `0`, `0.0`, `0j` | Any non-zero number |
| `""` (empty string) | Any non-empty string |
| `[]`, `()`, `{}`, `set()` | Any non-empty collection |
| `range(0)` | Non-empty range |

**Pythonic pattern:** `if items:` instead of `if len(items) > 0:`

---

## Comparison Operators

| Operator | Meaning |
|----------|---------|
| `==` | Equal value |
| `!=` | Not equal value |
| `<`, `>`, `<=`, `>=` | Ordering |
| `is` / `is not` | Identity (same object) |
| `in` / `not in` | Membership |

- **Chained:** `10 < x < 20` works in Python
- Use `is` only for `None`, `True`, `False`

---

## Logical Operators

| Operator | Returns |
|----------|---------|
| `a and b` | First falsy, or last if all truthy |
| `a or b` | First truthy, or last if all falsy |
| `not a` | `True` if `a` is falsy |

- **Precedence:** `not` > `and` > `or`
- **Short-circuit:** stops evaluating early
- **Default values:** `name = user_input or "Anonymous"`

---

## for Loop

```python
for item in iterable:
    ...
```

Iterates over **any iterable**: list, tuple, str, dict, set, range, file, generator.

### Iterating Different Types

```python
# List/Tuple/String
for x in [1, 2, 3]: ...

# Dict keys (default)
for key in d: ...

# Dict values
for val in d.values(): ...

# Dict key-value pairs
for k, v in d.items(): ...

# Set (unordered)
for x in {1, 2, 3}: ...
```

---

## range()

| Form | Output |
|------|--------|
| `range(5)` | 0, 1, 2, 3, 4 |
| `range(2, 6)` | 2, 3, 4, 5 |
| `range(0, 10, 2)` | 0, 2, 4, 6, 8 |
| `range(5, 0, -1)` | 5, 4, 3, 2, 1 |

- `stop` is **exclusive**
- `range` is **lazy** — O(1) memory regardless of size
- `x in range(...)` is O(1)

---

## enumerate()

```python
for i, val in enumerate(iterable, start=0):
    ...
```

- Returns `(index, value)` tuples
- **Always prefer** over `range(len(...))`
- `start` changes the index counter, not which elements are yielded

---

## zip()

```python
for a, b in zip(iter1, iter2):
    ...
```

- Pairs elements by position
- Stops at **shortest** iterable
- Create dict: `dict(zip(keys, values))`
- Unzip: `a, b = zip(*pairs)`
- For longest: `itertools.zip_longest(a, b, fillvalue=None)`

---

## while Loop

```python
while condition:
    ...
```

- Repeats as long as `condition` is `True`
- Must ensure condition eventually becomes `False`
- Common pattern: `while True: ... if done: break`

---

## Flow Control

| Keyword | Effect |
|---------|--------|
| `break` | Exit **innermost** loop immediately |
| `continue` | Skip to **next** iteration |
| `pass` | Do nothing (placeholder) |

### Loop else

```python
for x in iterable:
    if condition:
        break
else:
    # Runs only if NO break occurred
    ...
```

---

## Nested Loops

```python
for i in range(n):
    for j in range(m):
        ...  # O(n * m)
```

- Inner loop completes fully per outer iteration
- `break` only exits innermost loop
- To exit outer: use a flag variable or wrap in a function

---

## Common Patterns

### Accumulator
```python
total = 0
for x in nums:
    total += x
```

### Filter/Collect
```python
result = []
for x in items:
    if condition(x):
        result.append(x)
```

### Max/Min Tracking
```python
best = nums[0]
for x in nums[1:]:
    if x > best:
        best = x
```

### Frequency Count
```python
freq = {}
for x in items:
    freq[x] = freq.get(x, 0) + 1
```

### Two Pointers
```python
left, right = 0, len(arr) - 1
while left < right:
    # process arr[left] and arr[right]
    left += 1
    right -= 1
```

### Index-Based Pairs
```python
for i in range(len(arr)):
    for j in range(i + 1, len(arr)):
        # process (arr[i], arr[j])
```

---

## Walrus Operator `:=` (Python 3.8+)

```python
if (n := len(data)) > 10:
    print(f"Large: {n}")

while (line := input()) != "quit":
    process(line)
```

Assigns **and** returns value in one expression.

---

## Gotchas

| Gotcha | Problem | Fix |
|--------|---------|-----|
| Modify list during iteration | Skips elements | Iterate over a copy or use comprehension |
| `range(len())` | Verbose, error-prone | Use `enumerate()` |
| Off-by-one in `range()` | `range(5)` → 0..4, not 0..5 | Remember stop is exclusive |
| Infinite `while` | Forgot to update condition | Always update loop variable |
| `is` vs `==` | `is` checks identity, not value | Use `==` for values; `is` only for `None` |
| Loop variable leaks | Variable persists after loop | Be aware; don't rely on it |
| Float `==` comparison | `0.1+0.2 != 0.3` | Use `math.isclose()` |
| String `+=` in loop | O(n²) for large strings | Use `"".join(parts)` |
| `break` in nested loop | Only breaks inner loop | Use flag or function |

---

## Time Complexity

| Operation | Complexity |
|-----------|-----------|
| Single `for` over n items | O(n) |
| Nested `for` (n × m) | O(n·m) |
| `x in list` inside loop | O(n²) total |
| `x in set` inside loop | O(n) total |
| `enumerate()` | O(n) — same as plain for |
| `zip()` | O(min(n, m)) |
| `range()` creation | O(1) |
| `"".join(list)` | O(n) |
| String `+=` in loop | O(n²) |
