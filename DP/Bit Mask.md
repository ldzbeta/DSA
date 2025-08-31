## ✅ 1. What is a Bitmask?

A **bitmask** is an integer where **each bit represents a state (0 or 1)**.

* Example: `5` in binary is `101`.
* We can use this to represent subsets, states, or flags.
* If `n = 3`, the mask can range from `000` to `111` (0 to 7 in decimal).

**Why use bitmask?**

* Compact representation of subsets.
* Fast operations using bitwise operators (`&`, `|`, `^`, `<<`, `>>`).
* Common in **DP**, **subset problems**, **graph algorithms**, **optimization problems**.

---

## ✅ 2. Basic Bit Operations

* **Set a bit:** `mask | (1 << i)` → sets bit `i` to 1.
* **Unset a bit:** `mask & ~(1 << i)` → clears bit `i`.
* **Check a bit:** `(mask >> i) & 1` → is bit `i` set?
* **Toggle a bit:** `mask ^ (1 << i)` → flips bit `i`.

Example:

```cpp
int mask = 5; // binary: 101
bool isSet = (mask & (1 << 2)); // true (bit 2 is set)
```

---

## ✅ 3. Representing Subsets using Bitmask

For a set of size `n`, each subset can be represented by a mask of `n` bits.

Example: `{a,b,c}`

* `000` → {}
* `001` → {c}
* `010` → {b}
* `101` → {a,c}
* Total subsets = `2^n`.

---

### ✅ Enumerating all subsets:

```cpp
int n = 3;
for (int mask = 0; mask < (1 << n); mask++) {
    cout << "Subset: ";
    for (int i = 0; i < n; i++) {
        if (mask & (1 << i)) cout << i << " ";
    }
    cout << endl;
}
```

---

## ✅ 4. Where is Bitmask used?

* **Subset problems** (e.g., Traveling Salesman Problem).
* **DP on subsets**.
* **Max product with no common bits** (our problem).
* **Graph problems** (state compression).
* **Scheduling & optimization problems**.

---

## ✅ 5. DP + Bitmask: Key Concept

**State compression DP**:

* If you have `n` elements and you want to represent which elements are used or not → use a bitmask.
* Example: **TSP** → `dp[mask][i]` = minimum cost to visit all cities in `mask` and end at city `i`.

---

### **Example DP using bitmask**:

Find the **minimum sum path visiting all elements exactly once**.

#### State:

`dp[mask][i]` → min cost to cover set `mask` ending at node `i`.

#### Transition:

`dp[newMask][j] = min(dp[mask][i] + cost[i][j])` where `newMask = mask | (1 << j)`.

---

## ✅ 6. Apply Bitmask to Our Problem (No Common Bits)

Why bitmask helps here?

* Instead of comparing numbers directly, **convert each number to a mask of its bits**.
* If `(mask1 & mask2) == 0`, they have no common set bits.

---

### Example:

`nums = [2, 3, 4]`
Binary:

* `2 = 010` → mask = 010
* `3 = 011` → mask = 011
* `4 = 100` → mask = 100

Pairs:

* 2 & 3 → `010 & 011 = 010` (not zero → share bit)
* 2 & 4 → `010 & 100 = 000` (valid)
* 3 & 4 → `011 & 100 = 000` (valid)

So, product is between (2,4) and (3,4).

---

## ✅ 7. Common Bitmask Tricks in C++

✔ **Count set bits:** `__builtin_popcount(mask)`
✔ **Lowest set bit:** `mask & (-mask)`
✔ **Iterate over subsets of a mask:**

```cpp
for (int sub = mask; sub; sub = (sub-1) & mask) {
    // sub is a subset of mask
}
```

---

## ✅ 8. Practice Problems

* [LeetCode 318. Maximum Product of Word Lengths](https://leetcode.com/problems/maximum-product-of-word-lengths/) (similar to your problem)
* TSP using Bitmask DP
* Count subsets with certain properties

---

### ✅ Key Takeaways:

* **Bitmask = compact representation of states.**
* Perfect for **subset-based DP**.
* Operations are very fast (`O(1)` for check, set, unset).

---

