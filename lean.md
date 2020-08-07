---
#layout: page
title: Ring Theory in Lean
permalink: /lean/index
---

# About the project
The goal of the project is to formalize some of the fundamental mathematics that is central to Algebraic Number Theory, mainly focusing on dedekind domains and the many, many equvialent definitions, some of which included looking at discrete valuation rings (which coincidentally has many, many equivalent definitions). Most of the filled-in proofs however, consist of statements about other things, but in doing so, provides a stronger foundation for the future.

Special thanks to my project advisor, Apurva Nakade, who has helped explain the math some of the APIs ($$\varepsilon<0$$ documentation) are trying to capture, as well as answering a variety of questions from basic lean syntax to some really vague questions (left as comments in the code) that has made a proof magnitudes easier by using some mathlib lemma (summoned from where???). Also without the `Teaching Math to Computers` class, I would not doubt learning the basics of Lean would have much longer.

Also, big thanks to Jalex Stark, who has helped transform some really hacky solutions into ones that were less unidiomatic, as well as providing insight as to how to properly do things in Lean (such as instances which borderline on witchcraft). Not to mention the monumental task of trying to optimize spaghetti code.


## Some background

Here we deal exclusively with commutative rings with identity, as they are truth.

The localization of a ring at $$S$$ is the set of all elements $$\{\frac{r}{s} \ \| \ r \in R, \ s \in S \}$$. The set $$S$$ has some structure, which is that it is closed under multiplication (a multiplicative submonoid of R) and typically contains the multiplicative identity. An interesting result is that if $$0 \in S$$, then the localization of $$R$$ at $$S$$ is the `zero ring` which has only one element, $$0$$, and this is due to the fact that $$0$$ is assumed to be a unit, and the multiplication rule by $$0$$ still applies.

Somewhat confusingly, the localization of $$R$$ at a prime ideal $$P$$ is actually the localization of $$R$$ at $$R-P$$, which is the set-theoretic complement of $$P$$ (although the motivation is probably a mixture of laziness and wanting an interesting localization). If $$R$$ is an integral domain, this results in a `local ring` which has the property that the only nonzero prime ideal is $$P$$, and is of particular interest to the theory of algebraic geometry (although this is beyond me). More importantly, one of the definitions of a dedekind domain is that the localization at any nonzero prime is a discrete valuation ring (which is a special type of local ring).

Another good property for rings to have, which makes them really nice is the Noetherian property, which yet again, has many definitions. Fortunately, they mainly manifest in the following:
1. For any chain of ideals, the inclusions $$I_1 \subseteq I_2 \subseteq I_3 \subseteq I_4 \subseteq \ldots$$ is eventually constant. In other words, there exists $$n$$ such that $$ I_{n-1} \subset I_n = I_{n+1} = \ldots $$.
2. Any ideal of $$R$$ is finitely generated (as an $$R$$-submodule).
3. Any nonempty set of ideals of $$R$$ has a maximal ideal (not contained in any other ideal of the set).
One can analogously talk about Noetherian modules (here ideals get replaced by submodules).

## Code

### Localizations of integral domains
The following is a proof that any nontrivial localization of an integral domain, $$R$$, is yet another integral domain. Intuitively, this makes sense, as when we divide out by the things in $$S$$, we do not introduce zero divisors (also here we require that $$0 \nin S$$, as otherwise this would break rule 4 >! Also the zero ring is not an integral domain, as there does exist at least two distinct elements).

```lean
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
```

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

