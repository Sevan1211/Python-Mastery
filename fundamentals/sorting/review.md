# Sorting — Review

Spaced repetition prompts and common failure points. Cover answers, recall, then check.

---

## Spaced Repetition Prompts

### Q1: What's the difference between `sorted()` and `list.sort()`?
**A:** `sorted(it)` returns a **new** list and works on any iterable. `list.sort()` mutates the list **in place** and returns **`None`**. Never write `xs = xs.sort()`.

### Q2: What does the `key` parameter do?
**A:** It takes a function called once per item; the function's return value is what gets compared. The original items are sorted, but ordering is decided by the keys.

### Q3: How do you sort by multiple fields?
**A:** Return a tuple from the `key` function. Tuples compare lexicographically: primary, secondary, tertiary, etc. Example: `key=lambda x: (x.dept, x.name)`.

### Q4: How do you mix ascending and descending sort fields?
**A:** Three techniques:
1. **Negate numeric fields** in the tuple: `(x.dept, -x.salary)`.
2. **Two-pass sort** (relies on stability): sort by secondary first, then primary.
3. **`functools.cmp_to_key`** with a custom comparator (slow — last resort).

### Q5: Is Python's sort stable?
**A:** Yes — always. Equal-key items keep their original relative order. This is why two-pass sorting works.

### Q6: What sorting algorithm does Python use, and what are its key properties?
**A:** **Timsort**. O(n log n) worst-case, **O(n) on already-sorted input**, stable, adaptive, uses O(n) auxiliary space. Hybrid of merge sort and insertion sort.

### Q7: What's the time complexity of `sorted(xs)[:k]` vs `heapq.nlargest(k, xs)`?
**A:** `sorted(xs)[:k]` is **O(n log n)**. `heapq.nlargest(k, xs)` is **O(n log k)** — much faster when k is small relative to n.

### Q8: How do you sort a dict by value descending?
**A:** `sorted(d.items(), key=lambda kv: kv[1], reverse=True)` — returns a list of (key, value) pairs. To get a dict back: wrap in `dict(...)` (Python 3.7+ preserves insertion order).

### Q9: What does `sorted(["10", "2", "1"])` return, and how do you fix it?
**A:** `['1', '10', '2']` — lexicographic order. To sort numerically: `sorted(strs, key=int)`.

### Q10: How do you sort a list of objects by an attribute?
**A:** `sorted(objs, key=lambda o: o.attr)` or, faster, `sorted(objs, key=attrgetter("attr"))` from the `operator` module.

### Q11: What's the difference between `itemgetter` and `attrgetter`?
**A:** `itemgetter(k)` accesses by index/key (sequences and dicts). `attrgetter("name")` accesses by attribute (objects). Both are faster than equivalent lambdas because they're implemented in C.

### Q12: When should you use `functools.cmp_to_key`?
**A:** Only when comparison can't be expressed as a single key — e.g. the **largest-number** problem where you compare `a+b` vs `b+a`. It's much slower than `key=` because the comparator runs O(n log n) times instead of n.

### Q13: What signs does a `cmp` function return?
**A:** Negative if `a` should come before `b`, positive if `b` should come before `a`, zero if equal. (Same convention as C's `qsort`, Java's Comparator, etc.)

### Q14: Compare mergesort, quicksort, and heapsort.
**A:**
- **Mergesort:** O(n log n) worst, **stable**, **O(n) extra space**, not in-place.
- **Quicksort:** O(n log n) average, **O(n²) worst** (bad pivots), in-place, **not stable**.
- **Heapsort:** O(n log n) worst, **in-place**, O(1) extra space, **not stable**.

### Q15: Can you beat O(n log n) for sorting?
**A:** Yes — **for non-comparison-based sorts** like counting sort and radix sort, which run in O(n + k) and O(nk) respectively. They only work for integers (or fixed-width keys) within a known range.

### Q16: How do you sort a list of (name, score) by score DESC then name ASC?
**A:**
```python
sorted(data, key=lambda x: (-x[1], x[0]))
```
Negate score for descending; name remains ascending.

### Q17: How do you sort by a custom alphabet?
**A:**
```python
order = {ch: i for i, ch in enumerate(alphabet)}
sorted(words, key=lambda w: [order[c] for c in w])
```
The list-of-ints key compares element-by-element, lexicographically.

### Q18: How do you find the kth-largest element efficiently?
**A:** `heapq.nlargest(k, nums)[-1]` — O(n log k). For O(n) average, use Quickselect (partition + recurse on one side).

