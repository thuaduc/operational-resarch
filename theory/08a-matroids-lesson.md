# Matroids — from scratch

Teaching companion to [08-total-unimodularity-and-matroids](08-total-unimodularity-and-matroids.md). That file is the reference; **this one assumes you know nothing**.

Part of exam slot **E6**. Matroids appeared as a **full 14-point question on SS23 and SS24**, and
as a multiple-choice item on SS25. Almost all of it is "prove this is a matroid" or "show it
isn't" — a fixed three-step check, not creative work.

---

# Part 0 — What the exam actually asks

**SS24 P5** (14 pts) — one of each:
```
a) Prove that U₁ is a matroid.
b) Provide an example which shows that U₂ is not a matroid.
```

**SS23 E5** (14 pts) — definitions and short proofs:
```
a) Define a basis.
b) Show or disprove: all bases have the same number of elements.
c) Show or disprove: (B₁ ∪ B₂) \ (B₁ ∩ B₂) is a basis.
```

**SS25 E1e** — multiple choice: *"For which of these collections is `(E, I)` a matroid?"*

So: **the three axioms, the basis facts, and the ability to build a small counterexample.**

---

# Part 1 — What a matroid is *for*

Some optimisation problems are solved correctly by the dumbest possible strategy: **sort by
weight, take greedily whatever doesn't break anything.** Kruskal's minimum spanning tree is like this — sort the edges, add each one if it doesn't create a cycle, done. Optimal.

For most problems greedy fails badly. So the question is: **what structure makes greedy work?**

> **A matroid is exactly that structure.** Greedy finds the optimum for *every* weight function
> if and only if the underlying set system is a matroid.

That's why the concept exists. Everything below is the definition of "greedy-friendly".

---

# Part 2 — The vocabulary

```
E    the GROUND SET — the finite pile of things you're choosing from
I    a collection of subsets of E, called the INDEPENDENT sets
```

"Independent" here has no meaning of its own. It's whatever the problem declares to be an
allowed, non-conflicting selection.

**Example — the graphic matroid.** Let `E` be the edges of a graph, and call a set of edges
independent if it contains **no cycle**. So `I` = all forests.

```
       a
      / \          E = { ab, ac, bc }
     b───c
                   I = { ∅, {ab}, {ac}, {bc}, {ab,ac}, {ab,bc}, {ac,bc} }

                   NOT independent: {ab, ac, bc}  — that's a triangle, a cycle
```

A **basis** is a **maximal** independent set — one you cannot extend without breaking
independence. Above, the bases are the three 2-edge sets. Note they all have size 2; Part 6
shows that's no accident.

---

# Part 3 — The three axioms

`(E, I)` is a **matroid** if:

```
(1)  ∅ ∈ I
     the empty selection is allowed

(2)  B ∈ I  and  A ⊆ B   ⟹   A ∈ I
     HEREDITARY / downward-closed:
     any part of an allowed selection is itself allowed

(3)  A, B ∈ I  with  |A| < |B|   ⟹   ∃ x ∈ B \ A  with  A ∪ {x} ∈ I
     EXCHANGE / augmentation:
     a smaller allowed set can always grow using something from a bigger one
```

Axioms (1) and (2) alone make an **independence system**. Adding (3) makes it a **matroid**.

## Reading axiom 3 correctly

This is where people go wrong. Watch the direction:

```
  A          the SMALLER set — this is the one that GROWS
  B          the LARGER set  — this is where the new element COMES FROM
  x ∈ B \ A  the element must be in B and not already in A
```


Intuitively: you can never get stuck at a small maximal set while a bigger one exists. Which is
precisely why greedy can't paint itself into a corner.

---

# Part 4 — Proving something IS a matroid

**Three bullet points, in order. Always all three.**

```
1.  ∅ ∈ I                                     usually one line
2.  hereditary: A ⊆ B ∈ I ⟹ A ∈ I            usually one line
3.  exchange: construct the element x          the real work
```

## Worked: SS24 P5a

> `T = (V, E)` is a **tree**. Fix two distinct nodes `s, t`. Let
> `I₁ = { F ⊆ E : F is a subset of the edges of an s–t path in T }`.
> Prove `U₁ = (E, I₁)` is a matroid.

