# Lists — Cheatsheet

## Creating Lists

```python
lst = [1, 2, 3]             # literal
lst = []                     # empty (literal)
lst = list()                 # empty (constructor)
lst = list("abc")            # from string → ['a', 'b', 'c']
lst = list(range(5))         # from range → [0, 1, 2, 3, 4]
lst = "a b c".split()        # from split → ['a', 'b', 'c']
lst = [0] * 5                # repetition → [0, 0, 0, 0, 0]
lst = [x**2 for x in range(5)]  # comprehension → [0, 1, 4, 9, 16]
```

**Gotcha:** `[[0]*3]*3` creates 3 references to the SAME inner list. Use `[[0]*3 for _ in range(3)]` instead.

---

## Indexing

```python
lst[0]     # first element
lst[-1]    # last element
lst[-2]    # second to last
lst[i]     # i-th element (0-based)
```

**Out of bounds → `IndexError`** (unlike slicing which is safe).

---

## Slicing

```python
lst[start:stop]       # start (inclusive) to stop (exclusive)
lst[start:stop:step]  # with step
lst[:3]               # first 3
lst[3:]               # from index 3 to end
lst[:]                # full shallow copy
lst[::2]              # every other element
lst[::-1]             # reversed copy
lst[100:200]          # out of bounds → [] (no error)
```

### Slice Assignment (Mutating)

```python
lst[1:3] = [20, 30]       # replace range
lst[2:2] = [99, 98]       # insert at index 2
lst[1:4] = []              # delete range
```

---

## Mutability

Lists are **mutable** — you can change elements in place.

```python
lst[0] = "new"             # replace element
lst[1:3] = [10, 20, 30]   # replace slice (can change size)
```

**Key rule:** Many methods return `None` because they mutate in place:
`append()`, `insert()`, `extend()`, `remove()`, `sort()`, `reverse()`, `clear()`

---

## Adding Elements

| Method | What it does | Time | Returns |
|--------|-------------|------|---------|
| `lst.append(x)` | Add x to end | O(1) | `None` |
| `lst.insert(i, x)` | Insert x at index i | O(n) | `None` |
| `lst.extend(iter)` | Add all items from iterable | O(k) | `None` |
| `lst += [x, y]` | Same as extend | O(k) | — |

```python
lst.append([1, 2])   # adds [1,2] as ONE nested element
lst.extend([1, 2])   # adds 1 and 2 as separate elements
```

---

## Removing Elements

| Method | What it does | Time | Returns |
|--------|-------------|------|---------|
| `lst.remove(x)` | Remove first occurrence of x | O(n) | `None` |
| `lst.pop()` | Remove & return last | O(1) | removed item |
| `lst.pop(i)` | Remove & return item at i | O(n) | removed item |
| `del lst[i]` | Delete by index | O(n) | nothing |
| `del lst[a:b]` | Delete slice | O(n) | nothing |
| `lst.clear()` | Remove all items | O(n) | `None` |

**`remove()` raises `ValueError` if not found. `pop()` raises `IndexError` if empty.**

---

## Searching

| Method | What it does | Returns | Error? |
|--------|-------------|---------|--------|
| `x in lst` | Membership check | `bool` | No |
| `lst.index(x)` | Index of first x | `int` | `ValueError` if missing |
| `lst.index(x, start)` | Search from start | `int` | `ValueError` if missing |
| `lst.count(x)` | Number of x's | `int` | No |

**Safe pattern:**
```python
if x in lst:
    idx = lst.index(x)
```

---

## Sorting

| | `.sort()` | `sorted()` |
|---|---|---|
| Returns | `None` | New list |
| Mutates | Yes | No |
| Works on | Lists only | Any iterable |

```python
lst.sort()                     # in-place, ascending
lst.sort(reverse=True)         # in-place, descending
lst.sort(key=len)              # in-place, by key function
sorted(lst)                    # new sorted list
sorted(lst, key=str.lower)     # case-insensitive
sorted(lst, key=lambda x: x[1])  # by second element
```

