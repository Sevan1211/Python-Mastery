# Strings — Cheatsheet

## Creating Strings

```python
s = "hello"               # double quotes
s = 'hello'               # single quotes (identical)
s = """multi
line"""                    # triple quotes
s = r"no\nescape"         # raw string → literal backslashes
s = f"value is {x}"       # f-string (Python 3.6+)
s = "hello" "world"       # adjacent literal concatenation → "helloworld"
```

## Indexing & Slicing

```python
s = "Python"
s[0]          # 'P'       first character
s[-1]         # 'n'       last character
s[1:4]        # 'yth'     start:stop (stop excluded)
s[:3]         # 'Pyt'     first 3
s[3:]         # 'hon'     from index 3 to end
s[::2]        # 'Pto'     every 2nd character
s[::-1]       # 'nohtyP'  reversed
s[1:5:2]      # 'yh'      start:stop:step
```

**Out-of-bounds slicing is safe** — no error, just truncated result:
```python
s[0:100]      # 'Python'  (no IndexError)
s[100:]       # ''
```

**Single index out of bounds raises `IndexError`:**
```python
s[100]        # IndexError
```

---

## Immutability

Strings **cannot be modified in-place**. Every operation returns a **new** string.

```python
s = "hello"
s[0] = "H"       # TypeError!
s = "H" + s[1:]  # ✓ creates new string "Hello"
s = s.upper()    # ✓ must reassign
```

---

## Operators

| Operator | Example | Result |
|----------|---------|--------|
| `+` | `"ab" + "cd"` | `"abcd"` |
| `*` | `"ab" * 3` | `"ababab"` |
| `in` | `"ell" in "hello"` | `True` |
| `not in` | `"xyz" not in "hello"` | `True` |
| `==` | `"abc" == "abc"` | `True` |
| `<` | `"abc" < "abd"` | `True` (lexicographic) |

---

## Case Methods

| Method | Example | Result |
|--------|---------|--------|
| `.upper()` | `"hello".upper()` | `"HELLO"` |
| `.lower()` | `"HELLO".lower()` | `"hello"` |
| `.capitalize()` | `"hELLO".capitalize()` | `"Hello"` |
| `.title()` | `"hello world".title()` | `"Hello World"` |
| `.swapcase()` | `"Hello".swapcase()` | `"hELLO"` |
| `.casefold()` | `"Straße".casefold()` | `"strasse"` |

---

## Whitespace Methods

| Method | Example | Result |
|--------|---------|--------|
| `.strip()` | `"  hi  ".strip()` | `"hi"` |
| `.lstrip()` | `"  hi  ".lstrip()` | `"hi  "` |
| `.rstrip()` | `"  hi  ".rstrip()` | `"  hi"` |
| `.strip("xy")` | `"xxyyx".strip("xy")` | `""` |

---

## Searching

| Method | Returns | Not Found |
|--------|---------|-----------|
| `.find(sub)` | first index | `-1` |
| `.rfind(sub)` | last index | `-1` |
| `.index(sub)` | first index | `ValueError` |
| `.rindex(sub)` | last index | `ValueError` |
| `.count(sub)` | count of non-overlapping occurrences | `0` |
| `.startswith(s)` | `True` / `False` | — |
| `.endswith(s)` | `True` / `False` | — |

**Tip:** Prefer `.find()` over `.index()` — avoids exceptions.

---

## Splitting & Joining

```python
"a,b,c".split(",")           # ['a', 'b', 'c']
"  hello  world  ".split()   # ['hello', 'world']  ← splits on ANY whitespace, strips edges
"a,b,c".split(",", 1)        # ['a', 'b,c']  ← maxsplit
"a\nb\nc".splitlines()       # ['a', 'b', 'c']

",".join(["a", "b", "c"])    # "a,b,c"
" ".join(["hello", "world"]) # "hello world"
"".join(["a", "b", "c"])     # "abc"
```

**Critical:** `.split()` with no args handles multiple spaces and strips. `.split(" ")` does **not**.

```python
"  a  b  ".split()       # ['a', 'b']
"  a  b  ".split(" ")    # ['', '', 'a', '', 'b', '', '']
```

---

## Replacing

```python
"hello world".replace("world", "Python")   # "hello Python"
"aaa".replace("a", "b", 2)                 # "bba"  ← max replacements
```

---

## Boolean Checks (`is*` methods)

