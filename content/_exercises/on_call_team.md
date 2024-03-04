+++
title = "On-Call Team"
path = "on-call-team"
in_search_index = true
date = 2024-03-04
[taxonomies]
tags = ["icpc", "regionals-2023"]
+++
This problem was introduced as part of the 2023 ICPC Regionals Contest held on
Feb. 24th, 2024 for the SCUSA region. It is now archived on Kattis.

- [Problem Link](https://open.kattis.com/problems/oncallteam)
- [Test Cases and Solution](http://serjudging.vanb.org/)

{% spoiler(title="Reveal Solution") %}
The number of services indicated in the problem is limited to a maximum size of
20 services. The size of the powerset of these services is therefore limited to
\\(2^{20} \approx 10^6\\), which is solvable.

1. Calculate the corresponding _target bitstring_ of each selection of services
   associated with \\(k = 1\\).
   - Every bitstring used in this problem should be implemented as a 32-bit integer
     type.

   - First compute a _base bitstring_ that includes all but
     one of the number of needed services. Then compute the target bitstring by
     applying the logical OR of the base bitstring with each service
     individually. If the logical OR results in the same bitstring as the base
     bitstring, then that service was already selected and can be skipped.

2. Compute the number of engineers that could be assigned to any service in the
   selection. This is the number of _assignable engineers_.
   - Each engineer has a set of services that they can be assigned to indicated
     by a bitstring. This is the _engineer bitstring_.

   - Compute the bitwise AND of the target bitstring and each engineer
     bitstring. If the result is greater than \\(0\\), then the engineer is assignable to some service within the current selection of services.

3. If the number of assignable engineers is less than the number of selected
   services (\\(k\\)), then the solution robustness level is equal to \\(k - 1\\). If the
   resulting robustness level would be \\(0\\), then \\(-1\\) must be printed
   instead.

4. If all bitstrings associated with the current robustness level are
   satisfied, then repeat this algorithm with \\(k = k + 1\\) up to the maximum
   robustness level of \\(m\\).
   
See
- [Hall's Theorem](@/_theorems/halls_theorem.md)

Optimizations

1. The above algorithm can be further optimized by also computing the
   intersection of engineers that can be assigned to the same selection of services.
   - The intersection of engineers that can be assigned to a single service is
     the same as the set of engineers that are assignable to that service.

   - The intersection of engineers that can be assigned to two
     services can be computed by applying the bitwise AND between the target
     bitstring and engineer bitstring for each engineer that is assignable to
     either service. If the result is the same as the target bitstring, then the
     engineer is in the intersection.

   - The intersection of engineers that can be assigned to three or more
     services can be computed by taking the intersection of all but one service
     (computed earlier) and intersecting that with the remaining service using
     the process outlined above.

   - By storing the intersection related to selections of services T, we can
     compute the number of assignable engineers (\\(|N_g(T)|\\)) using the
     following equation. Let T be the set of services A, B, and C.

     \\[
        |N_g(A, B, C)| = |N_g(A, B)| + |N_g(C)| - |N_g(A, B) \cap N_g(C)|
     \\]

{% end %}
