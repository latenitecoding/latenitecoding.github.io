+++
title = "Cramming for Finals"
path = "cramming-for-finals"
in_search_index = true
date = 2024-03-07
[taxonomies]
tags = ["icpc", "regionals-2023"]
+++
This problem was introduced as part of the 2023 ICPC Regionals Contest held on Feb. 24th, 2024 for the SCUSA region. It is now archived on Kattis.

[Problem Link](https://open.kattis.com/problems/crammingforfinals)

[Test Cases and Solution](http://serjudging.vanb.org/?cat=47)

{% spoiler(title="Reveal Solution") %}
The grid size for this problem is unmanageably large. However, the number of students in much smaller in comparison. Each student is identified by the grid position they've chosen as their table and a radius around that table from which they can be heard. Even though this means that each student's coordinates define a 2D circle, that circle overlays a grid and can therefore be represented as a collection of intervals. These intervals represent the rows of tables that can hear that student. Given that there are \\(n\\) students each with a radius of \\(d\\), there are approximately \\(2dn\\) intervals, or about \\(5 \cdot 10^6\\), which is solvable.

1. Let there be a function \\(f(r, c) \rightarrow I \\) which computes a unique table number for a student sitting at row \\(r\\) and column \\(c\\). Iterate over the provided coordinates for the students and collect their unique table numbers into an array. Let this be the _student tables_.

    ```java
    long[] studentTables = new long[n];
    for (int i = 0; i < n; i++) {
        int[] rc = nextTuple();
        studentTables[i] = getTableNumber(rc[0], rc[1], rowLength);
    }
    ```

   - To compute a student's table number, treat the 2D grid as a single 1D array. When a 2D grid is laid out as a 1D array, the rows of the 2D grid exist as consecutive intervals in the 1D array, each with a length equal to the number of columns in the 2D grid. Thus, computing the table number is about using the row number to jump to a specific interval in the 1D array and using the column number as an offset within that interval.

       ```java
       public static long getTableNumber(int r, int c, int len) {
         return (r - 1) * l + (c - 1);
       }
       ```

      Don't forget to use the 64-bit `long` type for table numbers. The largest table number is \\(10^{18}\\) which cannot be stored in a 32-bit `int` type.

2. Sort the student tables from least to greatest. Let the first table be the _least table_ and start with the first row as the _current row_. If the least table cannot be heard from the current row, then Ashley can sit on the first row without hearing other students. The solution is \\(0\\).

    ```java
    Arrays.sort(studentTables);
    ```

   - Given a table number that corresponds to row \\(r\\), that table can be heard from any row \\(y\\) if \\(r-d \leq y \leq r+d\\).

   - Since table numbers are laid out in a 1D array, we can use integer division to determine which row from the 2D grid a table belongs to.

       ```java
       public static int getRow(long t, int len) {
         return (t - 1) / len;
       }
       ```

3. Let there by an array called _cache_ that represents the number of students that can be heard from each table on the current row. Iterate over the student tables starting from the least table until you reach a table that cannot be heard from the current row.

4. Let there be a function \\(len(t, r) \rightarrow I\\) that computes the number of tables on row \\(r\\) that can hear a student seated at table \\(t\\). For each student's table, compute the number of tables on the current row that can hear them. Let this length be \\(l\\). Let the column of the student's table be \\(c\\). The tables on the current row that can hear the student are defined by the interval \\([c-\frac{l}{2},c+\frac{l}{2}]\\). Update each of these indices by \\(+1\\) in the cache. Also compute the number of tables on the previous row that can hear them. Let this length by \\(k\\). The tables on the previous row that can hear the student are defined by the interval \\([c-\frac{k}{2},c+\frac{k}{2}]\\). Update each of these indices by \\(-1\\) in the cache.

   - You do not need to compute the number of tables on the previous row if the current row is \\(0\\).

   - Since table numbers are laid out in a 1D array, we can use modulus to determine which column from the 2D grid a table belongs to.

   ```java
   public static int getCol(long t, int len) {
     return (t - 1) % len;
   }
   ```

   - To compute the length of an interval, you'll need to use a bit of trigonometry. Given a table \\(t\\) that comes from row \\(y\\), you can define a right triangle whose height is \\(r - y\\) and whose hypotenuse is \\(d\\). The angle between is \\(\alpha = \arcsin(\frac{h}{d})\\). Therefore the base of the triangle is \\(b = \cos(\alpha) \cdot d\\). We only care about the discrete tables, so the interval length is \\(2 \cdot \lfloor b \rfloor + 1\\).

       ```java
       public static int getIntervalLength(int r, long t, int len, int d) {
         double h = r - getRow(t, len);
         double angle = Math.asin(h / d);
         double b = Math.cos(angle) * d;
         return 2 * ((int) Math.floor(b)) + 1;
       }
       ```

5. Let the number of students that can be heard from an unoccupied table be its _noise level_. Compute the minimum noise level for the current row by finding the table on the current row in the cache with the smallest value. Maintain the smallest noise level that you see across rows. If the noise level is ever \\(0\\), then the solution is \\(0\\).

   - In this problem, Ashley cannot sit at a table where a student is currently sitting. Since the noise level is guaranteed to be a non-negative number, you can use the sign of the noise level to indicate whether or not there is a student currently sitting at a given table. For example, if a table has a noise level of \\(+2\\), then that table is vacant and can hear two other students. However, if a table has a noise level of \\(-2\\), then that table is occupied and can hear two students (including the one seated at the table).

6. Iterate through the tables starting from the least table. For each table with column \\(c\\) s.t. the current row \\(y = c + d\\), remove this table's interval from the cache. The first table whose column \\(c\\) s.t. \\(y < c + d\\) becomes the least table. Then repeat this algorithm from step (4) until there are no more tables left to process. Once all tables have been processed, the minimum noise level is the solution.

Optimizations

- After processing the current row, if you discover that there is a table whose noise level is \\(0\\), then that is the solution.

{% end %}
