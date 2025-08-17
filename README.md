# Algorithm Design and Analysis

## Problem: Find maximum of $n$ numbers using split in half

---

### (a) Efficient Recursive Definition

Let $A[1..n]$ be the array.
Define the recursive function $\text{Max}(A, l, r)$:

$$
\text{Max}(A, l, r) =
\begin{cases} 
A[l], & \text{if } l = r \quad \text{(only one element remains, so it is the maximum)} \\
\max\big(\text{Max}(A, l, m), \ \text{Max}(A, m+1, r)\big), & \text{if } l < r
\end{cases}
$$

where $m = \lfloor \tfrac{l+r}{2} \rfloor$.

This follows the divide-and-conquer principle.

---

### (b) Worked Example with 12 Elements

Take $A = [15, 3, 27, 9, 42, 18, 33, 7, 21, 39, 11, 24]$.

1. Split into halves:
   Left = \[15, 3, 27, 9, 42, 18], Right = \[33, 7, 21, 39, 11, 24]

2. Left half = \[15, 3, 27, 9, 42, 18]

   * Split: \[15, 3, 27] and \[9, 42, 18]
   * For \[15, 3, 27]:

     * Split → \[15] vs \[3, 27]
     * \[3, 27] → split → \[3] vs \[27] → Max = 27
     * Max(15, 27) = 27
   * For \[9, 42, 18]:

     * Split → \[9] vs \[42, 18]
     * \[42, 18] → split → \[42] vs \[18] → Max = 42
     * Max(9, 42) = 42
   * Max(27, 42) = 42

3. Right half = \[33, 7, 21, 39, 11, 24]

   * Split: \[33, 7, 21] and \[39, 11, 24]
   * For \[33, 7, 21]:

     * Split → \[33] vs \[7, 21]
     * \[7, 21] → split → \[7] vs \[21] → Max = 21
     * Max(33, 21) = 33
   * For \[39, 11, 24]:

     * Split → \[39] vs \[11, 24]
     * \[11, 24] → split → \[11] vs \[24] → Max = 24
     * Max(39, 24) = 39
   * Max(33, 39) = 39

4. Final step: Max(42, 39) = **42**

---

### (c) Proof of Correctness

We prove by **induction on the number of elements $n$:**

* **Base Case ($n = 1$)**:
  If there is only one element left, the algorithm returns it. Clearly, this is the maximum.

* **Inductive Hypothesis:**
  Assume the algorithm correctly finds the maximum of any array with fewer than $n$ elements.

* **Inductive Step:**
  For input size $n$, the algorithm splits into two subarrays of sizes $\leq n/2$.
  By the hypothesis, it correctly computes the maximum of each half.
  Taking the maximum of these two values yields the overall maximum.

Thus, by induction, the algorithm is correct for all $n \geq 1$.

---

### (d) Time Complexity via Recurrence

Let $T(n)$ be the time for $n$ elements.

$$
T(n) = 2T\!\left(\tfrac{n}{2}\right) + \Theta(1), \quad T(1) = \Theta(1)
$$

Using the **Master Theorem**:

* $a = 2, b = 2, f(n) = \Theta(1)$
* $n^{\log_b a} = n^{\log_2 2} = n$
* Case 1 applies since $f(n) = O(n^{1-\epsilon})$.

$$
T(n) = \Theta(n)
$$

---

### (e) Final Algorithm (Pseudocode)

```text
FIND-MAX(A, l, r):
    if l == r:
        return A[l]              // only one element remains, hence maximum
    m = ⌊(l + r) / 2⌋
    leftMax = FIND-MAX(A, l, m)
    rightMax = FIND-MAX(A, m+1, r)
    return max(leftMax, rightMax)
```

**Data Structures Used:**

* Input array $A[1..n]$.
* Recursion stack (depth = $\log n$).

---

### (f) Final Time and Space Complexity

* **Time Complexity:** $\Theta(n)$.
* **Space Complexity:** $O(\log n)$ (recursion depth).

---

