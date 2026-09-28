# Matroids

Companion to [08-total-unimodularity-and-matroids](08-total-unimodularity-and-matroids.md).

Exam slot E6. A full 14-point question on SS23 and SS24, and a multiple-choice item on SS25. Almost always "prove this is a matroid" or "show it is not".

---

# Part 0: What the exam asks

**SS24 P5** (14 pts):
- a) Prove that $U_1$ is a matroid.
- b) Provide an example which shows that $U_2$ is not a matroid.

**SS23 E5** (14 pts):
- a) Define a basis.
- b) Show or disprove: all bases have the same number of elements.
- c) Show or disprove: $(B_1 \cup B_2) \setminus (B_1 \cap B_2)$ is a basis.

**SS25 E1e** (MC): *"For which of these collections is $(E, I)$ a matroid?"*

You need the three axioms, the basis facts, and small counterexamples.

---

# Part 1: Why matroids

Greedy (sort by weight, add each element if nothing breaks) is optimal for some problems, e.g. Kruskal's MST. A matroid is exactly the structure where this works:

> Greedy finds the optimum for every weight function if and only if the set system is a matroid.

---

# Part 2: Vocabulary

- $E$: ground set, the finite set of elements
- $I$: a collection of subsets of $E$, the independent sets

**Graphic matroid:** $E$ = edges of a graph, a set is independent if it contains no cycle.

```
       a
      / \          E = { ab, ac, bc }
     b───c
                   I = { ∅, {ab}, {ac}, {bc}, {ab,ac}, {ab,bc}, {ac,bc} }

                   not independent: {ab, ac, bc} (a cycle)
```

A **basis** is a maximal independent set: independent and not extendable. Here the bases are the three 2-edge sets, all of size 2 (see Part 6).

---

# Part 3: The three axioms

$(E, I)$ is a matroid if:

$$
\begin{aligned}
&(1) && \emptyset \in I \\
&(2) && B \in I \text{ and } A \subseteq B \;\Rightarrow\; A \in I && \text{(hereditary)} \\
&(3) && A, B \in I \text{ with } |A| < |B| \;\Rightarrow\; \exists\, x \in B \setminus A \text{ with } A \cup \{x\} \in I && \text{(exchange)}
\end{aligned}
$$

(1) and (2) alone define an **independence system**; (3) makes it a matroid.

In axiom 3 the smaller set $A$ grows, and the new element comes from the larger set $B$.

---

# Part 4: Proving a matroid

Check all three axioms, in order:

1. $\emptyset \in I$: usually one line
2. hereditary: $A \subseteq B \in I \Rightarrow A \in I$: usually one line
3. exchange: construct $x$: the real work

## Worked example: SS24 P5a

> $T = (V, E)$ is a tree. Fix distinct nodes $s, t$. Let $I_1 = \{ F \subseteq E : F \text{ is a subset of the edges of an } s\text{–}t \text{ path in } T \}$. Prove $U_1 = (E, I_1)$ is a matroid.

**(1)** $\emptyset$ is a subset of the path's edges, so $\emptyset \in I_1$.

**(2)** If $A \in I_1$ and $B \subseteq A$, then $B$ is also a subset of the path's edges, so $B \in I_1$.

**(3)** In a tree the $s$–$t$ path is unique; call its edge set $P$. So $I_1$ is the set of all subsets of $P$. For $A, B \subseteq P$ with $|A| < |B|$ there is some $e \in B \setminus A$, and $A \cup \{e\} \subseteq P$, so $A \cup \{e\} \in I_1$.

So $U_1$ is a matroid. The key observation is that the path is unique.

"All subsets of a fixed set $P$" is always a matroid (free matroid). So is "all subsets of size at most $k$" (uniform matroid). Both are useful small examples.

---

# Part 5: Disproving a matroid

One concrete counterexample suffices. Try the axioms in this order:

1. $\emptyset \in I$? e.g. "$|S|$ odd" fails, since $|\emptyset| = 0$
2. hereditary? find $B \in I$ and $A \subseteq B$ with $A \notin I$
3. exchange? find $A, B \in I$, $|A| < |B|$, where no $x \in B \setminus A$ works

For (3) you must show every $x \in B \setminus A$ fails, so keep the example to two or three elements.

## Worked example: SS24 P5b

> $S \subseteq V$ is stable if no two nodes of $S$ are adjacent. $I_2 = \{ S \subseteq V : S \text{ stable} \}$. Show $U_2 = (V, I_2)$ is not a matroid.

$I_2$ is hereditary, so attack axiom 3.

```
      b ── a ── c
```

$V = \{a, b, c\}$, $E = \{ \{a,b\}, \{a,c\} \}$

