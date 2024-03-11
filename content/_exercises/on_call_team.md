+++
title = "On-Call Team"
path = "on-call-team"
in_search_index = true
date = 2024-03-04
[taxonomies]
tags = ["icpc", "regionals-2023"]
+++
This problem was introduced as part of the 2023 ICPC Regionals Contest held on Feb. 24th, 2024 for the SCUSA region. It is now archived on Kattis.

[Problem Link](https://open.kattis.com/problems/oncallteam)

[Test Cases and Solution](http://serjudging.vanb.org/?cat=47)

{% spoiler(title="Reveal Solution") %}
The number of services indicated in the problem is limited to a maximum size of 20 services. The size of the [power set](@/_theory/set_theory.md#power-set) of these services is therefore limited to \\(2^{20} \approx 10^6\\), which is solvable.

1. Assume that the _target robustness level_ is the maximum number of services \\(m\\). Then, compute each possible selection of services by looking at each number in range 1..\\(2^m\\) (exclusive). Each of these numbers represents some selection of bits, where the jth bit indicates whether the jth service has been selected. Let's call this number the _mask_.

   - Given some jth service, you can determine whether that service has been selected by computing the bitwise AND of the mask and \\(2^j\\). If the result is greater than 0, then the jth service has been selected. See [Bitwise AND](@/_tricks/bitwise_ops.md#bitwise-and).

     ```java
     for (int mask = 1; mask < (1 << m); mask++) {
         if ((mask & (1 << j)) > 0) {
            // the jth service is in the selection
         }
     }
     ```

2. For each selected service, iterate over the engineers that are assignable to that service. Compute the total number of engineers that can be assigned to at least one of the selected services. This is the _number of assignable engineers_.

   - To assist with this step, you can assign each engineer to each service they can support when the engineers are first processed. Thus, the engineers that are assignable to each service will already be known when this step is reached. This also improves the performance of computing the union of assignable engineers by limiting our scope to only those engineers that are known to be within the union. The following snippet can be used to assign engineers to their respective services. Note that an array is used instead of an `ArrayList` or `HashSet`. See [Generics](@/_languages/java.md#generics).

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
     _See [Snippets](@/_snippets/caching_engineers.md)._

   - To compute the number of assignable engineers, you have to compute the size of the union of engineers that can be assigned to any of the services in the selection. Since you'll have to do this for each mask, you'll need to compute the sizes of many different unions. This process can easily lead to wasted performance and memory. To improve the process, you can reuse an `int[]` to mark which engineers are the current union. See [Fast Union Size](@/_tricks/set_ops.md#fast-union-size).

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
        _See [Snippets](@/_snippets/union_size.md)._

3. Let the number of selected services in the mask be \\(k\\). If the number of assignable engineers (union size) is less than \\(k\\), then the target robustness level must also be less than \\(k\\). It can be at most \\(k - 1\\). Store the minimum robustness level between the current target robustness level and \\(k - 1\\).

   ```java
   maxK = Math.min(maxK, k - 1);
   ```
   _See [Snippets](@/_snippets/min_two.md)._

   - The number of selected services can be computed using the mask bitcount, which is the number of 1-bits in the number.

       ```java
       int k = Integer.bitCount(mask);
       ```
       _See [Snippets](@/_snippets/bit_count.md)._

See
- [Hall's Theorem](@/_theory/halls_theorem.md)

Optimizations

- Before considering a mask, compute its bitcount. If its bitcount is larger than the current target robustness level, then ignore the mask. It represents a selection of services larger than the robustness level that can be obtained by the engineers.

- When computing the size of the union of engineers that can be assigned across the current selection of services, stop once you have enough engineers. As you compute the union, the number of engineers in the union can only grow larger. Once the number of engineers in the union is at least the number of selected services, Hall's theorem has been satisfied. You can move on to the next mask.

{% end %}
