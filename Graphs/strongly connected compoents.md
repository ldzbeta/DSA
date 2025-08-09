### **Kosaraju's Algorithm for Strongly Connected Components (SCCs)**

#### **Algorithm Overview**
Kosaraju's algorithm is used to find all strongly connected components (SCCs) in a directed graph. It consists of three main steps:
1. **First DFS Pass**: Perform a depth-first search (DFS) on the original graph and push nodes onto a stack in the order of their finishing times.
2. **Graph Reversal**: Reverse all edges of the original graph to obtain the transpose graph.
3. **Second DFS Pass**: Process nodes in the order defined by the stack (from the first step) on the reversed graph to identify SCCs.

#### **Code Implementation**
```cpp
void dfs1(vector<int> adj[], int u, vector<bool>& vis, stack<int>& st) {
    vis[u] = true;
    for (int v : adj[u])
        if (!vis[v]) dfs1(adj, v, vis, st);
    st.push(u);
}

void dfs2(vector<int> revAdj[], int u, vector<bool>& vis, vector<int>& comp) {
    vis[u] = true;
    comp.push_back(u);
    for (int v : revAdj[u])
        if (!vis[v]) dfs2(revAdj, v, vis, comp);
}

vector<vector<int>> kosaraju(int V, vector<int> adj[]) {
    // Step 1: Perform DFS and fill stack
    vector<bool> vis(V, false);
    stack<int> st;
    for (int u = 0; u < V; u++)
        if (!vis[u]) dfs1(adj, u, vis, st);

    // Step 2: Reverse the graph
    vector<int> revAdj[V];
    for (int u = 0; u < V; u++)
        for (int v : adj[u])
            revAdj[v].push_back(u);

    // Step 3: Process nodes in stack order
    fill(vis.begin(), vis.end(), false);
    vector<vector<int>> sccs;
    while (!st.empty()) {
        int u = st.top(); st.pop();
        if (!vis[u]) {
            vector<int> comp;
            dfs2(revAdj, u, vis, comp);
            sccs.push_back(comp);
        }
    }
    return sccs;
}
```

#### **Key Points**
- **Time Complexity**: \(O(V + E)\) for both DFS passes and graph reversal.
- **Space Complexity**: \(O(V + E)\) for storing the adjacency lists and auxiliary data structures.
- **Applications**: Used in graph analysis, such as detecting cycles, solving 2-SAT problems, and more.

#### **Notes**
- Ensure the graph is represented as an adjacency list (`vector<int> adj[]`).
- The algorithm assumes the graph is directed; undirected graphs are trivially strongly connected.