$$
\begin{aligned}
A &= \{a\} && \text{stable} \\
B &= \{b, c\} && \text{stable (} b \text{ and } c \text{ are not adjacent)} \\
|A| &= 1 < 2 = |B| \\[4pt]
A \cup \{b\} &= \{a, b\} && \text{not stable} \\
A \cup \{c\} &= \{a, c\} && \text{not stable}
\end{aligned}
$$

No $x$ works, so axiom 3 fails and $U_2$ is not a matroid.

## SS25 multiple choice

| Collection | Verdict | Reason |
|---|---|---|
| $\{S \subseteq E : \lvert S \rvert \text{ even}\}$ | not a matroid | not hereditary: $\{a,b\} \in I$, $\{a\} \notin I$ |
| $\{S \subseteq E : \lvert S \rvert \text{ odd}\}$ | not a matroid | $\emptyset \notin I$ |
| $\{S : S \subseteq E\}$ | matroid | free matroid |
| $\{S \subseteq E : S \text{ contains no cycle}\}$ | matroid | graphic matroid |

---

# Part 6: Bases

A basis is a maximal independent set (SS23 E5a).

**All bases of a matroid have the same size (SS23 E5b).**

Proof. Suppose bases $B_1, B_2$ have $|B_1| < |B_2|$. By axiom 3 there is $e \in B_2 \setminus B_1$ with $B_1 \cup \{e\} \in I$. That set is independent and larger than $B_1$, contradicting maximality. So $|B_1| = |B_2|$.

## SS23 E5c

> Is $(B_1 \cup B_2) \setminus (B_1 \cap B_2)$ (the symmetric difference) a basis?

No. Take
$$E = \{a, b\}, \qquad I = \{ \emptyset, \{a\}, \{b\} \}$$

This is a matroid ($\emptyset \in I$; hereditary; exchange only arises for $A = \emptyset$, and adding either element works). Bases: $B_1 = \{a\}$, $B_2 = \{b\}$.

$$(B_1 \cup B_2) \setminus (B_1 \cap B_2) = \{a, b\} \notin I$$
It is not even independent.

For "show or disprove" questions about a constructed set, try the smallest matroid first.

## Rank

$$r(B) = \max\{ |A| : A \subseteq B,\ A \in I \}$$

For a connected graphic matroid, $r(E) = |V| - 1$ (a spanning tree).

---

# Part 7: Greedy

1. Sort $E$ by weight.
2. $A = \emptyset$.
3. For each element in order: add it to $A$ if $A \cup \{e\}$ is independent.

- increasing order → minimum-weight basis
- decreasing order → maximum-weight basis

Runtime: $O(n \log n + n \cdot f(n))$, where $f(n)$ = cost of one independence test.

On the graphic matroid in increasing order, this is Kruskal's algorithm. The theorem is an if and only if: for a set system that is not a matroid, some weight function makes greedy fail.

---

# Part 8: Traps and practice

## Where points are lost

1. Reversing axiom 3. The smaller set grows; the element comes from the larger.
2. Proving only exchange. Write all three axioms; (1) and (2) are marked.
3. In a disproof, not checking every $x \in B \setminus A$.
4. Counterexamples that are too big.
5. Not testing $\emptyset$ and heredity first when disproving.
6. Writing "maximum" instead of "maximal" in the basis definition.

## Recall list

$$
\begin{aligned}
&(1) && \emptyset \in I \\
&(2) && B \in I,\ A \subseteq B \Rightarrow A \in I && \text{hereditary} \\
&(3) && A, B \in I,\ |A| < |B| \Rightarrow \exists\, x \in B \setminus A : A \cup \{x\} \in I && \text{exchange}
\end{aligned}
$$

- basis = maximal independent set; all bases have equal size
- $r(B) = \max\{|A| : A \subseteq B,\ A \in I\}$
- greedy: increasing → min basis, decreasing → max basis

## Exercises (untimed)

1. `D8.3` *Matroids* `[EXAM]`: start here.
2. `T8.1` *Matroids* `[EXAM]`: the intersection of two matroids is an independence system but not in general a matroid. Same prove-and-refute shape as SS24 P5.
3. `S8.1` *TU and dual of a Matroid* `[EXAM]`: TU and matroids together.

Sheet 8: `exercises/09-integer-programming-network-flow/sheet-08-exercises.pdf` (S8.x in the second half). CE-08: `central exercises/09-integer-programming-network-flow/ce-08-demo.pdf`.

## Papers (timed)

- SS24 P5 (14): prove one, disprove the other.
- SS23 E5 (14): basis definition, equal-size proof, symmetric-difference counterexample.

A matroid is where greedy is exact; TU is where the LP relaxation is exact ([08b](08b-tu-and-knapsack-dp-lesson.md)).
