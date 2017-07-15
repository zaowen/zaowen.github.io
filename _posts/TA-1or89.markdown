---
layout: post
title:  "Think Again! Project Euler 92"
date: 2016-07-17 00:00:00 -0600
categories: ProjectEuler
---
Let a function $$C(n)$$ be defined as adding the square of the digits in $$n$$ to form a new number. Prove that all integers through repeated application of $$C(n)$$ equal 1 or 89.

<hr><br>

Let $$n$$ be an integer greater than 243 and $$m$$ be an integer less than 1000. 

Since the greatest value of $$C(m)$$ is 243 $$( 9^2 + 9^2 + 9^2  = 243)$$ with simmilar reasoning for an integer less than 10000 $$( 9^2 + 9^2 + 9^2 + 9^2  = 324)$$ , the value of $$C(n)$$ is strictly less than the value of $$n$$.

Since there is no value of $$x$$ where $$C(n) > 243 > x$$, for any integer $$z$$ and a sufficently large $$y$$, $$C^y(z)$$ is less 243.

At this point the problem can be solved using brute force, the following numbers will yeild 1, while all others less than 243 will yeild 89.

1, 7, 10, 13, 19, 23, 28, 31, 32, 44, 49, 68, 70, 79, 82, 86, 91, 94, 97, 100, 103, 109, 129, 130, 133, 139, 167, 176, 188, 190, 192, 193, 203, 208, 219, 226, 230, 236, 239. $$\blacksquare$$

<br>

