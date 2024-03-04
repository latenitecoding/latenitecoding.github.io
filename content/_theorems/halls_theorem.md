+++
title = "Hall's Theorem"
path = "halls-theorem"
in_search_index = true
date = 2024-03-02
[taxonomies]
tags = ["graph-theory", "perfect-matching"]
+++
Hall's Theorem, also known as Hall's Marriage Theorem, can be used to decide
whether or not a **perfect matching** exists in a **bipartite graph**. This can be 
applied to solving constraint satisfaction problems where some members of a set X 
need to be matched with members of a set Y, but these members can only be matched
1-to-1.

The advantage of Hall's Theorem is that it allows us to use set logic to determine
whether or not a matching exists for some constraint satisfaction problem.
Typically, we would need to use a flow network to compute the maximal matching, or any matching, of a given graph. When applying Hall's Theorem, we don't need to construct a flow network or apply a max flow algorithm. We can instead
match members of the powerset of X to some selection of Y by only looking at the
set of neighbors that each vertex in X has.

If you're going to be working with Hall's Theorem, it would be good to
familiarize yourself with set union, set intersection, and the identities
related to the cardinality of a set. One helpful identity has been included
below.
- \\(|A \cup B| + |A \cap B| = |A| + |B|\\)


# Theorem

_For a bipartite graph G on the parts X and Y, the following conditions are
equivalent._

- (a) _There is a perfect matching of X into Y._
- (b) _For each \\(T \subseteq X \\), the inequality \\(|T| \leq |N_g(T)| \\) holds._

# Definitions

Let G be a simple graph, and let S be a subset of the edges of G. If no two
edges in S form a _path_ (i.e., if no two edges share a vertex), then we can say
that S is a **matching** of G. A matching S of G is a **perfect matching** if
every vertex of G participates in some edge of S.

Let G be a bipartite graph. The vertices of G can be partitioned into the parts
X and Y, s.t. no two vertices of X share an edge in G and no two vertices in Y
share an edge in G. Let S be a matching of G. If every vertex in X participates
in an edge of S, then we can say that S is a **perfect matching** of X into Y.

For a graph G and some subset T of the vertices of G, we can define the function
\\(N_g(T)\\) as the set of vertices of G that share an edge with the vertices of
T. If G is a bipartite with parts X and Y and T is some set of vertices of X,
then \\(N_g(T)\\) is some set of vertices of Y (i.e., it is the set of
candidates that can be matched to the members of T).

\\[N_g(T) = \text{ \\{ } v \in V(G) \text{ | } vw \in E(G) \text{ for some } w \in T \text{ \\} } \\]

# Exercises

- [On-Call Team](@/_exercises/on_call_team.md)
