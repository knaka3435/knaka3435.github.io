---
layout: page
title: Young Tableaux Explorer
permalink: /ytcore/index
---

# Background
## An Example
Suppose that we start with a partition and we want to know if we can successively remove dominoes from the outside while having it remain a valid partition at every step of the way. 


## The p-core
The $$p$$-core is harder to compute, although with 

## The p-quotient

In practice, the $$p$$-quotient is easiest to construct by first converting the partition into its corresponding step sequence, and from that, separating the sequence based off of the index modulo $$p$$. For example, if we have the partition $$(5,4,3,3,2,1)$$, we have that the corresponding step sequence is $(\overline{1}, 0,1,0,1,0,1,1,0,1,0,1,\overline{0})$, and if we wanted to take the $$3$$-quotient, we would separate out the sequence into $(0,1,1,0) = (0,1,1)$, $(1,0,0,1) = (0,0,1)$ and $(0,1,1)$.


# Code
All of the code is available as a jupyter notebook [here](https://github.com/mushokunosora/youngtableaux). Most of it is fairly self-explanatory and it also contains a fair bit of documentation.


## Some graphs
Pictured below are the number of $$p$$-cores of size $$n$$, first for $$p=2,3,4$$ (so it's not dwarfed by the larger $$p$$) and then for $$ 2 \le p \le 8$$ and $$p = 11$$, and $$1 \le n \le 50$$. There are a number of conjectures realted to this number (oftentimes denoted as $$c_p(n)$$), and is referred to as Stanton's conjecture Surprisingly, the $$p-$$cores of size $$n$$ are exceedingly rare, as it does not seem like a big restriction that the $$p-$$core cannot have any hook lengths divisible by $$p$$. However, the fact that as $$p$$ increases, so too does $$c_p(n)$$ is rather unsurprising, as it would seem that it gives you a larger degree of freedom to build your $$p$$-cores.

![p=2,3,4](/assets/smallcore.png) ![All values of p](/assets/largecore.png)


To round it off, a large number of animated gifs of the frequency of the different sizes of $$p-$$quotients is provided. At time $$1 \le n \le 50$$, the distribution of the total size of the $$p$$-quotients of all partitions of $$n$$ is displayed, with the leftmost point corresponding to an empty $$p$$-quotient (and full $$p$$-core) and the rightmost with a $$p$$-quotient that is as full as possible.

![p=2](/assets/p2distr.gif)
![p=3](/assets/p3distr.gif)
![p=4](/assets/p4distr.gif)
![p=5](/assets/p5distr.gif)
![p=6](/assets/p6distr.gif)
![p=7](/assets/p7distr.gif)
![p=8](/assets/p8distr.gif)
![p=11](/assets/p11distr.gif)

As an added bonus, there's also the following for $$p=11$$ and going up to $$n=74$$ (since it had taken over 9 hours to get that far)
![p=11](/assets/p11extdistr.gif)
## Some patterns
### $$p=2$$
The distribution looks a bit odd, however most of it can be explained readily.

In this case, the cores correspond to partitions of triangular numbers and look like a staircase. This isn't too surprising, as $$2$$-hooks are basically dominoes, and when the tableaux is given a chessboard coloring, every $$2$$-hook must cover a black and a white square. The only case in which a domino isn't able to be removed from a partition is if it's in a staircase-like pattern. So, in the distributions, you can see that the nonzero sizes of $$p$$-quotients is rather rare, with little spikes corresponding to triangular numbers. Also, since there is at most one $$p$$-core of a given size, it follows that the frequency of the nonzero sizes are monotone (due to the number of partitions of $$2$$ kinds being monotone as well).

### $$p=3$$
It's rather odd that there are periodic zeroes in the number of $$3$$-cores of size $$n$$. There's a [paper](https://www.ams.org/journals/tran/1996-348-01/S0002-9947-96-01481-X/S0002-9947-96-01481-X.pdf) by Granville and Ono which describes this phenomonon and why (in section 3, also mentioned on page 334). 

### $$p \ge 4$$
As the graph of $$p-$$cores of size $$n$$ suggest, it appears that both the number of $$p-$$cores increases with $$n$$, as well as the fact that as $$p$$ increases, so does the number of $$p-$$cores. There are a variety of cases that have been proved (typically Granville and Ono).

Additionally, the distrubtions suggest something else that is rather surprising, which is that as $$p$$ increases, the distribution of sizes of $$p$$-quotients goes somewhat leftward. 

### Odd Symmetric Young Tableaux
The only odd symmetric young tableaux is $$(1).$$ This most naturally follows from looking at Young's Lattice, and noticing that every walk to a symmetric partition also corresponds to a walk along the transposes of those same intermediate partitions. So, provided that there is no straight line of odd symmetric partitions to other odd symmetric partitions, this should give us an even number of walks. Note that at height 2, it only contains the partitions $$(2)$$ and $$(1,1)$$, and hence, we are done.

An alternative way (that is much less intuitive) to see this is that from a difficult fact, all odd partitions of $$n$$ must have a hook of length $$2^k$$, where $$2^k \le n < 2^{k+1}$$, and as a result, this hook must be unique. However, since the partition is symmetric, we have that there would have to be another hook of the same length, unless $$2^k$$ is odd, in which case the only partition is $$(1)$$

### $$\nu_2$$ Multiset
Empirically, all of the odd partitions of $$n$$ have the same multiset of 2-valuations, which from the fact that the $$(n)$$ partition is always odd, implies that it's the same as the multiset of 2-valuations of the numbers $$1, \ldots, n$$.


##### TODO:
Add a few more images corresponding to the following text!

Pictured below is a gif of several graphs of the number of partitions with empty $$p-$$core. Note that this is *relatively* well understood in the sense that we know the generating function. From the bijection between regular partitions and their cores and quotient, this is just the coefficient of $$x^n$$ of the generating function $$\pi(x)^p$$ (here $$\pi(x)$$ has the number of partitions of $$n$$ as the coefficient of $$x^n$$). 


Below is a comparison of the number of partitions whose quotient has at least one empty partition with $$p\pi(x)^{p-1}$$. This comparison was chosen because of the fact that $$c_p(n)$$ is very small, so it's basically asking about the number of ways to fill the other $$p-1$$ partitions, which is roughly $$p\pi(x)^{p-1}$$. 