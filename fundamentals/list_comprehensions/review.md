# List Comprehensions — Review

Spaced repetition prompts and common failure points. Cover the answers, recall, then check.

---

## Spaced Repetition Prompts

### Q1: What's the basic syntax of a list comprehension?
**A:** `[expression for var in iterable]`. Optionally add `if condition` at the end to filter, e.g. `[x*2 for x in xs if x > 0]`.

### Q2: What's the difference between `[x for x in xs if cond]` and `[a if cond else b for x in xs]`?
**A:** The first **filters** (drops items where `cond` is false). The second **transforms** every item (keeps all, picks `a` or `b`). The `if` after `for` is a filter; the `if/else` before `for` is a conditional expression.

### Q3: How do you build a 2D list of zeros, and what is the wrong way?
**A:** Right: `[[0]*cols for _ in range(rows)]`. Wrong: `[[0]*cols]*rows` — all "rows" are the same list object, so mutating one mutates all.

### Q4: What does `{x for x in xs}` produce vs `{x: 1 for x in xs}`?
**A:** The first is a **set comprehension** (deduped values). The second is a **dict comprehension** (key→value). The colon is what makes it a dict.

### Q5: What's a generator expression and when do you prefer it?
**A:** `(expr for x in iter)` — produces values lazily, one at a time, with O(1) memory. Prefer it when iterating once, aggregating (`sum`, `any`, `all`, `min`, `max`), or processing huge data. The parens can be omitted when it's the sole argument: `sum(x*x for x in xs)`.

### Q6: How do you flatten a 2D list with a comprehension?
**A:** `[x for row in matrix for x in row]`. Outer loop is leftmost, inner loop is rightmost — same order as nested `for` loops.

### Q7: How do you transpose a matrix with a comprehension?
**A:** Two ways:
- Nested comp: `[[row[i] for row in m] for i in range(len(m[0]))]`
- With zip (Pythonic): `[list(c) for c in zip(*m)]`

### Q8: How do you reverse a dict with a comprehension?
**A:** `{v: k for k, v in d.items()}`. Assumes values are unique and hashable.

### Q9: How do you build a frequency map of characters in a string? When is the comprehension version bad?
**A:** Comprehension: `{ch: s.count(ch) for ch in set(s)}` — but this is **O(n²)** because `.count()` scans the whole string for every unique char. For real code use `collections.Counter(s)`, which is O(n).

### Q10: What's the order of nested `for` clauses inside a comprehension?
**A:** Left → right is outer → inner: `[expr for outer in A for inner in B]` is equivalent to `for outer in A: for inner in B: ...`. The right loop can use the variable from the left loop (e.g. `for j in range(i+1, n)`).

### Q11: Why is `[print(x) for x in xs]` an anti-pattern?
**A:** A comprehension is for **building data**. `print()` is a side effect — the comprehension dutifully builds a useless `[None, None, ...]` list and discards it. Use a regular `for` loop for actions.

### Q12: How do you dedupe a list while preserving order?
**A:** `list(dict.fromkeys(items))` (Python 3.7+ — dicts preserve insertion order). A pure comprehension can't dedupe cleanly without a separate `seen` set.

### Q13: How do you find the indices of every occurrence of a target?
**A:** `[i for i, x in enumerate(items) if x == target]`. `enumerate` provides the (index, value) pair.

### Q14: What does the walrus operator `:=` do inside a comprehension?
**A:** Lets you assign-and-use in the same expression so you don't compute a value twice. Example: `[y for x in xs if (y := f(x)) > 0]` — `f(x)` runs once per item, the result is bound to `y` and used in both the filter and the output.

### Q15: When is a comprehension *less* readable than a loop?
**A:** When you need (a) side effects, (b) `try/except`, `break`, `continue`, (c) more than two levels of nesting, or (d) logic that doesn't fit on one clear line. In those cases use a regular `for` loop.

### Q16: Inside a comprehension, what does the `_` variable signal?
**A:** "I don't care about this value." Used when you only want to repeat something n times: `[0 for _ in range(n)]`.

### Q17: How do you do element-wise addition of two equal-length lists?
**A:** `[a + b for a, b in zip(list1, list2)]`. `zip` stops at the shortest list, so unequal lengths get truncated silently — be aware.

### Q18: What's the difference between `any(x > 0 for x in xs)` and `any([x > 0 for x in xs])`?
**A:** The first uses a **generator** — it short-circuits as soon as it finds a true value. The second builds the **whole list first**, then checks. Generator is faster and uses less memory; always prefer it for `any` / `all`.

