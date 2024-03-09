+++
title = "Union Size"
path = "union-size"
date = 2024-03-09
[taxonomies]
tags = ["snippets"]
+++

_**Java**_
```java
// assume that there is an int[] union = new int[n];
int unionSize = 0;
for (int i = 0; i < sizes[j]; i++) {
   int e = cache[j][i];
   if (union[e - 1] != i) {
       // e is not in the union
       union[e - 1] = i;
       unionSize++;
   }
}
```

