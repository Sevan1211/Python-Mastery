# Functions — Review

## Spaced Repetition Prompts

### Q1: What keyword defines a function in Python?
**A:** `def`, followed by the function name, parentheses with parameters, and a colon.

### Q2: What does a function return if there's no `return` statement?
**A:** `None` — every function implicitly returns `None` if no explicit return is reached.

### Q3: What's the difference between a parameter and an argument?
**A:** A **parameter** is a variable in the function definition. An **argument** is the actual value passed when calling the function.

### Q4: What is the mutable default argument trap?
**A:** Default values are evaluated **once** at definition time. If the default is mutable (list, dict, set), it's shared across all calls. Fix: use `None` as default and create the mutable inside the function body.

### Q5: What does `*args` collect?
**A:** Extra positional arguments as a **tuple**.

### Q6: What does `**kwargs` collect?
**A:** Extra keyword arguments as a **dictionary**.

### Q7: What is the LEGB rule?
**A:** Python's scope resolution order: **L**ocal → **E**nclosing → **G**lobal → **B**uilt-in. When a name is referenced, Python searches these scopes in order.

### Q8: When do you use `nonlocal`?
**A:** When you need to **modify** a variable in an enclosing (but not global) scope — typically inside a closure.

### Q9: What is a closure?
**A:** A function that captures and remembers variables from its enclosing scope, even after the outer function has returned.

### Q10: What is the late binding gotcha with lambdas in loops?
**A:** Lambdas created in a loop all reference the **final** value of the loop variable because the variable is looked up at call time, not creation time. Fix: capture with a default argument `lambda i=i: i`.

### Q11: What does `@functools.wraps(func)` do in a decorator?
**A:** It copies `__name__`, `__doc__`, `__annotations__`, and other attributes from the original function to the wrapper, preserving the function's identity.

### Q12: What's the difference between a decorator and a decorator factory?
**A:** A **decorator** takes a function and returns a function. A **decorator factory** takes arguments and returns a decorator: `@factory(args)` → `factory(args)` returns the actual decorator.

### Q13: What order are stacked decorators applied?
**A:** Bottom-up. `@a` over `@b` means `f = a(b(f))`. The innermost decorator (`@b`) is applied first.

### Q14: What are keyword-only parameters?
**A:** Parameters defined after `*` or `*args` must be passed by keyword name only. `def f(*, x):` — `f(x=1)` works, `f(1)` raises TypeError.

### Q15: What are positional-only parameters?
**A:** Parameters defined before `/` must be passed by position only. `def f(x, /):` — `f(1)` works, `f(x=1)` raises TypeError. (Python 3.8+)

### Q16: How does `*` unpack in a function call?
**A:** `f(*[1, 2, 3])` is equivalent to `f(1, 2, 3)`. It unpacks an iterable into positional arguments.

### Q17: How does `**` unpack in a function call?
**A:** `f(**{"a": 1, "b": 2})` is equivalent to `f(a=1, b=2)`. It unpacks a dict into keyword arguments.

### Q18: What makes a function "first-class" in Python?
**A:** Functions can be assigned to variables, passed as arguments to other functions, returned from functions, and stored in data structures — they're objects like any other value.

### Q19: What's the difference between `lambda` and `def`?
**A:** `lambda` creates anonymous functions limited to a single expression (no statements, no docstring). `def` creates named functions with full bodies. Both create function objects.

### Q20: What are the two rules of recursion?
**A:** 1) Must have a **base case** that stops the recursion. 2) Must make **progress** toward the base case with each recursive call.

---

## Common Failure Points

### Failure 1: Forgetting return
**BUG:**
```python
def double(x):
    x * 2
result = double(5)  # None!
```
**FIX:**
```python
def double(x):
    return x * 2
```
**RULE:** If you expect a value back, you need `return`.

### Failure 2: Mutable default argument
**BUG:**
```python
def add_item(item, lst=[]):
    lst.append(item)
    return lst
add_item(1)  # [1]
add_item(2)  # [1, 2] — not [2]!
```
**FIX:**
```python
def add_item(item, lst=None):
    if lst is None:
        lst = []
    lst.append(item)
    return lst
```
**RULE:** Never use mutable objects as default parameter values.

### Failure 3: Shadowing built-in names
**BUG:**
```python
list = [1, 2, 3]
# Later...
new_list = list(range(5))  # TypeError: 'list' object is not callable
```
**FIX:** Use descriptive names like `items`, `numbers`, `values` instead of built-in names.
**RULE:** Never name variables `list`, `dict`, `set`, `str`, `int`, `type`, `id`, `input`, `print`, `sum`, `min`, `max`, `len`, `map`, `filter`.

### Failure 4: Modifying caller's mutable object
**BUG:**
```python
def process(data):
    data.sort()  # Mutates the original list!
    return data
```
**FIX:**
```python
def process(data):
    return sorted(data)  # Returns new list
```
**RULE:** If you don't intend to modify the caller's data, work on a copy.

### Failure 5: Scope confusion — local vs global
**BUG:**
```python
count = 0
def increment():
    count += 1  # UnboundLocalError!
```
**FIX:**
```python
count = 0
def increment():
    global count
    count += 1
```
**RULE:** Assignment inside a function creates a local variable. Use `global` or `nonlocal` to modify outer scope (but prefer passing as parameter and returning).

### Failure 6: Missing `@functools.wraps` in decorator
**BUG:**
```python
def deco(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

@deco
def my_func():
    """My docstring."""
    pass

print(my_func.__name__)  # "wrapper" — wrong!
```
**FIX:** Add `@functools.wraps(func)` above `wrapper`.
**RULE:** Always use `@functools.wraps(func)` in every decorator you write.

### Failure 7: Recursive function missing base case
**BUG:**
```python
def sum_to(n):
    return n + sum_to(n - 1)  # Never stops → RecursionError
```
**FIX:**
```python
def sum_to(n):
    if n <= 0:
        return 0
    return n + sum_to(n - 1)
```
**RULE:** Every recursive function must have an explicit base case that returns without recursing.

### Failure 8: Confusing `print` with `return`
**BUG:**
```python
def calculate(x):
    print(x * 2)

result = calculate(5)  # Prints 10, but result is None
```
**FIX:**
```python
def calculate(x):
    return x * 2
```
**RULE:** `print` displays to console but doesn't give a value back. `return` gives a value back but doesn't display it. Use `return` for function outputs.

### Failure 9: Wrong argument order with mixed args
**BUG:**
```python
def f(a, b=10, c):  # SyntaxError!
    pass
```
**FIX:**
```python
def f(a, c, b=10):  # Non-default before default
    pass
```
**RULE:** Non-default parameters must come before default parameters (except keyword-only after `*`).

### Failure 10: Assignment in helper doesn't affect caller
**BUG:**
```python
def update(val):
    val = val + 1  # Rebinds local name, doesn't change caller's variable

x = 5
update(x)
print(x)  # Still 5
```
**FIX:** Return the new value and reassign: `x = update(x)`.
**RULE:** Python passes references, but reassignment only changes the local name binding. Integers, strings, and tuples are immutable — you can't modify them in-place.
