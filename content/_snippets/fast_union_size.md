+++
title = "Fast Union Size"
path = "fast-union-size"
date = 2024-03-07
[taxonomies]
tags = ["snippets"]
+++

_**Java**_
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
