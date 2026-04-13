# Tuples — Review (Spaced Repetition)

## Spaced Repetition Prompts

### Creation & Syntax

1. **How do you create a single-element tuple?** What goes wrong if you forget the comma?

2. **What are 4 ways to create a tuple?** (literal, packing, `tuple()`, `tuple(iterable)`)

3. **What does `x = 5,` create?** Why is this dangerous?

4. **What does `tuple("hello")` return?** What about `tuple([1, 2, 3])`?

### Immutability

5. **Explain shallow immutability.** Can you give an example where a tuple's contents appear to change?

6. **How do you "replace" an element at index i in a tuple?** Write the slicing pattern from memory.

7. **What happens when you run `t[0] += [3, 4]` where `t = ([1, 2],)`?** (Trick question — two things happen.)

### Unpacking

8. **Write the star-unpacking pattern to get the first element and all remaining.** What type is the "rest" variable?

9. **How do you ignore elements during unpacking?** Write an example with `_`.

10. **How do you swap two variables using tuple unpacking?** Why does this work without a temp variable?

### Methods & Built-ins

11. **Name both tuple methods.** What does each return? What error does `.index()` raise if the element is missing?

12. **Does `sorted(my_tuple)` return a tuple or a list?** How do you get a sorted tuple?

13. **List 5 built-in functions that work on tuples.** (`len`, `min`, `max`, `sum`, `sorted`, `any`, `all`, `enumerate`, `zip`)

### Hashability & Dict Keys

14. **When is a tuple hashable?** When is it NOT hashable? Give examples.

15. **Why can tuples be dict keys but lists cannot?** Connect this to the concept of hashability.

### Named Tuples

16. **Write the code to create a `Color` named tuple with fields `r`, `g`, `b` and instantiate it.** From memory.

17. **What does `._replace()` do?** Does it modify the original or return a new one?

18. **What do `._asdict()` and `._fields` return?**

### Patterns

19. **Write the pattern to sort a list of `(name, score)` tuples by score.** Use a `key` function.

20. **Write the pattern to use tuples as visited-set keys for grid coordinates.** E.g., BFS/DFS visited tracking.

---

## Common Failure Points

### 1. Single-Element Trap
- **Bug**: `single = ("hello")` — this is a string, not a tuple
- **Fix**: `single = ("hello",)` — trailing comma is mandatory
- **Prevention**: Always add the comma for single-element tuples. Look for this in code review.

### 2. Forgetting `sorted()` Returns a List
- **Bug**: Expecting `sorted((3,1,2))` to return `(1,2,3)`
- **Fix**: `tuple(sorted((3,1,2)))` → `(1,2,3)`
- **Prevention**: Always wrap `sorted()` in `tuple()` when you need a tuple back.

### 3. Trying to Mutate a Tuple
- **Bug**: `t[0] = 99` → `TypeError`
- **Fix**: Create a new tuple with slicing: `t = t[:0] + (99,) + t[1:]`
- **Prevention**: If you need to modify elements, use a list instead. Convert back to tuple when done.

### 4. Star Unpacking Returns a List
- **Bug**: Assuming `*rest` in `first, *rest = (1,2,3)` gives a tuple
- **Reality**: `rest` is `[2, 3]` — a **list**
- **Prevention**: Wrap with `tuple(rest)` if you need a tuple.

### 5. Accidental Tuple via Trailing Comma
- **Bug**: `x = 42,` → `x` is `(42,)` not `42`
- **Fix**: Remove the comma: `x = 42`
- **Prevention**: Watch for trailing commas in assignments. Linters can catch this.

### 6. Using Unhashable Tuple as Dict Key
- **Bug**: `d = {}; d[(1, [2,3])] = "val"` → `TypeError: unhashable type: 'list'`
- **Fix**: Use only immutable elements: `d[(1, (2,3))] = "val"` or `d[(1, frozenset({2,3}))] = "val"`
- **Prevention**: Before using a tuple as a key, ensure ALL nested elements are also immutable.

### 7. The `+=` Paradox
- **Bug**: `t = ([1,2],); t[0] += [3]` — raises TypeError BUT the list IS modified
- **Why**: `+=` first calls `list.extend()` (succeeds), then tries to reassign `t[0]` (fails on tuple)
- **Prevention**: Use `t[0].extend([3])` instead of `+=` for lists inside tuples.

### 8. `.index()` Crash on Missing Value
- **Bug**: `(1,2,3).index(99)` → `ValueError`
- **Fix**: Check with `in` first: `if 99 in t: t.index(99)` or use `try/except`
- **Prevention**: Always guard `.index()` calls. Consider writing a `safe_index` helper.

### 9. Confusing Tuple Comparison with Element Comparison
- **Bug**: Thinking `(1, 10) < (2, 1)` compares sums. It doesn't.
- **Reality**: Lexicographic comparison — compares element by element left to right
- **Prevention**: Remember: tuple comparison = dictionary ordering (first element wins unless tied).

### 10. Forgetting Tuples Are Iterable
- **Bug**: Writing `for i in range(len(t)): ... t[i]` instead of iterating directly
- **Fix**: `for item in t:` or `for i, item in enumerate(t):`
- **Prevention**: Always prefer direct iteration. Use `enumerate()` when you need the index.
