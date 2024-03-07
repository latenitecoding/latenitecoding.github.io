+++
title = "Set Theory"
path = "set-theory"
in_search_index = true
date = 2024-03-07
[taxonomies]
tags = ["set-theory"]
+++

# Definition
_A set is an unordered collection of objects s.t. each object in the set is unique._

Since a set is unordered, its elements have no assigned indices. Sets have no inherent structure, and the elements of a set can be represented in any order. For example, the sets A = {1, 2, 3} and B = {3, 2, 1} are the same sets. Sets are typically notated as a collection of elements between curly braces.

The elements of a set are unique, which means that each element in a given set can only appear once across the set. This makes sets extremely useful for determining which elements uniquely appear across some larger collection.

The common operations related to sets are cardinality, difference, intersection, members, and union.

### _Set Cardinality_
The **cardinality**, or size, of a set indicates the number of elements in the set. Given a set X, its cardinality is denoted \\(|X|\\).

### _Set Difference_
The **difference** of two sets A and B is denoted \\(A - B = C\\) and has the following property.

- _An element \\(x \in C\\) iff \\(x \in A\\) and \\(x \not\in B\\)._

In other words, the difference of sets A and B determines which elements only appear in A.

### _Set Intersection_
The **intersection** of two sets A and B is denoted \\(A \cap B = C\\) and has the following property.

- _An element \\(x \in C\\) iff \\(x \in A\\) and \\(x \in B\\)._

The intersection of two sets determines which elements appear in both sets.

### _Set Membership_
The notation \\(x \in X\\) is typically used to denote that some element x is a **member** of the set X. Lowercase letters are commonly used as variables of elements in a set while uppercase letters are typically used as variables for the sets themselves. Similarly, the notation \\(x \not\in X\\) is used to indicate that the element x is not in the set X.

### _Set Union_
The **union** of two sets A and B, denoted \\(A \cup B = C\\), has the following property.

- _An element \\(x \in C\\) iff \\(x \in A\\) or \\(x \in B\\)._

The union of two sets determines which elements appear in either of the two sets. Note that since the elements of a set must be unique, if an element appears in both set A and set B, then it only appears once in set C. As such, it should be understood that \\(|A \cup B| \neq |A| + |B|\\) in most cases. The equality only holds if the intersection of sets A and B is empty.
