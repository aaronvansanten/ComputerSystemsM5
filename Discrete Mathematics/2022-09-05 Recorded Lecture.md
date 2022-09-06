# Organization
## What is logic?
Studies correctness of reasoning.

### Basic connectives and truth tables
A proposition is a statement that is unambiguously true or false (not both).
Propositions are denoted with lower-case letters (p, q, r, etc.).

### Logical symbols
| Symbol | Name | Text |
| :----- | :--: | :----|
| $\neg p$ | Negation | not p|
| $p \land q$  | Conjunction | p and q|
| $p \lor q$ | Disjunction | p or q (or both) |
| $p \underline{\lor} q$ |  Exclusive disjunction | either p or q |
| $p \rightarrow q$ | Implication | if p then q |
| $p \leftrightarrow q$ | Equivalence | p if and only if q |

And brackets are used.
Using truth tables one can verify the truthness of compound statements.

### Definitions
Tautology: a statement for which the truth table consists merely of ones.
Contradiction: a statement for which the truth table consists merely of zeros.

### Logical equivalence
To distinguish primitive statements from compound statements we use p, q, r, etc, for primitive and Greek letters for compound statements.

Two statements $\alpha$ and $\beta$ are logically equivalent (notation: $\alpha \Leftrightarrow \beta$) if their truth tables are identical. In other words: two statements are logically equivalent if the statement $\alpha \leftrightarrow \beta$ is a tautology.

### Tautologies and Contradictions
All tautologies are logically equivalent. A tautology is denoted by $T_0$.
All contradictions are logically equivalent. A contradiction is denoted by $F_0$.

### Dual statement
Consider a statement $\alpha$ that merely contains the connectives $\neg$, $\land$ and $\lor$. The dual statement $\alpha^d$ of $\alpha$ is the statement that arises from $\alpha$ by replacing each:
- $\lor$ by $\land$
- $\land$ by $\lor$
- $T_0$ by $F_0$
- $F_0$ by $T_0$




