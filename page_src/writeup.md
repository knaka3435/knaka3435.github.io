---
layout: page
title: MIT Coding Portfolio Documentation
permalink: /writeup
---
# Important note
This write-up is best enjoyed on my [github website](https://mushokunosora.github.io/writeup/index.html) (as the gifs are actually animated). Admittedly, this is a bit of an odd setup, however, I believe that this medium is best suited for containing all of the relevant information as well as including some short animated gifs related to the Young Tableaux project (although a few are also posted on SlideRoom).

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

# Ring Theory in Lean

## About this project

The goal of the project is to formalize some of the fundamental mathematics that is central to Algebraic Number Theory, mainly focusing on dedekind domains and the many, many equvialent definitions, some of which included looking at discrete valuation rings (which coincidentally has many, many equivalent definitions). Most of the filled-in proofs however, consist of statements about other things, but in doing so, provides a stronger foundation for the future. As I started this project, I realized just how much more effort it took to prove the things that you don't even think twice in Lean. As such, I experienced several setbacks, and through the process of abstracting and learning what subgoals would lead to progress, I began to get a bit better at it (although the difficulty curve is still steep).

This project ended up in two PRs to mathlib, although they were changed significantly in order to be both shorter and to conform more to the library standards. One is about [Noetherianness](https://github.com/leanprover-community/mathlib/pull/3846/) and the other about alternative definitions of [Dedekind domains](https://github.com/leanprover-community/mathlib/pull/4000). 

This project was started during Canada/USA Mathcamp 2020 as an academic project guided by Apurva Nakade, who helped explain some of the API, answering a variety of questions from basic lean syntax to vague questions about type theory, as well as helping out code and finishing proofs. Also, big thanks to Jalex Stark, who has helped transform some hacky solutions into normal ones, as well as undertaking the monumental task of trying to optimize spaghetti code.

### About Lean

Lean is a proof assistant; in other words, it's a programming language that has a verified kernel that checks that every step that it is doing is indeed correct. There has been a large group of mathematicians who have been developing a library of mathematics (referred to as mathlib), and tries to rigorize all of modern mathematics by completing the proofs in Lean. For example, in recent years, there is the perfectoid space project which has taken something at the forefront of mathematics and proved it in Lean. As for how it works, in short, Lean uses dependent type theory, which assigns proofs and structures a type, and the way to prove something is to provide a term of that type.

### Some background

Here we deal exclusively with commutative rings with identity, as they are truth.

The localization of a ring at $$S$$ is the set of all elements $$\{\frac{r}{s} \mid \ r \in R, \ s \in S \}$$. The set $$S$$ has some structure, which is that it is closed under multiplication (a multiplicative submonoid of R) and typically contains the multiplicative identity. An interesting result is that if $$0 \in S$$, then the localization of $$R$$ at $$S$$ is the `zero ring` which has only one element, $$0$$, and this is due to the fact that $$0$$ is assumed to be a unit, and the multiplication rule by $$0$$ still applies.

Somewhat confusingly, the localization of $$R$$ at a prime ideal $$P$$ is actually the localization of $$R$$ at $$R-P$$, which is the set-theoretic complement of $$P$$ (although the motivation is probably a mixture of laziness and wanting an interesting localization). If $$R$$ is an integral domain, this results in a `local ring` which has the property that the only nonzero prime ideal is $$P$$, and is of particular interest to the theory of algebraic geometry (although this is beyond me). More importantly, one of the definitions of a dedekind domain is that the localization at any nonzero prime is a discrete valuation ring (which is a special type of local ring).

Another good property for rings to have, which makes them really nice is the Noetherian property, which yet again, has many definitions. Fortunately, they mainly manifest in the following:
1. For any chain of ideals, the inclusions $$I_1 \subseteq I_2 \subseteq I_3 \subseteq I_4 \subseteq \ldots$$ is eventually constant. In other words, there exists $$n$$ such that $$ I_{n-1} \subset I_n = I_{n+1} = \ldots $$.
2. Any ideal of $$R$$ is finitely generated (as an $$R$$-submodule).
3. Any nonempty set of ideals of $$R$$ has a maximal ideal (not contained in any other ideal of the set).
One can analogously talk about Noetherian modules (here ideals get replaced by submodules).

## Code

### Localizations of integral domains
The following is a proof that any nontrivial localization of an integral domain, $$R$$, is yet another integral domain. Intuitively, this makes sense, as when we divide out by the things in $$S$$, we do not introduce zero divisors (also here we require that $$0 \not\in S$$, as otherwise this would be the zero ring, which is not an integral domain).

{% highlight Lean %}
theorem local_id_is_id (S : submonoid R) (zero_non_mem : ((0 : R) ∉  S)) {f : localization_map S (localization S)} : 
  is_integral_domain (localization S) :=
begin
  --the `split` tactic allows us to take (condition 1) AND (condition 2) AND ... and lets us instead prove (condition 1), then (condition 2), etc. 
  split,
    { --Here we prove that this is not the zero ring by way that there exists two not equal elements.
      --We claim that the images of 0 and 1 in the localization are distinct
      use f.to_fun 1,
      use f.to_fun 0,
      --Proofs of the form not(P) are implemented in lean by saying that a proof of P implies a proof of false
      --Here we assume that 1 = 0.
      intro one_eq_zero, 
      --This is the statement that since 1 = 0 in the localization, then there exists an element c of S (and of R) such that 1*c = 0*c, 
      have h2 := (localization_map.eq_iff_exists f).1 one_eq_zero,
      --We can then reason about this value of c, and the equation `h2` which is 1*c = 0*c
      cases h2 with c h2,
      --Evidently, c = 0.
      rw [zero_mul, one_mul] at h2,
      --Since 0 is not a member of S, then c = 0, is not either
      rw ← h2 at zero_non_mem,
      --But this is a contradiction, since c is in S.
      exact zero_non_mem c.property },
    { exact mul_comm }, -- the proof that multiplication is commutative in the localization has already been done
    { --Now, we have to prove the fundamental property of integral domains, if x * y = 0, then x = 0 or y = 0.
      intros x y mul_eq_zero,
      --Since the mapping from R x S to the localization of S is surjective, there exists a,b in R x S such that x * a_2 = a_1 (clearing denominator of x)
      cases f.surj' x with a akey,
      cases f.surj' y with b bkey,
      --here we prove that x*(a_2)*y*(b_2) = 0, which follows since x*y=0.
      have h1 : x * (f.to_fun( a.snd)) * y * (f.to_fun(b.snd))= 0,
      { rw [mul_assoc x, ← mul_comm y, ← mul_assoc, mul_eq_zero], simp },
      --But, we can write this as a_1*b_1 = 0 (still in the localization of S).
      rw [akey, mul_assoc, bkey, ← f.map_mul', ← f.map_zero'] at h1,
      --And using the fact that if r = s in the localization, there exists c in S such that c * r = c * s (in R)
      rw f.eq_iff_exists' at h1,
      --Here we reason about this value of c, and the equation c*a_1*b_1=0
      cases h1 with c h1, 
      rw [zero_mul, mul_comm] at h1,
      --And since R is an integral domain, either c=0, or c*a_1*b_1 = 0.
      have h2 := eq_zero_or_eq_zero_of_mul_eq_zero h1,
      --Suppose (for sake of contradiction) that c = 0.
      cases h2 with c_eq_zero h2,
      { exfalso, --this is latin for "this is false"
        -- Since c = 0 is an element of S, and 0 is not in S, we have a contradiction
        rw ← c_eq_zero at zero_non_mem,
        exact zero_non_mem c.property },
      --Now, we're left with the case that a_1*b_1 = 0, which means that either a_1 = 0, or b_1 = 0 (also the word salad below is stating that 0/s = 0 for some s in S).
      replace h2 := eq_zero_or_eq_zero_of_mul_eq_zero h2,
      cases h2 with a_eq_zero b_eq_zero,
      { left, rw a_eq_zero at akey,
        exact localization_map.eq_zero_of_fst_eq_zero f akey rfl },
      { right, rw b_eq_zero at bkey,
        exact localization_map.eq_zero_of_fst_eq_zero f bkey rfl },
    },
end
{% endhighlight %}

As an immediate corollary, we have the fact that the localization at a prime ideal P, is also an integral domain.
```lean

instance local_at_prime_of_id_is_id (P : ideal R') (hp_prime : P.is_prime) : 
  integral_domain (localization.at_prime P) :=
begin
  --Since 0 is in P, we must have that 0 is not in the complement of P.
  have zero_non_mem : (0 : R') ∉ P.prime_compl,
  { have := ideal.zero_mem P, simpa },
  --And now we have a nice proof!
  have h1 := local_id_is_id R' P.prime_compl zero_non_mem,
  exact is_integral_domain.to_integral_domain (localization.at_prime P) h1,
  exact localization.of (ideal.prime_compl P),
end
```
### Noetherian Submodules

In terms of equivalent Noetherian submodules, so far, Lean has a statement which is essentially the first is equivalent to the second, but the third is not included.

I have found a truly marvelous proof that if $$X$$ is a module over $$R$$, and for every set of submodules of $$X$$, there exists a maximal submodule, then $$X$$ is a Noetherian module, which the margins of this website is too small to contain.

(although the converse is a work in progress)
### Misc.
The zero ideal is a prime ideal! This was fairly surprising that it hadn't been proven so far in Lean, but then again, most of the theorems aren't about integral domains.
```lean
--Lean has a more developed `lattice structure` which is a poset whose relation is inclusion. The weird symbol `bot` is the bottom of the lattice of submodules/ideals. 
lemma zero_prime [integral_domain R'] : (⊥ : ideal R').is_prime :=
begin
  split,
  { --Proving that the zero ideal is not the entire ring
    intro,
    --Suppose that it is.
    have h1 := (ideal.eq_top_iff_one) (⊥ : ideal R'),
    --Then since it contains the entire ring, it must also contain the element 1.
    rw h1 at a,
    --Since it is the zero ideal, we must also have that 1 = 0
    have : 1 = (0 : R'), tauto,
    --But this contradicts the fact that R is an integral domain
    simpa,
  },
  { --Proving that whenever x * y is in (0), then either x or y is in (0).
    intros,
    --Since it is the zero ideal, x * y must be 0.
    have h1 : x * y = 0, tauto,
    --Since it is an integral domain, either x or y must be zero.
    have x_or_y0 : x = 0 ∨ y = 0,
    exact zero_eq_mul.mp (eq.symm h1),
    --Evidently, either x or y must be in (0).
    tauto,
  },
end
```
## Noetherian

So the original lean proof is as follows, and it is essentially the following paper-pencil proof.

First, we wish to show that if any nonempty set of submodules has a maximal submodule, then it is finitely generated.
Let $$I$$ be any submodule, consider the set $$S = \{ J \ \mid \ J \subseteq I, \ J \ \textrm{is finitely generated}\}$$, this set has maximal submodule $$I$$ (as otherwise, we can indefinitely keep on throwing in more generators), and since $$I \in S$$, we have that $$I$$ is finitely generated.

Now, for the converse, we actually use the equivalent definition that for every submodule, there does not exist an infinitely ascending chain of submodule inclusions. For the sake of contradiction, suppose that there is a nonempty set such that it does not have a maximal submodule, then we can start off with some element of that set, and choose some element that contains it (this is always possible, as there is no maximal element), but since we can do this indefinitely, we have constructed an infinitely ascending chain, a contradiction.

```lean
theorem set_has_maximal_iff_noetherian {R'' : Type u} [ring R''] {X : Type u} [add_comm_group X] [module R'' X] : (∀(a : set $ submodule R'' X), a.nonempty → ∃ (M ∈ a), ∀ (I ∈ a), M ≤ I → I=M) ↔ is_noetherian R'' X := 
begin
  split; intro h,
  { --want to show that every submodule is finitely R-generated given that every nonempty set of submodules has a maximal one
    split,
    --let I be any submodule of X
    intro I,
    --Consider the set of all finitely generated submodules contained in I
    let S := {J : submodule R'' X | J ≤ I ∧ J.fg},
    --We claim that S is in fact nonempty, and contains the 0-submodule (⊥)
    have h2 : S.nonempty, { use (⊥ : submodule R'' X), convert submodule.fg_bot, simp },
    --Since S is nonempty, there exists a submodule, M, that is maximal in S
    --such that M ≤ I and M is finitely generated
    rcases h S h2 with ⟨ M, ⟨hMI, ⟨Mgen, hMgen⟩⟩, max⟩,
    --It suffices to find a finite set that spans I.
    rw submodule.fg_def,
    --We contrapose, and now we have that there does not exist a finite spanning set for I
    --and we wish to prove that there exists a submodule J such that J is in S, and J > M,
    contrapose! max,
    --since M is finitely generated, this is fairly obvious (lean is not as easy to convince)
    have : ∃ x ∈ I, x ∉ M,
    { have := max ↑Mgen (finset.finite_to_set Mgen), 
      contrapose! this, 
      rw hMgen, ext, tauto },
    --now, let's talk about this x!
    rcases this with ⟨x, hxI, hxM⟩,
    --we can make a larger submodule by using the generators of M and x.
    use submodule.span R'' (↑Mgen ∪ {x}), split,
    { --we prove that it is indeed in S.
      split,
      { --it suffices to show that all of the generators are elements of I, since the span of subsets is monotonic
        suffices : (↑Mgen : set X) ∪ {x} ⊆ I, { convert submodule.span_mono this, simp },
        --we verify that all of them are elements
        have : (↑Mgen : set X) ⊆ M, { convert submodule.subset_span, cc },
        apply set.union_subset, { exact set.subset.trans this hMI }, { simp [hxI] } },
      --since the union of finite sets are also finite, we have that it is finitely generated.
      { rw submodule.fg_def, use (↑Mgen ∪ {x}), split, { split, apply_instance,}, refl } },
    split, 
    -- M ≤ M+(x) since the generators of M are a subset of the generators of M+(x)
    { rw ← hMgen, convert submodule.span_mono _, simp },
    -- finally, we have that M ≠ M+(x) because otherwise, x ∈ M, contrary to assumption.
    { contrapose! hxM, rw ← hxM, apply submodule.subset_span, exact (↑Mgen : set X).mem_union_right rfl,} },
  { --Now, we prove the converse.
    --let A be some set with a ∈ A.
    rintros A ⟨a, ha⟩, 
    --we have that a module is notherian if and only if it > is well founded
    rw is_noetherian_iff_well_founded at h, 
    --more specifically, there cannot be an infinitely descending sequence.
    rw rel_embedding.well_founded_iff_no_descending_seq at h,
    --suppose that there is no maximal element of A, then for all elements of A
    --there exists one that is strictly larger than it.
    by_contra hyp,
    push_neg at hyp,
    --we show that this conflicts with the fact that there are no infinitely descending sequences.
    apply h,
    constructor,
    --we rephrase the earlier condition
    have h' : ∀ (M : submodule R'' X), M ∈ A → (∃ (I : submodule R'' X), I ∈ A ∧ M < I),
    {
      intros m mina,
      rcases hyp m mina with ⟨I, iina, mlei, mneqi⟩,
      use I, split, exact iina, split, exact mlei, intro ilem, apply mneqi, exact le_antisymm ilem mlei,
    },
    have h'' : ∀ M : A, ∃ I : A, (M : submodule R'' X) < I,
    { rintros ⟨M, M_in⟩,
      rcases h' M M_in with ⟨I, I_in, hMI⟩,
      exact ⟨⟨I, I_in⟩, hMI⟩ },
    --we recursively construct the chain by using dependent choice
    --after the nth element, we choose some element above it, and so on.
    let f : ℕ → A := λ n, nat.rec_on n ⟨a, ha⟩ (λ n M, classical.some (h'' M)),
    exact rel_embedding.nat_gt (coe ∘ f) (λ n, classical.some_spec (h'' $ f n)),
  },
end
```

But, the final result ended up being much shorter (literally one line of code), and ended up requiring to build up a bit more theory about well foundednes in relation to partially ordered sets.
```lean
theorem set_has_maximal_iff_noetherian {R M} [ring R] [add_comm_group M] [module R M] :
  (∀ a : set $ submodule R M, a.nonempty → ∃ M' ∈ a, ∀ I ∈ a, M' ≤ I → I = M') ↔ is_noetherian R M :=
by rw [is_noetherian_iff_well_founded, well_founded.well_founded_iff_has_max']
```