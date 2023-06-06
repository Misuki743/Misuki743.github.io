---
title: Intution behind generating function(OGF)
date: 2023-06-06 01:07:27
tags: 
mathjax: true
---

------------------------------------------Under construction------------------------------------------

# Intution behind generating function(OGF)

In this blog, I will show how to formulate various kind of counting problems into polynomial form, and the powerfulness of this method.

Let's see the following problem

> Problem: Given sets $A = \{2, 3\}, B = \{2, 4\}, C = \{3, 5, 7\}$, count the number of ways to choose one element from each set sum up to $n$.

> Answer: $\[x^n\](x^2 + x^3)(x^2 + x^4)(x^3 + x^5 + x^7)$

remark: $[x^n]f$ means taking the coefficient of $n$-th term of polynomial $f$. For example, $\[x^4\](x^2 + 3x^4 + 5x^5) = 3$ since the $4$-th term of polynomial is $3x^4$.

There are few ways to think about this, one is using distributive law to argue that the result is  equal to

- first choose $x^2$ or $x^3$
- then choose(multiply with) $x^2$ or $x^4$
- then choose(multiply with) $x^3$ or $x^5$ or $x^7$
- finally sum up results for all the choices, then  the number of ways to get $x^{a + b + c} = x^n$ is equal to the number of ways to choose elements $a, b, c$ from each set such that $a + b + c = n$.

But I prefer another way to think about it, that is, think the whole polynomial as the process of DP. 

There is an obvious way to fomulate this problem in DP.

$DP[3][n]: \text{consider first i sets, the number of ways to have sum j}$
$\text{Base case: } DP[0][0] = 1$
$\text{Transitions: }$
$DP[0][j] \rightarrow DP[1][j + 2], DP[1][j + 3]$
$DP[1][j] \rightarrow DP[2][j + 2], DP[2][j + 4]$
$DP[2][j] \rightarrow DP[3][j + 3], DP[3][j + 5], DP[3][j + 7]$

And get back to the polynomial form, let's see how each component is related to the DP formula.

First we have a $1$ as the base case. $\leftrightarrow$ $DP[0] = \{1\}$

Then we multiply $1$ with $(x^2 + x^3)$, giving us $(x^2 + x^3)$. $\leftrightarrow$ $DP[1] = \{0, 0, 1, 1\}$

which is equivalent to transitions from $DP[0][0]$ to $DP[1][0 + 2]$ and $DP[1][0 + 3]$

Then multiply $(x^2 + x^3)$ with $(x^2 + x^4)$, giving us $(x^4 + x^5 + x^6 + x^7)$ $\leftrightarrow$ $DP[2] = \{0, 0, 0, 0, 1, 1, 1, 1\}$
multiply $x^2$ with $x^2$ means transition $DP[1][2] \rightarrow DP[2][2 + 2]$.
And multiply $x^2$ with $x^4$ means transition $DP[1][2] \rightarrow DP[2][2 + 4]$ etc...

We can think multiplying with a polynomial $(x^a + x^b + x^c...)$ as transitions
$$
DP[i][j] \rightarrow DP[i + 1][j + a], DP[i + 1][j + b], DP[i + 1][j + c]...
$$

then it will become quite easy to come up with the respective polynomial for counting problems.
