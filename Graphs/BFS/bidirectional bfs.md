# 📑 Pseudocode: Bidirectional BFS

- **Function**: bidirectional_bfs(source, target, graph)  
- **Purpose**: Find the shortest path (in unweighted graphs) between source and target using bidirectional BFS.

``` 
function bidirectional_bfs(source, target, graph):
-    if source == target:
-        return 0

-    frontA = {source}        # BFS frontier from source
-    frontB = {target}        # BFS frontier from target
-    visitedA = {source}
-    visitedB = {target}
-    steps = 0

-    while frontA is not empty and frontB is not empty:
-        # Always expand the smaller frontier (optimization)
-        if size(frontA) > size(frontB):
-            swap(frontA, frontB)
-            swap(visitedA, visitedB)

-        nextFront = {}

-        for node in frontA:
-            for neighbor in graph.neighbors(node):
-                if neighbor in visitedB:
-                    return steps + 1   # meeting point found
-                if neighbor not in visitedA:
-                    visitedA.add(neighbor)
-                    nextFront.add(neighbor)

-        frontA = nextFront
-        steps = steps + 1

-    return -1   # no path
```

## Notes for contests
- **Works for**: unweighted graphs (shortest path).
- **Memory**: \(O(V)\)
- **Time**: about \(O(b^{d/2})\) instead of \(O(b^{d})\).
- **Implementation tips**:
  - Use hash sets (`unordered_set` in C++ / `set` in Python) for fast lookup.
  - Swap frontiers to always expand the smaller side (reduces branching).
- **Adaptations**:
  - Strings (Word Ladder).
  - Grids (shortest path in 2D/3D).
  - Puzzles (sliding tiles, Rubik’s cube).
