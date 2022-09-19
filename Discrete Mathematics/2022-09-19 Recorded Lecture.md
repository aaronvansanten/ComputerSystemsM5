# Recorded Lecture 2022-09-19
From now on, this course will go over the set-theory.

$x \in A$ means that x is part of A

$x \notin A$ means that x is not a part of A

The Cardinality is the amount of elements in a set. It is denoted by the letter of the set in between absolute lines.

## Standard sets
| Letter | Domain           | Contains                                 |
| ------ | ---------------- | ---------------------------------------- |
| N      | Natural numbers  | 0,1,2,3...                               |
| Z      | Integers         | -3,-2,-1,0,1,2,3                         |
| Q      | rational numbers | $\frac{a}{b} - a,b \in Z \land b \neq 0$ |
| R      | Real numbers     | $-\infin, \infin$                        |
| C      | Complex numbers  | x + iy                                   |
 
## Intervals
There are four types of intervals:
$$[a,b] = \{x \in R | a \leq x \leq b\}$$
$$[a,b) = \{x \in R | a \leq x < b\}$$
$$(a,b] = \{x \in R | a < x \leq b\}$$
$$(a,b) = \{x \in R | a < x < b\}$$

## Subsets
$A \subseteq B$ implies $\forall x[(x \in A) \rightarrow (x \in B)]$

$A \nsubseteq B$ implies $\neg \forall x[(x \in A) \rightarrow (x \in B)]$ or $\exists x[(x \in A) \land (x \notin B)]$

$A \subset B$ means that A is a proper subset of B but A and B are not equal so
$$ A \subset B = (A \subseteq B) \land (A \neq B)$$


## Equality of Sets
Sets are equal in case $A=B \rightarrow (A \subseteq B) \land (B \subseteq A)$. You can only prove this by proving that every elements in A is B and every element in B is in A.

Sets are equal in case $A \neq B \rightarrow (A \nsubseteq B) \lor (B \nsubseteq A)$. You can only prove this by proving that at least one element in A is not in B or by proving that at least one element in B is not in A.

## Empty set
$\emptyset$ denotes the empty set. Please not that $\emptyset \neq \{0\}$ and $\emptyset \neq \{\emptyset\}$

$|\emptyset| = 0$

## Power set
The power set of a set is the set of all subsets of the set.
$P(A) = \{B | B \subseteq A \}$. 

An example is $A = \{1,2,3 \}$ then 
$P(A) = \{\emptyset, \{1 \}, \{2 \},\{3 \},\{1,2 \},\{1,3 \},\{2,3 \},\{1,2,3 \}\}$

For each set A, we have $P(A) = 2 ^ {|A|}$ 

## Operations of Sets
The union of sets A and B mean $A \cup B = \{x \mid x \in A \lor x \in B \}$

The intersection of sets A and B mean $A \cap B = \{x \mid x \in A \land x \in B \}$

The difference of sets A and B mean $A - B = \{x \mid x \in A \land x \notin B \}$

The symmetric difference of sets A and B mean $A \triangle B = \{x \mid x \in A \cup B \land x \notin A \cap B \}$

The complement of sets A and B mean $\bar{A} = U - A = \{x \mid x \notin A \}$

Sets are disjoint if $A \cap B = \emptyset$ or if $A \cup B = A \triangle B$