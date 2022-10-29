# Prim's Min Span Tree

## Overview

This algorithm can be attributed to Czech mathematician Vojtěch Jarník, who discovered in 1930. It was later rediscovered and published by computer scientist Robert C. Prim in 1957. It is a *Greedy Algorithm* to construct a *minimum spanning tree* from a *weighted graph*. The graph can be *directed* or *undirected*. It can be shown that it produces the correct output.

Prim's algorithm takes a vertex-based approach to reducing a graph to a minimum spanning tree. It's solution is akin to Dijkstra's approach to producing a shortest path. In fact, if you use Dijkstra's algorithm to produce the shortest path from a vertex u to every other vertex in the same graph, then you would have Prim's algorithm.

## Pseudocode

```pseudo
procedure Prims_Minimum_Spanning_Tree(verts, edges, start)
=================================================================
PRE: forall e in edges, e = (u, v, weight), where u, v in verts
PRE: start in verts
PRE: graph (verts, edges) is completely connected
-----------------------------------------------------------------
visited <- boolean[false; |verts|]
visited[start] <- true
min_heap <- []
for e in edges s.t. e = (start, next, weight) do
    add e to min_heap
end for
(min_span_tree, tree_weight) <- ([], 0)
while |min_heap| > 0 do
    (u, v, u_v_weight) <- poll from min_heap
    if visited[v] is true, then
        continue
    end if
    visited[v] <- true
    for e in edges s.t. e = (v, next, v_next_weight) do
        add e to min_heap
    end for
    add e to min_span_tree
    tree_weight <- tree_weight + u_v_weight
end while
(mst, tree_weight)
end procedure
```

## Explanation

Prim's algorithm requires a start vertex to be pre-selected. If no start is given, the vertex with the smallest label can be chosen as a default. Vertices that have been chosen are maintained via a boolean array. If a vertice's index has been marked true, then the vertex is in the minimum spanning tree. Otherwise, the vertex is not in the minimum spanning tree.

In the beginning, only the start vertex is added to the minimum spanning tree. Once a vertex is added to the tree, it's connected edges are added to a min heap, which is used to store edges that could potentially be added to the minumum spanning tree. The min heap (or priority queue) orders edges by their weight, so the edge with the smallest weight is always polled.

While there are edges still left in the mean heap, we poll each edge one-by-one. If the edge connects to another vertex in the minimum spanning tree, then the edge must be skipped. Any new edge that would connect two vertices already in the minimum spanning tree results in a cycle. Minimum spanning trees cannot have cycles.

If an edge is discovered that connects to a vertex not in the minimum spanning tree, then that vertex is added to the tree, its connected edges are added to the min heap, and the selected edge and its weight are added to the tree.

## Efficiency

To construct the minimum spanning tree, Prim's algorithm must look at every vertex and every edge. As such, it's performance is O(|V| + |E|), where V is the vertices and E is the edges. The algorithm does not need any additional memor other than what is required to represent the input.

## Proof