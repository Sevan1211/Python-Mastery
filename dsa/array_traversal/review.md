# Array Traversal — Review

## Spaced Repetition Prompts

### Q1: What are the 8 core array-traversal patterns?
**A:** 1) Linear scan / find, 2) Single-pass aggregation (min/max/sum/count), 3) Running aggregate / best-so-far, 4) Frequency / counting, 5) Neighbour comparison, 6) Early exit / short-circuit, 7) Build result list, 8) Parallel / multi-array.

### Q2: What are the 4 in-place modification patterns?
**A:** 1) Slow/Fast write (Remove Element, Remove Duplicates), 2) Partition (Move Zeroes), 3) Reverse in place (two pointers from ends), 4) Rotate via 3-reverse trick.

### Q3: When should you use `enumerate` over `range(len(arr))`?
**A:** Almost always. `enumerate` gives you both index and value cleanly. Reach for `range(len(arr))` only when you need fine index control — skipping, jumping, or manipulating the index directly.

### Q4: What is the "best-so-far" template?
**A:**
```python
state = init_from(arr[0])
best  = state
for x in arr[1:]:
    state = update(state, x)
    best  = compare(best, state)
return best
```
This single template solves Kadane, Buy/Sell Stock, Max Consecutive Ones, etc.

### Q5: Walk through Kadane's Algorithm in one sentence.
**A:** Maintain `cur = max(x, cur + x)` (extend or restart at x) and `best = max(best, cur)`. The intuition: any optimal subarray ending at `i` either continues from `i-1` or starts fresh at `i`.

### Q6: Why does `best = arr[0]` (not `0`) matter for Kadane on all-negative input?
**A:** If you initialise `best = 0`, an all-negative array returns `0`, but the spec requires a **non-empty** subarray. Initialising with `arr[0]` guarantees you return at least one element.

### Q7: Why does Best Time to Buy/Sell Stock initialise `best = 0` (not `prices[0]`)?
**A:** Because if prices are monotonically falling, you don't trade — profit is 0, not a negative number. The spec says "0 if no profit possible."

### Q8: What's the slow/fast write skeleton?
**A:**
```python
slow = 0
for fast in range(len(nums)):
    if KEEP(nums[fast]):
        nums[slow] = nums[fast]
        slow += 1
return slow
```
`slow` is the **write index** for the kept region; `fast` scans the whole array.

### Q9: Why must Move Zeroes use a **swap** instead of an overwrite?
**A:** Overwriting works for new length (Remove Element / Remove Dupes) but doesn't put the zeroes at the tail. Swapping pushes the zero from `slow` to `fast`'s old position, so by the end the tail is filled with zeros.

### Q10: What's the 3-reverse trick for Rotate Array, and why does it work?
**A:** `reverse(all)` → `reverse(first k)` → `reverse(rest)`. Rotating right by k means the last k elements come to the front. Reversing all flips them to the front but in reversed order; the two partial reverses fix that. O(n) time, O(1) extra space.

### Q11: How do you handle `k > n` in rotate?
**A:** `k %= n` first. Rotating by `n` is identity, so the effective rotation is `k mod n`. Forgetting this is a classic interview flub.

### Q12: How do you do Plus One in O(1) extra space?
**A:** Scan from the **right**. While digit is 9, set it to 0 and carry. As soon as a digit < 9, increment it and return. If you exit the loop, the number was all 9s — prepend a 1.

### Q13: Pivot Index — what's the one-pass formula?
**A:** Pre-compute `total = sum(nums)`. Iterate with running `left`. At index `i`, the right sum is `total - left - nums[i]`. Pivot is the first `i` where `left == total - left - nums[i]`.

### Q14: Product of Array Except Self in O(1) extra — sketch.
**A:** Two passes. First pass: `out[i]` = product of everything to the **left** of `i`. Second pass (right-to-left): multiply `out[i]` by a running product of everything to the **right** of `i`. Output array doesn't count toward space.

### Q15: Why does XOR solve "Single Number" in O(1) space?
**A:** XOR is associative and commutative, `a ^ a = 0`, and `a ^ 0 = a`. XORing every element pairs the duplicates into 0s, leaving only the singleton.

