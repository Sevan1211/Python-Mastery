# Sets — Review (Spaced Repetition)

Use these prompts to test your recall. Cover the answer, try to answer from memory, then check.

---

## Recall Prompts

### Prompt 1: Empty Set Creation
**Q:** How do you create an empty set in Python? Why can't you use `{}`?
**A:** Use `set()`. Empty `{}` creates a dict, not a set, because `{}` was the dict literal syntax first. This is one of the most common Python gotchas.

### Prompt 2: Set from String
**Q:** What does `set("banana")` produce? Why?
**A:** `{'b', 'a', 'n'}` — three elements. `set()` iterates the string character by character, and duplicates are automatically removed. The two `a`s and two `n`s each collapse to one.

### Prompt 3: Hashability Rule
**Q:** What types can be elements of a set? What types cannot? Why?
**A:** Set elements must be **hashable** (immutable). Allowed: `int`, `float`, `str`, `bool`, `None`, `tuple` (of hashables), `frozenset`. Not allowed: `list`, `dict`, `set`. Sets use a hash table internally — mutable objects can't have stable hashes.

### Prompt 4: True and 1 Collision
**Q:** What is `len({True, 1, False, 0})` and why?
**A:** `2`. In Python, `True == 1` and `False == 0`, and they also have the same hash. So the set sees `True` and `1` as the same element, and `False` and `0` as the same element.

### Prompt 5: Membership Testing Time Complexity
**Q:** What is the time complexity of `x in my_set` vs `x in my_list`?
**A:** Set: **O(1)** average (hash lookup). List: **O(n)** (linear scan). This is the #1 reason to use sets — for fast membership testing.

### Prompt 6: add() vs update()
**Q:** What's the difference between `s.add("hello")` and `s.update("hello")`?
**A:** `add("hello")` adds the string `"hello"` as a single element → `{"hello"}`. `update("hello")` iterates the string and adds each character → `{"h", "e", "l", "o"}`. `update()` always iterates its argument.

### Prompt 7: remove() vs discard()
**Q:** What's the difference between `remove()` and `discard()`?
**A:** Both remove an element, but `remove()` raises `KeyError` if the element is missing, while `discard()` silently does nothing. Use `discard()` when you're not sure the element exists.

### Prompt 8: Union Operation
**Q:** How do you combine all elements from two sets? Give two syntaxes.
**A:** Operator: `a | b`. Method: `a.union(b)`. The method also accepts non-set iterables: `a.union([1, 2])`. The operator requires both sides to be sets.

### Prompt 9: Intersection Operation
**Q:** How do you find elements common to two sets? Give two syntaxes.
**A:** Operator: `a & b`. Method: `a.intersection(b)`. Returns a new set containing only elements present in both.

### Prompt 10: Difference Operation
**Q:** What does `a - b` return? Is it the same as `b - a`?
**A:** `a - b` returns elements in `a` but NOT in `b`. It is **not** the same as `b - a` — difference is not commutative. `{1,2,3} - {2,3,4} = {1}` but `{2,3,4} - {1,2,3} = {4}`.

### Prompt 11: Symmetric Difference
**Q:** What does `a ^ b` return? What's it equivalent to?
**A:** Elements in either `a` or `b` but **not both**. Equivalent to `(a | b) - (a & b)` or `(a - b) | (b - a)`. Think of it as XOR for sets.

### Prompt 12: Subset Check
**Q:** How do you check if set `a` is a subset of set `b`? What about proper subset?
**A:** Subset: `a <= b` or `a.issubset(b)`. Proper subset (must be smaller): `a < b`. A set is a subset of itself (`a <= a` is True) but not a proper subset of itself (`a < a` is False).

### Prompt 13: Disjoint Check
**Q:** How do you check if two sets share no elements?
**A:** `a.isdisjoint(b)` returns `True` if the intersection is empty. Equivalent to `len(a & b) == 0` but more readable and efficient.

### Prompt 14: In-Place Operations
**Q:** Name the four in-place set operation operators.
**A:** `|=` (union update), `&=` (intersection update), `-=` (difference update), `^=` (symmetric difference update). Each modifies the left-hand set in place instead of creating a new one.

### Prompt 15: Set Comprehension Syntax
**Q:** Write a set comprehension that gets unique word lengths from a list of words.
**A:** `{len(w) for w in words}` — same as list comprehension but with `{}`. Automatically deduplicates.

### Prompt 16: Frozenset Purpose
**Q:** What is a `frozenset` and when would you use one?
**A:** An immutable set. Use when you need a set as a dict key, as an element of another set, or when you want to guarantee the collection won't be modified. Supports all read operations but not add/remove/clear.

### Prompt 17: Order-Preserving Deduplication
**Q:** How do you remove duplicates from a list while preserving order?
**A:** Use a `seen` set and build a result list:
```python
seen = set()
result = []
for item in items:
    if item not in seen:
        seen.add(item)
        result.append(item)
```

### Prompt 18: Contains Duplicate Pattern
**Q:** What's the one-line way to check if a list has duplicates?
**A:** `len(nums) != len(set(nums))` — if the set is smaller, there were duplicates.

