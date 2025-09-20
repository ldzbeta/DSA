# Converting a Binary Tree to an Undirected Graph

When solving problems like *"All Nodes at Distance K in Binary Tree"*, it is often useful to **treat the binary tree as an undirected graph**.  
In this graph:
- Each node is a vertex.
- Edges connect a node with its children **and** its parent.

---

## Why Convert?
- Binary trees allow traversal **downwards** (to left and right children) easily.  
- To find distances in *all* directions (including **upwards** toward ancestors), we need parent links.  
- Representing the tree as a graph allows simple **BFS/DFS** from any node.

---

## Steps to Convert

### 1. Build Parent Map
Traverse the tree and record each node’s parent:
```cpp
unordered_map<TreeNode*, TreeNode*> parent;

void buildParents(TreeNode* node, TreeNode* par, 
                  unordered_map<TreeNode*, TreeNode*>& parent) {
    if (!node) return;
    parent[node] = par;
    buildParents(node->left, node, parent);
    buildParents(node->right, node, parent);
}
````

### 2. Think of Graph Connections

For every node:

* It connects to its `left` child (if exists).
* It connects to its `right` child (if exists).
* It connects to its `parent` (from the map).

This gives an **undirected adjacency list**.

---

## Example: \[3,5,1,6,2,0,8,null,null,7,4]

Tree:

```
        3
       / \
      5   1
     / \ / \
    6  2 0  8
      / \
     7   4
```

Graph edges:

```
3 -- 5, 3 -- 1
5 -- 6, 5 -- 2, 5 -- 3
1 -- 0, 1 -- 8, 1 -- 3
2 -- 7, 2 -- 4, 2 -- 5
```

---

## 3. BFS from Target

Once parent links are known, do a BFS starting from the `target` node:

* Neighbors = `{left, right, parent}`.
* Track visited nodes to avoid cycles.
* Stop when distance = `k`.

```cpp
queue<pair<TreeNode*, int>> q;
unordered_set<TreeNode*> vis;

q.push({target, 0});
vis.insert(target);

while (!q.empty()) {
    auto [node, dist] = q.front(); q.pop();

    if (dist == k) { /* collect results */ }

    for (TreeNode* nei : {node->left, node->right, parent[node]}) {
        if (nei && !vis.count(nei)) {
            vis.insert(nei);
            q.push({nei, dist + 1});
        }
    }
}
```

---

## Key Takeaways

* **Binary tree → Undirected graph** by adding parent edges.
* Use BFS/DFS to explore neighbors in all directions.
* This approach avoids complex stack tracking and works reliably for distance queries.
