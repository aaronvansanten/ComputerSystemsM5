# 2.1 - Q17
Dawn has eaten the pie. Only if he lies, the conditions are met.

# 2.2 - Q17
*Question a*
| Step                                               | Law |
| -------------------------------------------------- | --- |
| $[(p \lor q) \land (p \lor \neg q)] \lor q$        | L5  |
| $\Leftrightarrow [p \lor (q \land \neg q)] \lor q$ | L8  |
| $(p \lor F_0) \lor q$                              | L7  |
| $p \lor q$                                         | -   |

*Question b*
| Step                                                          | Law |
| ------------------------------------------------------------- | --- |
| $(p \rightarrow q) \land [\neg q \land (r \lor \neg q)]$      | L10 |
| $\Leftrightarrow (p \rightarrow q) \land \neg q$              | L12 |
| $\Leftrightarrow (\neg p \lor q) \land \neg q$                | L3  |
| $\Leftrightarrow \neg q \land (\neg p \lor q)$                | L5  |
| $\Leftrightarrow (\neg q \land \neg p) \lor (\neg q \land q)$ | L8  |
| $\Leftrightarrow (\neg q \land \neg p) \lor F_0$              | L7  |
| $\Leftrightarrow \neg q \land \neg p$                         | L2  |
| $\Leftrightarrow \neg(q \land p$                              |     |

# 2.2 - Q19
*Question a*
| Step                          | Law |
| ----------------------------- | --- |
| $p \lor [p \land (p \lor q)]$ | L10 |
| $\Leftrightarrow p \lor p$    | L6  |
| $\Leftrightarrow p$           |     |

*Question b*
| Step                                                                          | Law |
| ----------------------------------------------------------------------------- | --- |
| $\Leftrightarrow (p \lor q) \lor ((\neg p \land \neg q) \land r)$             | L2  |
| $\Leftrightarrow (p \lor q) \lor (\neg (p \lor q) \land r)$                   | L5  |
| $\Leftrightarrow [(p \lor q) \lor (\neg (p \lor q)] \land (p \lor q \lor r))$ | L8  |
| $\Leftrightarrow T_0 \land (p \lor q \lor r))$                                | L7  |
| $\Leftrightarrow p \lor q \lor r$                                             | -   |


*Question c*
| Step                                                                      | Law |
| ------------------------------------------------------------------------- | --- |
| $\Leftrightarrow [(\neg p \lor \neg q) \rightarrow (p \land q \land r)$   | L12 |
| $\Leftrightarrow \neg (\neg p \lor \neg q) \lor (p \land q \land r)$      | L2  |
| $\Leftrightarrow (\neg \neg p \lor \neg \neg q) \lor (p \land q \land r)$ | L1  |
| $\Leftrightarrow (p \lor q) \lor (p \land q \land r)$                     | L10 |
| $\Leftrightarrow p \land q$                                               | -   |




# 2.3 - Q7
The argument:
$$[p \land (p \rightarrow q) \land (s \lor r) \land (r \rightarrow \neg q)] \rightarrow (s \lor t)$$

| Steps num | Step                   | Reasons                    |
| --------- | ---------------------- | -------------------------- |
| 1         | $q$                    | Premise                    |
| 2         | $p \rightarrow q$      | Premise                    |
| 3         | $q$                    | steps 1, 2 and R1          |
| 4         | $r \rightarrow \neg q$ | Premise                    |
| 5         | $q \rightarrow \neg r$ | Step 4 and dubbel negative |
| 6         | $\neg r$               | steps 3,5 and R1           |
| 7         | $s \lor r$             | premise                    |
| 8         | $s$                    | steps 6,7 and R5           |
| 9         | $\therefore s \lor t$  | step 8 and R8              |


# 2.3 - Q9
*Question a*
| Steps num | Step                              | Reasons                            |
| --------- | --------------------------------- | ---------------------------------- |
| 1         | $\neg(\neg q \rightarrow s)$      | negation of premise                |
| 2         | $\neg q \land \neg s$             | Step 1 and L12 and L2              |
| 3         | $\neg s$                          | Step 2 and R7                      |
| 4         | $\neg r \lor s$                   | Premise                            |
| 5         | $\neg r$                          | Steps 3,4 and R5                   |
| 6         | $p \rightarrow q$                 | Premise                            |
| 7         | $\neg q$                          | Step 2 and R7                      |
| 8         | $\neg p$                          | Steps 6,7 and R3                   |
| 9         | $p \lor r$                        | Premise                            |
| 10        | $r$                               | Steps 8,9 and R5                   |
| 11        | $\neg r \land r$                  | Steps 5,10 and R4                  |
| 12        | $\therefore \neg q \rightarrow s$ | Step 11 and proof of contradiction |

*Question b*
| Steps num | Step                        | Reasons         |
| --------- | --------------------------- | --------------- |
| 1         | $p \rightarrow q$           | Premise         |
| 2         | $\neg q \rightarrow \neg p$ | L 13            |
| 3         | $p \lor r$                  | Premise         |
| 4         | $\neg p \rightarrow r$      | step 3 and L12  |
| 5         | $\neg q \rightarrow r$      | Steps 2,4 R2    |
| 6         | $\neg r \lor s$             | Premise         |
| 7         | $r \rightarrow s  $         | Step 6 and L12  |
| 8         | $\neg q \rightarrow s$      | Step 5,7 and R2 |



# 2.3 - Q10
*Question a*
| Steps num | Step                 | Reasons          |
| --------- | -------------------- | ---------------- |
| 1         | $p \land q$          | Premise          |
| 2         | $p$                  | Step 1 and R7    |
| 3         | $r$                  | Premise          |
| 4         | $P \land r$          | Steps 2,3 and R4 |
| 5         | $(p \land r) \lor q$ | Step 4 and R8    |


*Question b*
| Steps num | Step                         | Reasons       |
| --------- | ---------------------------- | ------------- |
| 1         | $p \land ( p \rightarrow q)$ | Premise       |
| 2         | $q$                          | Step 1 and R1 |
| 3         | $\neg q \land r$             | Premise       |
| 4         | $r$                          | Step 4 + R5   |

*Question c*
| Steps num | Step                         | Reasons       |
| --------- | ---------------------------- | ------------- |
| 1 | $p \rightarrow q$ | Premise |
| 2 | $\neg p$ | Step 1 and R3 |
| 3 | $\neg p \land \neg r$ | Premise|
| 4 | $\neg (p \lor r)$ | Steps 2,3 and L2|

*Question d*
| Steps num | Step                         | Reasons       |
| --------- | ---------------------------- | ------------- |
| 1 | $r \rightarrow \neg q$ | Premise|
| 2 | $r$ | Premise |
| 3 | $\neg q$ | Step 1,2 and R1|
| 4 | $p \rightarrow q$ | Premise|
| 5 | $\neg p$ | Step 3,4 and R3 |


# 2.3 - Q11
*Question a*
| primitive statement | value |
| ------------------- | ----- |
| p                   | 1     |
| q                   | 0     |
| r                   | 1     |

  *Question c*
| primitive statement | value |
| ------------------- | ----- |
| p                   | 1     |
| q                   | 1     |
| r                   | 1     |
| s                   | 0     |




