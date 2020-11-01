---
layout: page
title: Young Tableaux Explorer
permalink: /ytcore/index
---

# Background
## An Example
Suppose that we start with a partition and we want to know if we can successively remove dominoes from the outside while having it remain a valid partition at every step of the way. 


## The p-core


## The p-quotient

In practice, the $$p$$-quotient is easiest to construct by first converting the partition into its corresponding step sequence, and from that, separating the sequence based off of the index modulo $$p$$.


# Code
All of the code is available as a jupyter notebook [here](https://github.com/mushokunosora/youngtableaux). Most of it is fairly self-explanatory and it also contains a fair bit of documentation.


## Some graphs
Pictured below are the number of $$p$$-cores of size $$n$$ where the gif cycles between $$2 \le p \le 11$$ and $$1 \le n \le 50$$. There are a number of conjectures realted to this number (oftentimes denoted as $$c_p(n)$$), and the monotonicity has been proven relatively recently. Surprisingly, the $$p-$$cores of size $$n$$ are exceedingly rare, as it does not seem like a big restriction that the $$p-$$core cannot have any hook lengths divisible by $$p$$. However, the fact that as $$p$$ increases, so too does $$c_p(n)$$ is rather unsurprising, as it would seem that it gives you a larger degree of freedom.



Pictured below is a gif of several graphs of the number of partitions with empty $$p-$$core. Note that this is *relatively* well understood in the sense that we know the generating function. From the bijection between regular partitions and their cores and quotient, this is just the coefficient of $$x^n$$ of the generating function $$\pi(x)^p$$ (here $$\pi(x)$$ has the number of partitions of $$n$$ as the coefficient of $$x^n$$). 


Below is a comparison of the number of partitions whose quotient has at least one empty partition with $$p\pi(x)^{p-1}$$. This comparison was chosen because of the fact that $$c_p(n)$$ is very small, so it's basically asking about the number of ways to fill the other $$p-1$$ partitions, which is roughly $$p\pi(x)^{p-1}$$. **check constant**


To round it off, a gif of the frequency of the different sizes of $$p-$$cores is provided (both raw and histogram). 





## Some patterns

### Odd Symmetric Young Tableaux
The only odd symmetric young tableaux is $$(1).$$ This most naturally follows from looking at Young's Lattice, and noticing that every walk to a symmetric partition also corresponds to a walk along the transposes of those same intermediate partitions. So, provided that there is no straight line of odd symmetric partitions to other odd symmetric partitions, this should give us an even number of walks. Note that at height 2, it only contains the partitions $$(2)$$ and $$(1,1)$$, and hence, we are done.

An alternative way (that is much less intuitive) to see this is that from a difficult fact, all odd partitions of $$n$$ must have a hook of length $$2^k$$, where $$2^k \le n < 2^{k+1}$$, and as a result, this hook must be unique. However, since the partition is symmetric, we have that there would have to be another hook of the same length, unless $$2^k$$ is odd, in which case the only partition is $$(1)$$

###
Appearance of zeroes in $$c_3(n)$$. Credit to Ken Ono.



### $$\nu_2$$ Multiset
Empirically, all of the odd partitions of $$n$$ have the same multiset of 2-valuations, which from the fact that the $$(n)$$ partition is always odd, implies that it's the same as the multiset of 2-valuations of the numbers $$1, \ldots, n$$.