**(1)** `∅ ∈ I₁` — the empty set is a subset of the edges of the `s–t` path, vacuously. ✓

**(2)** Let `A ∈ I₁` and `B ⊆ A`. Then `A` is a subset of the path's edges, so `B` is too.
Hence `B ∈ I₁`. ✓

**(3)** Let `A, B ∈ I₁` with `|A| < |B|`. **In a tree the path between two nodes is unique** —
call its edge set `P`. So `I₁` is just *all subsets of `P`*. Since `A, B ⊆ P` and `|A| < |B|`,
there is some `e ∈ B \ A`, and `A ∪ {e} ⊆ P`, so `A ∪ {e} ∈ I₁`. ✓

Therefore `U₁` is a matroid. ∎

**The key sentence is "the path is unique".** That collapses `I₁` into "all subsets of one fixed
set", which makes every axiom trivial. Finding that observation *is* the question.

> A set system of the form "all subsets of a fixed set `P`" is always a matroid. So is "all
> subsets of `E` of size at most `k`". These are the **uniform matroids**, and they're your
> go-to source of small examples.

---

# Part 5 — Disproving it

You need **one** concrete counterexample. Try the axioms in this order — the earlier ones are
cheaper to break:

```
1.  Is ∅ ∈ I?              e.g. "|S| is odd" fails instantly, since |∅| = 0 is even
2.  Is it hereditary?      find B ∈ I and A ⊆ B with A ∉ I
3.  Does exchange fail?    find A, B ∈ I, |A| < |B|, where NO x ∈ B \ A works
```

For (3) you must check **every** `x ∈ B \ A` and show each fails. That's why you keep the example
tiny — two or three elements.

## Worked: SS24 P5b

> `S ⊆ V` is **stable** if no two nodes of `S` are joined by an edge. Let
> `I₂ = { S ⊆ V : S is stable }`. Show `U₂ = (V, I₂)` is **not** a matroid.

Note that `I₂` *is* hereditary — a subset of a stable set is stable. So axiom 2 won't break;
attack axiom 3.

**Counterexample.** A three-node path:
```
      b ── a ── c            V = {a, b, c},  E = { {a,b}, {a,c} }
```
```
A = {a}       stable ✓  (single node, no edge inside)
B = {b, c}    stable ✓  (b and c are NOT adjacent — no edge between them)
|A| = 1 < 2 = |B|
```
Now check every candidate in `B \ A = {b, c}`:
```
A ∪ {b} = {a, b}   — but {a,b} ∈ E, so NOT stable  ✗
A ∪ {c} = {a, c}   — but {a,c} ∈ E, so NOT stable  ✗
```
No `x` works, so axiom 3 fails and `U₂` is not a matroid. ∎

**Three nodes, two edges.** That's all it takes. Note how `a` is the hub: it blocks both
candidates at once.

## The SS25 multiple-choice item, resolved

> For which collections `I ⊆ {S : S ⊆ E}` is `(E, I)` a matroid?

| Collection | Verdict | Why |
|---|---|---|
| `I = {S ⊆ E : \|S\| is even}` | ✗ | not hereditary — `{a,b} ∈ I` but `{a} ∉ I` |
| `I = {S ⊆ E : \|S\| is odd}` | ✗ | `∅ ∉ I` — fails axiom 1 immediately |
| `I = {S : S ⊆ E}` (everything) | ✓ | all three trivially; the **free matroid** |
| `I = {S ⊆ E : S contains no cycle}` | ✓ | the **graphic matroid** |

Both false options die on axioms 1 or 2 — you never even reach exchange.

---

# Part 6 — Bases, and why they're all the same size

**A basis is a maximal independent set** — independent, and not contained in any larger
independent set. (SS23 E5a is exactly this one line.)

> **All bases of a matroid have the same cardinality.**

That's SS23 E5b, and the proof is four lines of pure axiom 3:

**Proof.** Suppose not. Take bases `B₁, B₂` with `|B₁| < |B₂|`. By axiom 3 there exists
`e ∈ B₂ \ B₁` with `B₁ ∪ {e} ∈ I`. But then `B₁ ∪ {e}` is independent and strictly larger than
`B₁`, contradicting the **maximality** of `B₁`. Hence `|B₁| = |B₂|`. ∎

