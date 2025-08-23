## (a) Pseudocode for Optimal Merge Sequence (Greedy)

We are given `n` files with sizes `f[1..n]`.
We want the minimum total cost of merging all files into one file, where merging two files of sizes `a` and `b` costs `a+b`.

The greedy rule:
**Always merge the two smallest files first.**

---

### Pseudocode (CLRS style)

```text
OPTIMAL-MERGE(f[1..n]):
    // Input: array of file sizes
    // Output: minimum total merge cost

    1. create a min-priority queue Q
    2. for i = 1 to n:
            INSERT(Q, f[i])
    3. totalCost ← 0
    4. while SIZE(Q) > 1:
            x ← EXTRACT-MIN(Q)
            y ← EXTRACT-MIN(Q)
            z ← x + y
            totalCost ← totalCost + z
            INSERT(Q, z)
    5. return totalCost
```

* We use a **min-heap** to efficiently extract the two smallest files each time.
* Running time: `O(n log n)`.

---

## (b) Correctness Proof of Greedy Algorithm

We need to show two things (standard greedy proof):

1. **Greedy-choice property**: There exists an optimal solution in which the two smallest files are merged first.
2. **Optimal substructure**: After merging them, the remaining problem is again an optimal merge problem.

---

### Step 1: Greedy-choice property

Let the two smallest files be `f[i]` and `f[j]` with sizes `a` and `b` (assume `a ≤ b ≤ others`).
Suppose there exists an optimal merge tree `T*` that does *not* merge `(a,b)` first.

Instead, `a` is merged with some file `c ≥ b`.
Then the cost contribution of that step is `a + c`.
If we swap the merge so that `a` merges with `b` first, the cost contribution becomes `a+b ≤ a+c`.

The remaining merges can be restructured (since order of merges can be rearranged) without increasing the total cost.
Thus, any optimal solution can be transformed into one that merges the two smallest first.
Therefore, choosing `(a,b)` first is **safe**.

---

### Step 2: Optimal substructure

After merging `a` and `b` into a new file of size `a+b`, we reduce the problem to one with `(n-1)` files.
The total cost = `(a+b)` + optimal cost of solving the reduced problem.
Thus, solving the smaller problem optimally leads to an optimal solution overall.

This shows the problem has **optimal substructure**.

---

### Step 3: Proof by induction

**Base case (n=2):** Only one merge possible, trivially optimal.

**Inductive step:** Assume the greedy algorithm works optimally for `n-1` files.
For `n` files, by greedy-choice property, we can safely merge the two smallest first.
By induction, the recursive solution to the smaller problem is optimal.
Thus, the whole algorithm is optimal. ✅

---

## (c) Final Result

* **Algorithm**: Always merge the two smallest files first using a min-heap.
* **Time complexity**: `O(n log n)` (each of `n-1` merges requires 2 extractions + 1 insertion into heap).
* **Space complexity**: `O(n)` (heap).
* **Correctness**: Proven by greedy-choice property + optimal substructure.

---

# 1. Recursive Definition (Greedy Optimal Merge)

**Recursive idea**:

* Base case: If only one file remains, cost = `0`.
* Recursive case: Pick two smallest files, merge them (cost = sum), and solve recursively on the reduced set.

---

# 2. Recursive Pseudocode — Heap Version

We assume we have a **min-heap** `H`.

```text
OPTIMAL-MERGE-HEAP(H):
    // Input: min-heap of file sizes
    // Output: minimum merge cost

    if SIZE(H) == 1:
        return 0

    x ← HEAP-EXTRACT-MIN(H)   // smallest file
    y ← HEAP-EXTRACT-MIN(H)   // second smallest file
    z ← x + y

    HEAP-INSERT(H, z)

    return z + OPTIMAL-MERGE-HEAP(H)
```

* Each recursive call reduces the heap size by 1.
* Cost accumulates as the recursion unwinds.
* Complexity: still `O(n log n)` because each recursion performs `O(log n)` heap operations.

---

# 3. Recursive Pseudocode — Balanced BST Version

We assume a balanced BST `T`.

```text
OPTIMAL-MERGE-BST(T):
    // Input: balanced BST of file sizes
    // Output: minimum merge cost

    if SIZE(T) == 1:
        return 0

    x ← TREE-MIN(T)              // smallest
    y ← TREE-SUCCESSOR(x)        // second smallest

    TREE-DELETE(T, x)
    TREE-DELETE(T, y)

    z ← x + y
    TREE-INSERT(T, z)

    return z + OPTIMAL-MERGE-BST(T)
```

* Same structure, except instead of a heap we use BST operations.
* Complexity: `O(n log n)` for the same reason.

---

# 4. Why Recursive Version Works

* The **greedy-choice property** ensures picking two smallest first is always safe.
* The **optimal substructure** ensures the reduced problem is itself an optimal merge problem.
* Recursion naturally encodes this structure.

---

✅ So:

* Heap-based recursion is efficient and practical.
* BST-based recursion is conceptually clean but less efficient in practice.

---
