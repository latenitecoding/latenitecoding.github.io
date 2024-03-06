+++
title = "Bitwise Ops"
path = "bitwise-ops"
in_search_index = true
date = 2024-03-06
[taxonomies]
tags = ["bitwise"]
+++

# Bitwise AND
The bitwise AND operation (typically notated as &) is an arithmetic operation
that takes two numbers (e.g., 32-bit integers) and computes a new number of the
same type with the following property.

    - Given A & B = C, if the jth bit of A and the jth bit of B are both 1, then
      the jth bit of C is also a 1. Otherwise, the jth bit of C is a 0.

The purpose of this operation is to determine which bits are shared between two
numbers or to specifically _mask_ away bits that can be ignored. Given the above
property, the following identities hold.

    - Given A & B = C, where B only includes 0 bits, C is the number 0.

    - Given A & B = C, where B only includes 1 bits, C is the same as A.

Imagine that you store two 16-bit integers (shorts) in one 32-bit integer
(int). How could you access each short individually?

You can produce an upper mask, where the upper 16-bits are 1s and the lower
16-bits are 0s, and a lower mask, where the upper 16-bits are 0s and the lower
16-bits are 1s. Given some 32-bit integer A, if you compute the bitwise AND of
A and the upper mask, then you've computed the first 16-bit short. Note that
this short is still stored in the upper 16-bits of a 32-bit integer, so the
result needs to be right-shifted by 16-bits. You must also be careful to
use an unsigned right-shift so that no additional 1s are introduced into the
result.

To compute the second short, you compute the bitwise AND of A and the lower
mask. No right-shifting is required for the second short since it is already
stored in the lower 16-bits.

This process is called _masking_, which is why the two values are called masks.
