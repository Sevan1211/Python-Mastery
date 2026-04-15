# Two Pointers — Cheatsheet

## When to Use Two Pointers

- Brute force has nested loops searching/comparing
- Input is **sorted** (or can be sorted without losing info)
- Need to find **pairs/triplets** satisfying a condition
- Need to modify an array **in-place** with O(1) space
- Comparing/merging **two sorted sequences**
- Checking **palindromes** or reversing strings

---

## The 6 Patterns

| # | Pattern | Setup | Move Logic | Problems |
|---|---------|-------|------------|----------|
| 1 | **Opposite Ends** | `L=0, R=n-1` | Sum small→L++, big→R-- | Two Sum II, Container |
| 2 | **Slow/Fast** | `slow=0, for fast` | Keep? Write at slow++ | Remove Dupes, Move Zeroes |
| 3 | **Multi-Sum** | Fix i, 2P on rest | Same as #1 inside loop | 3Sum, 4Sum |
| 4 | **Dutch Flag** | `lo=0, mid=0, hi=n-1` | 0→swap lo, 1→mid++, 2→swap hi | Sort Colors |
| 5 | **Merge** | `i=0, j=0` | Pick smaller, advance | Merge Sorted Array |
| 6 | **String** | `L=0, R=n-1` | Compare ends, move in | Palindrome, Reverse |

---

## Pattern Templates

### 1. Opposite Ends
```python
left, right = 0, len(nums) - 1
while left < right:
    s = nums[left] + nums[right]
    if s == target:
        return [left, right]
    elif s < target:
        left += 1
    else:
        right -= 1
```

### 2. Slow/Fast
```python
slow = 0
for fast in range(len(nums)):
    if should_keep(nums[fast]):
        nums[slow] = nums[fast]  # or swap
        slow += 1
return slow  # new length
```

### 3. 3Sum
```python
nums.sort()
result = []
for i in range(len(nums) - 2):
    if i > 0 and nums[i] == nums[i-1]: continue  # skip dupes
    left, right = i + 1, len(nums) - 1
    while left < right:
        total = nums[i] + nums[left] + nums[right]
        if total == 0:
            result.append([nums[i], nums[left], nums[right]])
            while left < right and nums[left] == nums[left+1]: left += 1
            while left < right and nums[right] == nums[right-1]: right -= 1
            left += 1; right -= 1
        elif total < 0: left += 1
        else: right -= 1
```

### 4. Dutch National Flag
```python
low, mid, high = 0, 0, len(nums) - 1
while mid <= high:
    if nums[mid] == 0:
        nums[low], nums[mid] = nums[mid], nums[low]
        low += 1; mid += 1
    elif nums[mid] == 1:
        mid += 1
    else:
        nums[mid], nums[high] = nums[high], nums[mid]
        high -= 1  # DON'T increment mid!
```

### 5. Merge Two Sorted (from back)
```python
p1, p2, write = m - 1, n - 1, m + n - 1
while p1 >= 0 and p2 >= 0:
    if nums1[p1] > nums2[p2]:
        nums1[write] = nums1[p1]; p1 -= 1
    else:
        nums1[write] = nums2[p2]; p2 -= 1
    write -= 1
while p2 >= 0:
    nums1[write] = nums2[p2]; p2 -= 1; write -= 1
```

### 6. Palindrome Check
```python
left, right = 0, len(s) - 1
while left < right:
    if s[left] != s[right]:
        return False
    left += 1; right -= 1
return True
```

---

## Keyword → Pattern Mapping

| Keyword / Phrase | Pattern |
|------------------|---------|
| "sorted array" + "pair/sum" | Opposite Ends |
| "container", "water", "trapping" | Opposite Ends |
| "3Sum", "triplet", "three numbers" | Multi-Sum |
| "remove duplicates" + "in-place" | Slow/Fast |
| "move zeroes", "remove element" | Slow/Fast |
| "sort colors", "0s 1s 2s" | Dutch Flag |
| "merge sorted" | Merge |
| "palindrome", "reverse" + "in-place" | String |
| "subsequence" check | Same Direction |
| "O(1) extra space" + modification | Slow/Fast or Dutch Flag |

---

## Two Pointers vs Other Patterns

| Situation | Use |
|-----------|-----|
| Pair sum, unsorted | HashMap |
| Pair sum, sorted | Two Pointers |
| Subarray sum/length | Sliding Window |
| Cycle in linked list | Fast/Slow (Floyd's) |
| Find target in sorted | Binary Search |

---

## Complexity

| Pattern | Time | Space |
|---------|------|-------|
| Opposite Ends | O(n) | O(1) |
| Slow/Fast | O(n) | O(1) |
| 3Sum | O(n²) | O(1)* |
| 4Sum | O(n³) | O(1)* |
| Dutch Flag | O(n) | O(1) |
| Merge | O(n+m) | O(1) or O(n+m) |
| Palindrome | O(n) | O(1) |

*Excluding output space.

---

## Critical Gotchas

1. **Forgot to sort** → Two pointer pair-sum on unsorted array gives wrong answer
2. **`left < right` vs `left <= right`** → Pairs: `<`. Dutch Flag/Squares: `<=`
3. **Dutch Flag: `mid++` after swap with high** → WRONG. Must re-examine swapped element
4. **3Sum: missing duplicate skip** → Must skip at all THREE levels (i, left, right)
5. **Sorting destroys indices** → If problem asks for original indices, use HashMap instead
6. **Merge from front overwrites** → Merge in-place from the BACK
7. **Off-by-one in reverse** → `left, right = 0, len(s) - 1` (not `len(s)`)

---

## Interview Execution Checklist

1. **Clarify:** Sorted? Return indices or values? Duplicates? In-place?
2. **Brute force:** "Nested loops would be O(n²). Since it's sorted, two pointers gives O(n)."
3. **Explain pointer strategy:** Where they start, how they move, what invariant they maintain
4. **Edge cases:** Empty, single element, all duplicates, no valid answer
5. **Code it**
6. **Trace** through a small example
7. **State complexity:** Time O(n), Space O(1)
