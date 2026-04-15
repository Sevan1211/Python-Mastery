# Loops & Conditionals — Review

## Spaced Repetition Prompts

### Conditionals

**Q1:** What order does Python evaluate `if`/`elif`/`else` branches?
**A1:** Top to bottom. The **first** branch whose condition is `True` executes. All subsequent branches are skipped — even if their conditions are also true.

**Q2:** Name all the falsy values in Python.
**A2:** `False`, `None`, `0`, `0.0`, `0j`, `""`, `[]`, `()`, `{}`, `set()`, `range(0)`. Everything else is truthy.

**Q3:** What do `and` and `or` actually return — booleans or values?
**A3:** They return **values**, not necessarily booleans. `and` returns the first falsy value (or the last value if all truthy). `or` returns the first truthy value (or the last value if all falsy).

**Q4:** What's the difference between `==` and `is`?
**A4:** `==` checks **value equality** (do they have the same content?). `is` checks **identity** (are they the exact same object in memory?). Use `is` only for `None`, `True`, `False`.

**Q5:** Write the ternary expression syntax from memory.
**A5:** `value_if_true if condition else value_if_false`

**Q6:** What does short-circuit evaluation mean for `and`/`or`?
**A6:** Python stops evaluating as soon as the result is determined. `False and X` never evaluates `X`. `True or X` never evaluates `X`. This is useful for safe access patterns like `if data and data["key"]:`.

### Loops

**Q7:** What's the difference between `for` and `while` in Python? When do you use each?
**A7:** `for` iterates over a known iterable (use ~90% of the time). `while` repeats until a condition becomes false (use when iterations are unknown — convergence, polling, retry logic).

**Q8:** What are the three forms of `range()` and what does each return?
**A8:** `range(stop)`: 0 to stop-1. `range(start, stop)`: start to stop-1. `range(start, stop, step)`: start to stop-1 in increments of step. The stop value is always **exclusive**.

**Q9:** Why is `enumerate()` better than `range(len(...))`?
**A9:** It's more Pythonic, less error-prone (no manual indexing), and gives you both the index and value in a clean tuple unpack: `for i, val in enumerate(items):`.

**Q10:** What does `zip()` do with iterables of different lengths?
**A10:** It stops at the **shortest** iterable. Extra elements from longer iterables are silently dropped. Use `itertools.zip_longest()` to include all elements with a fill value.

**Q11:** What does the `else` clause on a `for`/`while` loop do?
**A11:** It runs **only if the loop completed without hitting a `break`**. If `break` was executed, the `else` block is skipped. Think: "else = no break happened."

**Q12:** Does `break` exit all nested loops or just the innermost?
**A12:** Just the **innermost** loop. To break out of multiple levels, use a flag variable or wrap the nested loops in a function and `return`.

### Patterns

**Q13:** Write the frequency counting pattern from memory.
**A13:**
```python
freq = {}
for x in items:
    freq[x] = freq.get(x, 0) + 1
```

**Q14:** Write the two-pointer palindrome check pattern from memory.
**A14:**
```python
left, right = 0, len(s) - 1
while left < right:
    if s[left] != s[right]:
        return False
    left += 1
    right -= 1
return True
```

**Q15:** What's the time complexity of string concatenation with `+=` inside a loop? What's the fix?
**A15:** O(n²) because strings are immutable — each `+=` creates a new string. Fix: collect parts in a list, then `"".join(parts)` at the end — O(n).

### Advanced

**Q16:** Why is `range()` memory-efficient even for huge ranges?
**A16:** `range()` is a **lazy** object — it doesn't store all numbers, just start/stop/step. It computes elements on demand. A `range(1_000_000)` uses only ~48 bytes, while `list(range(1_000_000))` uses ~8MB.

**Q17:** What does the walrus operator `:=` do?
**A17:** Assigns a value **and** returns it in the same expression. `if (n := len(data)) > 10:` assigns `len(data)` to `n` and checks if it's > 10 in one step. Python 3.8+.

**Q18:** How do you iterate over a dictionary's keys, values, and key-value pairs?
**A18:** Keys: `for k in d:` or `for k in d.keys():`. Values: `for v in d.values():`. Both: `for k, v in d.items():`.

**Q19:** What's the `in` operator's time complexity for list vs set?
**A19:** List: O(n) — linear scan. Set: O(1) average — hash lookup. When doing membership checks inside a loop, converting to a set first can change O(n²) to O(n).

**Q20:** What problem does modifying a list during iteration cause?
**A20:** Elements get skipped because the iterator's internal index advances while the list shrinks/shifts underneath it. Fix: iterate over a copy (`for x in lst[:]:`), use list comprehension to filter, or build a new list.

---

## Common Failure Points

### 1. Off-by-one with range()
**BUG:** `for i in range(1, 5):` — expects 5 to be included but it's not.
**FIX:** `range(start, stop)` excludes `stop`. Use `range(1, 6)` to include 5.
**RULE:** Always remember: range stop is exclusive. When in doubt, add 1 to stop.

### 2. Modifying list during iteration
**BUG:** `for x in nums: if x < 0: nums.remove(x)` — skips elements.
**FIX:** `nums = [x for x in nums if x >= 0]` or iterate over `nums[:]`.
**RULE:** Never `remove()`, `append()`, or `pop()` from a list you're iterating over.

### 3. Forgetting to increment in while loop
**BUG:** `while i < 10: print(i)` — infinite loop because `i` never changes.
**FIX:** Add `i += 1` inside the loop body.
**RULE:** Every `while` loop must modify the condition variable.

### 4. Using `=` instead of `==` in conditions
**BUG:** `if x = 5:` — SyntaxError (assignment, not comparison).
**FIX:** `if x == 5:`.
**RULE:** Python catches this as a SyntaxError, but watch for `=` in complex expressions.

### 5. Wrong operator precedence with `not`, `and`, `or`
**BUG:** `if not x == 5 or y == 10:` — `not` binds to `x == 5` only, not the whole expression.
**FIX:** `if not (x == 5 or y == 10):` — use explicit parentheses.
**RULE:** When mixing `not`, `and`, `or`, always use parentheses to be explicit.

### 6. break only exits innermost loop
**BUG:** Using `break` in a nested loop expecting to exit both loops.
**FIX:** Use a flag variable (`found = True; break`) and check it in the outer loop, or wrap in a function and use `return`.
**RULE:** `break` only affects the **immediately enclosing** loop.

### 7. Using `is` for value comparison
**BUG:** `if x is 1000:` — may be `False` even when `x == 1000`.
**FIX:** `if x == 1000:`. Only use `is` for `None`, `True`, `False`.
**RULE:** `is` checks identity (same object), `==` checks equality (same value).

### 8. String concatenation in loops (O(n²))
**BUG:** `result = ""; for s in strings: result += s` — O(n²) for large inputs.
**FIX:** `result = "".join(strings)` — O(n).
**RULE:** Never build large strings with `+=` in a loop. Use `join()`.

### 9. Returning too early in a loop
**BUG:** `for n in nums: if n > 0: return True else: return False` — only checks first element.
**FIX:** Only `return False` inside the loop. `return True` **after** the loop completes.
**RULE:** For "check all" patterns, only return the negative case inside the loop; return the positive case after.

### 10. Forgetting loop variable leaks
**BUG:** Relying on a loop variable after the loop when the loop might not execute (`for x in []:` — `x` is undefined).
**FIX:** Initialize the variable before the loop: `x = None`.
**RULE:** In Python, loop variables persist after the loop, but only if the loop body executed at least once.
