# Sorting — Cheatsheet

Fast-recall reference for Python sorting.

---

## The Two Ways

| | `sorted(it, ...)` | `list.sort(...)` |
|---|---|---|
| Returns | new list | `None` |
| Mutates input | no | yes |
| Works on | any iterable | lists only |

```python
new_list = sorted(xs)        # don't lose the original
xs.sort()                    # don't make a copy
```

> ⚠️ `result = xs.sort()` is the #1 bug. `.sort()` returns `None`.

---

## Parameters

```python
sorted(iterable, *, key=None, reverse=False)
list.sort(*, key=None, reverse=False)
```

| Param | What |
|-------|------|
| `key` | function called on each item; the result is what gets compared |
| `reverse` | `True` for descending |

---

## The `key` Parameter — Patterns

```python
# By length
sorted(words, key=len)

# By absolute value
sorted(nums, key=abs)

# Case-insensitive
sorted(words, key=str.lower)

# By tuple field
sorted(pairs, key=lambda p: p[1])

# By dict field
sorted(records, key=lambda r: r["age"])

# By object attribute
from operator import attrgetter
sorted(objs, key=attrgetter("score"))

# Multi-field (primary, secondary, ...)
sorted(data, key=lambda x: (x.dept, x.name))

# Mixed asc/desc (numeric secondary)
sorted(data, key=lambda x: (x.dept, -x.salary))
```

---

## `operator` helpers (faster than lambdas)

```python
from operator import itemgetter, attrgetter

sorted(rows, key=itemgetter("age"))
sorted(rows, key=itemgetter("age", "name"))    # multi-field
sorted(objs, key=attrgetter("score"))
sorted(objs, key=attrgetter("addr.city"))      # nested
```

---

## Mixed Asc / Desc (3 techniques)

1. **Negate** numeric fields: `key=lambda x: (x.dept, -x.salary)`
2. **Two-pass** (relies on stability): sort by secondary first, primary second
   ```python
   step1 = sorted(data, key=lambda x: x.salary, reverse=True)
   final = sorted(step1, key=lambda x: x.dept)
   ```
3. **Custom comparator** with `functools.cmp_to_key` (slow, last resort)

---

## Stability

Python sort is **always stable** — equal-key items keep their relative order. This is what makes the two-pass technique work.

---

## Custom Comparator (rare)

```python
from functools import cmp_to_key

def cmp(a, b):
    if a + b > b + a: return -1
    if a + b < b + a: return  1
    return 0

sorted(strs, key=cmp_to_key(cmp))
```

Return: negative → a first, positive → b first, zero → equal.

---

## Sorting Common Containers

```python
# String → list of chars (rejoin if needed)
"".join(sorted("python"))         # 'hnopty'

# Tuple → list (sorted always returns list)
sorted((3, 1, 2))                 # [1, 2, 3]

# Set → list
sorted({3, 1, 2})

# Dict → list of KEYS
sorted({"b":1, "a":2})            # ['a', 'b']

# Dict by VALUE → list of items
sorted(d.items(), key=lambda kv: kv[1])

# Dict in sorted order (3.7+ preserves insertion)
dict(sorted(d.items(), key=lambda kv: -kv[1]))
```

---

## Top-K (use a heap, not sort)

```python
import heapq
heapq.nlargest(k, xs)      # O(n log k) — beats sort for K << n
heapq.nsmallest(k, xs)
heapq.nlargest(k, xs, key=lambda x: x.score)
```

For full sort:    O(n log n)
For top-K via heap: O(n log k)

---

## Timsort — Python's Algorithm

| Property | Value |
|----------|-------|
| Best | O(n) on already-sorted data |
| Avg / Worst | O(n log n) |
| Space | O(n) auxiliary |
| **Stable** | yes |
| Adaptive | yes — exploits existing runs |

---

## Classic Algorithms — Memorise

| Algorithm | Avg | Worst | Space | Stable | In-place |
|-----------|-----|-------|-------|--------|----------|
| Bubble    | O(n²) | O(n²) | O(1) | yes | yes |
| Insertion | O(n²) | O(n²) | O(1) | yes | yes |
| Selection | O(n²) | O(n²) | O(1) | no  | yes |
| Merge     | O(n log n) | O(n log n) | O(n) | yes | no |
| Quick     | O(n log n) | O(n²) | O(log n) | no  | yes |
| Heap      | O(n log n) | O(n log n) | O(1) | no  | yes |
| Timsort   | O(n log n) | O(n log n) | O(n) | yes | mostly |
| Counting  | O(n+k) | O(n+k) | O(k) | yes | no |
| Radix     | O(nk) | O(nk) | O(n+k) | yes | no |

**Quick recall:**
- *Stable comparison sort?* → mergesort, insertion, bubble, **Timsort**
- *In-place + O(n log n) worst?* → heapsort
- *In-place + O(n log n) average but O(n²) worst?* → quicksort
- *Beats O(n log n)?* → counting / radix (non-comparison, integer-range)

---

## Common Interview Patterns

```python
# Top K
heapq.nlargest(k, nums)

# Frequency sort
from collections import Counter
sorted(Counter(s).items(), key=lambda kv: (-kv[1], kv[0]))

# Group anagrams
from collections import defaultdict
g = defaultdict(list)
for w in words: g["".join(sorted(w))].append(w)

# Merge intervals
intervals.sort(key=lambda x: x[0])
out = []
for s, e in intervals:
    if out and s <= out[-1][1]: out[-1][1] = max(out[-1][1], e)
    else: out.append([s, e])

# Meeting rooms
starts, ends = sorted(m[0] for m in M), sorted(m[1] for m in M)
# two-pointer sweep

# Custom alphabet
order = {ch: i for i, ch in enumerate(alphabet)}
sorted(words, key=lambda w: [order[c] for c in w])

# Largest concat number
sorted(map(str, nums), key=cmp_to_key(lambda a,b: -1 if a+b > b+a else 1))
```

---

## Anti-Patterns

| Bad | Why | Fix |
|-----|-----|-----|
| `result = xs.sort()` | returns `None` | `result = sorted(xs)` |
| `sorted(xs)[0]` for min | O(n log n) | `min(xs)` — O(n) |
| `sorted(xs)[:k]` for top-k on huge data | O(n log n) | `heapq.nsmallest(k, xs)` — O(n log k) |
| `sorted([1, "a"])` | TypeError in Py3 | provide a key that returns same-typed values |
| `sorted([3, None, 1])` | TypeError | filter Nones, or `key=lambda x: (x is None, x)` |
| `sorted(["10","2"])` for numeric | lexicographic order | `key=int` |
| `cmp_to_key` everywhere | slow | use `key=` whenever possible |

---

## Mental Cheats

- "Need a new list?" → `sorted()`
- "Already a list and don't need original?" → `.sort()`
- "Sort by X?" → `key=...`
- "Multiple fields?" → `key=lambda x: (a, b, c)`
- "Mixed asc/desc?" → negate numeric or two-pass for non-numeric
- "Top K?" → `heapq.nlargest(k, ...)`
- "Sort by frequency?" → `Counter` + `key=lambda kv: (-kv[1], kv[0])`
- "Group anagrams?" → bucket by `"".join(sorted(word))`
