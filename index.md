---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
permalink: /
layout: home
---

Hello! Currently this website hasn't been fleshed out much, but check out the following pages!

## Projects

[Mathcamp 2020 Project - Ring theory in Lean](lean/index.html): The initial goal was to get somewhere in striking distance of proving quadratic reciprocity using the cycotomic fields and the splitting conditions, however, this proved to be extremely hard because, well... the crucial theorems haven't been proven, and the theorems were not able to be proven yet because the definitions they depend on haven't been formalized, and well, you get the idea. What ended up happening was that I found that instead of proving some really big theorems that were rather narrow in scope, I had to prove several lemmas that were more general, and in the process, found that I had to learn quite a few new things.

[Young Tableaux - p-core and p-quotients explorer \[WIP\]](ytcore/index.html): There had been a few particularly interesting problem that my MIT PRIMES reading group had been looking at for awhile that involved how many partitions of $$n$$ had an odd number of standard young tableaux, as well as other problems like how many partitions of $$2n$$ were able to be tiled by dominoes. The first problem's answer was that for $$n=2^{k_1}+2^{k_2}+2^{k_3}+\ldots+2^{k_\ell}$$ (as the binary representation of $$n$$), there were exactly $$2^{k_1+k_2+\ldots+k_\ell}$$ partitions, which is a very strange number. Anyways, in order to solve those problems, the theory of p-cores and p-quotients was necessary, and to get a better intuition and form hypothesis, more computer-generated data was nice.

## Math
### Notes
[Representations of the Symmetric Group \[WIP\]](symmrepth/index.html): The main philosophy of representation theory is to use linear algebra, which we know a lot about, in order to understand something that we don't know much about, groups. There are a great number of surprising theorems in representation theory, but some of the more interesting ones can be applied to young tableaux through the symmetric group. In particular, there is a theorem, based off of modular representations of $$S_n$$, which states that there exists a bijection between partitions of $$n$$ and $$p$$-cores of size $$k$$ and tuples of $$p$$ partitions with total size $$\frac{n-k}{p}$$, although this will require a good understanding of modular representation theory, and in turn, require a great deal of understanding of traditional representation theory. These notes are by no means comprehensive, and mostly contains a brief summary and some intuition behind it. Mainly based off of Sagan's "The Symmetric Group" and Steinberg's "Representations of Finite Groups" but may have interludes/elements from other sources.

[Algebraic Number Theory \[WIP\]](algnt/index.html): Fermat's Last Theorem has a tantalizing theorem statement, and since the $$n$$th cyclotomic field is a very natural field to consider solutions, a majority of the early attempts to prove this had been based on this, and it was possible to prove that a number of these were unique factorization domains for the ring of integers. However, as the history of the problem suggests, there was a major flaw: unique factorization failed for not just a few, but all primes $$p \ge 23$$. So, Kummer had suggested ideals, and ideal factorization, but this was still not enough. This reading project on Marcus's "Number Fields" was to get a better grasp of the general field, which is fairly clean, as well as to see why exactly this hadn't been enough to finally prove it. This was originally started during Mathcamp 2019, but a majority of the progress has been made during the following year or so. Additionally, some content from Neukirch serves as further, much-needed background.

### Selected Problems
This is mostly just a collection of problems that I thought were particularly nice, had a novel idea to them, or were particularly instructive.

## Miscellaneous
In other words, a giant TODO list.
### Reading List
* Les Miserables in original French by Victor Hugo.
* L'hote in original French by Albert Camus.
* Genki I (elementary japanese textbook) by Eri Banno et al.
* (Eventually) Kokoro in original Japanese by Natsume Souseki.
* (Eventually) Haru to Shura in original Japanese by Kenji Miyazawa.
* Born a Crime by Trevor Noah.
* Pachinko by Min Jin Lee
* Later chapters of Abstract Algebra by Artin.
* Introduction to Commutative Algebra by Atiyah and Macdonald. 
* The Rising Sea by Vakil.

### Unsolved problems
Or at least problems I have not solved.
* Prove or disprove that for all odd primes $$p$$, positive $$k$$, that $$\Phi_{2p} \left( p^{p^k}\right)$$ is composite, where $$\Phi_n$$ is the $$n$$th cyclotomic polynomial. Note: not particularly important, pathologically large, just seems true and provable. 
* Prove that if $$p \mid \Phi_n(b)$$ then either $$p \mid n$$ or $$p \equiv 1 \mod n$$ by gluing together cyclotomic fields and field norms.
* Chapter 3, exercise 20 and 21 of Daniel Marcus's "Number Fields."

### Random stuff
Aside from all of the above, I also enjoy music, and I want to start practicing violin again, as well as learn some piano, although there are not nearly enough hours in the day for all of this!