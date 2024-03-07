+++
title = "Bit Count"
path = "bit-count"
date = 2024-03-07
[taxonomies]
tags = ["snippets"]
+++

_**C**_
```c
int bits = 0;
for (int i = mask; i > 0; i >>= 1) {
    if (i & 1) bits++;
}
```

_**Go**_
```go
k := bits.OnesCount(mask)
```

_**Java**_
```java
int k = Integer.bitCount(mask);
```

_**Python**_
```python
k = int.bit_count(mask)
```

_**Rust**_
```rust
let k = mask.count_ones();
```
