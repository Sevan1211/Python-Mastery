# Dictionaries — Review (Spaced Repetition)

Use this file for spaced repetition. Cover the **A** (answer), try to recall it from the **Q** (question), then reveal to check.

---

## Recall Prompts

### Q1: What is the time complexity of looking up a key in a dict?
**A:** O(1) average, O(n) worst case (hash collisions). In practice, always treat it as O(1).

---

### Q2: What happens when you access `d["key"]` and the key doesn't exist?
**A:** Raises `KeyError`. Use `d.get("key")` or `d.get("key", default)` for safe access.

---

### Q3: What's the difference between `.get()` and `.setdefault()`?
**A:** `.get(key, default)` returns the default but does **not** modify the dict. `.setdefault(key, default)` returns the existing value if the key exists, or **inserts** the default and returns it if the key is missing. `setdefault` actually changes the dict; `get` never does.

---

### Q4: What does `in` check on a dict — keys or values?
**A:** **Keys only.** `"Alice" in d` checks if `"Alice"` is a key. To check values, use `"Alice" in d.values()`. Key check is O(1); value check is O(n).

---

### Q5: Name 4 ways to remove entries from a dict and what each returns.
**A:**
1. `del d[key]` — returns nothing, raises `KeyError` if missing
2. `d.pop(key)` — returns the value, raises `KeyError` if missing
3. `d.pop(key, default)` — returns value or default, never raises
4. `d.popitem()` — returns `(key, value)` tuple of last item, `KeyError` if empty
5. `d.clear()` — returns `None`, removes everything

---

### Q6: What are dict views (`.keys()`, `.values()`, `.items()`)? Are they live?
**A:** Dict views are **live views** of the dict. They reflect changes to the dict automatically. If you add or remove a key, the existing view objects update instantly. They are NOT snapshots.

---

### Q7: Write the most Pythonic way to count frequencies of items in a list.
**A:**
```python
freq = {}
for item in items:
    freq[item] = freq.get(item, 0) + 1
```
Even better: use `collections.Counter(items)` (covered in a later module).

---

### Q8: What's wrong with `dict.fromkeys(["a", "b", "c"], [])`?
**A:** All keys share the **same list object**. Mutating one mutates all. Fix: use `{k: [] for k in keys}` to create separate lists.

---

### Q9: Name 3 ways to merge two dicts. Which creates a new dict?
**A:**
1. `d1.update(d2)` — in-place (modifies d1)
2. `{**d1, **d2}` — **new dict** (Python 3.5+)
3. `d1 | d2` — **new dict** (Python 3.9+)
4. `d1 |= d2` — in-place (Python 3.9+)

---

### Q10: What types can be dictionary keys? What types cannot? Why?
**A:** Keys must be **hashable** (have an immutable hash value). Allowed: `int`, `float`, `str`, `bool`, `None`, `tuple` (of hashables), `frozenset`. Not allowed: `list`, `dict`, `set` — they're mutable, so their hash could change.

---

### Q11: What happens with `{True: "a", 1: "b"}` and why?
**A:** Result is `{True: "b"}` with only 1 key. Because `True == 1` and `hash(True) == hash(1)`, Python considers them the same key. The value gets overwritten but the original key (`True`) is kept.

---

### Q12: What's the difference between `d.copy()` and `copy.deepcopy(d)`?
**A:** `d.copy()` is a **shallow copy** — creates a new dict, but mutable values (lists, nested dicts) are shared between original and copy. `copy.deepcopy(d)` recursively copies everything, making a completely independent copy.

---

### Q13: How do you find the key with the maximum value in a dict?
**A:**
```python
max_key = max(d, key=d.get)
```
`max(d)` alone returns the max **key** alphabetically/numerically. The `key=d.get` parameter tells `max` to compare by **values** instead.

---

### Q14: Show the grouping pattern with `.setdefault()`.
**A:**
```python
groups = {}
for item in items:
    key = get_group_key(item)
    groups.setdefault(key, []).append(item)
```
This creates a new empty list for each new key, then appends to it. One line instead of 3-4 with an `if` check.

---

### Q15: What error do you get if you modify a dict while iterating over it?
**A:** `RuntimeError: dictionary changed size during iteration`. Fix: iterate over `list(d.items())` to make a snapshot, or build a new dict with a comprehension.

---

### Q16: Write the Two Sum solution using a dict.
**A:**
```python
def two_sum(nums, target):
    seen = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    return []
```
O(n) time, O(n) space. The dict maps values to their indices.

---

### Q17: Are Python dicts ordered? Since when?
**A:** Yes, since **Python 3.7** (language specification guarantee). CPython had ordered dicts since 3.6 as an implementation detail, but 3.7 made it official. Updating an existing key does NOT change its position. Deleting and re-inserting a key moves it to the end.

---

### Q18: What's the difference between `=` assignment and `.copy()` for dicts?
**A:** `b = a` makes `b` an **alias** — both names point to the **same dict object**. Changing one changes the other. `b = a.copy()` creates a **new dict** (shallow). Changes to `b`'s top-level keys don't affect `a`.

---

### Q19: How do you safely access deeply nested dict values without crashing?
**A:** Chain `.get()` calls:
```python
val = d.get("level1", {}).get("level2", {}).get("level3", "default")
```
Each `.get()` returns an empty dict if the key is missing, so the chain never crashes.

---

