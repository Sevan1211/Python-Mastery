# Array Traversal — Cheatsheet

## When to Reach for Plain Traversal

- Single array, scalar/aggregate answer
- Brute force is already O(n) — no fancier pattern needed
- Tracking "best so far," "first occurrence," "running sum," etc.
- Need to modify the array **in place** with O(1) extra space

If you need pairs/triplets → **two pointers** or **hash map**.
If you need a window → **sliding window**.
If you need range sums → **prefix sum**.

---

## The 8 Core Traversal Patterns

| # | Pattern | State | Template |
|---|---------|-------|----------|
| 1 | Linear scan / find | nothing | `for i, x in enumerate(arr): if x == t: return i` |
| 2 | Aggregation (min/max/sum) | accumulator | `for x in arr: best = max(best, x)` |
| 3 | Best-so-far | best + auxiliary | Kadane, Stock I |
| 4 | Frequency / counting | dict / set | `counts[x] = counts.get(x, 0) + 1` |
| 5 | Neighbour compare | previous element | `for a, b in zip(arr, arr[1:])` |
| 6 | Early exit / short-circuit | flag | `any(...)`, `all(...)`, `return True` |
| 7 | Build result list | output list | list comprehension or `out.append(...)` |
| 8 | Parallel / multi-array | indices into each | `for x, y in zip(a, b)` |

---

## The 4 In-Place Patterns

| # | Pattern | Setup | Move Logic | Examples |
|---|---------|-------|------------|----------|
| 1 | **Slow/Fast write** | `slow = 0` | `if keep: nums[slow]=nums[fast]; slow+=1` | Remove Element, Remove Dupes |
| 2 | **Partition (Move Zeroes)** | `slow = 0` | `if non_zero: swap(slow, fast); slow+=1` | Move Zeroes |
| 3 | **Reverse in place** | `l=0, r=n-1` | swap, l++, r--; stop when `l < r` fails | Reverse Array |
| 4 | **Rotate (3-reverse trick)** | `k %= n` | reverse all → reverse first k → reverse rest | Rotate Array |

---

## Loop Style Quick Reference

| Need | Use |
|------|-----|
| Just values | `for x in arr` |
| Index + value | `for i, x in enumerate(arr)` |
| Manual index control | `for i in range(len(arr))` |
| Adjacent pairs | `for a, b in zip(arr, arr[1:])` |
| Reverse | `for x in reversed(arr)` |
| Reverse with index | `for i in range(len(arr) - 1, -1, -1)` |
| Triangular pairs (i < j) | `for i in range(n): for j in range(i+1, n):` |
| Parallel | `for x, y in zip(a, b)` |
| Skip every k | `for x in arr[::k]` or `for i in range(0, n, k)` |

---

## Best-So-Far Skeleton (memorise!)

```python
state = init_from(arr[0])
best  = state
for x in arr[1:]:
    state = update(state, x)
    best  = compare(best, state)
return best
```

This single template covers Kadane, Best Time to Buy/Sell Stock, Max Consecutive Ones, Max Product Subarray, etc.

---

## Kadane's Algorithm (Maximum Subarray)

```python
def max_subarray(arr):
    cur = best = arr[0]
    for x in arr[1:]:
        cur  = max(x, cur + x)   # extend or restart
        best = max(best, cur)
    return best
```

For the **circular** variant: `max(kadane_max, total - kadane_min)`, with a special case if all numbers are negative (return `kadane_max`).

---

## Best Time to Buy/Sell Stock (single transaction)

```python
def max_profit(prices):
    if not prices: return 0
    lo, best = prices[0], 0
    for p in prices[1:]:
        best = max(best, p - lo)
        lo   = min(lo, p)
    return best
```

For unlimited transactions (II): `sum(max(0, prices[i]-prices[i-1]) for i in range(1, n))`.

---

## Slow/Fast Write Skeleton

```python
slow = 0
for fast in range(len(nums)):
    if KEEP(nums[fast]):
        nums[slow] = nums[fast]
        slow += 1
return slow         # new length
```

Variants: Remove Element (`KEEP = nums[fast] != val`), Remove Duplicates from Sorted (`KEEP = fast == 0 or nums[fast] != nums[fast-1]`).

---

## Move Zeroes Skeleton (swap form keeps order AND zeros the tail)

