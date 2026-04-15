# HashMap / HashSet — Review

## Spaced Repetition Prompts

### Q1: What is the #1 signal that you should use a HashMap?
**A:** Your brute force has nested loops where the inner loop is **searching** for something. Replace the inner search with a hash map lookup to go from O(n²) to O(n).

### Q2: Name the 7 HashMap/Set patterns.
**A:** 1) Complement Lookup, 2) Frequency Map, 3) Seen Set, 4) Group By Key, 5) Index Map, 6) Prefix Sum + HashMap, 7) Sliding Window + HashMap.

### Q3: In Two Sum, why do you check for the complement BEFORE storing the current element?
**A:** To avoid matching an element with itself. If you store first and then check, `seen[num] = i` would allow `num` to be its own complement when `target = 2 * num`.

### Q4: What's the time complexity of dict/set lookup in Python?
**A:** O(1) average, O(n) worst case (hash collisions). In interviews, say O(1).

### Q5: Why must you initialize the prefix sum map with `{0: 1}`?
**A:** To handle subarrays that start at index 0. If `current_sum == k`, you need `current_sum - k = 0` to be in the map. The `{0: 1}` represents the empty prefix with sum 0.

### Q6: What types can be dict keys / set members?
**A:** Only **hashable** types: int, float, str, tuple, frozenset, bool, None. Lists, sets, and dicts are NOT hashable. Convert lists to tuples.

### Q7: When do you use a set vs a dict?
**A:** Use a **set** when you only care about membership (is X present?). Use a **dict** when you need to associate a value with each key (what's the count/index/data for X?).

### Q8: In the Longest Consecutive Sequence problem, what makes it O(n)?
**A:** Only start counting from **sequence beginnings** — elements where `num - 1` is NOT in the set. Each element is visited at most twice (once in outer loop, once in while loop).

### Q9: What is the "Group By Key" pattern?
**A:** Compute a signature/key for each element, then group elements with the same key using `defaultdict(list)`. The hard part is choosing the right key function.

### Q10: For Group Anagrams, what are two valid key functions?
**A:** 1) `tuple(sorted(word))` — O(k log k) per word. 2) Character frequency tuple (count of each letter) — O(k) per word. Both produce hashable keys that identify anagrams.

### Q11: In the sliding window + hashmap pattern, what's the general loop structure?
**A:** Expand right pointer (add to map), shrink left pointer while window is invalid (remove from map), update result. The right pointer runs the outer loop; the left pointer catches up inside.

### Q12: What's the difference between `Counter` subtraction and regular subtraction?
**A:** `Counter - Counter` drops keys with zero or negative counts. `Counter({'a': 2}) - Counter({'a': 3})` gives `Counter()`, not `Counter({'a': -1})`.

### Q13: When you hear "subarray sum equals K", what pattern do you use?
**A:** Prefix Sum + HashMap. Track `current_sum` and store counts of prior prefix sums. For each position, check if `current_sum - k` exists in the map.

### Q14: How do you handle the "sliding window with at most K distinct characters" problem?
**A:** Use a dict to track character frequencies in the window. Expand right (add char). While `len(freq) > k`, shrink left (decrement/remove char). Track `max(result, right - left + 1)`.

### Q15: What's the window size formula in sliding window?
**A:** `right - left + 1`. Common off-by-one: forgetting the `+ 1`.

### Q16: In the Contiguous Array (equal 0s and 1s) problem, what's the clever transformation?
**A:** Replace every 0 with -1. Now finding equal 0s and 1s becomes finding a subarray with sum 0 — a prefix sum + HashMap problem.

### Q17: How does 4Sum II work in O(n²)?
**A:** Split into two groups: compute all sums of nums1[i] + nums2[j] (store in hashmap), then for each nums3[k] + nums4[l], check if the complement exists. Two O(n²) passes instead of one O(n⁴) pass.

### Q18: What's the key insight for the Valid Sudoku problem?
**A:** Use three sets: one per row, one per column, one per 3x3 box. The box index is `(row // 3, col // 3)`. Check each filled cell against all three sets.

### Q19: When the problem says "return indices" instead of "return values", what changes?
**A:** Use a dict (value → index) instead of a set. You need to remember WHERE you saw each value, not just WHETHER you've seen it.

### Q20: What is encoding/decoding for a list of strings, and why is it tricky?
**A:** You need to serialize `["hello", "world"]` into a single string and back. It's tricky because strings can contain any character including delimiters. Solution: length-prefix encoding like `"5#hello5#world"`.

---

## Common Failure Points

### Failure 1: Store before check in Two Sum
**BUG:**
```python
seen[num] = i  # Store first
if comp in seen: ...  # Now num might match itself
```
**FIX:** Check for complement FIRST, then store.
**RULE:** In complement lookup, always check before storing.

### Failure 2: Missing {0: 1} in prefix sum
**BUG:** `prefix_count = {}` → misses subarrays starting at index 0.
**FIX:** `prefix_count = {0: 1}`.
**RULE:** The empty prefix (sum = 0, count = 1) must always be in the initial map.

### Failure 3: Using list as dict key
**BUG:** `groups[sorted(word)].append(word)` → TypeError.
**FIX:** `groups[tuple(sorted(word))].append(word)`.
**RULE:** Always convert lists to tuples before using as keys.

### Failure 4: Window size off-by-one
**BUG:** `result = max(result, right - left)` → one less than actual window.
**FIX:** `result = max(result, right - left + 1)`.
**RULE:** A window from index `left` to index `right` has `right - left + 1` elements.

### Failure 5: Not checking left pointer in sliding window
**BUG:**
```python
if s[right] in char_idx:
    left = char_idx[s[right]] + 1  # Might move left BACKWARDS!
```
**FIX:** `left = max(left, char_idx[s[right]] + 1)` — never move left backwards.
**RULE:** The left pointer should only move forward.

### Failure 6: Consecutive sequence without start check
**BUG:** Starting a while loop from every element → O(n²).
**FIX:** Only start when `num - 1 not in num_set` (it's the beginning of a sequence).
**RULE:** In consecutive sequence, only count from sequence starts.

### Failure 7: KeyError when counting
**BUG:** `freq[ch] += 1` when `ch` not in dict.
**FIX:** Use `freq.get(ch, 0) + 1`, or `defaultdict(int)`, or `Counter`.
**RULE:** Always handle missing keys — use `.get()`, `defaultdict`, or `Counter`.

### Failure 8: Forgetting edge case with empty input
**BUG:** Code crashes on `[]` or `""`.
**FIX:** Add guard clause: `if not nums: return 0`.
**RULE:** Always handle empty input — it's the first edge case interviewers check.

### Failure 9: Confusing `discard` vs `remove` for sets
**BUG:** `s.remove(x)` raises KeyError if x not present.
**FIX:** Use `s.discard(x)` for safe removal.
**RULE:** Use `discard` when you're not sure the element exists.

### Failure 10: Counter equality with extra keys
**BUG:** Assuming `Counter("abc") != Counter("aabbc")` catches length differences.
**FIX:** Counter comparison handles this correctly (different counts → not equal). But adding a `len(s) != len(t)` check at the start is a good O(1) early exit.
**RULE:** For anagram checks, check lengths first as a quick optimization.
