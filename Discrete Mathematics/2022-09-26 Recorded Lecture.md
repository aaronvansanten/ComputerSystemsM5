# Lecture 2022 - 09 - 26

## Mathematical induction
Mathematical induction implies that you have proven it for some value, that this is true for all values.
The first step in this method is the **Basis Step**. This is usually a small step that proves the initial value. The second step is the **induction step**. In this step, we suppose that the statement holds for an undefined value *k*. This is the **induction hypothesis (IH)**. The following step is proving that the following is also true, thus *(k+1)*. When proving this, the IH is almost always required. Make clear where you use this. Then you can prove this by some 'basic' algebra. If you prove this, you prove that this statement holds for all 'k'.

## Theorem
The following theorem is stated about mathematical induction:

Let $n \in Z^+$ be an open statement, then S(n) is true for all $n \in Z^+$ if:
1. S(1) is true
2. for all $k \in Z^+$, the implication S(k) -> S(k + 1) holds

Another principle is the  Wel;-orderings principle. This states: If we take a subset of $Z^+$ (if not empty), there is always a smallest element.