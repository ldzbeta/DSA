# Shortest Path Algorithms in C++ — Cheat Sheet 📚✨

Graph representation (recommended for sparse graphs)

* Use: `vector<vector<pair<int, long long>>> adj;`

  * `adj[u]` contains `(v, w)`
* Distance type: `long long` — use `const ll INF = (ll)1e18;`
* All examples assume 0-based node indices.

---

## 1) Dijkstra (min-heap / priority\_queue) 🟢

* Use when: All edge weights are non-negative.
* Complexity: O((V + E) log V) with binary heap.
* Notes: Fast and standard. Store `parent` to reconstruct path.

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

* Use when you prefer decrease-key by erasing old pair.
* Complexity: similar, erases cost log V.
* Stores `parent` easily.

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

## 3) Bellman–Ford 🔶

* Use when: Graph may have negative edges; detects negative cycles reachable from source.
* Complexity: O(V \* E).
* Good for single-source with negative weights.

Template:

```cpp
pair<vector<ll>, bool> bellman_ford(int n, int src, const vector<tuple<int,int,ll>> &edges) {
    vector<ll> dist(n, INF);
    dist[src] = 0;
    for (int i = 0; i < n-1; ++i) {
        bool changed = false;
        for (auto [u, v, w] : edges) {
            if (dist[u] != INF && dist[v] > dist[u] + w) {
                dist[v] = dist[u] + w;
                changed = true;
            }
        }
        if (!changed) break;
    }
    bool hasNegCycle = false;
    for (auto [u, v, w] : edges) {
        if (dist[u] != INF && dist[v] > dist[u] + w) {
            hasNegCycle = true;
            break;
        }
    }
    return {dist, hasNegCycle};
}
```

How it works:
Repeat V-1 times: go through every edge and relax it (update distance if smaller). After that, one extra pass checks for negative cycles.

---

## 4) SPFA (queue-based Bellman–Ford variant) ⚠️

* Use when: Practical speed-ups on many graphs; but worst-case very slow.
* Complexity: Usually fast, worst-case exponential.
* Detects negative cycles by counting relaxations.

Template:

```cpp
vector<ll> spfa(int n, int src, const vector<vector<pair<int,ll>>> &adj) {
    vector<ll> dist(n, INF);
    vector<int> inq(n, 0), cnt(n, 0);
    queue<int> q;
    dist[src] = 0; q.push(src); inq[src] = 1; cnt[src] = 1;
    while (!q.empty()) {
        int u = q.front(); q.pop(); inq[u] = 0;
        for (auto [v, w] : adj[u]) {
            if (dist[v] > dist[u] + w) {
                dist[v] = dist[u] + w;
                if (!inq[v]) {
                    q.push(v);
                    inq[v] = 1;
                    if (++cnt[v] > n) return {}; // negative cycle
                }
            }
        }
    }
    return dist;
}
```

How it works:
Start from the source, relax edges for vertices in the queue. If a vertex gets updated, push it into the queue for future processing.

---

## 5) Floyd–Warshall (All-pairs) 🔁

* Use when: Dense graphs or small n (n <= \~400).
* Handles negative weights; negative cycle if dist\[i]\[i] < 0.
* Complexity: O(n^3).

Template:

```cpp
vector<vector<ll>> floyd_warshall(int n, const vector<vector<ll>> &w) {
    const ll INF = (ll)1e18;
    vector<vector<ll>> dist = w;
    for (int k = 0; k < n; ++k)
        for (int i = 0; i < n; ++i)
            if (dist[i][k] != INF)
                for (int j = 0; j < n; ++j)
                    if (dist[k][j] != INF && dist[i][j] > dist[i][k] + dist[k][j])
                        dist[i][j] = dist[i][k] + dist[k][j];
    return dist;
}
```

How it works:
For each intermediate vertex k, update all pairs (i, j) if going through k gives a shorter path.

---

## 6) Johnson’s algorithm 🧭

Johnson’s algorithm is used to compute shortest paths between all pairs of vertices in a weighted directed graph, which may have negative edge weights, but no negative weight cycles.

* O(VE+V2logV)

```cpp
vector<vector<long long>> johnson(int n, vector<Edge> &edges) {
    int dummy = n;
    for (int i = 0; i < n; i++) edges.push_back({dummy, i, 0});

    vector<long long> h;
    if (!bellmanFord(n + 1, dummy, edges, h)) {
        throw runtime_error("Negative weight cycle detected");
    }

    vector<vector<pair<int,long long>>> adj(n);
    for (auto &e : edges) {
        if (e.u != dummy) {
            long long newW = e.w + h[e.u] - h[e.v];
            adj[e.u].push_back({e.v, newW});
        }
    }

    vector<vector<long long>> distMatrix(n, vector<long long>(n, INF));
    for (int u = 0; u < n; u++) {
        auto d = dijkstra(n, u, adj);
        for (int v = 0; v < n; v++) {
            if (d[v] != INF) distMatrix[u][v] = d[v] - h[u] + h[v];
        }
    }
    return distMatrix;
}
```

How it works:
Add a dummy node, run Bellman-Ford to compute potential values, reweight all edges to remove negatives, then run Dijkstra from every vertex.

---

## 7) Multi-source shortest paths 🔀

* Push all sources initially with distance 0 (or connect a virtual source to all sources with 0-weight edges).
* Useful for “distance to nearest source”.

Template:

```cpp
vector<ll> multi_source_dijkstra(int n, const vector<int> &sources, const vector<vector<pair<int,ll>>> &adj) {
    vector<ll> dist(n, INF);
    priority_queue<pair<ll,int>, vector<pair<ll,int>>, greater<pair<ll,int>>> pq;
    for (int s : sources) {
        dist[s] = 0;
        pq.push({0, s});
    }
    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d != dist[u]) continue;
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
Run Dijkstra starting with all sources at distance 0. Propagate updates to neighbors like normal Dijkstra.

---

## 8) DAG Shortest Path (Using Topological Sort)

* O(V + E)

```cpp
vector<long long> shortestPathDAG(int n, int src, const vector<vector<pair<int, long long>>> &adj) {
    stack<int> st;
    vector<bool> visited(n, false);
    for (int i = 0; i < n; i++) {
        if (!visited[i]) topologicalSortUtil(i, visited, st, adj);
    }

    vector<long long> dist(n, INF);
    dist[src] = 0;

    while (!st.empty()) {
        int u = st.top(); st.pop();
        if (dist[u] != INF) {
            for (auto [v, w] : adj[u]) {
                if (dist[v] > dist[u] + w) {
                    dist[v] = dist[u] + w;
                }
            }
        }
    }

    return dist;
}
```

How it works:
Perform topological sort, then process nodes in order, relaxing edges once since DAG has no cycles.

---

## Path reconstruction 🔁

Keep `parent[v] = u` when you relax an edge u→v. To get path from `src` to `t`: backtrack and reverse the sequence.

---

## Practical tips & gotchas ⚙️

* Use `long long` for distances; INF = 1e18. Check for overflow when adding weights.
* For dense graphs or small n, Floyd–Warshall is simple and robust.
* For non-negative weights, prefer Dijkstra + binary heap (priority\_queue).
* For negative edges: Bellman–Ford for single-source. For all-pairs with negatives, use Johnson’s algorithm.
* When using `set` as priority queue: erase prior pair before inserting new one.
* SPFA can be fast in practice but may fail on crafted inputs (detect via `cnt[v] > n`).