### Q16: Boyer-Moore Majority Vote — what's the invariant?
**A:** Maintain a candidate and a count. Each element either supports the candidate (`+1`) or contests it (`-1`). When count drops to 0, replace the candidate. The true majority element (>n/2) survives. (Verify with a second pass if existence isn't guaranteed.)

### Q17: Squares of a Sorted Array — why does two-pointer-from-ends work?
**A:** The largest square is at one of the **two ends** (most negative or most positive). Compare absolute values, take the larger, write to the END of the output, and shrink the chosen pointer inward.

### Q18: What's the 6-edge-case checklist before any array problem?
**A:** Empty, single element, all equal, negatives, sorted-vs-unsorted, k > n.

### Q19: When mutating a list during iteration, what goes wrong?
**A:** When you `arr.remove(x)` mid-loop, indices shift and the iterator skips the next element. Solution: iterate over a copy (`for x in arr[:]:`) or use slow/fast in place, or build a new list.

### Q20: Compare `arr.reverse()` vs `reversed(arr)` vs `arr[::-1]`.
**A:**
- `arr.reverse()` — in place, O(n) time, O(1) space, returns `None`.
- `reversed(arr)` — returns an iterator, O(1) creation, O(n) to consume, doesn't allocate a new list.
- `arr[::-1]` — slice, allocates a new reversed list, O(n) time and space.

### Q21: How do you spot when array traversal is sufficient vs needs a richer pattern?
**A:** Plain traversal works if you can answer with O(1) state per element. If you need pairs (→ two pointers / hash map), windows (→ sliding window), range sums (→ prefix sum), or monotonic stack info (→ stack), you've moved beyond plain traversal.

---

## Common Failure Points (Read Before Every Mock Interview)

1. **Off-by-one in neighbour loops.** `range(len(arr) - 1)` for adjacent pairs, NOT `range(len(arr))`. Or use `zip(arr, arr[1:])` and never think about indices.

2. **Empty-array crash on aggregation.** `min(arr)` / `arr[0]` blows up on `[]`. Guard at the top: `if not arr: return ...`.

3. **Kadane initialised to 0.** Fails on all-negative input. Initialise to `arr[0]`.

4. **Stock profit initialised to `prices[0]`.** Should be `0`. Profit can't be negative.

5. **Mutating list while iterating.** `for x in arr: arr.remove(x)` skips items. Iterate a copy, or use slow/fast in place.

6. **Forgetting `k %= n` in rotate.** When `k > n`, you do redundant work and tests fail.

7. **Confusing argmax with max.** Returning `arr[i]` instead of `i`. Read the prompt carefully.

8. **Using `arr[1:]` in tight loops.** It allocates a fresh list each iteration. Use `enumerate(arr, start=1)` or `range(1, len(arr))` instead.

9. **Returning a new list when in-place was required.** Many "remove" problems demand O(1) extra space. Read the constraints.

10. **Slow/Fast skeleton: forgetting to bump `slow`.** The `slow += 1` must happen ONLY when you keep the element.

11. **Move Zeroes with overwrite (not swap).** Overwriting leaves stale data after `slow` instead of zeros. Swap form is correct.

12. **Plus One: handling all-9s.** If every digit is 9, the result grows by one digit. Don't forget `return [1] + digits`.

13. **Single Number with sum trick instead of XOR.** Sum trick (`2*sum(set(nums)) - sum(nums)`) needs O(n) extra space; XOR is O(1).

14. **Boyer-Moore: assuming the candidate is always the majority.** It is **only** if a majority is guaranteed to exist. If not, do a verification pass.

15. **Trapping Rain Water: forgetting that water at index `i` is `min(L[i], R[i]) - h[i]`, not `max(...)`.**

16. **Circular max subarray: missing the all-negative case.** `total - kadane_min` becomes 0 (empty middle), but the spec requires non-empty. If `kadane_max < 0`, return `kadane_max`.

17. **`enumerate(arr, start=1)` start parameter ignored.** Easy to forget the keyword; just `enumerate(arr, 1)` works too.

18. **`zip` truncates to the shortest input silently.** When iterating two arrays of unequal length, you may lose data. Use `itertools.zip_longest` if needed.

---

## Mastery Drill (do once a week until automatic)

Without looking at the lesson, implement from scratch in 60 seconds each:

1. `max_subarray(nums)` (Kadane)
2. `max_profit(prices)` (Stock I)
3. `move_zeroes(nums)` (in place)
4. `remove_duplicates(nums)` (sorted, in place)
5. `rotate(nums, k)` (3-reverse)
6. `plus_one(digits)`
7. `pivot_index(nums)`
8. `product_except_self(nums)`
9. `single_number(nums)` (XOR)
10. `sorted_squares(nums)` (two pointers)

When all 10 take <60 seconds each with no off-by-one bugs on the first try, this module is mastered.
