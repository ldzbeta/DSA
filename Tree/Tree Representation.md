### 1. Array Representation: Count Nodes at a Given Level in a Rooted Tree

Given a rooted tree and a level `k`, write a function to count the number of nodes that exist at level `k` from the root.

#### Implementation in C++:
```cpp
#include <stdio.h>
#include <stdlib.h>

int level[100], child_count[100], adj[100][100];

void bfs(int root, int k, int n) {
    int queue[n], front = 0, rear = 0;
    int count = 0;

    queue[rear++] = root;
    level[root] = 0;

    while (front < rear) {
        int node = queue[front++];
        if (level[node] == k) count++;

        for (int i = 0; i < child_count[node]; i++) {
            int child = adj[node][i];
            level[child] = level[node] + 1;
            queue[rear++] = child;
        }
    }

    printf("%d", count);
}

int main() {
    int n;
    scanf("%d", &n);
    int arr[n], root;

    for (int i = 0; i < n; i++) {
        scanf("%d", &arr[i]);
        if (arr[i] == -1) {
            root = i;
        } else {
            adj[arr[i]][child_count[arr[i]]++] = i;
            // For arr[i] (parent), assign child at child_count position as i.
        }
    }

    int k;
    scanf("%d", &k);
    bfs(root, k, n);
}
```

---

### 2. Parenthesis Representation

- **Objective**: Parse and extract elements from a string representation of a tree (e.g., `(A(B)(C))`).
- **Approach**: Traverse the string to identify nodes and their relationships based on parentheses.

---

### 3. Node-Based Representation (Binary Tree)

- **Note**: This approach is specific to binary trees and may not generalize to `m`-ary trees.
- **Implementation**: Use a `Node` class or struct to represent each node, with pointers to left and right children.

---

### Key Points:
- **Array Representation**: Efficient for static trees with known sizes.
- **Parenthesis Representation**: Useful for parsing tree structures from strings.
- **Node-Based Representation**: Flexible for dynamic trees but limited to binary trees unless extended.
