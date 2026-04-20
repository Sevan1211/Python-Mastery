# List Comprehensions — Cheatsheet

Fast-recall reference for list, set, dict, and generator comprehensions.

---

## Syntax Skeleton

```python
[ expression  for var in iterable  if condition ]
   ^output       ^source             ^optional filter
```

| Brackets | Builds |
|----------|--------|
| `[ ... ]` | **list** |
| `{ ... }` (no `:`) | **set** |
| `{ k: v ... }` | **dict** |
| `( ... )` | **generator** (lazy) |

---

## Filter vs Transform — the key distinction

| Where the `if` goes | What it does | Shape |
|---------------------|--------------|-------|
| **After `for`** | **Filters** — drops items | `[x for x in xs if cond]` |
| **Before `for`** (with `else`) | **Transforms** — keeps every item, picks value | `[a if cond else b for x in xs]` |
| Both combined | Filter then transform | `[a if cond else b for x in xs if keep]` |

---

## Equivalents

```python
# Map
[f(x) for x in xs]               # ↔ list(map(f, xs))

# Filter
[x for x in xs if pred(x)]       # ↔ list(filter(pred, xs))

# Map + filter
[f(x) for x in xs if pred(x)]
```

---

## All four comprehension types

```python
[x*x for x in nums]              # list
{x*x for x in nums}              # set (deduped)
{x: x*x for x in nums}           # dict
(x*x for x in nums)              # generator (lazy)
```

---

## Nested loops vs nested comprehensions

```python
# Nested LOOPS — order is left→right, outer→inner. Produces a flat list.
[(r, c) for r in range(3) for c in range(3)]

# Nested COMPREHENSION — outer expression is itself a comprehension.
# Produces a list of lists.
[[r*c for c in range(3)] for r in range(3)]
```

---

## Common Patterns (memorise these)

```python
# Map
doubled  = [x * 2 for x in xs]

# Filter
evens    = [x for x in xs if x % 2 == 0]

# Map + filter
squared_evens = [x*x for x in xs if x % 2 == 0]

# Conditional transform (keep all)
clamped  = [x if x >= 0 else 0 for x in xs]

# Flatten 2D
flat     = [x for row in matrix for x in row]

# Transpose
trans    = [list(c) for c in zip(*matrix)]

# 2D zeros (NEVER use [[0]*n]*m)
grid     = [[0]*cols for _ in range(rows)]

# Identity matrix
I        = [[1 if r == c else 0 for c in range(n)] for r in range(n)]

# Cartesian product
pairs    = [(a, b) for a in A for b in B]

# Triangular pairs (i < j)
pairs    = [(i, j) for i in range(n) for j in range(i+1, n)]

# Indices of every match
idxs     = [i for i, x in enumerate(xs) if x == target]

# Element-wise op on parallel lists
sums     = [a + b for a, b in zip(A, B)]

# Differences (sliding pairs)
diffs    = [b - a for a, b in zip(xs, xs[1:])]

# Project records
names    = [r["name"] for r in records if r["active"]]

# Index records by id
by_id    = {r["id"]: r for r in records}

# Invert dict
inv      = {v: k for k, v in d.items()}

# Filter dict by value
high     = {k: v for k, v in d.items() if v >= threshold}

# Dedupe (preserves order, Python 3.7+)
unique   = list(dict.fromkeys(items))

# Frequency map (tiny inputs only — O(n²); use Counter for real data)
freq     = {ch: s.count(ch) for ch in set(s)}
```

---

## Generator Expressions

```python
sum(x*x for x in xs)             # parens optional when sole arg
any(x > 0 for x in xs)           # short-circuits
all(x > 0 for x in xs)           # short-circuits
max(len(w) for w in words)
min(d['age'] for d in records)
```

Use a generator when:
- aggregating (sum / min / max / any / all)
- iterating once
- data is huge

Use a list when:
- you need indexing, len(), or multiple passes

---

## Walrus `:=` (Python 3.8+)

Compute once, use in both filter and output:

```python
[y for x in data if (y := expensive(x)) > threshold]
```

---

## Performance Notes

- Comprehension > manual `for + append` (1.5–2× faster)
- Generator > list when you only iterate once (constant memory)
- `sum([... ])` → drop the brackets, use `sum(...)` directly
- `{ch: s.count(ch) for ch in s}` is **O(n²)**. Use `collections.Counter(s)`

---

## Anti-Patterns (Don't Do)

| Bad | Why | Fix |
|-----|-----|-----|
| `[print(x) for x in xs]` | Side effect → builds `[None, None, ...]` | Use a `for` loop |
| `[[0]*n]*m` | All rows are the same object | `[[0]*n for _ in range(m)]` |
| `{x for x in xs}` when you wanted dict | Forgot the `:` → it's a set | `{x: 0 for x in xs}` |
| `[x for x in xs if cond1 else cond2 ...]` | Invalid syntax — `else` doesn't go after `for` | Move `if/else` before `for` |
| Triple-nested comprehensions | Unreadable | Refactor with helper or loop |
| `sum([x*x for x in big])` | Wastes memory | `sum(x*x for x in big)` |
| Mutating source in the comprehension | Logic bugs | Build a new list |

---

## When NOT to Use a Comprehension

- Side effects (printing, IO, mutation) → use a `for` loop
- Need `try/except`, `break`, or `continue` → can't be done
- More than ~2 levels of nesting → unreadable
- Logic exceeds one logical line → readability suffers
- You don't need the result → use a generator or loop

---

## Mental Cheats

- "transform every item, drop none" → `[expr for ...]` (no if)
- "drop some items" → `if` goes **after** `for`
- "keep all, but change some values" → `a if cond else b` **before** `for`
- "build a 2D grid" → outer dimension MUST use a comprehension
- "memory-light aggregate" → generator expression (parens)
- "unique" → set comprehension `{ }`
- "key→value mapping" → dict comprehension `{ : }`

---

## Built-ins You Pair With Comprehensions

| Builtin | Use |
|---------|-----|
| `enumerate(xs)` | when you need index + value |
| `zip(a, b)` | parallel iteration over equal-length iterables |
| `range(n)` | numeric ranges |
| `reversed(xs)` | iterate backwards |
| `sorted(xs, key=...)` | wrap a comprehension's output |
| `set(xs)` / `dict.fromkeys` | dedupe |
| `sum / min / max / any / all` | aggregate generator output |
| `collections.Counter` | when freq map is needed (use *instead* of comp) |
