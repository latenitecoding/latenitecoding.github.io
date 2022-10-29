# Prim's Min Span Tree

## Overview

This algorithm can be attributed to Czech mathematician Vojtěch Jarník, who discovered in 1930. It was later rediscovered and published by American computer scientist Robert C. Prim in 1957. It is a *Greedy Algorithm* to construct a *minimum spanning tree* from a *weighted graph*. The graph can be *directed* or *undirected*. This algorithm can be shown to produce the correct solution for any given input.

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

Prim's algorithm requires a start vertex to be pre-selected. If no start is given, the vertex with the smallest label can be chosen as a default. Vertices that have been chosen are maintained via a boolean array. If a vertex's index has been marked true, then that vertex is in the minimum spanning tree. Otherwise, the vertex is not in the tree.

In the beginning, only the start vertex is added to the minimum spanning tree. Once a vertex is added to the tree, it's connected edges are added to a min heap, which is used to store edges that could potentially be added to the minumum spanning tree later. The min heap (or priority queue) orders edges by their weight, so the edge with the smallest weight is always polled.

While there are edges still left in the min heap, we poll each edge one-by-one. If the edge connects to another vertex in the minimum spanning tree, then the edge must be skipped. Any new edge that would connect two vertices already in the minimum spanning tree results in a cycle. Minimum spanning trees cannot have cycles.

If an edge is discovered that connects to a vertex not in the minimum spanning tree, then that vertex is added to the tree, its connected edges are added to the min heap, and the selected edge and its weight are added to the tree.

## Efficiency

To construct the minimum spanning tree, Prim's algorithm must look at every vertex and every edge, which is O(|V| + |E|). Each edge is added to a min heap, which is O(|E|log|E|). The efficiency of Prim's is therefore O(|E|log|E|), since that is the dominant term.

The memory efficiency of Prim's requires an additional boolean array to mark vertices as being visited and a min heap to order edges. The memory required for these two arrays is O(|V|) and O(|E|), respectively. Therefore, the memory efficiency is O(|V| + |E|).

Something important to note a minimum spanning tree is that it must include every vertex in a graph but not every edge. A graph can contain significantly more edges than vertices. In fact, a completely connected graph, where every vertex has an edge to every other vertex, has up to |V|^2 edges. In such cases, it would be beneficial to constrain the algorithm to its vertices rather than its edges, which we can do.

## Improvements

We can improve the efficiency of Prim's algorithm to O(|E| + |V|log(|V|)) performance and O(|V|) memory by using a Fibonacci heap.

## Proof