### Prompt 19: When Set vs List vs Dict
**Q:** When should you choose a set over a list? Over a dict?
**A:** Choose set over list when: you need fast membership testing (O(1) vs O(n)), you need deduplication, you need mathematical set operations, and you don't need ordering or indexing. Choose dict over set when: you need to associate values with keys, not just track membership.

### Prompt 20: Operator vs Method Difference
**Q:** What's the key difference between set operators (`|`, `&`, `-`, `^`) and their method equivalents?
**A:** Operators require **both sides to be sets** — `s | [1,2]` raises `TypeError`. Methods accept **any iterable** — `s.union([1,2])` works fine. Use methods when working with non-set iterables.

---

## Common Failure Points

### 1. Empty Set: `{}` Creates a Dict

```python
# BUG
empty = {}
empty.add(1)  # AttributeError: 'dict' has no attribute 'add'

# FIX
empty = set()
empty.add(1)  # Works ✓
```
**Prevention:** Always use `set()` for empty sets. `{}` is always a dict.

### 2. Modifying Set During Iteration

```python
# BUG
s = {1, 2, 3, 4, 5}
for x in s:
    if x % 2 == 0:
        s.remove(x)  # RuntimeError: Set changed size during iteration

# FIX — iterate over a copy
for x in s.copy():
    if x % 2 == 0:
        s.remove(x)

# FIX — use comprehension (preferred)
s = {x for x in s if x % 2 != 0}
```
**Prevention:** Never modify a set while iterating over it. Use `.copy()` or rebuild with a comprehension.

### 3. Using `update()` Instead of `add()` with a String

```python
# BUG — wanted to add "hello" as one element
s = set()
s.update("hello")
print(s)  # {'h', 'e', 'l', 'o'} — wrong!

# FIX
s = set()
s.add("hello")
print(s)  # {'hello'} — correct
```
**Prevention:** `add()` for single elements, `update()` for iterables. Strings are iterables of characters.

### 4. Operator Requires Sets on Both Sides

```python
# BUG
s = {1, 2, 3}
result = s | [4, 5]  # TypeError: unsupported operand type(s)

# FIX — use method
result = s.union([4, 5])

# FIX — convert to set
result = s | set([4, 5])
```
**Prevention:** Use `.union()`, `.intersection()`, etc. when the other operand might not be a set.

### 5. Trying to Index a Set

```python
# BUG
s = {10, 20, 30}
first = s[0]  # TypeError: 'set' object is not subscriptable

# FIX — convert to list or use next(iter())
first = next(iter(s))     # One arbitrary element
first = sorted(s)[0]      # Smallest element
all_items = list(s)        # Full list (arbitrary order)
```
**Prevention:** Sets have no order. If you need indices, use a list. If you need one element, use `next(iter(s))` or `.pop()`.

### 6. Unhashable Elements

```python
# BUG
s = {[1, 2], [3, 4]}  # TypeError: unhashable type: 'list'

# FIX — use tuples (immutable)
s = {(1, 2), (3, 4)}

# FIX — use frozenset for set-of-sets
groups = {frozenset({1, 2}), frozenset({3, 4})}
```
**Prevention:** Lists → tuples, sets → frozensets, dicts → frozensets of items.

### 7. Assuming Set Preserves Insertion Order

```python
# BUG — relying on iteration order
s = set()
for x in [5, 3, 1, 4, 2]:
    s.add(x)
first = list(s)[0]  # NOT guaranteed to be 5!

# FIX — use a list if order matters
ordered = []
seen = set()
for x in [5, 3, 1, 4, 2]:
    if x not in seen:
        seen.add(x)
        ordered.append(x)
```
**Prevention:** Unlike dicts (Python 3.7+), sets do NOT preserve insertion order. Use list + set combo for ordered unique elements.

### 8. remove() on Missing Element

```python
# BUG
s = {1, 2, 3}
s.remove(5)  # KeyError: 5

# FIX — use discard() for safe removal
s.discard(5)  # No error, s unchanged

# FIX — check first
if 5 in s:
    s.remove(5)
```
**Prevention:** Default to `discard()` unless you specifically want `KeyError` for missing elements.

### 9. Comparing Set with Empty Dict

```python
# BUG
s = set()
if s == {}:
    print("empty!")  # NEVER prints — {} is a dict!

# FIX
if s == set():
    print("empty!")  # Works

# BETTER FIX — Pythonic emptiness check
if not s:
    print("empty!")  # Works ✓
```
**Prevention:** Use `not s` to check for empty set. Never compare with `{}`.

### 10. Forgetting set() Copies Are Shallow

```python
# BUG (only matters with mutable elements in frozenset scenarios)
# For simple sets with immutable elements, copy() is sufficient
original = {1, 2, 3}
copy = original.copy()
copy.add(4)
print(original)  # {1, 2, 3} — this IS correct for simple sets

# The shallow vs deep issue matters more with dicts/lists
# Sets can only contain immutable elements, so shallow copy = deep copy
# for sets (unlike lists of lists or dicts)
```
**Note:** Since set elements must be hashable (immutable), shallow copy behaves like deep copy for sets. This is different from lists of lists where shallow vs deep matters.
