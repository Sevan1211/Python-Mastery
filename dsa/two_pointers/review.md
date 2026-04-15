# Two Pointers — Review

## Spaced Repetition Prompts

### Q1: What are the 6 Two Pointer sub-patterns?
**A:** 1) Opposite Ends, 2) Slow/Fast, 3) Multi-Sum (3Sum/4Sum), 4) Dutch National Flag, 5) Merge Two Sorted, 6) String Manipulation.

### Q2: What structural guarantee makes opposite-end two pointers work?
**A:** The array is **sorted** (or has a monotonic invariant). This lets you decide which pointer to move: too small → move left, too big → move right. Each move eliminates candidates.

### Q3: In the Slow/Fast pattern, what role does each pointer play?
**A:** `slow` marks the **write position** (end of the "kept" region). `fast` **scans ahead** through the full array. Everything before `slow` is the result built in-place.

### Q4: Why does 3Sum require duplicate skipping at THREE levels?
**A:** Level 1: Skip duplicate values for `i` (outer loop). Level 2: Skip duplicate `left` values after a match. Level 3: Skip duplicate `right` values after a match. Missing any one produces duplicate triplets.

### Q5: In Dutch National Flag, why don't you increment `mid` after swapping with `high`?
**A:** The element swapped from `high` is from the **unexplored region** — it hasn't been examined yet. You must check it before moving on. When swapping with `low`, both `low` and `mid` advance because the element from `low` is already known.

### Q6: When should you use `left < right` vs `left <= right`?
**A:** `left < right`: When pointers should NOT meet (pair problems, palindromes — middle element doesn't need checking). `left <= right`: When the meeting point still needs processing (Dutch Flag, Squares of Sorted Array).

### Q7: Why does Merge Sorted Array fill from the back?
**A:** Filling from the front would overwrite elements in `nums1` that haven't been processed yet. The extra zeroed space is at the end, so filling backward avoids this.

### Q8: How do you decide between Two Pointers and HashMap for a pair-sum problem?
**A:** **Sorted** and returns values → Two Pointers (O(1) space). **Unsorted** or returns indices → HashMap (sorting destroys original positions).

### Q9: What's the time complexity of 3Sum and why?
**A:** O(n²). Sort is O(n log n). Outer loop is O(n) × inner two-pointer is O(n) = O(n²). The sort is dominated by n².

### Q10: How does Container With Most Water work without a sorted array?
**A:** The invariant isn't sorting — it's that **moving the shorter line is the only way to potentially increase area**. Keeping the shorter line while shrinking width can only decrease area.

### Q11: How does Trapping Rain Water use two pointers?
**A:** Track `left_max` and `right_max`. Always process the side with the **smaller** max first. If `left_max < right_max`, water at `left` is bounded by `left_max`. Add `left_max - height[left]`, advance left.

### Q12: What's the key insight for Squares of a Sorted Array with negatives?
**A:** The largest square is at one of the **two ends** (largest positive or most negative). Fill the result array from the back, picking the larger absolute value each time.

### Q13: How do you check if `s` is a subsequence of `t`?
**A:** One pointer per string, both moving forward. Advance `t` pointer every step. Advance `s` pointer only on match. If `s` pointer reaches the end, it's a subsequence.

### Q14: For Rotate Array, what's the three-reverse trick?
**A:** To rotate right by k: 1) Reverse entire array, 2) Reverse first k elements, 3) Reverse remaining elements. Don't forget `k = k % n`.

### Q15: What's the counting trick in 3Sum Smaller?
**A:** When `nums[i] + nums[left] + nums[right] < target`, ALL indices from `left` to `right` with `left` fixed also work (since right values are even smaller going left). So add `right - left` to the count, then `left += 1`.

### Q16: How do you handle interval intersection with two pointers?
**A:** One pointer per list. Intersection is `[max(start1, start2), min(end1, end2)]` when `start <= end`. Advance the pointer whose interval **ends first** — the other interval might still overlap with the next one.

