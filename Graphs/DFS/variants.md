###1.using stack - adj list
```cpp
void DFSIterative(int start) {
        vector<bool> visited(V, false);
        stack<int> stack;

        stack.push(start);

        while (!stack.empty()) {
            int v = stack.top();
            stack.pop();

            if (!visited[v]) {
                visited[v] = true;
                cout << v << " ";

                // Push all adjacent vertices that haven't been visited
                // Note: We push in reverse order to match recursive DFS order
                for (auto it = adj[v].rbegin(); it != adj[v].rend(); ++it) {
                    if (!visited[*it]) {
                        stack.push(*it);
                    }
                }
            }
        }
        cout << endl;
    }
```
### 2.recursive - adj list
```cpp
void DFSUtil(int v, vector<bool>& visited) {
        visited[v] = true;
        cout << v << " ";

        for (int neighbor : adj[v]) {
            if (!visited[neighbor]) {
                DFSUtil(neighbor, visited);
            }
        }
    }
```
---
- Directions: up, down, left, right (and optionally 4 diagonals)
```cpp
const vector<pair<int, int>> directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
```

### 3. Recursive DFS for 2D matrix
```cpp
void dfsRecursive(vector<vector<int>>& matrix, vector<vector<bool>>& visited, int i, int j) {
    int rows = matrix.size();
    int cols = matrix[0].size();
    
    // Mark current cell as visited
    visited[i][j] = true;
    cout << "Visiting: (" << i << ", " << j << ") = " << matrix[i][j] << endl;
    
    // Explore all 4 neighbors
    for (auto& dir : directions) {
        int ni = i + dir.first;
        int nj = j + dir.second;
        
        // Check if neighbor is within bounds, not visited, and meets condition
        if (ni >= 0 && ni < rows && nj >= 0 && nj < cols && 
            !visited[ni][nj] && matrix[ni][nj] == 1) {
            dfsRecursive(matrix, visited, ni, nj);
        }
    }
}
```
### 4. Iterative DFS for 2D matrix using stack
```cpp
void dfsIterative(vector<vector<int>>& matrix, int start_i, int start_j) {
    int rows = matrix.size();
    int cols = matrix[0].size();
    vector<vector<bool>> visited(rows, vector<bool>(cols, false));
    stack<pair<int, int>> stk;
    
    stk.push({start_i, start_j});
    visited[start_i][start_j] = true;
    
    cout << "Iterative DFS starting from (" << start_i << ", " << start_j << "):\n";
    
    while (!stk.empty()) {
        auto current = stk.top();
        stk.pop();
        int i = current.first, j = current.second;
        
        cout << "Visiting: (" << i << ", " << j << ") = " << matrix[i][j] << endl;
        
        // Explore all 4 neighbors
        for (auto& dir : directions) {
            int ni = i + dir.first;
            int nj = j + dir.second;
            
            if (ni >= 0 && ni < rows && nj >= 0 && nj < cols && 
                !visited[ni][nj] && matrix[ni][nj] == 1) {
                visited[ni][nj] = true;
                stk.push({ni, nj});
            }
        }
    }
}
```
