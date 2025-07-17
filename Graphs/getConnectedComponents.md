## Using DFS
- Replace vector with unordered set for fast lookup(search) and duplication not allowed - use here
```cpp
void dfs(int node,unordered_map<int,vector<int>> &graph ,vector<bool> &visit,vector<int> &component){
        visit[node]=true;
        component.push_back(node);
        for(int neighbour:graph[node]){
            if(visit[neighbour]==false)
                dfs(neighbour,graph,visit,component);
        }
    }
    vector<vector<int>> getComponents(int V, vector<vector<int>>& edges) {
        // create adjacency list
        unordered_map<int,vector<int>> graph;
        for(auto edge:edges){
            graph[edge[0]].push_back(edge[1]);
            graph[edge[1]].push_back(edge[0]);
        }
        vector<bool> visit(V,false);
        vector<vector<int>> components;
        for(int i=0;i<graph.size();i++){
            if(visit[i]==false){
                vector<int> component;
                dfs(i,graph,visit,component);
                components.push_back(component);
            }
        }
        for(int i=0;i<V;i++){
            if(visit[i]==false)
                components.push_back({i});
        }
        return components;
    }
```
## Using Union Find
