 **when a problem allows binary search**, look for this pattern:

---

### 🔑 Key Principle:

> If the answer lies in a range of values and the **validity of the answer is monotonic**, you can apply binary search.

---

### 🔍 In your case:

* You are asked to **find the minimum time `t`** such that some condition (`valid substrings ≥ k`) becomes true.
* If the condition is **true at time `t`**, it will **stay true** for all future times `t+1, t+2, ..., n-1`.

✅ This is called a **monotonic property**.

---

### ✅ Common signs a problem can use binary search:

| Situation                                                                        | Can you binary search? | Why?                             |
| -------------------------------------------------------------------------------- | ---------------------- | -------------------------------- |
| You’re finding the **minimum** or **maximum** value where a condition is met     | ✅                      | That’s classic binary search use |
| The result lies in a **numeric range** (like 0 to n)                             | ✅                      | You can search the range         |
| The condition is **monotonic** — once true, always true (or false, always false) | ✅                      | Helps divide the search space    |

---

### 📘 In LeetCode 3639:

* You’re searching for the **minimum time `t`** where string becomes active.
* The number of valid substrings only **increases** over time → ✅ **monotonic**
* So instead of checking from `t = 0 to n-1` linearly, we can **binary search** the smallest `t` that makes condition true.

---

### 🧠 General Tip:

When you see:

* “Minimum time”, “maximum size”, “least value”, “smallest index” that **satisfies a condition**,
* And testing a value is **expensive** or **can be reused**,
  → **Think binary search**.

---
