---
layout: page
title: MIT Coding Portfolio Documentation
permalink: /writeup/index
---
# Important note
This write-up is best enjoyed on my [github website](https://mushokunosora.github.io/writeup/index.html) (as the gifs are actually animated). Admittedly, this is a bit of an odd setup, however, I believe that this medium is best suited for containing all of the relevant information as well as including some short animated gifs related to the Young Tableaux project (although a few are also posted on SlideRoom). Additionally, some of the code presented later on is cut off as a pdf, so it's not clear what is happening.

Most of these projects were geared towards people who were studying the same math topics as me, and as such, at times the explanations might be a bit lacking. Also, a majority of this writeup has been taken from those individual project's writeups, and although not recommended, the Young Tableaux project write up is available separately [here](https://mushokunosora.github.io/ytcore/index.html), and similarly, the Ring Theory in lean project is [here](https://mushokunosora.github.io/lean/index.html).

# Young Tableaux Explorer
## About the project
One of the bigger difficulties in accomplishing this project was constructing the algorithms. Although there was a visual one that I knew of for computing the $$p$$-cores and $$p-$$quotients, implementing this directly seemed to be very inefficient and prone to errors. After reading some more literature, I had come across a paper that featured step sequences in a proof concerning the number of ways to add/remove a hook of a certain size, and I realized that this would allow me to compute these efficiently. Although there were a few bugs here and there, most of the work had already been done after understanding what to do. Also, the runtime for the data collection was rather significant, for iterating over all of the partitions of sizes ranging from $$1$$ to $$50$$, it took close to an hour or so, meanwhile from $$50$$ to $$74$$ had taken around $$12$$ hours (conveniently about how long someone sleeps in).

Also, when solving some problems, I came across a remarkable theorem which stated that there exists a bijection between a partition and the tuple containing the $$p-$$core and the $$p-$$quotient of that same partition, and when trying to reconstruct a partition from it's tuple, I found that it was nearly impossible to do so. By trying to codify a lot of the algorithms, I had hoped to also make progress on this end, however, it seems like it might be awhile before I'm able to come up with a reconstruction algorithm.

### Motivation
There had been a few particularly interesting problem that my MIT PRIMES reading group had been looking at for awhile that involved how many partitions of $$n$$ had an odd number of standard young tableaux, as well as other problems like how many partitions of $$2n$$ were able to be tiled by dominoes. The first problem's answer was that for $$n=2^{k_1}+2^{k_2}+2^{k_3}+\ldots+2^{k_\ell}$$ (as the binary representation of $$n$$), there were exactly $$2^{k_1+k_2+\ldots+k_\ell}$$ partitions, which is a very strange number. Anyways, in order to solve those problems, the theory of p-cores and p-quotients was necessary, and to get a better intuition and form hypothesis, more computer-generated data was nice.

## Background
### Young Tableaux, Step sequences
We have that $$\lambda = (\lambda_1,\lambda_2,\ldots,\lambda_k)$$ is a partition of $$n$$ if $$\lambda_1 \ge \lambda_2 \ge \ldots \ge \lambda_k$$ and $$\sum_{i=1}^k \lambda_i = n$$, and write it as $$\lambda \vdash n$$. We identity partitions with a Young Tableaux, which is a north-west aligned bunch of cells such that the $$i$$th row contains $$\lambda_i$$ cells.

Also associated with a partition is a *step sequence*, in which we push the Young Tableaux into a corner of a wall, and we outline the exposed edge (including the wall) with the (perhaps not standard) notion that a vertical edge is a $$1$$ and a horizontal step is a $$0$$. For example, we have that the partition $$(1,1)$$ has step sequence $$(\overline{1}, 0,1,1,\overline{0})$$, where $$\overline{1}$$ and $$\overline{0}$$ represent an infinite sequence of $$1$$s and $$0$$s respectively. As another example, we have that $$(4,3,1,1)$$ has step sequence $$(\overline{1},0,1,1,0,0,1,0,1,\overline{0})$$. For convenience, we typically omit the $$\overline{1}$$ and the $$\overline{0}$$'s and identify sequences with leading $$1$$'s with the same without, and similarly for trailing $$0$$'s, so $$(1,1,0,0,1,0,0)=(1,1,1,1,0,0,1)=(0,0,1)$$.

Although longer, the step sequence often offers an efficient way of computing various things, and as such, is the backbone of quite a bit of the code.

### The p-core
The $$p$$-core is harder to compute, although with the step sequence, it becomes more manageable. A property of the step sequence is that if the $$i$$th element is a $$0$$, and the $$i+j$$the element is a $$1$$, then these positions correspond to a hook of length $$j$$. Now, if you swap the $$i$$th element with the $$i+j$$th, you removing that hook. So, basically what we want to do is to push all the $$1$$'s in the step sequence as far left as we can by iteratively moving them back $$p$$ positions. If we wanted to take the $$3$$-core of $$(0,1,0,1,1,0,0,0,1)$$, we can move the last $$1$$ to become $$(0,1,0,1,1,1)$$, and then the second to get $$(1,1,0,0,1,1)$$, and finally, we can move the last $$1$$ again to get $$(1,1,1,0,1) = (0,1)$$.

Algorithmically, one can accomplish this in linear time by starting with the first $$0$$ you find and looking from the back of the list (decreasing index by $$p$$) and towards the front while remembering where you last looked.

### The p-quotient

In practice, the $$p$$-quotient is easiest to construct by first converting the partition into its corresponding step sequence, and from that, separating the sequence based off of the index modulo $$p$$. For example, if we have the partition $$(5,4,3,3,2,1)$$, we have that the corresponding step sequence is $$(0,1,0,1,0,1,1,0,1,0,1)$$, and if we wanted to take the $$3$$-quotient, we would separate out the sequence into $(0,1,1,0) = (0,1,1)$, $(1,0,0,1) = (0,0,1)$ and $(0,1,1)$.


## Code
All of the code for the Young Tableaux explorer is available as a jupyter notebook [here](https://github.com/mushokunosora/mushokunosora.github.io/blob/master/YT.ipynb). Note that github allows one to view the code in a browser, although it is not interactive or executable. Most of the code is fairly self-explanatory and it also contains a fair bit of documentation, as well as examples.


### Some graphs
Pictured below are the number of $$p$$-cores of size $$n$$, first for $$p=2,3,4$$ (so it's not dwarfed by the larger $$p$$) and then for $$ 2 \le p \le 8$$ and $$p = 11$$, and $$1 \le n \le 50$$. There are a number of conjectures realted to this number (oftentimes denoted as $$c_p(n)$$), and is referred to as Stanton's conjecture Surprisingly, the $$p-$$cores of size $$n$$ are exceedingly rare, as it does not seem like a big restriction that the $$p-$$core cannot have any hook lengths divisible by $$p$$. However, the fact that as $$p$$ increases, so too does $$c_p(n)$$ is rather unsurprising, as it would seem that it gives you a larger degree of freedom to build your $$p$$-cores.

![p=2,3,4](/assets/yt/smallcore.png){:width="400px"} ![All values of p](/assets/yt/largecore.png){:width="400px"}


To round it off, a large number of animated gifs of the frequency of the different sizes of $$p-$$quotients is provided. At time $$1 \le n \le 50$$, the distribution of the total size of the $$p$$-quotients of all partitions of $$n$$ is displayed, with the leftmost point corresponding to an empty $$p$$-quotient (and full $$p$$-core) and the rightmost with a $$p$$-quotient that is as full as possible.

![p=2](/assets/yt/p2distr.gif){:width="400px"}
![p=3](/assets/yt/p3distr.gif){:width="400px"}
![p=4](/assets/yt/p4distr.gif){:width="400px"}
![p=5](/assets/yt/p5distr.gif){:width="400px"}
![p=6](/assets/yt/p6distr.gif){:width="400px"}
![p=7](/assets/yt/p7distr.gif){:width="400px"}
![p=8](/assets/yt/p8distr.gif){:width="400px"}
![p=11](/assets/yt/p11distr.gif){:width="400px"}

As an added bonus, there's also the following for $$p=11$$ and going up to $$n=74$$ (since it had taken over 9 hours to get that far)

![p=11](/assets/yt/p11extdistr.gif){:width="400px"}
## Some patterns
### $$p=2$$
The distribution looks a bit odd, however most of it can be explained readily.

In this case, the cores correspond to partitions of triangular numbers and look like a staircase. In fact, this is the only such case. So, in the distributions, you can see that the nonzero sizes of $$p$$-quotients is rather rare, with little spikes relating to triangular numbers. Also, since there is at most one $$p$$-core of a given size, it follows that the frequency of the nonzero sizes are monotone (due to the number of partitions of $$2$$ kinds being monotone as well).

### $$p=3$$
It's rather odd that there are periodic zeroes in the number of $$3$$-cores of size $$n$$. There's a [paper](https://www.ams.org/journals/tran/1996-348-01/S0002-9947-96-01481-X/S0002-9947-96-01481-X.pdf) by Granville and Ono which describes this phenomonon and why (in section 3, also mentioned on page 334). 

### $$p \ge 4$$
As the graph of $$p-$$cores of size $$n$$ suggest, it appears that both the number of $$p-$$cores increases with $$n$$, as well as the fact that as $$p$$ increases, so does the number of $$p-$$cores. There are a variety of cases that have been proved (typically Granville and Ono).

Additionally, the distrubtions suggest something else that is rather surprising, which is that as $$p$$ increases, the distribution of sizes of $$p$$-quotients goes somewhat leftward. 
# Ring Theory in Lean

## About this project

The goal of the project is to formalize some of the fundamental mathematics that is central to Algebraic Number Theory, mainly focusing on dedekind domains and many equvialent definitions, some of which included discrete valuation rings. Most of the filled-in proofs however, consist of other statements, but in doing so, provides a stronger foundation. As I started this project, I realized just how much more effort it took to prove the things that you don't even think twice in Lean. As such, I experienced several setbacks, and through the process of abstracting and learning what subgoals would lead to progress, I began to get a bit better at it (although the difficulty curve is still steep).

This project ended up in two PRs to mathlib, although they were changed significantly in order to be both shorter and to conform more to the library standards. One is about [Noetherianness](https://github.com/leanprover-community/mathlib/pull/3846/) and the other about alternative definitions of [Dedekind domains](https://github.com/leanprover-community/mathlib/pull/4000). 

This project was started during Canada/USA Mathcamp 2020 as an academic project guided by Apurva Nakade, who helped explain some of the API, answering a variety of questions from basic lean syntax to vague questions about type theory, as well as helping out code and finishing proofs. Also, big thanks to Jalex Stark, who has helped transform some hacky solutions into normal ones, as well as undertaking the monumental task of trying to optimize spaghetti code.

### About Lean

Lean is a proof assistant; in other words, it's a programming language that has a verified kernel that checks that every step that it is doing is indeed correct. There has been a large group of mathematicians who have been developing a library of mathematics (referred to as mathlib), and tries to rigorize all of modern mathematics by completing the proofs in Lean. For example, in recent years, there is the perfectoid space project which has taken something at the forefront of mathematics and proved it in Lean. As for how it works, in short, Lean uses dependent type theory, which assigns proofs and structures a type, and the way to prove something is to provide a term of that type.

### Results

The localization of a ring at $$S$$ is the set of all elements $$\{\frac{r}{s} \mid \ r \in R, \ s \in S \}$$. The set $$S$$ has some structure, which is that it is closed under multiplication (a multiplicative submonoid of R) and typically contains the multiplicative identity. An interesting result is that if $$0 \in S$$, then the localization of $$R$$ at $$S$$ is the `zero ring` which has only one element, $$0$$, and this is due to the fact that $$0$$ is assumed to be a unit, and the multiplication rule by $$0$$ still applies.

Somewhat confusingly, the localization of $$R$$ at a prime ideal $$P$$ is actually the localization of $$R$$ at $$R-P$$, which is the set-theoretic complement of $$P$$. If $$R$$ is an integral domain, this results in a `local ring`, which is closely related to one definition of Dedekind domain. I succeeded in proving that any nontrivial localization of an integral domain resulted in yet another integral domain, and as a nice corollary, got the fact that localizations at primes were also integral domains.

Another good property for rings to have, which makes them really nice is the Noetherian property with the following equivalent definitions:
1. For any chain of ideals, the inclusions $$I_1 \subseteq I_2 \subseteq I_3 \subseteq I_4 \subseteq \ldots$$ is eventually constant. In other words, there exists $$n$$ such that $$ I_{n-1} \subset I_n = I_{n+1} = \ldots $$.
2. Any ideal of $$R$$ is finitely generated (as an $$R$$-submodule).
3. Any nonempty set of ideals of $$R$$ has a maximal ideal (not contained in any other ideal of the set).
One can analogously talk about Noetherian modules (here ideals get replaced by submodules).

## Noetherian
In terms of equivalent Noetherian submodules, so far, Lean has a statement which is essentially the first is equivalent to the second, but the third is not included, and so I present to you, a proof.

So the original lean proof was essentially the following paper-pencil proof.

First, we wish to show that if any nonempty set of submodules has a maximal submodule, then it is finitely generated.
Let $$I$$ be any submodule, consider the set $$S = \{ J \ \mid \ J \subseteq I, \ J \ \textrm{is finitely generated}\}$$, this set has maximal submodule $$I$$ (as otherwise, we can indefinitely keep on throwing in more generators), and since $$I \in S$$, we have that $$I$$ is finitely generated.

Now, for the converse, we actually use the equivalent definition that for every submodule, there does not exist an infinitely ascending chain of submodule inclusions. For the sake of contradiction, suppose that there is a nonempty set such that it does not have a maximal submodule, then we can start off with some element of that set, and choose some element that contains it (this is always possible, as there is no maximal element), but since we can do this indefinitely, we have constructed an infinitely ascending chain, a contradiction.

But, the final result ended up being much shorter (literally one line of code), but required building up more theory about well foundednes in relation to posets.
```lean
theorem set_has_maximal_iff_noetherian {R M} [ring R] [add_comm_group M] [module R M] :
  (∀ a : set $ submodule R M, a.nonempty → ∃ M' ∈ a, ∀ I ∈ a, M' ≤ I → I = M') ↔ is_noetherian R M :=
by rw [is_noetherian_iff_well_founded, well_founded.well_founded_iff_has_max']
```