# Strings — Review (Spaced Repetition)

## Recall Prompts

Test yourself by answering each prompt from memory before checking the answer.

---

### Prompt 1: Immutability
**Q:** What happens when you call `s.upper()` without reassigning?  
**A:** Nothing changes. Strings are immutable — `.upper()` returns a new string. You must do `s = s.upper()`.

---

### Prompt 2: Indexing
**Q:** Given `s = "Python"`, what is `s[-2]`?  
**A:** `"o"` — negative indexing counts from the end: -1 is 'n', -2 is 'o'.

---

### Prompt 3: Slicing
**Q:** How do you reverse a string in one expression?  
**A:** `s[::-1]`

---

### Prompt 4: split() behavior
**Q:** What is the difference between `"  a  b  ".split()` and `"  a  b  ".split(" ")`?  
**A:** `.split()` → `['a', 'b']` (splits on any whitespace, strips edges, no empties). `.split(" ")` → `['', '', 'a', '', 'b', '', '']` (splits on each single space literally).

---

### Prompt 5: find() vs index()
**Q:** What happens when the substring isn't found for `.find()` vs `.index()`?  
**A:** `.find()` returns `-1`. `.index()` raises `ValueError`.

---

### Prompt 6: is* on empty strings
**Q:** What does `"".isalpha()` return?  
**A:** `False`. All `is*` methods return `False` on empty strings.

---

### Prompt 7: join()
**Q:** How do you join a list of strings with a comma?  
**A:** `",".join(my_list)` — the separator calls `.join()`, the list is the argument.

---

### Prompt 8: String concatenation performance
**Q:** Why is `+=` in a loop slow for strings? What's the fix?  
**A:** Strings are immutable, so each `+=` creates a new string and copies everything — O(n²) total. Fix: collect parts in a list, then `"".join(parts)`.

---

### Prompt 9: f-string formatting
**Q:** How do you format a float to 2 decimal places in an f-string?  
**A:** `f"{value:.2f}"`

---

### Prompt 10: Palindrome check
**Q:** Write a one-line palindrome check that ignores case and non-alphanumeric characters.  
**A:** `cleaned = "".join(c.lower() for c in s if c.isalnum()); cleaned == cleaned[::-1]`

---

### Prompt 11: Anagram check
**Q:** What's the simplest way to check if two strings are anagrams?  
**A:** `sorted(s1.lower()) == sorted(s2.lower())` — or use `Counter` for O(n) instead of O(n log n).

---

### Prompt 12: partition()
**Q:** What does `"user@example.com".partition("@")` return?  
**A:** `('user', '@', 'example.com')` — a 3-tuple of (before, sep, after).

---

### Prompt 13: Escape characters
**Q:** How do you create a string with a literal backslash?  
**A:** `"\\"` or use a raw string `r"\"`. In raw strings, backslashes are literal.

---

### Prompt 14: str + int TypeError
**Q:** Why does `"age: " + 25` fail? How do you fix it?  
**A:** Python doesn't auto-convert int to str. Fix: `f"age: {25}"` or `"age: " + str(25)`.

---

### Prompt 15: Slicing out of bounds
**Q:** Does `"abc"[0:100]` raise an error?  
**A:** No — slicing is forgiving. Returns `"abc"`. But `"abc"[100]` raises `IndexError`.

---

### Prompt 16: Character frequency
**Q:** Write code to count character frequencies in a string.  
**A:** `from collections import Counter; freq = Counter(s)` — or build manually with a dict loop.

---

### Prompt 17: ord() and chr()
**Q:** What does `ord('A')` return? What does `chr(97)` return?  
**A:** `ord('A')` → `65`. `chr(97)` → `'a'`.

---

### Prompt 18: String comparison
**Q:** In what order does Python compare strings: digits, uppercase, lowercase?  
**A:** Digits (48–57) < Uppercase (65–90) < Lowercase (97–122). Comparison is lexicographic by Unicode code point.

---

### Prompt 19: strip() with arguments
**Q:** What does `"xxhelloxx".strip("x")` return?  
**A:** `"hello"` — strips all leading and trailing characters in the argument set (not the substring).

---

### Prompt 20: replace() count
**Q:** How do you replace only the first 2 occurrences of a substring?  
**A:** `s.replace("old", "new", 2)` — the third argument is the max number of replacements.

---

## Common Failure Points

### 1. Forgetting to reassign after string methods
```python
# BUG
name = "alice"
name.capitalize()  # returns "Alice" but name is still "alice"

# FIX
name = name.capitalize()
```

### 2. Using split(" ") when split() is intended
```python
# BUG — creates empty strings
"  hello   world  ".split(" ")  # ['', '', 'hello', '', '', 'world', '', '']

# FIX
"  hello   world  ".split()  # ['hello', 'world']
```

### 3. Concatenating string + non-string
```python
# BUG
"count: " + 5  # TypeError

# FIX
f"count: {5}"  # or "count: " + str(5)
```

### 4. Using .find() result without checking
```python
# BUG — .find() returns -1 if not found, which is a valid negative index
s = "hello"
idx = s.find("z")  # -1
s[idx]              # "o" ← wrong! Wanted "not found"

# FIX
idx = s.find("z")
if idx != -1:
    print(s[idx])
```

### 5. Building strings with += in a loop
```python
# SLOW — O(n²) total
result = ""
for word in words:
    result += word + " "

# FAST — O(n) total
result = " ".join(words)
```

### 6. Confusing split() max-split direction
```python
"a.b.c.d".split(".", 1)   # ['a', 'b.c.d']  ← splits from LEFT
"a.b.c.d".rsplit(".", 1)  # ['a.b.c', 'd']   ← splits from RIGHT
```

### 7. Case-insensitive comparison without .lower()
```python
# BUG
user_input = "Yes"
if user_input == "yes":  # False!
    ...

# FIX
if user_input.lower() == "yes":
    ...
```

### 8. Off-by-one with slicing
```python
s = "abcde"
s[1:3]  # "bc" — stop index is EXCLUDED
# To get characters at index 1, 2, 3 → s[1:4]
```
