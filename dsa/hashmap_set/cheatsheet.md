# HashMap / HashSet — Cheatsheet

## When to Use HashMap/Set

**The #1 signal:** Your brute force has nested loops where the inner loop SEARCHES for something. Replace the inner loop with a hash map lookup → O(n²) becomes O(n).

---

## The 7 Patterns

| # | Pattern | When to Use | Data Structure | Template |
|---|---------|-------------|----------------|----------|
| 1 | **Complement Lookup** | Find pair satisfying condition | `dict` or `set` | Store seen, check `target - num` |
| 2 | **Frequency Map** | Count, compare distributions | `Counter` / `dict` | Count occurrences, query counts |
| 3 | **Seen Set** | Duplicates, uniqueness | `set` | `if x in seen` |
| 4 | **Group By Key** | Bucket by computed property | `defaultdict(list)` | Compute key, group by it |
| 5 | **Index Map** | Need positions, not just values | `dict` (val→idx) | Store value→index |
| 6 | **Prefix Sum + Map** | Subarray sum = K | `dict` (sum→count) | Running sum, check `sum - k` |
| 7 | **Window + Map** | Substring with constraints | `dict` (char→count) | Expand right, shrink left |

---

## Pattern Templates

### 1. Complement Lookup (Two Sum)
```python
def two_sum(nums, target):
    seen = {}  # value → index
    for i, num in enumerate(nums):
        comp = target - num
        if comp in seen:
            return [seen[comp], i]
        seen[num] = i
    return []
```

### 2. Frequency Map
```python
from collections import Counter
freq = Counter(arr)
freq.most_common(k)       # Top k
Counter(s) == Counter(t)  # Anagram check
```

### 3. Seen Set
```python
seen = set()
for x in arr:
    if x in seen: return True  # Duplicate
    seen.add(x)
```

### 4. Group By Key
```python
from collections import defaultdict
groups = defaultdict(list)
for item in items:
    key = compute_key(item)  # The hard part
    groups[key].append(item)
return list(groups.values())
```

### 5. Index Map
```python
index_map = {}  # value → last seen index
for i, num in enumerate(nums):
    if num in index_map and i - index_map[num] <= k:
        return True
    index_map[num] = i
```

### 6. Prefix Sum + HashMap
```python
def subarray_sum(nums, k):
    prefix_count = {0: 1}  # MUST initialize with {0: 1}
    current_sum = 0
    count = 0
    for num in nums:
        current_sum += num
        if current_sum - k in prefix_count:
            count += prefix_count[current_sum - k]
        prefix_count[current_sum] = prefix_count.get(current_sum, 0) + 1
    return count
```

### 7. Sliding Window + HashMap
```python
def longest_unique(s):
    char_idx = {}
    left = 0
    result = 0
    for right in range(len(s)):
        if s[right] in char_idx and char_idx[s[right]] >= left:
            left = char_idx[s[right]] + 1
        char_idx[s[right]] = right
        result = max(result, right - left + 1)
    return result
```

---

## Keyword → Pattern Mapping

| Problem Keywords | Pattern |
|-----------------|---------|
| "two numbers that sum to" | Complement Lookup |
| "anagram", "permutation" | Frequency Map |
| "duplicate", "repeating" | Seen Set |
| "consecutive sequence" | Seen Set |
| "group", "categorize" | Group By Key |
| "return indices" | Index Map |
| "subarray sum equals K" | Prefix Sum + HashMap |
| "contiguous subarray" + sum | Prefix Sum + HashMap |
| "longest substring" + constraint | Sliding Window + HashMap |
| "minimum window" | Sliding Window + HashMap |

---

## Python Tools

| Tool | Best For | Key Feature |
|------|----------|-------------|
| `dict` | General key-value | `.get(key, default)` |
| `set` | Membership only | O(1) `in`, `.add()`, `.discard()` |
| `defaultdict(type)` | Auto-initialize | `defaultdict(list)`, `defaultdict(int)` |
| `Counter` | Counting | `.most_common(k)`, arithmetic, `Counter(s) == Counter(t)` |

---

## Complexity

| Operation | dict/set Average | Worst Case |
|-----------|-----------------|------------|
| Insert | O(1) | O(n) |
| Lookup | O(1) | O(n) |
| Delete | O(1) | O(n) |
| Build n items | O(n) | O(n²) |

**In interviews:** Always say O(1) average. Mention worst-case O(n) only if asked.

**Space tip:** If bounded alphabet (26 lowercase letters), space is O(1).

---

## Critical Gotchas

| Gotcha | Problem | Fix |
|--------|---------|-----|
| Unhashable keys | `d[[1,2]] = x` → TypeError | Convert to `tuple` |
| KeyError | `d[key] += 1` when missing | Use `.get(key, 0)` or `defaultdict` |
| Modify during iteration | `del d[k]` in loop → RuntimeError | Iterate over `list(d.keys())` |
| Prefix sum missing base | Misses subarrays starting at index 0 | Initialize `{0: 1}` |
| Window size off-by-one | `right - left` instead of `right - left + 1` | Window length = `right - left + 1` |
| Store before check (Two Sum) | Returns same index twice | Check complement BEFORE storing |
| Consecutive sequence O(n²) | Start counting from every element | Only start when `num - 1 not in set` |

---

## Interview Execution Checklist

1. **State brute force** (30s): "Nested loops → O(n²)"
2. **Identify optimization** (30s): "HashMap for O(1) lookup → O(n)"
3. **Explain approach** (1min): Walk through the pattern
4. **Edge cases** (30s): Empty, single element, no solution, duplicates
5. **Code it** (5-10min): Clean, using pattern template
6. **Trace example** (2min): Show map state at each step
7. **Complexity** (30s): "Time O(n), Space O(n)"