✅ Final Answer: The recursive algorithm finds the maximum element of $n$ numbers in **linear time** with **logarithmic space recursion overhead**.
The partition can also not be at the mid - point
---

## Problem: Find maximum of $n$ numbers using a **tournament method**

---

### (a) Efficient Recursive Definition

Let `tournament_max(list)` be the recursive function.

$$
\text{tournament\_max}(L) =
\begin{cases}
L[0], & \text{if } |L| = 1 \quad \text{(a single element is its own maximum)} \\
\max(L[0], L[1]), & \text{if } |L| = 2 \quad \text{(directly compare the two elements)} \\
\max(\text{tournament\_max}(L_{\text{left}}),\ \text{tournament\_max}(L_{\text{right}})), & \text{otherwise}
\end{cases}
$$

---

### (b) Worked Example with 12 Elements

Take

$$
L = [15, 3, 27, 9, 42, 18, 33, 7, 21, 39, 11, 24]
$$

#### Round 1 (pairwise comparisons via base case $|L| = 2$):

* Compare (15, 3) → 15
* Compare (27, 9) → 27
* Compare (42, 18) → 42
* Compare (33, 7) → 33
* Compare (21, 39) → 39
* Compare (11, 24) → 24

Winners: \[15, 27, 42, 33, 39, 24]

#### Round 2:

* Compare (15, 27) → 27
* Compare (42, 33) → 42
* Compare (39, 24) → 39

Winners: \[27, 42, 39]

#### Round 3:

* Compare (27, 42) → 42
* Compare (42, 39) → 42

Final Winner: **42**

---

### (c) Proof of Correctness

**Base cases:**

* If the list has **1 element**, that element is trivially the maximum.
* If the list has **2 elements**, the direct comparison returns the larger, which is the correct maximum.

**Induction step:**
Assume the algorithm works for lists smaller than $n$. For a list of size $n$, we divide into two halves, solve recursively, and then compare the two winners. By hypothesis, the recursive calls are correct, and the final comparison ensures correctness globally.

Thus, the algorithm is correct for all $n \geq 1$.

---

### (d) Time Complexity via Recurrence

For $n = 1$, $T(1) = \Theta(1)$.
For $n = 2$, $T(2) = \Theta(1)$ (one comparison).

For $n > 2$:

$$
T(n) = T(\lfloor n/2 \rfloor) + T(\lceil n/2 \rceil) + 1
$$

If $n$ is a power of 2,

$$
T(n) = 2T(n/2) + 1
$$

This solves to

$$
T(n) = n - 1
$$

So, **Θ(n)** comparisons.

---

### (e) Final Algorithm (Pseudocode)

```text
FUNCTION tournament_max(list):
    // Base case 1: single element
    IF list.length == 1:
        RETURN list[0]

    // Base case 2: two elements
    IF list.length == 2:
        IF list[0] > list[1]:
            RETURN list[0]
        ELSE:
            RETURN list[1]

    // Recursive case: divide into halves
    mid_point = list.length / 2
    left_half = list from index 0 to mid_point - 1
    right_half = list from index mid_point to list.length - 1

    max_left = tournament_max(left_half)
    max_right = tournament_max(right_half)

    // Compare winners
    IF max_left > max_right:
        RETURN max_left
    ELSE:
        RETURN max_right
```

---

### (f) Final Time and Space Complexity

* **Time Complexity:** $n - 1$ comparisons → $\Theta(n)$
* **Space Complexity:** $O(\log n)$ (recursion depth)

---

✅ Final Answer: With the **two-element base case**, the tournament method naturally mimics a real knockout tournament. It runs in **Θ(n) time with n−1 comparisons** and uses **O(log n) space**.

---
## Problem: Find maximum of $n$ numbers using a tournament, where partition is not necessarily at the midpoint

---

### (a) Efficient Recursive Definition

Let `tournament_max(list)` be defined as:

$$
\text{tournament\_max}(L) =
\begin{cases}
L[0], & \text{if } |L| = 1 \\
\max(L[0], L[1]), & \text{if } |L| = 2 \\
\max(\text{tournament\_max}(L_{\text{left}}), \ \text{tournament\_max}(L_{\text{right}})), & \text{if } |L| > 2
\end{cases}
$$

