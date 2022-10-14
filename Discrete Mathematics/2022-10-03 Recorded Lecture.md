# 2022-10-03 Recorded Lecture

## Cartesian Products and Relationships
The Cartesian product is defined for sets by:
$$A \times B = (a,b) a \in A, b \in B  $$
It is important to note the way it is noted
$$ A \times B \neq B \times A$$

A relation between two sets A and B is a subset of $A \times B$. The number of relations between these is equal to $2^{{|A \times B|}}$

The following theorem holds:
- $A \times (B \cap C) = (A \times B) \cap (A \times C)$
- $A \times (B \cup C) = (A \times B) \cup (A \times C)$
- $(A \times B) \cap C = (A \times c) \cap (B \times C)$
- $(A \times B) \cup C = (A \times c) \cup (B \times C)$


## Functions
A function `f` is a relation `R` from `A` to `B`. Each element of `A` appears exactly once as the first component of `R`. It is notated as:
$$f: A \rightarrow B / f(a) = b $$
For example, we take $A = (1,2,4)$ and $B = (2,3,5,6)$ then:
- [(1,1), (2,3), (4,5)] is not a relationship
- [(1,5), (2,3), (4,3)] is a relationship.

A one-to-one function is a function where every element of `B` appears once as an image of the element `A`.

### Access function
The Access function is a special function. It takes a two dimensional array as input and converts it to a 1-dimensional array. This is given by $f(a) = (i - 1)n + j$ where `i` denotes the row and `j` denotes the column.