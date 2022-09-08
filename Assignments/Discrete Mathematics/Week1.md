# 2.1 - Q17
Dawn has eaten the pie. Only if he lies, the conditions are met.

# 2.2 - Q17
*a*
| Step                                                | Law |
| --------------------------------------------------- | --- |
| $[(p \vee q) \wedge (p \vee \neg q)] \vee q$        | L5  |
| $\Leftrightarrow [p \vee (q \wedge \neg q)] \vee q$ | L8  |
| $(p \vee F_0) \vee q$                               | L7  |
| $p \vee q$                                          | -   |

*b*
| Step                                                            | Law |
| --------------------------------------------------------------- | --- |
| $(p \rightarrow q) \wedge [\neg q \wedge (r \vee \neg q)]$      | L10 |
| $\Leftrightarrow (p \rightarrow q) \wedge \neg q$               | L12 |
| $\Leftrightarrow (\neg p \vee q) \wedge \neg q$                 | L3  |
| $\Leftrightarrow \neg q \wedge (\neg p \vee q)$                 | L5  |
| $\Leftrightarrow (\neg q \wedge \neg p) \vee (\neg q \wedge q)$ | L8  |
| $\Leftrightarrow (\neg q \wedge \neg p) \vee F_0$               | L7  |
| $\Leftrightarrow \neg q \wedge \neg p$                          | L2  |
| $\Leftrightarrow \neg(q \wedge p$                               |     |

# 2.2 - Q19
*a*
| Step                           | Law |
| ------------------------------ | --- |
| $p \vee [p \wedge (p \vee q)]$ | L10 |
| $\Leftrightarrow p \vee p$     | L6  |
| $\Leftrightarrow p$            |     |

*b*
| Step                                                                           | Law |
| ------------------------------------------------------------------------------ | --- |
| $\Leftrightarrow (p \vee q) \vee ((\neg p \wedge \neg q) \wedge r)$            | L2  |
| $\Leftrightarrow (p \vee q) \vee (\neg (p \vee q) \wedge r)$                   | L5  |
| $\Leftrightarrow [(p \vee q) \vee (\neg (p \vee q)] \wedge (p \vee q \vee r))$ | L8  |
| $\Leftrightarrow T_0 \wedge (p \vee q \vee r))$                                | L7  |
| $\Leftrightarrow p \vee q \vee r$                                              | -   |


*c*
| Step                                                                        | Law |
| --------------------------------------------------------------------------- | --- |
| $\Leftrightarrow [(\neg p \vee \neg q) \rightarrow (p \wedge q \wedge r)$   | L12 |
| $\Leftrightarrow \neg (\neg p \vee \neg q) \vee (p \wedge q \wedge r)$      | L2  |
| $\Leftrightarrow (\neg \neg p \vee \neg \neg q) \vee (p \wedge q \wedge r)$ | L1  |
| $\Leftrightarrow (p \vee q) \vee (p \wedge q \wedge r)$                     | L10 |
| $\Leftrightarrow p \wedge q$                                                | -   |




# 2.3 - Q7
The argument:
$$[p \wedge (p \rightarrow q) \wedge (s \vee r) \wedge (r \rightarrow \neg q)] \rightarrow (s \vee t)$$

| Steps num | Step                   | Reasons                    |
| --------- | ---------------------- | -------------------------- |
| 1         | $q$                    | Premise                    |
| 2         | $p \rightarrow q$      | Premise                    |
| 3         | $q$                    | steps 1, 2 and R1          |
| 4         | $r \rightarrow \neg q$ | Premise                    |
| 5         | $q \rightarrow \neg r$ | Step 4 and dubbel negative |
| 6         | $\neg r$               | steps 3,5 and R1           |
| 7         | $s \vee r$             | premise                    |
| 8         | $s$                    | steps 6,7 and R5           |
| 9         | $\therefore s \vee t$  | step 8 and R8              |


# 2.3 - Q9
*a*
| Steps num | Step                              | Reasons                            |
| --------- | --------------------------------- | ---------------------------------- |
| 1         | $\neg(\neg q \rightarrow s)$      | negation of premise                |
| 2         | $\neg q \wedge \neg s$            | Step 1 and L12 and L2              |
| 3         | $\neg s$                          | Step 2 and R7                      |
| 4         | $\neg r \vee s$                   | Premise                            |
| 5         | $\neg r$                          | Steps 3,4 and R5                   |
| 6         | $p \rightarrow q$                 | Premise                            |
| 7         | $\neg q$                          | Step 2 and R7                      |
| 8         | $\neg p$                          | Steps 6,7 and R3                   |
| 9         | $p \vee r$                        | Premise                            |
| 10        | $r$                               | Steps 8,9 and R5                   |
| 11        | $\neg r \wedge r$                 | Steps 5,10 and R4                  |
| 12        | $\therefore \neg q \rightarrow s$ | Step 11 and proof of contradiction |

| Steps num | Step                        | Reasons         |
| --------- | --------------------------- | --------------- |
| 1         | $p \rightarrow q$           | Premise         |
| 2         | $\neg q \rightarrow \neg p$ | L 13            |
| 3         | $p \vee r$                  | Premise         |
| 4         | $\neg p \rightarrow r$      | step 3 and L12  |
| 5         | $\neg q \rightarrow r$      | Steps 2,4 R2    |
| 6         | $\neg r \vee s$             | Premise         |
| 7         | $r \rightarrow s  $         | Step 6 and L12  |
| 8         | $\neg q \rightarrow s$      | Step 5,7 and R2 |



# 2.3 - Q10
*a*

*b*
*c*
*d*


# 2.3 - Q11
*a*
| primitive statement | value |
| ------------------- | ----- |
| p                   | 1     |
| q                   | 0     |
| r                   | 1     |

*c*
| primitive statement | value |
| ------------------- | ----- |
| p                   | 1     |
| q                   | 1     |
| r                   | 1     |
| s                   | 0     |




