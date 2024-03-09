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

The union of two sets determines which elements appear in either of the two sets. Note that since the elements of a set must be unique, if an element appears in both set A and set B, then it only appears once in set C. As such, it should be understood that \\(|A \cup B| \neq |A| + |B|\\) in most cases. The equality only holds if the intersection of sets A and B is empty. This leads to a commonly known set theorem.

- _\\(|A \cup B| + |A \cap B| = |A| + |B|\\)_

# Empty Set
_There is a special set called the empty set denoted \\(\empty\\) that contains no elements. It is therefore also true that \\(|\empty| = 0\\)._

# Subsets
_A set X is called a subset of Y denoted \\(X \subseteq Y\\) if every element \\(x \in X\\) is also an element of Y (\\(x \in Y\\)). If \\(X \subseteq Y\\) and there is some element \\(y \in Y\\) that is not in X (\\(y \not\in X\\)), then we can say that X is a **proper** subset of Y denoted \\(X \subset Y\\)._

The use of subsets provides a convenient notation to describe some collection of elements when all possible elements are known. The collection of all possible elements is typically referred to as the **universal set** and is denoted \\(U\\). The idea opposite to the universal set would be the empty set, which contains no elements. Two interesting axioms follow from these ideas.

- _Given some universal set \\(U\\), for any set X, \\(X \subseteq U\\)._
- _Given some set X, \\(\empty \subseteq X\\)._

The last axiom is the most surprising and should be committed to memory. The empty set is considered to be a subset of all other sets, including itself.

# Power Set
_Given a set Y, we can define the power set of Y denoted \\(P(Y)\\) s.t. all sets X where \\(X \subseteq Y\\), \\(X \in P(Y)\\)._

The power set of a given set Y is a collection of all subsets of Y, including the empty set. It is useful for expressing all possible selections of all possible sizes that could be taken from Y. Given its construction, the size of the power set is always known.

- _Given some set Y, \\(|P(Y)| = 2^{|Y|}\\)._
