# Disjoint Set (Union-Find) 🧩

```cpp
class DisjointSet {
    vector<int> rank, parent, size;
public:
    DisjointSet(int n) {
        rank.resize(n + 1, 0);
        parent.resize(n + 1);
        size.resize(n + 1);
        for (int i = 0; i <= n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }

    int findUPar(int node) {
        if (node == parent[node])
            return node;
        return parent[node] = findUPar(parent[node]);
    }

    void unionByRank(int u, int v) {
        int ulp_u = findUPar(u);
        int ulp_v = findUPar(v);
        if (ulp_u == ulp_v) return;
        if (rank[ulp_u] < rank[ulp_v]) {
            parent[ulp_u] = ulp_v;
        }
        else if (rank[ulp_v] < rank[ulp_u]) {
            parent[ulp_v] = ulp_u;
        }
        else {
            parent[ulp_v] = ulp_u;
            rank[ulp_u]++;
        }
    }

    void unionBySize(int u, int v) {
        int ulp_u = findUPar(u);    //ulp = ultimate parent
        int ulp_v = findUPar(v);
        if (ulp_u == ulp_v) return;
        if (size[ulp_u] < size[ulp_v]) {
            parent[ulp_u] = ulp_v;
            size[ulp_v] += size[ulp_u];
        }
        else {
            parent[ulp_v] = ulp_u;
            size[ulp_u] += size[ulp_v];
        }
    }
};
```

## Overview 📚
- This class implements a Disjoint Set Union (DSU) / Union-Find data structure.
- It supports:
  - `findUPar`: find with *path compression* (find the ultimate parent).
  - `unionByRank`: union operation that uses tree *rank* (approximate height).
  - `unionBySize`: union operation that uses subtree *size* to attach smaller to larger.
- Typical uses: connected components, Kruskal's MST, dynamic connectivity problems.

## Members & Initialization 🔧
- `vector<int> rank, parent, size;`
  - `rank`: heuristic to keep trees shallow (tracks depth approximation).
  - `parent`: parent pointer for each node; a node is root if `parent[node] == node`.
  - `size`: size of the connected component (number of nodes in the set).
- Constructor `DisjointSet(int n)`
  - Resizes arrays to `n + 1` (1-based and 0 included).
  - Initializes each node as its own parent and sets component `size` to 1.
  - `rank` initialized to 0 for all.

## Functions ✨

### findUPar(int node) 🔎
- Purpose: Returns the *ultimate parent* (root) of `node`.
- Uses *path compression*: after finding root, sets `parent[node]` directly to root.
- Complexity: amortized nearly O(1) (inverse-Ackermann).

### unionByRank(int u, int v) 📏
- Purpose: Merge sets containing `u` and `v` using rank heuristic.
- Steps:
  1. Find roots `ulp_u` and `ulp_v`.
  2. If they are same, do nothing.
  3. Attach the root with smaller `rank` to the root with larger `rank`.
  4. If ranks are equal, attach `ulp_v` to `ulp_u` and increment `rank[ulp_u]`.
- Effect: Keeps tree depth small.

### unionBySize(int u, int v) ⚖️
- Purpose: Merge sets containing `u` and `v` using size heuristic.
- Steps:
  1. Find roots `ulp_u` and `ulp_v`.
  2. If same root, do nothing.
  3. Attach the smaller-size root under the larger-size root.
  4. Update the `size` of the new root accordingly.
- Effect: Keeps the bigger tree as the parent to reduce height.

## Usage Tips ✅
- Choose one union strategy and use it consistently (`unionByRank` or `unionBySize`). Mixing is possible but unnecessary.
- Always use `findUPar` before unions to get current roots.
- For 0-based indexing, adjust constructor and loops accordingly.
- Path compression + either heuristic yields nearly constant time per operation.

## Complexity 📈
- Almost amortized O(α(n)) per operation, where α is the inverse Ackermann function (practically constant).

## Notes & Gotchas ⚠️
- Constructor uses `0..n` inclusive: be mindful of intended indexing (supports node 0).
- `rank` is not the exact height after path compression — it’s a heuristic useful only for comparisons.
- `size` must be updated correctly during unions (this implementation does so).
