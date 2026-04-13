# Lists — Review (Spaced Repetition)

## Recall Prompts

### Prompt 1: Mutability
**Q:** What is the fundamental difference between strings and lists?
**A:** Strings are immutable (cannot change in place), lists are mutable (can change elements, add, remove). `s[0] = "x"` → TypeError. `lst[0] = "x"` → works.

---

### Prompt 2: sort() vs sorted()
**Q:** What does `nums.sort()` return? What does `sorted(nums)` return?
**A:** `.sort()` returns `None` (sorts in place). `sorted()` returns a new sorted list (original unchanged). The #1 beginner gotcha: `result = nums.sort()` sets result to `None`.

---

### Prompt 3: Aliasing
**Q:** After `a = [1,2,3]; b = a; b.append(4)`, what is `a`?
**A:** `a` is `[1, 2, 3, 4]`. `b = a` creates an alias — both variables point to the same list object. To copy: `b = a[:]` or `b = a.copy()` or `b = list(a)`.

---

### Prompt 4: append vs extend
**Q:** What's the difference between `lst.append([1,2])` and `lst.extend([1,2])`?
**A:** `append` adds the entire object as ONE element → `[..., [1,2]]`. `extend` adds each item individually → `[..., 1, 2]`. `+=` is equivalent to `extend`.

---

### Prompt 5: Removing Elements
**Q:** Name 4 ways to remove items from a list and what each returns.
**A:** `remove(x)` → `None` (removes first x, ValueError if missing). `pop()` / `pop(i)` → returns the removed item. `del lst[i]` → nothing. `clear()` → `None` (empties list).

---

### Prompt 6: Shallow vs Deep Copy
**Q:** After `a = [[1,2],[3,4]]; b = a[:]; b[0].append(99)`, what is `a`?
**A:** `a` is `[[1, 2, 99], [3, 4]]`. Shallow copy (`[:]`) copies the outer list but inner lists are still shared references. Use `copy.deepcopy()` for full independence.

---

### Prompt 7: Mutable Default Arguments
**Q:** Why is `def f(lst=[])` a bug?
**A:** The default list is created ONCE and shared across all calls. Each call that mutates it affects future calls. Fix: `def f(lst=None): if lst is None: lst = []`.

---

### Prompt 8: 2D Grid Creation
**Q:** Why does `[[0]*3]*3` not work for a 2D grid?
**A:** `*3` creates 3 references to the SAME inner list. Modifying any "row" affects all rows. Fix: `[[0]*3 for _ in range(3)]` — creates independent inner lists.

---

### Prompt 9: Slicing Safety
**Q:** What happens when you slice beyond the list bounds? What about indexing?
**A:** Slicing is safe: `[1,2,3][100:200]` → `[]`. Indexing raises `IndexError`: `[1,2,3][100]` → error.

---

### Prompt 10: Mutation During Iteration
**Q:** Why does removing items while iterating forward skip elements?
**A:** When you remove an item, all subsequent elements shift left. The iterator's internal index advances, skipping the element that shifted into the current position. Fix: iterate over a copy (`lst[:]`), use comprehension, or iterate backwards.

---

### Prompt 11: Unpacking
**Q:** How do you unpack the first element and capture the rest?
**A:** `first, *rest = [1, 2, 3, 4]` → `first=1`, `rest=[2,3,4]`. Also: `*start, last = [...]` and `first, *mid, last = [...]`.

---

### Prompt 12: in Operator
**Q:** How does `in` behave differently for strings vs lists?
**A:** For strings, `in` checks substrings: `"he" in "hello"` → True. For lists, `in` checks exact element membership: `1 in [[1,2]]` → False (the element is `[1,2]`, not `1`).

---

### Prompt 13: Time Complexity
**Q:** What's the time complexity of `append()`, `pop()`, `pop(0)`, `insert(0, x)`, and `x in lst`?
**A:** `append()` → O(1) amortized. `pop()` → O(1). `pop(0)` → O(n). `insert(0, x)` → O(n). `x in lst` → O(n). Beginning-of-list operations are expensive.

---

### Prompt 14: Enumerate
**Q:** What's the Pythonic way to get both index and value during iteration?
**A:** `for i, val in enumerate(lst):` — always preferred over `for i in range(len(lst)):` when you need both index and value. Supports `start` parameter: `enumerate(lst, start=1)`.

---

### Prompt 15: zip
**Q:** What does `zip()` do and how does it handle unequal lengths?
**A:** `zip(a, b)` pairs elements from two (or more) iterables. Stops at the shortest. `zip([1,2,3], ["a","b"])` → `[(1,"a"), (2,"b")]` — 3 is dropped.

