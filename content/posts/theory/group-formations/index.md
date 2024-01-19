---
title: "Group formations"
date: 2023-02-09T17:08:44+01:00
draft: false
summary: "Basic group formations ways. Variations, permutations and combinations. Explanation and difference between them."
tags: ["theory", "group formations", "logic", "basic"]
---
{{< katex >}}



## Introduction

Let's explain some of the most elementary things in software development that have an enormous application when solving real problems. Sometimes we need to work with different aggrupations in a set of elements, or just know the number of possible combinations to be able to do a loop through them. E.g., the number of different words that can be formed with n letters.

For this purpose we will use the following 3 group formation methods:

* Variations
* Permutations
* Combinations




## Variations
We can understand the variations of a set as a particular order of n elements of that set in a certain position. I.e. It's not the same (a, b) than (b,a). They are both two variations of the same n elements of the set. Although the elements are the same, the position is not. In the variations the position is important. There are two types of variations:

* Variations without repetition.
* Variations with repetition.


### Variations without repetition.
---
Having a finit and not empty set **\\(S\\)** where **\\(n = |S|\\)**. And having the max number of elements in the variation **\\(r \in N\\)** and **\\(1 \le r \le n\\)**

* There aren't repeated elements from the set in each tuple.
* Two tuples are different if they haven't got the same elements in the same positions.

We can calculate the number of variations with the next formula: **$$V_{n,r} = n!/(n - r)!$$**

>E.g.
>
>If we want to calculate the number of variations with 3 positions with the set of elements **\\(S = \langle{s_{1}, s_{2}, s_{3}, s_{4}, s_{5}}\rangle\\)** where **$n = 5$**, the solution is: **$$V_{5,3} = 5!/(5 - 3)! = 60$$**


### Variations with repetition.
---
Having a finit and not empty set **\\(S\\)** where **\\(n = |S|\\)**. Then having the number of positions to do the variations **\\(r \in N\\)**.

* In each tuple can be repeated elements of **\\(S\\)**.
* Two tuples are different while they don't have the same elements in the same positions.

We can calculate the number of variations with repetition with the formula: **$$VR_{n,r} = n^{r}$$**

>E.g.
>
>A good example for this one could be the binary numbers. How many numbers could we represent with 4 positions? Knowing the set of the binary numbers is just 0 and 1 then **\\(n = 2\\)**. We could solve it as follows: **$$VR_{2,4} = 2^{4} = 16$$**




## Permutations
A permutation is the disposition of a specific order from a set of elements.  It means that every time we change the order of those elements we are doing a permutation of the set. 

Now we can think about how many ways we have to order the same set. For this, we have two different scenarios:

* **Permutations without repetition:** In this case, the elements ordered can not be repeated. 
* **Permutations with repetition:** The elements can appear more than one time in the order.


### Permutations without repetition.
---
Having a non empty and finit set **\\(S\\)** and being **\\(n = |S|\\)**. We say the permutation without repetition, **\\(P_{n}\\)**, are the variations without repetition of **\\(n\\)** elements taken **\\(n\\)** by **\\(n\\)**. We calculate it with the following formula: **$$P_{n} = n!$$**

>E.g.
>
>Let's supose we have the following set **\\(S = \langle{s_{1}, s_{2}, s_{3}, s_{4}, s_{5}}\rangle\\)** and **\\(|S| = 5\\)**. The number of total permutations in the set will be: **$$P_{5} = 5! = 120$$**


### Permutations with repetition.
---
Having a non empty and finit set **\\(S = \langle{s_{1}, s_{2}, \dots, s_{n}}\rangle\\)** and being **\\(n = |S|\\)**. Being the size of the permutation **\\(p \in N\\)** and **\\( p \ge n\\)** and being the number of repetitions of each element of the set **\\(r_{1}, r_{2}, \dots, r_{n}\\)** where each **\\(r_{i} \in N \cup \langle{0}\rangle\\)** and **\\(r_{1} + r_{2} + \dots + r_{n} = p\\)**.

* In each tuple the element **\\(s_{j}\\)** must appear **\\(r_{j}\\)** times.
* Two tupples are different if their elements are not equal.

It's calculated with the formula: **$$PR_{p^{r_{1}, r_{2}, \dots, r_{n}}} = p!/(r_{1}! * r_{2}! * \dots * r_{n}!)$$**

>E.g.
>
>Consider we have set **\\(S = \langle{s_{1}, s_{2}, s_{3}}\rangle\\)** and **\\(|S| = 3\\)**. And we want to calculate the permutations repeating each element two times. Then **\\(p = 2 + 2 + 2\\)**. The total ammount of permutations is: **$$PR_{6^{2, 2, 2}} = 6!/(2! * 2! * 2!) = 90$$**




## Combinations
A combination is a selection of n elements of a set in any order. Unlike the variations or the permutations, the order is not relevant in the combinations. I.e. (a,b) is the same that (b,a). We can separate them in two: 

* Combinations without repetition.
* Combinations with repetition.


### Combinations without repetition.
---
Having a finit and not empty set **\\(S\\)** where **\\(n = |S|\\)**. And having the max number of elements to combine **\\(r \in N\\)** and **\\(1 \le r \le n\\)**.
We can use the formula: **$$C_{n,r} = Binom(n,r)$$**

>E.g.
>
>Let's supose we want to calculate the combinations of 3 from a set **\\(S = \langle{s_{1}, s_{2}, s_{3}, s_{4}, s_{5}, s_{6}}\rangle\\)** where **\\(n = 6\\)**. It will be: **$$C_{6,3} = Binom(6,3) = 6!/(3! * 3!) = 20$$**


### Combinations with repetition.
---
Having a finit and not empty set **\\(S\\)** where **\\(n = |S|\\)**. And having the max number of positions to combine the elements **\\(r \in N\\)**.

* Elements can repeate in each tuple
* Two tuples are the same if they have exactly the same elements. Although the elements are in different order.

Then we can use the next formula: **$$C_{n,r} = Binom(n + r - 1, r)$$**

>E.g.
>
>In this case we will calculate how many combinations can we do with 5 letters in 3 spaces. Remember that in this case the letters can be repeated. Then with **\\(n = 5\\)** and **\\(r = 3\\)**: **$$C_{5,3} = Binom(5 + 3 - 1, 3) = 7!/(4! * 3!) = 35$$**
