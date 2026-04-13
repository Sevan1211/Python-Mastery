# Mistakes Log

Track repeated mistakes to build prevention habits.

## Format

Each entry must include:
- **Bug**: What went wrong
- **Why**: Root cause
- **Fix**: Corrected pattern
- **Rule**: Prevention rule going forward

---

### 1. `.join()` argument order
- **Bug**: Tried `words.join(", ")` instead of `", ".join(words)`
- **Why**: Intuition says the collection should call join, but Python puts the separator first
- **Fix**: `separator.join(iterable)` — the separator is what you call `.join()` on
- **Rule**: Read it as "use this separator to join that list"

### 2. Case-insensitive comparison
- **Bug**: Forgot to normalize case before comparing/counting substrings
- **Why**: `"Hello".count("hello")` returns 0 — string methods are case-sensitive by default
- **Fix**: Always `.lower()` both sides when case doesn't matter
- **Rule**: Any time a problem says "case-insensitive", normalize immediately
