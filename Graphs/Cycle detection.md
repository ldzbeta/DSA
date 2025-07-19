## ✅ 1. Undirected Graph

### a) DFS Recursive (Undirected)
```cpp
bool dfs(int node, int parent, vector<vector<int>>& graph, vector<bool>& visited) {
    visited[node] = true;
    for (int neighbor : graph[node]) {
        if (!visited[neighbor]) {
            if (dfs(neighbor, node, graph, visited)) return true;
        } else if (neighbor != parent) {
            return true; // cycle detected
        }
    }
    return false;
}

bool hasCycleUndirectedDFS(int n, vector<vector<int>>& graph) {
    vector<bool> visited(n, false);
    for (int i = 0; i < n; ++i) {
        if (!visited[i] && dfs(i, -1, graph, visited)) return true;
    }
    return false;
}
```

---

### b) DFS Iterative (Undirected)
```cpp
bool hasCycleUndirectedDFSIter(int n, vector<vector<int>>& graph) {
    vector<bool> visited(n, false);
    for (int i = 0; i < n; ++i) {
        if (visited[i]) continue;
        stack<pair<int, int>> stk;
        stk.push({i, -1});

        while (!stk.empty()) {
            auto [node, parent] = stk.top();
            stk.pop();
            if (visited[node]) continue;
            visited[node] = true;

            for (int neighbor : graph[node]) {
                if (!visited[neighbor]) {
                    stk.push({neighbor, node});
                } else if (neighbor != parent) {
                    return true; // cycle detected
                }
            }
        }
    }
    return false;
}
```

---

### c) BFS (Undirected)
```cpp
bool hasCycleUndirectedBFS(int n, vector<vector<int>>& graph) {
    vector<bool> visited(n, false);
    for (int i = 0; i < n; ++i) {
        if (visited[i]) continue;

        queue<pair<int, int>> q;
        q.push({i, -1});
        visited[i] = true;

        while (!q.empty()) {
            auto [node, parent] = q.front();
            q.pop();

            for (int neighbor : graph[node]) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    q.push({neighbor, node});
                } else if (neighbor != parent) {
                    return true;
                }
            }
        }
    }
    return false;
}
```

---

## ✅ 2. Directed Graph

### a) DFS Recursive (Directed)
```cpp
bool dfs(int node, vector<vector<int>>& graph, vector<int>& state) {
    state[node] = 1; // visiting
    for (int neighbor : graph[node]) {
        if (state[neighbor] == 1) return true; // back edge
        if (state[neighbor] == 0 && dfs(neighbor, graph, state)) return true;
    }
    state[node] = 2; // visited
    return false;
}

bool hasCycleDirectedDFS(int n, vector<vector<int>>& graph) {
    vector<int> state(n, 0); // 0 = unvisited, 1 = visiting, 2 = visited
    for (int i = 0; i < n; ++i) {
        if (state[i] == 0 && dfs(i, graph, state)) return true;
    }
    return false;
}
```

---

### b) DFS Iterative (Directed)
```cpp
bool hasCycleDirectedDFSIter(int n, vector<vector<int>>& graph) {
    vector<int> state(n, 0);
    for (int i = 0; i < n; ++i) {
        if (state[i] != 0) continue;
        stack<pair<int, bool>> stk;
        stk.push({i, false});

        while (!stk.empty()) {
            auto [node, backtrack] = stk.top();
            stk.pop();

            if (backtrack) {
                state[node] = 2;
                continue;
            }

            if (state[node] == 1) return true;
            if (state[node] == 2) continue;

            state[node] = 1;
            stk.push({node, true});
            for (int neighbor : graph[node]) {
                if (state[neighbor] != 2)
                    stk.push({neighbor, false});
            }
        }
    }
    return false;
}
```

---

### c) BFS (Kahn’s Algorithm) (Directed)
```cpp
bool hasCycleDirectedBFS(int n, vector<vector<int>>& graph) {
    vector<int> indegree(n, 0);
    for (int i = 0; i < n; ++i) {
        for (int neighbor : graph[i]) {
            indegree[neighbor]++;
        }
    }

    queue<int> q;
    for (int i = 0; i < n; ++i)
        if (indegree[i] == 0)
            q.push(i);

    int count = 0;
    while (!q.empty()) {
        int node = q.front(); q.pop();
        count++;
        for (int neighbor : graph[node]) {
            if (--indegree[neighbor] == 0)
                q.push(neighbor);
        }
    }

    return count != n;
}
```