| Method | True When |
|--------|-----------|
| `.isalpha()` | all alphabetic, len > 0 |
| `.isdigit()` | all digits, len > 0 |
| `.isalnum()` | all alphanumeric, len > 0 |
| `.isspace()` | all whitespace, len > 0 |
| `.isupper()` | all cased chars are upper, has ≥1 cased |
| `.islower()` | all cased chars are lower, has ≥1 cased |
| `.istitle()` | titlecase rules |
| `.isnumeric()` | all numeric (includes ½, ², etc.) |
| `.isdecimal()` | all decimal digits (strict) |
| `.isascii()` | all chars are ASCII (Python 3.7+) |
| `.isidentifier()` | valid Python identifier |
| `.isprintable()` | all printable characters |

**Gotcha:** All `is*` methods return `False` on empty strings.

---

## Padding & Alignment

```python
"hi".center(10)        # '    hi    '
"hi".center(10, "-")   # '----hi----'
"hi".ljust(10)         # 'hi        '
"hi".rjust(10)         # '        hi'
"42".zfill(5)          # '00042'
"-42".zfill(5)         # '-0042'  ← preserves sign
```

---

## Formatting

```python
# f-strings (preferred)
name = "Alice"
f"Hello, {name}!"                  # "Hello, Alice!"
f"{3.14159:.2f}"                   # "3.14"
f"{42:05d}"                        # "00042"
f"{'hi':>10}"                      # "        hi"
f"{'hi':<10}"                      # "hi        "
f"{'hi':^10}"                      # "    hi    "

# .format()
"Hello, {}!".format("Bob")         # "Hello, Bob!"
"{0} and {1}".format("a", "b")     # "a and b"
"{name}".format(name="Alice")      # "Alice"

# % formatting (legacy)
"Hello, %s! You are %d." % ("Bob", 25)
```

---

## Escape Characters

| Escape | Meaning |
|--------|---------|
| `\n` | newline |
| `\t` | tab |
| `\\` | literal backslash |
| `\'` | single quote |
| `\"` | double quote |
| `\0` | null |

Use raw strings `r"..."` to disable escaping.

---

## Partition

```python
"user@example.com".partition("@")     # ('user', '@', 'example.com')
"user@example.com".rpartition("@")    # ('user', '@', 'example.com')
"no-at-sign".partition("@")           # ('no-at-sign', '', '')
```

Returns a 3-tuple: `(before, separator, after)`. Useful for splitting on the first/last occurrence.

---

## Built-in Functions

```python
len("hello")              # 5
ord("A")                  # 65
chr(65)                   # 'A'
sorted("cba")             # ['a', 'b', 'c']  ← returns list
"".join(sorted("cba"))    # "abc"
list("abc")               # ['a', 'b', 'c']
str(42)                   # "42"
repr("hello\n")           # "'hello\\n'"  ← shows escape chars
```

---

## Iteration Patterns

```python
# Character by character
for ch in "hello":
    print(ch)

# With index
for i, ch in enumerate("hello"):
    print(i, ch)

# Build new string from parts
result = "".join(ch.upper() for ch in "hello" if ch != "l")  # "HEO"
```

---

## Common Gotchas

1. **Forgot to reassign:** `s.upper()` does nothing unless you do `s = s.upper()`
2. **`split()` vs `split(" ")`:** No-arg version handles multiple spaces
3. **`is*` on empty string:** Always `False`
4. **`str + int`:** `"age: " + 42` → TypeError. Use f-string: `f"age: {42}"`
5. **`.find()` returns -1:** Don't use result as index without checking
6. **Negative slicing:** `s[-1:]` is last char, `s[:-1]` is all but last
7. **`replace()` isn't in-place:** Must reassign
8. **Concatenation in loops:** Use `"".join(parts)` instead of `+=`

---

## Common Interview Patterns

| Pattern | Technique |
|---------|-----------|
| Reverse a string | `s[::-1]` |
| Check palindrome | `cleaned == cleaned[::-1]` |
| Check anagram | `sorted(s1) == sorted(s2)` or Counter |
| Character frequency | `Counter(s)` or dict loop |
| First unique char | frequency dict → first with count 1 |
| String compression | track current char + count |
| Longest substring without repeats | sliding window + set |
| Valid parentheses | stack |

---

## Performance Notes

| Operation | Time Complexity |
|-----------|----------------|
| `s[i]` indexing | O(1) |
| `s[a:b]` slicing | O(b-a) |
| `len(s)` | O(1) |
| `s + t` concat | O(len(s) + len(t)) |
| `s += t` in loop (n times) | O(n²) total — avoid! |
| `"".join(list)` | O(total length) — use this instead |
| `s.find(sub)` | O(n*m) worst case |
| `s.replace(old, new)` | O(n) |
| `s.split()` | O(n) |
| `sorted(s)` | O(n log n) |
