### **DP Problem: Relaxation and Optimization**
Dynamic Programming (DP) problems often use **relaxation** to optimize solutions. The idea is to iteratively refine the solution by considering suboptimal results and adjusting them to achieve the desired minimum or maximum.

#### **Key Concept:**
- Start with a candidate result that is not the minimum/maximum.
- Add new constraints or adjustments to refine the solution.
- Derive a formula to transform the suboptimal result into the optimal one.

#### **Example: Shortest Path Problem**
**Problem:** Find the shortest path from node `A` to node `D` in a graph with edges `(A,B,3)`, `(A,C,5)`, `(B,D,2)`, `(C,D,4)`.

1. **Initial Relaxation:**
   - Assume the shortest path is `A → C → D` with cost `5 + 4 = 9`.
   - This is not the minimum.

2. **Adjustment:**
   - Consider the alternative path `A → B → D` with cost `3 + 2 = 5`.
   - Compare and update the minimum cost to `5`.

3. **Formula:**
   - For each node, the shortest path can be defined as:
    `
     dist[v] = min(dist[v], dist[u] + weight(u, v))
     `
   - This relaxation step ensures the minimum path is found.

