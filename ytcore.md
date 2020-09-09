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


# Code
All of the code is available as a jupyter notebook [here](https://github.com/mushokunosora/youngtableaux). Most of it is fairly self-explanatory and it also contains a fair bit of documentation.


## Some graphs
Pictured below are the number of 

## Some patterns

### Odd Symmetric Young Tableaux
The only odd symmetric young tableaux is $$(1).$$ This most naturally follows from looking at Young's Lattice, and noticing that every walk to a symmetric partition also corresponds to a walk along the transposes of those same intermediate partitions. So, provided that there is no straight line of odd symmetric partitions to other odd symmetric partitions, this should give us an even number of walks. Note that at height 2, it only contains the partitions $$(2)$$ and $$(1,1)$$, and hence, we are done.

An alternative way (that is much less intuitive) to see this is that from a difficult fact, all odd partitions of $$n$$ must have a hook of length $$2^k$$, where $$2^k \le n < 2^{k+1}$$, and as a result, this hook must be unique. However, since the partition is symmetric, we have that there would have to be another hook of the same length, unless $$2^k$$ is odd, in which case the only partition is $$(1)$$

### $$\nu_2$$ Multiset
Empirically, all of the odd partitions of $$n$$ have the same multiset of 2-valuations, which from the fact that the $$(n)$$ partition is always odd, implies that it's the same as the multiset of 2-valuations of the numbers $$1, \ldots, n$$.

