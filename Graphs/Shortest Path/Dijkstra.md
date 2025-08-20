# Graph representation (recommended for sparse graphs) 📚

- Use: `vector<vector<pair<int, long long>>> adj;`  
  - `adj[u]` contains `(v, w)`
- Distance type: `long long` — use `const ll INF = (ll)1e18;`
- All examples assume 0-based node indices.

---

## 1) Dijkstra (min-heap / priority_queue) 🟢

- Use when: All edge weights are non-negative.  
- Complexity: O((V + E) log V) with binary heap.  
- Notes: Fast and standard. Store `parent` to reconstruct path.

Template:

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const ll INF = (ll)1e18;

vector<ll> dijkstra(int n, int src, const vector<vector<pair<int,ll>>> &adj) {
    vector<ll> dist(n, INF);
    dist[src] = 0;
    priority_queue<pair<ll,int>, vector<pair<ll,int>>, greater<pair<ll,int>>> pq;
    pq.push({0, src});
    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d != dist[u]) continue; // outdated entry
        for (auto [v, w] : adj[u]) {
            if (dist[v] > d + w) {
                dist[v] = d + w;
                pq.push({dist[v], v});
            }
        }
    }
    return dist;
}
```

How it works:
Take the vertex with the smallest distance, check all its neighbors, calculate the cost from the vertex, and update if smaller. Repeat until all vertices are processed.

---

## 2) Dijkstra using set (ordered set) 🟦

- Use when you prefer decrease-key by erasing old pair.  
- Complexity: similar, erases cost log V.  
- Stores `parent` easily.

Template:

```cpp
vector<ll> dijkstra_set(int n, int src, const vector<vector<pair<int,ll>>> &adj) {
    vector<ll> dist(n, INF);
    vector<int> parent(n, -1);
    dist[src] = 0;
    set<pair<ll,int>> s;
    s.insert({0, src});
    while (!s.empty()) {
        auto [d, u] = *s.begin(); s.erase(s.begin());
        for (auto [v, w] : adj[u]) {
            if (dist[v] > d + w) {
                if (dist[v] != INF) s.erase({dist[v], v});
                dist[v] = d + w;
                parent[v] = u;
                s.insert({dist[v], v});
            }
        }
    }
    return dist;
}
```

How it works:
Same as Dijkstra with priority queue, but uses an ordered set to always pick the vertex with the smallest distance and supports deleting outdated entries.

---

## For storing *all* shortest paths to a node (why it matters) ✨

- Problem: If node `v` can be reached from multiple nodes (`u1`, `u2`) with the **same** shortest distance, a standard Dijkstra that keeps a single `parent[v] = u` will only record the first parent discovered (e.g., `u1`). When the algorithm later finds an equal-length path from `u2`, it does nothing because it only updates on strictly shorter distances.
- Consequence: You lose information about alternative shortest paths — you cannot enumerate or count all shortest paths to `v`.

Solution idea:
- Instead of a single `parent[v]`, store a list of parents: `vector<vector<int>> parents(n);`
- When you find a strictly shorter path to `v`, clear `parents[v]` and push the new parent.
- When you find an equal-length path (dist[v] == d + w), push the additional parent without clearing.
- After Dijkstra finishes, reconstruct all shortest paths using DFS/BFS from target back to source using `parents`.

Mini-template sketch:

```cpp
vector<vector<int>> parents(n);
vector<ll> dist(n, INF);
dist[src] = 0;
pq.push({0, src});
while (!pq.empty()) {
  auto [d, u] = pq.top(); pq.pop();
  if (d != dist[u]) continue;
  for (auto [v,w] : adj[u]) {
    ll nd = d + w;
    if (nd < dist[v]) {
      dist[v] = nd;
      parents[v].clear();
      parents[v].push_back(u);
      pq.push({nd, v});
    } else if (nd == dist[v]) {
      parents[v].push_back(u);
    }
  }
}
```

Notes:
- Use `parents` to reconstruct or count all shortest paths (watch for exponential number of paths when enumerating).
- To count paths modulo M, you can also maintain `ways[v]` and update:
  - On shorter path: `ways[v] = ways[u]`
  - On equal path: `ways[v] += ways[u] (mod M)`
---
