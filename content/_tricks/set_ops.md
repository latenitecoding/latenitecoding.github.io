+++
title = "Set Ops"
path = "set-ops"
in_search_index = true
date = 2024-03-06
[taxonomies]
tags = ["set-theory"]
+++

### _Fast Union Size_
See [Set Union](@/_theory/set_theory.md#set-union).

There are several ways to compute the union of sets that range in complexity from \\(O(n)\\) to \\(O(n^3)\\), where \\(n = |S_1| + |S_2| + ...\\). The notation \\(|X|\\) represents the _cardinality_ or _size_ of the set X (i.e., the number of elements that it contains). This particular approach to computing the union of sets is \\(O(n)\\). Its purpose is to increase the performance of computing the union of sets across multiple **successive computations** of a set union by reducing mem allocations. It can generally be applied in cases where only the size of the union is needed rather than the individual elements in the union.

Given sets A, B, and C, to compute \\(|A \cup B \cup C|\\), we would normally compute \\(A \cup B\\) and then compute \\((A \cup B) \cup C\\). In this case, two set unions would be computed. In general, given \\(m\\) sets, \\(m - 1\\) set unions have to be computed. In the previous example, the intermediate set \\(A \cup B\\) has to be computed first. Space has to be allocated to hold this set. In order to improve performance, this intermediate should be reused across successive union operations to prevent wasted mem allocations. However, if many different set unions have to be computed that will not be combined--or it is at least unclear if they will be combined--then separate intermediate sets have to be allocated for, which results in reduced performance. In such cases, this algorithm can help by employing an array that can be reused across successive unions.

Requirements:
- Computing many unions across sets A, B, C, ..., where each set is a subset of some known universal set U whose size is \\(n\\).

```java
int[] union = new int[n];
int[][] sets = { ... };
for (int i = 1; i < sets.length; i++) {
    int size = 0;
    for (int j = 0; j < sets[i - 1].length; j++) {
        int x = sets[i - 1][j]; // assume that values of x start at 1
        if (union[x - 1] == i) continue; // already in the union
        union[x - 1] = i; // not already in the union
        size++;
    }
    for (int k = 0; k < sets[i].length; k++) {
        int x = sets[i][k];
        if (union[x - 1] == i) continue;
        union[x - 1] = i;
        size++;
    }
    System.out.printf("Set %d union Set %d has size %d\n", i - 1, i, size);
}
```
_See [Snippets](@/_snippets/fast_union_size.md)._

