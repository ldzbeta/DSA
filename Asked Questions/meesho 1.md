This problem was recently asked in Meesho OA in August 2025

🧠 Problem Statement:
Given a tree with N nodes and an array A[1...N] where A[i] is the value at node i, find the number of valid paths (i, j) such that only one node in the path from i to j contains a prime number.

🔒 Constraints:
1 <= N <= 100000
1 <= A[i] <= 1000000

🧩 Understanding the Problem:
Think of each node as either:

⚪ White if A[i] is not prime

⚫ Black if A[i] is prime

Now, reframe the problem:
👉 Find the number of paths in the tree where exactly one black (prime-valued) node is present.

⚡ Step 1: Precomputation – Sieve of Eratosthenes
To identify prime values in A[1...N] efficiently:

// Sieve for D = 10^6
vector isPrime(D+1, true);
isPrime[0] = isPrime[1] = false;
for (int i = 2; i * i <= D; ++i)
if (isPrime[i])
for (int j = i * i; j <= D; j += i)
isPrime[j] = false;
⏱ Time Complexity: O(D log log D) for D = 1e6

🔍 Brute Force Idea (Intuition Builder)
Start with the idea:

For each black node u, we count how many valid (i, j) paths exist such that u is the only black node in the path.

Subtree decomposition (Think from the perspective of one black node at a time):

Count how many white-only nodes are connected in each of its child branches.

👨‍💻 Brute Force Code (not optimal for large N):

⏱ Time Complexity: O(N^2)

💡 Optimization: Tree DP + Upward DP
✅ This is where Tree DP shines!

🍃 Define:
Let dp[i] = number of continuous white nodes in the subtree rooted at i, including i (if i is white).

if (color[i] == white):
dp[i] = 1 + dp[child_1] + dp[child_2] + ...
else:
dp[i] = 0

⬆️ Upward DP:
We also define up_dp[i] = number of white nodes above node i including node i(not in its subtree).

Formula for upward DP:

up_dp[child] = 1 + up_dp[parent] + (dp[parent] - 1 - dp[child]) {If child and parent both are white}
📌 Note: Only add 1 if parent is white and the further formula depends on color of parent

🔁 Final Steps
For each black node:

Collect its child dp values {dp[c1], dp[c2], ..., dp[ck]}

Also include up_dp[i] (nodes above)

Then, count total valid paths using combinatorics: for every pair from the array {X1, X2, ..., Xk}, answer += X1 * X2

📊 Total Time: O(N)

🧮 Final Formula
Let parts = {X1, X2, ..., Xk} (white node counts in all branches)

Then total valid paths for this black node =
Σ (X_i * X_j) for all i < j
= (sum^2 - sum of squares) / 2

📘 Further Optimization

Sum of other children dp => dp[parent] - 1 - dp[child]
...instead of recomputing siblings’ subtree white counts repeatedly

🧠 Key Takeaways:
Classic coloring + path problem on trees

Prime classification → problem reframing

Use Tree DP + Upward DP instead of brute force

Keep formulas tight to avoid redundant computation
