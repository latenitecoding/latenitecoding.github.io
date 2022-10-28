---
layout: post
title: Find the Min/Max Element of an Array
category: algorithms
tag: arrays
---

A simple but foundational algorithm to work with arrays is to calculate its minimum or maximum element.

## Find Min

```pseudo
procedure FindMin(Arr)
======================
 PRE: |Arr| > 0
POST: min in Arr
----------------------
min <- Arr[0]
for a in Arr do
  if a < min then
    min <- a
  end if
end for
min
======================
```
## Find Max

```pseudo
procedure FindMax(Arr)
======================
 PRE: |Arr| > 0
POST: max in Arr
----------------------
max <- Arr[0]
for a in Arr do
  if a > max then
    max <- a
  end if
end for
max
======================
```