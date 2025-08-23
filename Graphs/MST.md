# 🌲 Minimum Spanning Tree (MST) Algorithms — C++ Notes

## 🔑 Key Ideas

* **Spanning Tree**: A subset of edges that connects all vertices without cycles.
* **Minimum Spanning Tree (MST)**: A spanning tree with minimum total edge weight.
* Graph must be **connected & undirected**.

---

## 📘 Kruskal’s Algorithm

* **Approach**: Greedy, edge-based.

1. Sort all edges by weight.
2. Initialize each vertex as its own set (Union-Find/DSU).
3. Pick the smallest edge that doesn’t form a cycle.
4. Repeat until you have `n-1` edges.

* **Complexity**:

  * Sorting: `O(E log E)`
  * Union-Find ops: `O(E α(V))` (≈ linear)
  * Total: **`O(E log E)`**

### C++ Implementation

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Edge {
    int u, v, w;
    bool operator<(const Edge &other) const {
        return w < other.w;
    }
};

struct DSU {
    vector<int> parent, rank;
    DSU(int n) : parent(n), rank(n, 0) {
        iota(parent.begin(), parent.end(), 0);
    }
    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }
    bool unite(int x, int y) {
        x = find(x), y = find(y);
        if (x == y) return false;
        if (rank[x] < rank[y]) swap(x, y);
        parent[y] = x;
        if (rank[x] == rank[y]) rank[x]++;
        return true;
    }
};

int main() {
    int n, m; // n = vertices, m = edges
    cin >> n >> m;
    vector<Edge> edges(m);
    for (int i = 0; i < m; i++)
        cin >> edges[i].u >> edges[i].v >> edges[i].w;

    sort(edges.begin(), edges.end());
    DSU dsu(n+1);
    int cost = 0;
    vector<Edge> mst;

    for (auto &e : edges) {
        if (dsu.unite(e.u, e.v)) {
            cost += e.w;
            mst.push_back(e);
        }
    }

    cout << "MST cost: " << cost << "\n";
    for (auto &e : mst)
        cout << e.u << " - " << e.v << " (" << e.w << ")\n";
}
```

---

## 📘 Prim’s Algorithm

* **Approach**: Greedy, vertex-based.

1. Start from any node.
2. Use a **min-heap (priority queue)** to pick the cheapest edge connecting the tree to a new vertex.
3. Add that edge & vertex to the MST.
4. Repeat until all vertices are included.

* **Complexity**:

  * With min-heap + adjacency list: `O(E log V)`
  * Dense graphs: `O(V^2)` (with adjacency matrix)

### C++ Implementation

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Edge {
    int v, w;
};
using P = pair<int,int>; // (weight, vertex)

int main() {
    int n, m; cin >> n >> m;
    vector<vector<Edge>> adj(n+1);
    for (int i=0; i<m; i++) {
        int u,v,w; cin >> u >> v >> w;
        adj[u].push_back({v,w});
        adj[v].push_back({u,w});
    }

    vector<bool> inMST(n+1,false);
    priority_queue<P, vector<P>, greater<P>> pq;
    pq.push({0,1}); // start from vertex 1

    int cost=0;
    while(!pq.empty()) {
        auto [w,u] = pq.top(); pq.pop();
        if (inMST[u]) continue;
        inMST[u] = true;
        cost += w;
        for (auto &e : adj[u]) {
            if (!inMST[e.v])
                pq.push({e.w, e.v});
        }
    }
    cout << "MST cost: " << cost << "\n";
}
```

---

## ⚖️ Kruskal vs Prim

| Feature        | Kruskal          | Prim           |
| -------------- | ---------------- | -------------- |
| Works best     | Sparse graphs    | Dense graphs   |
| Data structure | Union-Find (DSU) | Priority Queue |
| Complexity     | `O(E log E)`     | `O(E log V)`   |
| Style          | Edge-based       | Vertex-based   |

---
