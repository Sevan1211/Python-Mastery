# DSA — Learning Path

**MANDATORY: Complete modules in this exact order. Do not skip ahead.**

Each module must be fully completed and mastered (90%+ confidence, 3 clean solves, 1 no-look recap) before moving to the next. Earlier patterns are prerequisites for later ones.

**Prerequisite**: Complete ALL Fundamentals modules first (or at minimum through module 19 — Binary Search Fundamentals).

---

## Module Order (Easiest → Hardest)

### Phase 1: Linear Patterns
*Single-pass and hash-based patterns — the most common interview category.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 1 | **Array Traversal** | `array_traversal/` | Fundamentals: Lists, Loops, Functions | The simplest pattern — iterate through arrays. Covers linear scan, finding min/max, counting, accumulating. Must be automatic before anything else. |
| 2 | **HashMap / HashSet** | `hashmap_set/` | Array Traversal, Fundamentals: Dicts, Sets | The #1 tool for optimizing brute force. Two-sum pattern, frequency maps, seen-sets. O(n) instead of O(n²). |
| 3 | **Two Pointers** | `two_pointers/` | Array Traversal, HashMap/Set | Left/right convergence on sorted arrays, fast/slow on sequences. Pair-sum, remove duplicates, container problems. |
| 4 | **Sliding Window** | `sliding_window/` | Two Pointers, HashMap/Set | Variable and fixed window patterns. Substring/subarray optimization. Builds on pointer movement from two pointers. |

### Phase 2: Stack & Search Patterns
*Patterns that require understanding LIFO ordering and divide-and-conquer.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 5 | **Stack** | `stack/` | Sliding Window, Fundamentals: Stacks | Monotonic stack, parentheses matching, next greater element. Requires stack fluency from fundamentals. |
| 6 | **Binary Search** | `binary_search/` | Stack, Fundamentals: Binary Search Fundamentals, Sorting | Search space reduction. Sorted arrays, rotated arrays, search-on-answer. The most commonly tested O(log n) pattern. |
| 7 | **Prefix Sum** | `prefix_sum/` | Binary Search, Array Traversal | Precompute cumulative sums for O(1) range queries. Subarray sum equals K, range sum queries. |

### Phase 3: Linked Structures
*Pointer manipulation — a different mental model from array indexing.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 8 | **Linked List** | `linked_list/` | Prefix Sum, Fundamentals: OOP | Node/pointer traversal, reversal, merge, cycle detection basics. Requires OOP for node classes. |
| 9 | **Fast & Slow Pointers** | `fast_slow_pointers/` | Linked List, Two Pointers | Floyd's cycle detection, finding midpoint, detecting loops. Combines linked list + two pointer intuition. |

### Phase 4: Tree & Heap Patterns
*Hierarchical data — recursion becomes essential.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 10 | **Trees DFS** | `trees_dfs/` | Fast & Slow Pointers, Fundamentals: Recursion, OOP | Preorder, inorder, postorder traversal. Max depth, path sum, validate BST. Recursion is the primary tool. |
| 11 | **Trees BFS** | `trees_bfs/` | Trees DFS, Fundamentals: Queues/Deques | Level-order traversal, right-side view, zigzag. Uses queue-based iteration. |
| 12 | **Heap / Top K** | `heap_top_k/` | Trees BFS, Fundamentals: Heaps | Top-K elements, merge-K sorted, median from stream. Requires heap fluency from fundamentals. |

### Phase 5: Graph Patterns
*Multiple nodes, multiple edges — the most complex traversal patterns.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 13 | **Graph DFS/BFS** | `graph_dfs_bfs/` | Heap/Top K, Trees DFS, Trees BFS | Connected components, shortest path, cycle detection in graphs. Extends tree traversal to general graphs. |
| 14 | **Backtracking** | `backtracking/` | Graph DFS/BFS, Recursion | Permutations, combinations, subsets, N-queens. Systematic exploration with pruning. |
| 15 | **Greedy** | `greedy/` | Backtracking, Sorting | Interval scheduling, jump game, activity selection. Requires sorting + optimal substructure intuition. |
| 16 | **Union Find** | `union_find/` | Graph DFS/BFS | Disjoint sets, connected components, redundant connections. Alternative to DFS for connectivity. |
| 17 | **Trie** | `trie/` | Union Find, HashMap/Set, OOP | Prefix tree for string problems. Word search, autocomplete, spell check. |

### Phase 6: Dynamic Programming
*The hardest interview category — requires all previous patterns as foundation.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 18 | **DP 1D** | `dp_1d/` | Trie, Recursion, All prior patterns | Fibonacci, climbing stairs, house robber, coin change. Overlapping subproblems + optimal substructure. |
| 19 | **DP 2D** | `dp_2d/` | DP 1D, 2D Arrays | Grid paths, longest common subsequence, edit distance. Extends 1D DP to matrices. |
| 20 | **Advanced Graphs** | `advanced_graphs/` | DP 2D, Graph DFS/BFS, Heap | Dijkstra, Bellman-Ford, topological sort, MST. The capstone of DSA prep. |

---

## Gate Rules

Before advancing from module N to module N+1:

- [ ] All 5 syntax drills solved
- [ ] All 5 easy pattern drills solved
- [ ] At least 2 of 3 medium interview questions solved
- [ ] Optimized-from-scratch challenge attempted
- [ ] Whiteboard explanation recorded (verbal or written)
- [ ] All `test_cases.py` passing
- [ ] Can explain brute force → optimized path from memory
- [ ] Can state time/space complexity without looking
- [ ] Agent has updated `mastery_score.md` and `progress.md`

## Current Position

**Prerequisite**: Complete Fundamentals first.
**Active Module**: Array Traversal — content created (lesson, drills, practice, cheatsheet, review). Practice drills not yet completed.