```python
slow = 0
for fast in range(len(nums)):
    if nums[fast] != 0:
        nums[slow], nums[fast] = nums[fast], nums[slow]
        slow += 1
```

---

## Rotate Array (3-reverse trick)

```python
def rotate(nums, k):
    n = len(nums)
    if n == 0: return
    k %= n
    nums.reverse()
    nums[:k] = reversed(nums[:k])
    nums[k:] = reversed(nums[k:])
```

---

## Plus One (carry from the right)

```python
def plus_one(digits):
    digits = digits[:]
    for i in range(len(digits) - 1, -1, -1):
        if digits[i] < 9:
            digits[i] += 1
            return digits
        digits[i] = 0
    return [1] + digits
```

---

## Pivot Index

```python
def pivot_index(nums):
    total, left = sum(nums), 0
    for i, x in enumerate(nums):
        if left == total - left - x:
            return i
        left += x
    return -1
```

---

## Product of Array Except Self (no division, O(1) extra)

```python
def product_except_self(nums):
    n = len(nums)
    out = [1] * n
    p = 1
    for i in range(n):
        out[i] = p
        p *= nums[i]
    p = 1
    for i in range(n - 1, -1, -1):
        out[i] *= p
        p *= nums[i]
    return out
```

---

## Single Number (XOR trick)

```python
def single_number(nums):
    x = 0
    for n in nums:
        x ^= n
    return x
```

---

## Boyer-Moore Majority Vote

```python
def majority(nums):
    cand, count = None, 0
    for x in nums:
        if count == 0:
            cand = x
        count += 1 if x == cand else -1
    return cand
```

---

## Squares of a Sorted Array (two pointers from ends)

```python
def sorted_squares(nums):
    n = len(nums)
    out = [0] * n
    l, r, w = 0, n - 1, n - 1
    while l <= r:
        if abs(nums[l]) > abs(nums[r]):
            out[w] = nums[l] ** 2
            l += 1
        else:
            out[w] = nums[r] ** 2
            r -= 1
        w -= 1
    return out
```

---

## Edge Case Checklist

Before coding ANY traversal, answer:

1. What if the array is **empty**?
2. What if it has **one** element?
3. What if **all elements are equal**?
4. What if elements are **negative**?
5. Is the array **sorted** (or guaranteed sorted)?
6. Could **k > n** (rotation, window)?
7. Are we expected to **mutate in place**, or return a new array?

---

## Common Bugs

| Bug | Cause | Fix |
|-----|-------|-----|
| `IndexError` on `arr[i+1]` | loop went too far | `range(len(arr) - 1)` for neighbour pairs |
| Skipping elements when removing | mutating list while iterating | iterate over a copy or build a new list |
| `min`/`max` crashes on `[]` | `arr[0]` access | guard with `if not arr` |
| Kadane returns 0 on all-negative input | initial `best = 0` | initialise `best = cur = arr[0]` |
| Stock profit returns `prices[0]` | initial `best = prices[0]` | initialise `best = 0` |
| Off-by-one in slow/fast | wrong start index | start `slow = 0`, `for fast in range(n)` |
| Rotate with k > n is wrong | forgot `k %= n` | always `k %= len(nums)` first |
| Argmax returns the value | mistakenly assigned `arr[i]` | assign `i`, not `arr[i]` |

---

## Complexity Reference

| Operation | Time |
|-----------|------|
| Single pass | O(n) |
| Two passes (left + right) | O(n) |
| Nested pair loop (brute) | O(n²) |
| `arr[1:]` (slice) | O(n) — allocates! |
| `arr.reverse()` (in place) | O(n) |
| `reversed(arr)` (iterator) | O(1) creation |
| `min(arr)` / `max(arr)` / `sum(arr)` | O(n) |
| `len(arr)` | O(1) |
| `x in arr` (list) | O(n) |
| `x in s` (set) | O(1) average |

---

## Interview Verbal Framework

1. **Restate** the problem.
2. **Edge cases** — say them out loud.
3. **Brute force + complexity.**
4. **Identify the state** for one-pass.
5. **Code** with `enumerate` / `zip`.
6. **Trace** a small example.
7. **Final complexity**: time AND space.
8. **Follow-up:** sorted? streaming? in-place?

If you can do this on autopilot, array problems are free points.