Here, $L_{\text{left}}$ and $L_{\text{right}}$ are **any two nonempty partitions of $L$**, not necessarily of equal size.

---

### (b) Worked Example with 12 Elements (unbalanced split)

Let

$$
L = [15, 3, 27, 9, 42, 18, 33, 7, 21, 39, 11, 24]
$$

Suppose we split into **first 4 elements** and **remaining 8 elements** (not equal).

#### Step 1:

Left = \[15, 3, 27, 9], Right = \[42, 18, 33, 7, 21, 39, 11, 24]

#### Left part \[15, 3, 27, 9]:

* Split → \[15, 3] vs \[27, 9]
* \[15, 3] → compare → 15
* \[27, 9] → compare → 27
* Compare winners (15 vs 27) → 27

#### Right part \[42, 18, 33, 7, 21, 39, 11, 24]:

* Split → \[42, 18, 33, 7] vs \[21, 39, 11, 24]
* \[42, 18, 33, 7] → split → \[42, 18] (winner 42), \[33, 7] (winner 33) → max = 42
* \[21, 39, 11, 24] → split → \[21, 39] (winner 39), \[11, 24] (winner 24) → max = 39
* Compare (42 vs 39) → 42

#### Final Comparison:

Max(27, 42) = **42**

---

### (c) Proof of Correctness

* **Base cases:**

  * For a list of size 1, returning that element is correct.
  * For a list of size 2, the direct comparison yields the maximum.

* **Induction step:**
  Suppose the algorithm works correctly for lists smaller than $n$. For size $n$, partition into two nonempty sublists $L_{\text{left}}$ and $L_{\text{right}}$. By the inductive hypothesis, the recursive calls correctly return the maximum of each sublist. The larger of these two is the global maximum.

Therefore, correctness holds regardless of whether the partition is balanced or unbalanced.

---

### (d) Time Complexity via Recurrence

Let $T(n)$ = time to find maximum of size $n$.

$$
T(n) = T(k) + T(n-k) + 1
$$

where $1 \leq k \leq n-1$ depends on the chosen split.

* **Best case:** $k = 1$ → recurrence is $T(n) = T(1) + T(n-1) + 1$. Solves to $T(n) = \Theta(n)$.
* **Balanced case:** $k \approx n/2$ → recurrence $T(n) = 2T(n/2) + 1$. Solves to $T(n) = \Theta(n)$.
* **Worst case:** Even if partition is extremely skewed at every step, the recurrence telescopes to $T(n) = (n-1)$.

Thus in **all cases**:

$$
T(n) = n - 1 = \Theta(n)
$$

---

### (e) Final Algorithm (Pseudocode)

```text
FUNCTION tournament_max(list):
    // Base case 1: single element
    IF list.length == 1:
        RETURN list[0]

    // Base case 2: two elements
    IF list.length == 2:
        IF list[0] > list[1]:
            RETURN list[0]
        ELSE:
            RETURN list[1]

    // Partition: not necessarily at midpoint
    // Choose any index p (1 <= p < list.length)
    partition_index = choose_partition_index(list.length)

    left_half = list from index 0 to partition_index - 1
    right_half = list from index partition_index to list.length - 1

    // Recursive calls
    max_left = tournament_max(left_half)
    max_right = tournament_max(right_half)

    // Compare winners
    IF max_left > max_right:
        RETURN max_left
    ELSE:
        RETURN max_right
```

---

### (f) Final Time and Space Complexity

* **Time Complexity:** Always $n-1 = \Theta(n)$ comparisons, regardless of partition balance.
* **Space Complexity:**

  * Balanced partition → recursion depth $O(\log n)$
  * Skewed partition (worst case) → recursion depth $O(n)$.

---

✅ Final Answer: In the **unbalanced tournament method**, the number of comparisons remains optimal at $n-1$, but the recursion depth can degrade to $O(n)$ if partitions are chosen poorly.

---