### Q19: How do you build a list of all (i, j) pairs with i < j?
**A:** `[(i, j) for i in range(n) for j in range(i+1, n)]`. The inner range starts at `i+1`, which uses the outer loop variable — perfectly legal.

### Q20: What's the cleanest way to project specific fields from a list of dicts?
**A:**
```python
trimmed = [{k: r[k] for k in fields} for r in records]
```
Outer comp iterates records; inner dict comp picks only the requested fields.

---

## Common Failure Points

### F1: Confusing filter vs transform `if`
**Symptom:** Comprehension produces wrong shape (too few or wrong values).
**Fix:** "After `for`" = filter (drops). "Before `for` with else" = transform (keeps all). Memorise.

### F2: `[[0]*n]*m` shared-row trap
**Symptom:** Mutating `grid[0][0]` magically changes the whole column.
**Fix:** Always build the outer dimension with a comp: `[[0]*n for _ in range(m)]`.

### F3: Forgetting the colon → set instead of dict
**Symptom:** `{x for x in xs}` returns a `set`, not the dict you expected.
**Fix:** Dicts need `key: value`.

### F4: Using a comprehension for side effects
**Symptom:** `[print(x) for x in xs]` works but builds `[None, None, ...]`.
**Fix:** Use a real `for` loop for prints, IO, API calls, mutations.

### F5: Building a list just to consume it once
**Symptom:** `sum([x*x for x in big])` uses lots of memory.
**Fix:** Drop the brackets: `sum(x*x for x in big)`.

### F6: Quadratic frequency map
**Symptom:** `{ch: s.count(ch) for ch in s}` is slow on large strings (O(n²)).
**Fix:** `from collections import Counter; Counter(s)` — O(n).

### F7: Three or more levels of nesting
**Symptom:** Nobody (including future-you) can read it.
**Fix:** Refactor with helper functions or convert to a real loop.

### F8: Putting `else` after the `for`
**Symptom:** `SyntaxError`. `[x for x in xs if cond else other]` is illegal.
**Fix:** Move `if/else` to the *expression* part: `[x if cond else other for x in xs]`.

### F9: Mutating the iterable while comprehending
**Symptom:** Bizarre off-by-one or skipped elements.
**Fix:** Build a *new* list and replace; never modify the source mid-iteration.

### F10: Misordering nested for clauses
**Symptom:** Got pairs in the wrong order or wrong shape.
**Fix:** Read `for a in A for b in B` as outer-to-inner. Test with a tiny case first.

### F11: Forgetting that `zip` truncates to the shorter iterable
**Symptom:** Pairwise comprehension drops trailing items.
**Fix:** Use `itertools.zip_longest` if you need the longer length, or assert lengths first.

### F12: Generator exhaustion
**Symptom:** `sum(g)` returns 0 the second time you call it.
**Fix:** Generators are one-shot. Re-create the generator or materialise to a list if you need multiple passes.

### F13: Wrong assumption about operator precedence in conditional expression
**Symptom:** `[a if cond1 else b if cond2 else c for x in xs]` looks fine but you're confused about associativity.
**Fix:** It chains right-to-left: equivalent to `(a if cond1 else (b if cond2 else c))`. Add parens for clarity.

### F14: Variable name shadowing
**Symptom:** Comprehension variable accidentally collides with an outer name.
**Fix:** In Python 3, comprehension variables are local to the comp — they don't leak — so this is rarely a real bug, but pick distinct names anyway for readability.

### F15: Mixing comprehension types
**Symptom:** Wrote a list comp but expected a dict (or vice versa).
**Fix:** Look at the brackets. `[]` = list. `{}` no colon = set. `{:}` = dict. `()` = generator.

---

## Self-Check Before Advancing

You're ready to move on to **Sorting** when you can:

- [ ] Read any comprehension at a glance and translate it into the equivalent `for` loop
- [ ] Choose the right type (list / set / dict / generator) without thinking
- [ ] Place `if` / `if-else` correctly for filter vs transform on every problem
- [ ] Write `flatten`, `transpose`, `dedupe`, `frequency map`, `index by key` from memory
- [ ] Build a 2D grid correctly (no `*` trap) every time
- [ ] Justify when a `for` loop is more readable than a comprehension
- [ ] Identify quadratic comprehensions and rewrite them
- [ ] Use `enumerate`, `zip`, `range`, walrus, and `any`/`all` with comprehensions fluently

If any item is shaky, redo the corresponding section in `lesson.ipynb` and `practice.ipynb`.
