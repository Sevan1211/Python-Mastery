# Fundamentals — Learning Path

**MANDATORY: Complete modules in this exact order. Do not skip ahead.**

Each module must be fully completed and mastered (90%+ confidence, 3 clean solves, 1 no-look recap) before moving to the next. Earlier modules are prerequisites for later ones.

---

## Module Order (Easiest → Hardest)

### Phase 1: Core Building Blocks
*Learn what data IS in Python before you do anything with it.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 1 | **Primitive Data Types** | `primitive_data_types/` | None | Everything starts here — int, float, str, bool, None. You can't do anything without knowing what types exist. |
| 2 | **Strings** | `strings/` | Primitive Data Types | Strings are the most common data type you'll encounter. Indexing, slicing, and methods here lay the groundwork for lists. |
| 3 | **Lists** | `lists/` | Strings | Lists reuse indexing/slicing from strings but add mutability. This is the #1 data structure in Python and interviews. |
| 4 | **Tuples** | `tuples/` | Lists | Tuples are immutable lists. Quick module — understand when to use tuples vs lists and tuple unpacking. |

### Phase 2: Core Data Structures
*Learn the key-value and uniqueness structures that power 80% of real code.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 5 | **Dictionaries** | `dicts_sets/` | Lists, Tuples | Dicts are the second most important structure. Key-value mapping is everywhere — APIs, configs, frequency counting. |
| 6 | **Sets** | `sets/` | Dictionaries | Sets are dict-like (hash-based) but store unique values. Critical for deduplication and O(1) lookup patterns. |

### Phase 3: Control Flow & Functions
*Learn how to make decisions and organize reusable code.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 7 | **Loops & Conditionals** | `loops_conditionals/` | Lists, Dicts, Sets | Now that you know the data structures, learn to iterate and branch over them. for/while/if/elif/else, break/continue, nested loops. |
| 8 | **Functions** | `functions/` | Loops & Conditionals | Functions organize your loops and logic into reusable blocks. Covers def, args, kwargs, return, scope, closures. |

### Phase 4: Pythonic Patterns
*Write Python the way Python is meant to be written.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 9 | **List Comprehensions** | `list_comprehensions/` | Functions, Loops | The Pythonic way to transform data. Replaces verbose loops with clean one-liners. Covers filtering, nested comprehensions, dict/set comprehensions. |
| 10 | **Sorting** | `sorting/` | List Comprehensions, Functions | sorted(), .sort(), key functions, lambda, reverse. Sorting is used in nearly every interview problem. |
| 11 | **Common Built-ins** | `builtins/` | Sorting, Functions | enumerate, zip, map, filter, any, all, min, max, sum, range, len, reversed. These make your code Pythonic and fast. |

### Phase 5: Interview Data Structures
*The data structures that show up in DSA but need to be fluent BEFORE you start DSA.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 12 | **Stacks** | `stacks/` | Lists, Functions | Stacks use lists as the backing structure. LIFO pattern — append/pop. Foundation for parentheses matching, DFS, undo systems. |
| 13 | **Queues / Deques** | `queues_deques/` | Stacks | FIFO pattern using collections.deque. Foundation for BFS, job queues, sliding window. |
| 14 | **Heaps** | `heaps/` | Queues, Sorting | Priority queue via heapq. Used in top-K, merge-K, Dijkstra. Must understand before DSA heap problems. |
| 15 | **Counter / defaultdict** | `counter_defaultdict/` | Dicts, Functions | Specialized dict subclasses from collections. Counter for frequency counting, defaultdict for cleaner dict patterns. Massive interview utility. |

### Phase 6: Advanced Fundamentals
*Patterns that require everything above and bridge into DSA.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 16 | **Classes / OOP** | `oop/` | Functions, Dicts | Classes bundle data + behavior. Covers __init__, self, methods, inheritance, dunder methods. Needed for linked lists, trees, graphs. |
| 17 | **Recursion** | `recursion/` | Functions, OOP, Stacks | Recursion requires solid function understanding and mental stack model. Base case, recursive case, call stack, memoization intro. |
| 18 | **2D Arrays** | `two_d_arrays/` | Lists, Loops, Functions | Nested lists representing grids/matrices. Traversal patterns (row-major, column-major, diagonal). Foundation for graph/DP problems. |
| 19 | **Binary Search Fundamentals** | `binary_search_fundamentals/` | Sorting, Functions, Loops | The first real algorithm pattern. Requires sorted data intuition. Left/right pointers, boundary conditions, search space reduction. |

---

## Gate Rules

Before advancing from module N to module N+1:

- [ ] All `practice.py` drills solved without hints
- [ ] All `test_cases.py` passing
- [ ] Can explain every concept from memory
- [ ] Can identify edge cases for the topic
- [ ] Completed `drills.ipynb` interactively
- [ ] Reviewed `review.md` prompts and answered correctly
- [ ] Agent has updated `mastery_score.md` and `progress.md`

## Current Position

**Active Module**: 1 — Primitive Data Types
**Status**: In Progress
