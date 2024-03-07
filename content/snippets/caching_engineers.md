+++
title = "Caching Engineers"
path = "caching-engineers"
date = 2024-03-07
[taxonomies]
tags = ["snippets"]
+++

_**C**_
```c
int sizes[m];
int cache[m][n];
for (int i = 0; i < n; i++) {
    char* eBits = fgets(str, m + 1, stdin);
    for (int j = 0; j < m; j++) {
        if (eBits[j] == '0') continue;
        cache[j][sizes[j]++] = i + 1;
    }
}
```

_**Go**_
```go
var sizes [m]int
var cache [m][n]int
for i := 0; i < n; i++ {
line, _ := rdr.ReadString('\n')
    eBits, _ := strconv.ParseInt(line, 2, 32)
    for j, b := 0, 1; j < m; j, b = j + 1, b << 1 {
        if (eBits & b) == 0 {
            continue
        }
        cache[j][sizes[j] + 1] = i + 1
        sizes[j]++
    }
}
```

_**Java**_
```java
int[] sizes = new int[m];
int[][] cache = new int[m][n];
for (int i = 0; i < n; i++) {
    int eBits = Integer.parseInt(reader.readLine(), 2);
    for (int j = 0, b = 1; j < m; j++, b <<= 1) {
        if ((eBits & b) == 0) continue;
        cache[j][sizes[j]++] = i + 1;
    }
}
```

_**Python**_
```python
cache = [[] for _ in range(m)]
for i in range(n):
    eBits, b = int(input(), 2), 1
    for j in range(m):
        if (eBits & b) == 0: continue
        cache[j].append(i + 1)
        b <<= 1
```

_**Rust**_
```rust
let cache: Vec<Vec<u32>> = vec![];
for _ in 0..m {
    cache.push(vec![]);
}
for i in 0..(n as u32) {
    io::stdin().read_line(&mut buffer)?;
    let eBits = u32::from_str_radix(buffer.trim(), 2)?;
    buffer.clear();
    let mut b = 1u32;
    for j in 0..m {
        if (eBits & b) == 0 {
            continue;
        }
        cache[j].push(i + 1);
    }
}
```
