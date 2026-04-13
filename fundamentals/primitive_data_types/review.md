# Primitive Data Types — Review

## Spaced Repetition Prompts

Answer these from memory before checking:

1. Name Python's 5 primitive types.
2. What does `type(True + 1)` return? Why?
3. List all falsy values in Python.
4. Why does `0.1 + 0.2 != 0.3`? How do you fix it?
5. What's the difference between `/`, `//`, and `%`?
6. Why use `is None` instead of `== None`?
7. What happens when you do `int("10.5")`? How do you handle it?
8. Is `-1` truthy or falsy?
9. Can Python handle `10 ** 1000`? Why?
10. What's the difference between `isinstance(x, int)` and `type(x) == int`?

## Common Failure Points

- Forgetting that `10 / 2` returns `5.0` (float), not `5` (int)
- Thinking `"0"` is falsy (it's truthy — it's a non-empty string)
- Using `== None` instead of `is None`
- Trying `int("10.5")` directly (need two-step conversion)
- Assuming `0.1 + 0.2 == 0.3` is True
- Forgetting that Python floor division rounds toward negative infinity: `-7 // 2 == -4`, not `-3`