---

### Prompt 16: Empty List Truthiness
**Q:** Is `[]` truthy or falsy? How do you check for an empty list?
**A:** `[]` is falsy. Pythonic check: `if not lst:` (empty) or `if lst:` (non-empty). Avoid `if len(lst) == 0:`.

---

### Prompt 17: += vs +
**Q:** What's the difference between `lst += [x]` and `lst = lst + [x]`?
**A:** `+=` mutates in place (like extend) — aliases see the change. `= ... +` creates a new list and reassigns — aliases see the old list.

---

### Prompt 18: sort() Key Function
**Q:** How do you sort strings by length? Case-insensitively?
**A:** By length: `sorted(words, key=len)`. Case-insensitive: `sorted(words, key=str.lower)`. Both `sort()` and `sorted()` accept `key` and `reverse` parameters.

---

### Prompt 19: clear() vs Reassignment
**Q:** What's the difference between `lst.clear()` and `lst = []`?
**A:** `clear()` empties the same object — all aliases see empty. `lst = []` creates a new empty list and rebinds the variable — aliases still point to the old (unchanged) list.

---

### Prompt 20: Swap Without Temp
**Q:** How do you swap two variables or two list elements without a temp variable?
**A:** `a, b = b, a` for variables. `lst[i], lst[j] = lst[j], lst[i]` for list elements. This is tuple unpacking — Python evaluates the right side first.

---

## Common Failure Points

### 1. Assigning sort() result to a variable

```python
# BUG
nums = [3, 1, 2]
result = nums.sort()
print(result)  # None

# FIX
result = sorted(nums)  # returns new sorted list
```

---

### 2. Mutating a list you don't own (aliasing)

```python
# BUG
def process(items):
    items.sort()      # mutates the caller's list!
    return items[0]

original = [3, 1, 2]
process(original)
print(original)  # [1, 2, 3] — BAD, caller didn't expect this

# FIX
def process(items):
    sorted_items = sorted(items)  # work on a copy
    return sorted_items[0]
```

---

### 3. Removing while iterating forward

```python
# BUG
nums = [1, 2, 3, 4, 5, 6]
for n in nums:
    if n % 2 == 0:
        nums.remove(n)
# Result: [1, 3, 5, 6] — missed 6!

# FIX — use comprehension
odds = [n for n in nums if n % 2 != 0]
```

---

### 4. Mutable default argument

```python
# BUG
def append_to(item, lst=[]):
    lst.append(item)
    return lst

append_to(1)  # [1]
append_to(2)  # [1, 2] — BUG!

# FIX
def append_to(item, lst=None):
    if lst is None:
        lst = []
    lst.append(item)
    return lst
```

---

### 5. Shallow copy with nested lists

```python
# BUG
grid = [[1, 2], [3, 4]]
copy = grid[:]
copy[0][0] = 99
print(grid)  # [[99, 2], [3, 4]] — original changed!

# FIX
import copy
deep = copy.deepcopy(grid)
deep[0][0] = 99
print(grid)  # [[1, 2], [3, 4]] — unchanged
```

---

### 6. 2D grid with reference sharing

```python
# BUG
grid = [[0] * 3] * 3
grid[0][0] = 1
print(grid)  # [[1,0,0], [1,0,0], [1,0,0]] — all rows affected!

# FIX
grid = [[0] * 3 for _ in range(3)]
grid[0][0] = 1
print(grid)  # [[1,0,0], [0,0,0], [0,0,0]] — only row 0 changed
```

---

### 7. Off-by-one with neighbor comparison

```python
# BUG
nums = [1, 2, 3, 4, 5]
for i in range(len(nums)):  # IndexError on last iteration
    if nums[i] < nums[i + 1]:
        pass

# FIX — stop one early
for i in range(len(nums) - 1):
    if nums[i] < nums[i + 1]:
        pass
```

---

### 8. Using append when you mean extend

```python
# BUG
result = [1, 2]
result.append([3, 4])
print(result)  # [1, 2, [3, 4]] — nested!

# FIX
result = [1, 2]
result.extend([3, 4])
print(result)  # [1, 2, 3, 4] — flat
```

---

### 9. Forgetting pop() returns a value

```python
# MISSED OPPORTUNITY
stack = [1, 2, 3]
stack.pop()  # 3 is returned but discarded

# BETTER — capture it
last = stack.pop()  # last = 3
```

---

### 10. Checking membership in nested lists

```python
# BUG
nested = [[1, 2], [3, 4]]
print(1 in nested)      # False — checks top-level elements only

# FIX
print(any(1 in row for row in nested))  # True
```
