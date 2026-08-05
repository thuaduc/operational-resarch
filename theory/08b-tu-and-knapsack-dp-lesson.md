# Total unimodularity and knapsack DP — from scratch

Teaching companion to the other two halves of
[08-total-unimodularity-and-matroids](08-total-unimodularity-and-matroids.md). Matroids are in
[08a](08a-matroids-lesson.md).

Both are **E6** blocks with a ~30-minute budget each. Mechanics only — skip every proof.

---

# PART ONE — TOTAL UNIMODULARITY

## What it's for

Integer programs are NP-hard in general. But **some** IPs are secretly easy: solve the LP
relaxation with plain simplex and the answer comes out integral anyway, for free.

TU is the condition that guarantees it.

```
A totally unimodular  +  b integral   ⟹   every vertex of {x ≥ 0 : Ax ≤ b} is INTEGRAL
                                      ⟹   the LP relaxation already solves the IP
                                      ⟹   the IP is in P
```

That sentence is the single most quoted fact from this chapter. It's why assignment and network
flow are easy while GAP and bin packing are not.

## The definition

```
A is TU  :⟺  EVERY square submatrix of A has determinant in {−1, 0, +1}
```

Immediate consequence: taking `1×1` submatrices, **every entry must itself be in `{−1,0,+1}`**.
An entry of `2` disproves TU instantly.

## Why integrality follows (one line, worth knowing)

A vertex is `x_B = B⁻¹b`. By Cramer's rule `x_i = det(Bᵢ)/det(B)`, where `Bᵢ` is `B` with column
`i` replaced by `b`. TU forces `det(B) = ±1`, and integral `b` makes the numerator an integer.
Dividing an integer by `±1` gives an integer. ∎

## How to PROVE a matrix is TU

**Route A — recognise it.** Fastest, and it's what SS21 A5b accepts:

```
incidence matrix of a BIPARTITE graph        →  TU
incidence matrix of a DIRECTED graph         →  TU
consecutive-ones property (interval matrix)  →  TU
```

> SS21's own solution says: *"The matrix is totally unimodular because it is the incidence matrix
> of a bipartite graph (this alone would suffice as justification)."*

**Route B — the three sufficient conditions.** State them as three numbered checks:

```
1.  every entry is in {−1, 0, +1}
2.  every COLUMN has at most 2 non-zero entries
3.  the rows can be split into two groups M₁, M₂ such that, for each column with two
    non-zeros:
        same sign      →  the two rows go in DIFFERENT groups
        opposite signs →  the two rows go in the SAME group
```

If no conflict arises, exhibit `(M₁, M₂)` and conclude TU.

