Why this matters for multiple shortest paths:

If node v can be reached from multiple nodes (u1, u2) with the same shortest distance, Dijkstra should count both paths.
But in your code:

The first time v gets updated, parent[v] = u1.

When another equal-length path from u2 comes, you do nothing (you only update parent when a shorter path is found).