### Q20: Why does `.update()` return `None`?
**A:** Because it modifies the dict **in-place**, following Python's convention that in-place mutating methods return `None` (same as `list.sort()`, `list.append()`, etc.). Don't write `result = d.update(...)` — `result` will be `None`.

---

## Common Failure Points

### Failure 1: KeyError on missing key

**BUG:**
```python
user = {"name": "Alice"}
email = user["email"]  # KeyError: 'email'
```

**WHY:** Bracket access crashes when the key doesn't exist.

**FIX:**
```python
email = user.get("email", "unknown")
```

**PREVENTION:** Default to `.get()` for any key that might be missing. Only use `[]` when you're certain the key exists or intentionally want an error.

---

### Failure 2: Modifying dict during iteration

**BUG:**
```python
scores = {"Alice": 95, "Bob": 60, "Charlie": 72}
for name, score in scores.items():
    if score < 70:
        del scores[name]  # RuntimeError!
```

**WHY:** You can't change a dict's size while iterating over it.

**FIX:**
```python
# Option A: iterate over a snapshot
for name, score in list(scores.items()):
    if score < 70:
        del scores[name]

# Option B: build a new dict (cleaner)
scores = {k: v for k, v in scores.items() if v >= 70}
```

**PREVENTION:** Never use `del` inside a `for k, v in d.items()` loop. Use comprehensions to filter.

---

### Failure 3: fromkeys with mutable default

**BUG:**
```python
groups = dict.fromkeys(["a", "b", "c"], [])
groups["a"].append(1)
print(groups)  # {'a': [1], 'b': [1], 'c': [1]} — shared!
```

**WHY:** `fromkeys` creates ONE list object and assigns it to ALL keys.

**FIX:**
```python
groups = {k: [] for k in ["a", "b", "c"]}
```

**PREVENTION:** Never pass a mutable object (list, dict, set) as the default to `fromkeys`. Always use a comprehension.

---

### Failure 4: Alias instead of copy

**BUG:**
```python
original = {"a": 1, "b": 2}
backup = original
backup["c"] = 3
print(original)  # {'a': 1, 'b': 2, 'c': 3} — modified!
```

**WHY:** `=` creates an alias, not a copy. Both variables point to the same object.

**FIX:**
```python
backup = original.copy()
```

**PREVENTION:** Anytime you want an independent copy, explicitly call `.copy()` or `dict(original)`.

---

### Failure 5: Shallow copy with nested mutable values

**BUG:**
```python
original = {"data": [1, 2, 3]}
backup = original.copy()
backup["data"].append(4)
print(original)  # {'data': [1, 2, 3, 4]} — changed!
```

**WHY:** `.copy()` is shallow — nested lists/dicts are still shared.

**FIX:**
```python
import copy
backup = copy.deepcopy(original)
```

**PREVENTION:** If your dict contains mutable values (lists, nested dicts), always use `copy.deepcopy()`.

---

### Failure 6: Checking values with `in` instead of `in d.values()`

**BUG:**
```python
d = {"admin": "Alice", "user": "Bob"}
print("Alice" in d)  # False — checking keys, not values!
```

**WHY:** `in` checks **keys** by default.

**FIX:**
```python
print("Alice" in d.values())  # True
```

**PREVENTION:** Remember: `in d` → keys, `in d.values()` → values, `in d.items()` → (key, value) tuples.

---

### Failure 7: Mutable default argument in function

**BUG:**
```python
def add_user(name, users={}):
    users[name] = True
    return users

print(add_user("Alice"))  # {'Alice': True}
print(add_user("Bob"))    # {'Alice': True, 'Bob': True} — shared!
```

**WHY:** Default mutable arguments are created once at function definition time and shared across calls.

**FIX:**
```python
def add_user(name, users=None):
    if users is None:
        users = {}
    users[name] = True
    return users
```

**PREVENTION:** Never use `{}`, `[]`, or `set()` as default arguments. Always use `None` and create inside the function.

---

### Failure 8: .update() returns None

**BUG:**
```python
config = {"host": "localhost"}
result = config.update({"port": 8080})
print(result)  # None — not the updated dict!
```

**WHY:** `.update()` modifies in-place and returns `None`, following Python's convention.

**FIX:**
```python
config = {"host": "localhost"}
config.update({"port": 8080})
result = config  # use config directly
# Or create a new dict:
result = {**config, "port": 8080}
```

**PREVENTION:** Never assign the return value of `.update()`. It always returns `None`.

---

### Failure 9: True/1 and False/0 key collision

**BUG:**
```python
d = {}
d[True] = "yes"
d[1] = "one"
print(d)  # {True: 'one'} — only one key!
```

**WHY:** `True == 1` and `hash(True) == hash(1)`, so Python treats them as the same key.

**FIX:** Don't mix boolean and integer keys. If you must, be aware they collide.

**PREVENTION:** In practice, this rarely matters. But in interviews, it's a popular trick question.

---

### Failure 10: Forgetting that .get() doesn't insert

**BUG:**
```python
d = {}
d.get("count", 0)
print(d)  # {} — nothing was inserted!
d["count"] += 1  # KeyError! 'count' still doesn't exist
```

**WHY:** `.get()` returns the default but never modifies the dict. Use `.setdefault()` if you want insertion.

**FIX:**
```python
d.setdefault("count", 0)  # Inserts 0 if missing
d["count"] += 1           # Now works

# Or the idiomatic counting pattern:
d["count"] = d.get("count", 0) + 1
```

**PREVENTION:** Know the difference: `.get()` = read-only fallback, `.setdefault()` = insert-if-missing.