Worth noticing: this is *why* the axiom is called the exchange or augmentation property, and it's
the fact that makes greedy terminate at the right size.

## SS23 E5c — the trap

> Is `(B₁ ∪ B₂) \ (B₁ ∩ B₂)` a basis? *(that's the symmetric difference)*

**No.** Counterexample, two elements:
```
E = {a, b}        I = { ∅, {a}, {b} }
```
This *is* a matroid (check: `∅ ∈ I` ✓; hereditary ✓; exchange — the only case is `A = ∅`,
`B = {a}` or `{b}`, and `∅ ∪ {x} ∈ I` ✓).

Its bases are `B₁ = {a}` and `B₂ = {b}`. Then:
```
(B₁ ∪ B₂) \ (B₁ ∩ B₂)  =  {a,b} \ ∅  =  {a, b}
```
But `{a,b} ∉ I` — it isn't even independent, let alone a basis. ∎

**Two elements.** When a "show or disprove" asks about a *constructed* set, try the smallest
matroid you can write down before attempting a proof.

## Rank

```
r(B) = max{ |A| : A ⊆ B, A ∈ I }
```
The size of the largest independent set inside `B`. For a connected graphic matroid,
`r(E) = |V| − 1` — a spanning tree.

---

# Part 7 — The greedy algorithm

```
1. Sort E by weight.
2. Start with A = ∅.
3. Take each element in turn; add it to A if A ∪ {e} is still independent.
4. Stop when you've been through everything.
```

```
increasing order  →  MINIMUM-weight basis
decreasing order  →  MAXIMUM-weight basis

runtime: O(n log n + n·f(n))       f(n) = cost of one independence test
```

Run it on the graphic matroid in increasing order and it *is* Kruskal's algorithm. The matroid
axioms are the reason Kruskal is correct.

The theorem is an **if and only if**: greedy is optimal for every weight function precisely when
the system is a matroid. If a set system isn't a matroid, some weight function defeats greedy.

---

# Part 8 — Traps and drills

## Where points are lost

1. **Reversing axiom 3.** The *smaller* set grows; the element comes from the *larger*.
2. **Proving only exchange.** All three axioms, every time — (1) and (2) are one line each and
   they're marked.
3. **For a disproof, not checking every `x ∈ B \ A`.** "No `x` works" needs each one refuted.
4. **Counterexamples that are too big.** Two or three elements. SS24's uses three nodes.
5. **Skipping the cheap axioms when disproving.** Test `∅ ∈ I` and heredity first — "`|S| odd`"
   dies on axiom 1 without any thought.
6. **Not saying "maximal"** in the basis definition. Maximal, not maximum — though in a matroid
   they coincide, which is Part 6's point.

## Say these without looking

```
(1) ∅ ∈ I
(2) B ∈ I, A ⊆ B ⟹ A ∈ I                     hereditary
(3) A,B ∈ I, |A|<|B| ⟹ ∃x ∈ B\A : A∪{x} ∈ I   exchange
basis = maximal independent set; all bases equicardinal
r(B) = max{|A| : A ⊆ B, A ∈ I}
greedy: increasing → min basis, decreasing → max basis
```

## Warm-up ladder (untimed)

1. `D8.3` *Matroids* — `[EXAM]` start here.
2. `T8.1` *Matroids* — `[EXAM]` shows the intersection of two matroids is an independence system
   but **not** generally a matroid. Same prove-then-refute shape as SS24 P5.
3. `S8.1` *TU and dual of a Matroid* — `[EXAM]` covers TU and matroids together, which is
   efficient given the half-day budget.

Sheet 8 is `exercises/09-integer-programming-network-flow/sheet-08-exercises.pdf`; the self-study
section (S8.x) is in the second half of the same file. CE-08 is
`central exercises/09-integer-programming-network-flow/ce-08-demo.pdf`.

## Then the papers (timed)

- **SS24 P5** (14) — prove one, disprove the other. The model question.
- **SS23 E5** (14) — basis definition, equicardinality proof, symmetric-difference counterexample.

## Connection

A matroid is where **greedy** is exactly right. Total unimodularity is where the **LP relaxation**
is exactly right. Both are answers to "when is this combinatorial problem secretly easy?" — which
is the theme of the whole `theory/08` chapter.
