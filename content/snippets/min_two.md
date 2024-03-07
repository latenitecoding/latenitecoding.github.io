+++
title = "Min Two"
path = "min-two"
date = 2024-03-07
[taxonomies]
tags = ["snippets"]
+++

_**C**_
```c
maxK = (maxK < k - 1) ? maxK : k - 1;
```

_**Go**_
```go
if maxK < k - 1 {
   maxK = k - 1
}
```

_**Java**_
```java
maxK = Math.min(maxK, k - 1);
```

_**Python**_
```python
maxK = max(maxK, k - 1)
```

_**Rust**_
```rust
maxK = cmp::max(maxK, k - 1);
```