---

## Reversing

| Method | Mutates? | Returns |
|--------|----------|---------|
| `lst.reverse()` | Yes | `None` |
| `reversed(lst)` | No | Iterator |
| `lst[::-1]` | No | New list |

---

## Copying

| Method | Type | Syntax |
|--------|------|--------|
| Shallow copy | Outer only | `lst[:]`, `lst.copy()`, `list(lst)` |
| Deep copy | Full recursive | `import copy; copy.deepcopy(lst)` |

**Shallow copies share inner mutable objects!**

```python
# Shallow: inner lists are still references
a = [[1, 2], [3, 4]]
b = a[:]
b[0].append(99)    # BOTH a[0] and b[0] change!
```

---

## Common Built-ins

```python
len(lst)            # length
min(lst)            # smallest
max(lst)            # largest
sum(lst)            # sum of numbers
any(lst)            # True if any truthy
all(lst)            # True if all truthy
enumerate(lst)      # (index, value) pairs
zip(a, b)           # parallel iteration (stops at shortest)
map(func, lst)      # apply func to each
filter(func, lst)   # keep items where func returns True
reversed(lst)       # reverse iterator
```

---

## Unpacking

```python
a, b, c = [1, 2, 3]                # basic
first, *rest = [1, 2, 3, 4]        # star: first=1, rest=[2,3,4]
*start, last = [1, 2, 3, 4]        # star: start=[1,2,3], last=4
first, *mid, last = [1, 2, 3, 4]   # star: first=1, mid=[2,3], last=4
a, b = b, a                         # swap
```

---

## List Comprehensions

```python
[x for x in lst]                    # copy
[x * 2 for x in lst]                # transform
[x for x in lst if x > 0]           # filter
[x if x > 0 else 0 for x in lst]    # transform with condition
[x for row in matrix for x in row]  # flatten 2D
```

---

## Traversal Patterns

| Pattern | Code |
|---------|------|
| By value | `for x in lst:` |
| By index | `for i in range(len(lst)):` |
| Enumerated | `for i, x in enumerate(lst):` |
| Reverse | `for x in reversed(lst):` |
| Reverse index | `for i in range(len(lst)-1, -1, -1):` |
| Pairs (zip) | `for a, b in zip(lst1, lst2):` |
| Neighbors | `for i in range(len(lst)-1): lst[i], lst[i+1]` |
| Accumulator | `total = 0; for x in lst: total += x` |

---

## Time Complexity

| Operation | Time |
|-----------|------|
| `lst[i]` | O(1) |
| `lst.append(x)` | O(1) amortized |
| `lst.pop()` | O(1) |
| `lst.pop(0)` | O(n) |
| `lst.insert(0, x)` | O(n) |
| `lst.remove(x)` | O(n) |
| `x in lst` | O(n) |
| `lst.index(x)` | O(n) |
| `lst.sort()` | O(n log n) |
| `lst[a:b]` | O(b-a) |
| `len(lst)` | O(1) |

---

## Common Gotchas

| Gotcha | What happens | Fix |
|--------|-------------|-----|
| `result = lst.sort()` | `result` is `None` | Use `sorted(lst)` |
| `b = a` (aliasing) | Both point to same list | Use `a[:]` or `a.copy()` |
| `def f(lst=[])` | Default list shared across calls | Use `lst=None` |
| `[[0]*3]*3` | All rows are same object | Use comprehension |
| Mutating during `for` loop | Skips elements | Iterate over copy or use comprehension |
| `lst += [x]` vs `lst = lst + [x]` | `+=` mutates, `+` creates new | Know which you want |
| Empty list is falsy | `if []:` → `False` | Use `if not lst:` |
| `in` checks top-level only | `1 in [[1,2]]` → `False` | Check nested with `any()` |