### Q17: What makes two pointers O(n)?
**A:** Each pointer moves in only one direction and moves at most n times total. No position is revisited. Combined work across both pointers is at most 2n = O(n).

### Q18: How does partition (Slow/Fast) differ from Dutch Flag?
**A:** Partition separates into **2 groups** (pass/fail). Dutch Flag separates into **3 groups** (0/1/2). Partition uses slow/fast. Dutch Flag uses low/mid/high.

### Q19: What's the difference between Two Pointers and Sliding Window?
**A:** Two Pointers: usually converging from opposite ends, or slow/fast for in-place ops. Sliding Window: both pointers same direction, maintaining a **contiguous subarray/substring**. Sliding Window is for subarray problems; Two Pointers is for pair/partition problems.

### Q20: Why is Two Pointers preferred over HashMap when applicable?
**A:** O(1) space vs O(n) space. Same time complexity, better space. Interviewers appreciate the space optimization.

---

## Common Failure Points

### Failure 1: Two pointers on unsorted array
**BUG:** Applying opposite-end logic to `[3, 1, 4, 1, 5]` — pointer moves don't eliminate valid candidates.
**FIX:** Sort first. If you need original indices, use HashMap instead.
**RULE:** Opposite-end two pointers requires sorted input or a structural invariant.

### Failure 2: Forgetting to sort in 3Sum
**BUG:** `for i in range(len(nums)-2): left, right = i+1, len(nums)-1` — without sorting first.
**FIX:** `nums.sort()` as the first line.
**RULE:** Multi-Sum always starts with sorting.

### Failure 3: Wrong loop condition (< vs <=)
**BUG:** Using `left <= right` in palindrome check — one wasted comparison. Using `left < right` in Dutch Flag — skips the last element.
**FIX:** Pairs/palindromes: `<`. Processing every element (Dutch Flag, Squares): `<=`.
**RULE:** Ask: "Do I need to process the element when pointers meet?"

### Failure 4: Missing duplicate skips in 3Sum
**BUG:** Only skipping at the `i` level, not at `left`/`right` after finding a match.
**FIX:** After appending a result, skip identical `left` and `right` values with while loops. Then advance both.
**RULE:** 3Sum needs THREE levels of duplicate skipping.

### Failure 5: Incrementing mid after high swap (Dutch Flag)
**BUG:** `mid += 1` in the `nums[mid] == 2` branch — skips examining the swapped element.
**FIX:** Remove `mid += 1` from the else branch. Only increment mid when swapping with low or when element is already correct.
**RULE:** Only advance past an element after you've confirmed its category.

### Failure 6: Merge from front overwrites data
**BUG:** Writing `nums1[0]`, `nums1[1]`, etc. overwrites values not yet processed.
**FIX:** Start from the back: `write = m + n - 1`, `p1 = m - 1`, `p2 = n - 1`.
**RULE:** In-place merge → fill from the end.

### Failure 7: Move Zeroes — overwrite without swap
**BUG:** `nums[slow] = nums[fast]` without swapping back — leaves stale non-zero values after slow.
**FIX:** Use swap: `nums[slow], nums[fast] = nums[fast], nums[slow]`. Or do two-pass: copy non-zeros, then fill zeros.
**RULE:** In-place rearrangement usually requires swaps, not overwrites.

### Failure 8: Forgetting k % n in Rotate Array
**BUG:** `k = 10` on array of length 3 — reversing wrong sections.
**FIX:** `k = k % len(nums)` before any reversals.
**RULE:** Always normalize rotation amount.

### Failure 9: Container With Most Water — moving the wrong side
**BUG:** Moving the taller side. Width decreases and height can't improve → area guaranteed to decrease.
**FIX:** Always move the **shorter** side to have a chance at finding a taller line.
**RULE:** In Container, move the bottleneck (shorter line).

### Failure 10: Right pointer initialization off by one
**BUG:** `right = len(nums)` instead of `len(nums) - 1` → IndexError.
**FIX:** `right = len(nums) - 1`.
**RULE:** Right pointer starts at the last valid index, not the length.
