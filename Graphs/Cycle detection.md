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
Just using visit will fail for:
```markdown
1 -> 2
|    |
V    V
3 -> 4
```
To be cycle ,the node should be visited in same direction of path , back trach and visit.
- instead of storing new data structure for visited for current path , we are using states.

### For, Topological Sort 

So, in DFS:
If you're visiting node A, you must first visit all its neighbors (courses that depend on A).
Only after finishing all of them, you add A to the result.
This is called postorder traversal 
- add extra result vector for topological order
- in dfs before return , push back node to result
- reverse at the end, before return to convert correct postorder traversal

#### 🔴Difference between dfs in Directed and undirected
1)Difference in Cycle Detection:

| Graph Type  | Cycle Detection Logic                                                                 |
|-------------|---------------------------------------------------------------------------------------|
| Undirected  | If neighbor is visited and not parent → cycle                                         |
| Directed    | Use recursion stack or 3-color method (visited + onPath)                              |

2)Difference in graph implementation

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
- Replace `count` by `vector<int> topo` and `count++` by `topo.push_back(node)` to change the for Topological sort