### Q19: What's wrong with `sorted([1, None, 2])`?
**A:** `TypeError` — `None` isn't comparable to ints in Python 3. Either filter Nones first, or supply a key that handles them: `key=lambda x: (x is None, x)` puts Nones at the end.

### Q20: How do you implement a merge step (the merge in mergesort)?
**A:**
```python
def merge(a, b):
    out, i, j = [], 0, 0
    while i < len(a) and j < len(b):
        if a[i] <= b[j]: out.append(a[i]); i += 1
        else:            out.append(b[j]); j += 1
    out.extend(a[i:]); out.extend(b[j:])
    return out
```
The `<=` (not `<`) preserves stability.

---

## Common Failure Points

### F1: `xs = xs.sort()`
**Symptom:** `xs` becomes `None`. Looks like sort destroyed your list.
**Fix:** `.sort()` returns `None`. Use either `xs.sort()` (no assignment) or `xs = sorted(xs)`.

### F2: Forgetting `key=`
**Symptom:** `sorted(words, len)` raises `TypeError` — `len` was passed as positional.
**Fix:** `key`, `reverse` are keyword-only: `sorted(words, key=len)`.

### F3: Sorting strings of digits
**Symptom:** `["10","2","9"]` → `["10","2","9"]` (lexicographic).
**Fix:** `key=int` to sort numerically.

### F4: Mixed types
**Symptom:** `TypeError: '<' not supported between 'int' and 'str'`.
**Fix:** Provide a key that produces same-typed values, or partition the list.

### F5: `None` in the data
**Symptom:** TypeError comparing `None` to numbers.
**Fix:** `key=lambda x: (x is None, x)` puts Nones last; or filter them out first.

### F6: Mutating items inside the key function
**Symptom:** Random reorderings, hard-to-debug bugs.
**Fix:** Key functions must be pure — return a value, don't mutate.

### F7: Sorting then taking [0] for min
**Symptom:** Slow on large input.
**Fix:** Use `min(xs)` (O(n)). Sorting is O(n log n).

### F8: Sorting then slicing for top K
**Symptom:** Slow for huge n with small k.
**Fix:** `heapq.nlargest(k, xs)` — O(n log k).

### F9: Using `cmp_to_key` when a key would work
**Symptom:** Slow sort.
**Fix:** Express the comparison as a key. Use `cmp_to_key` only when truly impossible (e.g. largest-number).

### F10: Two-pass sort in the wrong order
**Symptom:** Final order is wrong.
**Fix:** Sort by **secondary** key first, **primary** key second. (The last sort wins; stability preserves the earlier sort within ties.)

### F11: Forgetting that strings sort by Unicode code point
**Symptom:** `"Z" < "a"` (uppercase before lowercase). `"app" < "apple"`.
**Fix:** Use `key=str.lower` for case-insensitive; understand prefix rule.

### F12: Sorting heterogeneous custom objects
**Symptom:** Custom class doesn't define `__lt__` → TypeError.
**Fix:** Either implement `__lt__` (or use `@dataclass(order=True)`), or always pass a `key=`.

### F13: Sorting a dict and expecting a dict back
**Symptom:** `sorted(d)` returns a list of keys.
**Fix:** Wrap with `dict(sorted(d.items(), key=...))` to keep it as a dict.

### F14: Confusing `reverse=True` with negation in the key
**Symptom:** Multi-field sort gets the secondary direction wrong.
**Fix:** `reverse=True` flips the **whole** sort. For per-field control use negation or two-pass.

### F15: Counting on quicksort being stable
**Symptom:** "Stable quicksort" isn't a thing in standard implementations.
**Fix:** Use mergesort or Timsort for stability. Python's sort is stable — relax.

---

## Self-Check Before Advancing

- [ ] Can recite `sorted` vs `.sort()` differences instantly
- [ ] Can write multi-field key with mixed asc/desc without thinking
- [ ] Know when to use `itemgetter` / `attrgetter` over lambda
- [ ] Know when `cmp_to_key` is required (e.g. largest-number)
- [ ] Can recite Timsort properties (stable, O(n log n), O(n) on sorted)
- [ ] Can recite the algorithm complexity table
- [ ] Can implement insertion, merge, quick sort by hand
- [ ] Reach for `heapq.nlargest` for top-K instead of full sort
- [ ] Can solve merge intervals, group anagrams, custom alphabet, reorder logs

If any item is shaky, redo the corresponding section in `lesson.ipynb` and re-attempt the failing drill in `practice.ipynb`.
