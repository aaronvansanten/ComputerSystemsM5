# Lecture 2
## Brackets
In case two ore more consecutive $ \vee $ or $ \wedge $ wihtout brakets, they are applied from left to right. However, changing the brackets around an expression allows to create different 'combinations' and change the order of execution. This allows you to apply the [[Laws of Logic]] in more cases.


## Logical implications
The statement $ \alpha _1 ... \alpha _n $ are said to logically imply statement $\beta$ if $$ (\alpha _1 \wedge \alpha _2 \wedge ... \alpha _n) \rightarrow \beta$$

Here, $\alpha _1 \wedge \alpha _2 \wedge ... \alpha _n$ are called premises and $\beta$ is called the conclusion. If this is shown to be an tautology, we can denote this by $ \Rightarrow $ to show that is has been proven. Therefore, $\beta$ would de deduible from $ \alpha _1 ... \alpha _n $. To show $ \beta $ is true, we only have to prove that all $ \alpha _1 ... \alpha _n $ are true. This can be done using a truth table.


The argument $$ (\alpha _1 \wedge \alpha _2 \wedge ... \alpha _n) \rightarrow \beta $$ can be rewritten in tabular form as: 
$ \alpha _1 $ $ \alpha _2 $ ... $ \alpha _n$
$ \therefore$ $\beta$


## Rules of Inference
The [[Rules of Inference]] are similar to the laws of logic. They can be used in similar fashion to prove that statements are equal to eachother. 

## Rule of Contradiction
The implication $ p \rightarrow q $ and $ (p \wedge \neg q) \rightarrow F_0 $
This can be used to simplify some logical formulas.

In other words, if one can deduce a contradiciton, $F_0$ from the premisises $ \alpha _1 $ $ \alpha _2 $ ... $ \alpha _n$ , togehter with the extra premise $q$ then 1 is deducible from the premises $ \alpha _1 $ $ \alpha _2 $ ... $ \alpha _n$.

## Deduction theorem
*No clue for to note this down*

## Proving an argument
If you want to prove that an argument holds, you need to consider ***all*** possible cases which the premises are true and the conclusion is true. This can be done using a truth table or the [[Laws_of_Logic]] or the [[Rules_of_Inference]].
If you want to prove that an argument does not hold, you need to consider ***1*** possible case where the premise is true and the conclusion is false. (A counterexample).