> **Do not write "by Ghouila-Houri".** The lecture never names this criterion, so the name earns
> nothing. **Reproduce the three conditions.** (This is in the plan's corrections list.)

If the split conflicts, retry on `Aᵀ`, `−A`, or `[A, I]` — all preserve TU.

## How to DISPROVE it

```
1. Any entry outside {−1, 0, +1}?  →  done, not TU.
2. Otherwise hunt for a square submatrix with |det| ≥ 2.
3. Exhibit it, compute the determinant, conclude.
```

**Shortcut that saves time:** a `2×2` submatrix containing a zero always has determinant in
`{−1,0,+1}`. So **only zero-free `2×2` blocks can break it** — check those first.

```
⎡ 1  1 ⎤
⎢      ⎥   det = 1·1 − 1·(−1) = 2   →  NOT TU
⎣ −1 1 ⎦
```

## Closure facts

```
A TU  ⟹  −A, Aᵀ, A⁻¹, [A, I]  all TU
```

The `[A, I]` one matters: **adding slack variables never destroys TU**, which is why the
standard-form version of a TU problem is still TU.

---

# PART TWO — KNAPSACK BY DYNAMIC PROGRAMMING

## The problem

```
max Σⱼ vⱼ xⱼ    s.t.  Σⱼ wⱼ xⱼ ≤ W,   x ∈ {0,1}
```
Pick items to maximise value without exceeding a weight budget.

## The table

```
B[i, w] := the best total value using only items 1..i, with capacity w
```

Rows = items considered so far, columns = capacity available. **Row 0 and column 0 are all
zeros** — no items or no capacity means no value.

## The recurrence

For each cell, you're deciding whether to take item `i`:

```
if wᵢ ≤ w :   B[i,w] = max{  B[i−1, w]  ,   vᵢ + B[i−1, w − wᵢ]  }
                              ↑                ↑
                          skip item i      take item i, and spend wᵢ
                                           of the capacity

else      :   B[i,w] = B[i−1, w]           item doesn't fit at all
```

Answer: `B[n, W]`, the bottom-right corner.

## Backtracking — which items?

```
Start at (n, W). Then repeatedly:
   B[i,w] = B[i−1,w]   →  item i was NOT taken; move to (i−1, w)
   otherwise           →  item i WAS taken; record it, move to (i−1, w − wᵢ)
Stop at i = 0.
```

## The complexity question — it's asked directly

You can index the table by **weight** or by **value**:

```
by weight:  O(n · W_max)
by value:   O(n · V_max)
```

**Run whichever bound is smaller.** SS23 E4a asks exactly this and awards marks for the
justification.

Note this is *pseudopolynomial*, not polynomial — `W` is a number, and writing it down takes
only `log W` bits. That's why knapsack is still NP-hard.

## Worked: SS23 E4

> Five records, carry limit 5 kg. Maximise value.

| record | value | weight |
|---|---|---|
| Beatles | 2 | 3 |
| Pink Floyd | 3 | 2 |
| Led Zeppelin | 4 | 4 |
| Queen | 1 | 2 |
| Nirvana | 2 | 1 |

**(a) Which breakdown?**
```
W_max = 5        V_max = 2+3+4+1+2 = 12
5 < 12   ⟹   index by WEIGHT
```

**(b) Fill the table.** Rows = items added one at a time, columns = capacity 0…5:

```
capacity:        0   1   2   3   4   5
 0 records       0   0   0   0   0   0
 + Beatles       0   0   0   2   2   2
 + Pink Floyd    0   0   3   3   3   5
 + Led Zeppelin  0   0   3   3   4   5
 + Queen         0   0   3   3   4   5
 + Nirvana       0   2   3   5   5   6
```

Two sample cells, to see the recurrence working:
```
Pink Floyd (v=3, w=2) at capacity 5:
   max{ B[1,5]=2 ,  3 + B[1,3]=3+2=5 }  =  5    ✓ take it

Nirvana (v=2, w=1) at capacity 5:
   max{ B[4,5]=5 ,  2 + B[4,4]=2+4=6 }  =  6    ✓ take it
```

**(c) Which records, and what profit?** Maximum profit **6**. Backtracking gives two optima:
```
Led Zeppelin + Nirvana         4+2 = 6 €,   4+1 = 5 kg
Pink Floyd + Queen + Nirvana   3+1+2 = 6 €, 2+2+1 = 5 kg
```

## FPTAS, in case it's asked

```
1. θ = ε·v_max / n
2. scale values:  vᵢ* = ⌊vᵢ/θ⌋     (weights and capacity unchanged)
3. run the VALUE-indexed DP on the scaled instance
4. report the chosen items at their ORIGINAL values
Guarantee:  V_approx ≥ (1−ε)·V_opt
```

Placement in the hierarchy: `P ⊆ FPTAS ⊆ PTAS ⊆ APX`. Knapsack has an FPTAS. **Max clique, set
cover and max independent set are not in APX at all** — no constant-factor approximation exists.

---

# Traps and drills

## Where points are lost

1. **Writing "by Ghouila-Houri".** Unnamed in the lecture — reproduce the three conditions.
2. **Checking every `2×2`** instead of just the zero-free ones.
3. **Forgetting "+ integral `b`"** in the integrality statement. TU alone isn't enough.
4. **Choosing the wrong DP index.** Compare `W_max` against `V_max` and *say why*.
5. **Not backtracking.** "Maximum value 6" is half the answer; the item set is the other half.
6. **Calling knapsack DP polynomial.** It's *pseudo*polynomial.

## Say these without looking

```
TU  ⟺  every square submatrix has det ∈ {−1,0,+1}
TU + b integral  ⟹  integral polyhedron  ⟹  IP solvable in poly time
incidence matrix of a bipartite or directed graph is TU
B[i,w] = max{ B[i−1,w] , vᵢ + B[i−1,w−wᵢ] }
O(nW) vs O(nV) — run the smaller
```

## Warm-up and papers

```
TU     →  D8.1 Unimodularity [EXAM], then S8.1 TU + matroid dual [EXAM]
          paper: SS21 A5b
DP     →  D7.1 / T7.1 Cutting [EXAM]
          paper: SS23 E4 (14 pts)
```

`S8.1` covers TU and matroids together — efficient given the half-day budget.

## Connection

TU is "when is the **LP relaxation** exactly right". A matroid is "when is **greedy** exactly
right" ([08a](08a-matroids-lesson.md)). Both answer *when is this combinatorial problem secretly
easy* — the theme of the whole chapter.
