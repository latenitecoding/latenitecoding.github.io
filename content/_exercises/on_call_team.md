+++
title = "On-Call Team"
path = "on-call-team"
in_search_index = true
date = 2024-03-04
[taxonomies]
tags = ["icpc", "regionals-2023"]
+++
This problem was introduced as part of the 2023 ICPC Regionals Contest held on Feb. 24th, 2024 for the SCUSA region. It is now archived on Kattis.

- [Problem Link](https://open.kattis.com/problems/oncallteam)
- [Test Cases and Solution](http://serjudging.vanb.org/)

{% spoiler(title="Reveal Solution") %}
The number of services indicated in the problem is limited to a maximum size of 20 services. The size of the powerset of these services is therefore limited to \\(2^{20} \approx 10^6\\), which is solvable.

1. Assume that the _target robustness level_ is the maximum number of services \\(m\\). Then, compute each possible selection of services by looking at each number in range 1..\\(2^m\\) (exclusive). Each of these numbers represents some selection of bits, where the jth bit indicates whether the jth service has been selected. Let's call this number the _mask_.

   - Given some jth service, you can determine whether that service has been selected by computing the bitwise AND of the mask and \\(2^j\\). If the result is greater than 0, then the jth service has been selected. See [Bitwise AND](@/_tricks/bitwise_ops.md#bitwise-and).

2. For each selected service, iterate over the engineers that are assignable to that service. Compute the total number of engineers that can be assigned to at least one of the selected services by computing the size of the union across engineers that can be assigned to the selected services. This is the _number of assignable engineers_. See [Fast Union Size](@/_tricks/set_ops.md#fast-union-size).

   - You can improve the performance of this step by assigning each engineer to each service that they can support when the engineers are first processed. Thus, the engineers that are assignable to each service will already be known when this step is reached. This improves the performance of computing the union by limiting our scope to only those engineers that are known to be within the union. The following snippet can be used to assign engineers to their respective services. Note that an array is used instead of an `ArrayList` or `HashSet`. See [Generics](@/_languages/java.md#generics).

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

3. Let the number of selected services in the mask be \\(k\\). If the number of assignable engineers is less than \\(k\\), then the target robustness level must also be less than \\(k\\). It can be at most \\(k - 1\\). Store the minimum robustness level between the current target robustness level and \\(k - 1\\).

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

   _**Go**_
   ```go
   if maxK < k - 1 {
       maxK = k - 1
   }
   ```

   - The number of selected services can be computed using the mask bitcount, which is the number of 1-bits in the number.

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

   _**Go**_
   ```go
   k := bits.OnesCount(mask)
   ```

See
- [Hall's Theorem](@/_theorems/halls_theorem.md)

Optimizations

- Before considering a mask, compute its bitcount. If its bitcount is larger than the current target robustness level, then ignore the mask. It represents a selection of services larger than the robustness level that can be obtained by the engineers.

- When computing the size of the union of engineers that can be assigned across the current selection of services, stop once you have enough engineers. As you compute the union, the number of engineers in the union can only grow larger. Once the number of engineers in the union is at least the number of selected services, Hall's theorem has been satisfied. You can move on to the next mask.

{% end %}
