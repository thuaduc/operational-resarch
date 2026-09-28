# Total unimodularity and knapsack DP

Companion to the TU and DP parts of [08-total-unimodularity-and-matroids](08-total-unimodularity-and-matroids.md). Matroids are in [08a](08a-matroids-lesson.md).

---

# Part one: Total unimodularity

## Purpose

IPs are NP-hard in general, but for some IPs the LP relaxation already has an integral optimal vertex. Total unimodularity (TU) is a condition that guarantees this.

## Definition

$$A \text{ is TU} \;:\iff\; \text{every square submatrix of } A \text{ has determinant in } \{-1, 0, +1\}$$

With $1\times 1$ submatrices, every entry must be in $\{-1, 0, +1\}$. A single entry of 2 disproves TU.

## Why integrality follows

A vertex is $x_B = B^{-1}b$. By Cramer's rule $x_i = \det(B_i)/\det(B)$, where $B_i$ is $B$ with column $i$ replaced by $b$. TU gives $\det(B) = \pm 1$, and integral $b$ makes the numerator an integer, so $x$ is integral.

## Proving TU

**Route A: recognise the matrix.** Accepted in SS21 A5b.

$$
\begin{aligned}
&\text{incidence matrix of a bipartite graph} &&\to \text{TU} \\
&\text{incidence matrix of a directed graph} &&\to \text{TU} \\
&\text{consecutive-ones property (interval matrix)} &&\to \text{TU}
\end{aligned}
$$

**Route B: three sufficient conditions.** Write them as three numbered checks:

1. every entry is in $\{-1, 0, +1\}$
2. every column has at most 2 non-zero entries
3. the rows split into two groups $M_1, M_2$ such that for each column with two non-zeros:
    - same sign $\to$ the two rows are in different groups
    - opposite signs $\to$ the two rows are in the same group

If there is no conflict, give $(M_1, M_2)$ and conclude TU. If the split conflicts, try $A^T$, $-A$ or $[A, I]$, which preserve TU.

## Disproving TU

1. Any entry outside $\{-1, 0, +1\}$? $\to$ not TU.
2. Otherwise find a square submatrix with $\lvert\det\rvert \ge 2$.
3. Show it and compute the determinant.

A $2\times 2$ submatrix containing a zero always has determinant in $\{-1, 0, 1\}$, so check the zero-free $2\times 2$ blocks first.

$$\det\begin{pmatrix} 1 & 1 \\ -1 & 1 \end{pmatrix} = 1\cdot 1 - 1\cdot(-1) = 2 \quad\to\quad \text{not TU}$$

## Closure

$$A \text{ TU} \implies -A,\; A^T,\; A^{-1},\; [A, I] \text{ all TU}$$

$[A, I]$ means adding slack variables keeps TU.

---

# Part two: Knapsack by dynamic programming

## Problem

$$\max \sum_j v_j x_j \quad \text{s.t.} \quad \sum_j w_j x_j \le W,\quad x \in \{0,1\}$$

## Table

$$B[i, w] := \text{best total value using only items } 1..i \text{ with capacity } w$$

Rows are items, columns are capacities. Row 0 and column 0 are all zeros.

## Recurrence

$$
\begin{aligned}
&\text{if } w_i \le w: && B[i,w] = \max\{\underbrace{B[i-1, w]}_{\text{skip item } i},\; \underbrace{v_i + B[i-1, w - w_i]}_{\text{take item } i}\} \\
&\text{else}: && B[i,w] = B[i-1, w] && \text{item } i \text{ does not fit}
\end{aligned}
$$

Answer: $B[n, W]$.

## Backtracking

Start at $(n, W)$:

1. $B[i,w] = B[i-1,w]$ $\to$ item $i$ not taken; go to $(i-1, w)$
2. otherwise $\to$ item $i$ taken; record it; go to $(i-1, w - w_i)$
3. Stop at $i = 0$.

## Which index

$$
\begin{aligned}
&\text{by weight:} && O(n \cdot W_{\max}) \\
&\text{by value:} && O(n \cdot V_{\max})
\end{aligned}
$$

Use the smaller bound and say why. SS23 E4a asks this directly.

Both are pseudopolynomial: $W$ is a number that takes only $\log W$ bits to write, so knapsack is still NP-hard.

## Worked example: SS23 E4

> Five records, carry limit 5 kg. Maximise value.

| record | value | weight |
|---|---|---|
| Beatles | 2 | 3 |
| Pink Floyd | 3 | 2 |
| Led Zeppelin | 4 | 4 |
| Queen | 1 | 2 |
| Nirvana | 2 | 1 |

**(a) Index.**

$$W_{\max} = 5 \qquad V_{\max} = 2+3+4+1+2 = 12 \qquad 5 < 12 \implies \text{index by weight}$$

**(b) Table.**
```
capacity:        0   1   2   3   4   5
 0 records       0   0   0   0   0   0
 + Beatles       0   0   0   2   2   2
 + Pink Floyd    0   0   3   3   3   5
 + Led Zeppelin  0   0   3   3   4   5
 + Queen         0   0   3   3   4   5
 + Nirvana       0   2   3   5   5   6
```

Two cells:

$$
\begin{aligned}
&\text{Pink Floyd } (v=3, w=2), \text{ capacity 5}: && \max\{B[1,5]=2,\; 3 + B[1,3] = 5\} = 5 \\
&\text{Nirvana } (v=2, w=1), \text{ capacity 5}: && \max\{B[4,5]=5,\; 2 + B[4,4] = 6\} = 6
\end{aligned}
$$

**(c) Items and profit.** Maximum 6, with two optima:

$$
\begin{aligned}
&\text{Led Zeppelin + Nirvana} && 4+2 = 6\text{ €}, && 4+1 = 5\text{ kg} \\
&\text{Pink Floyd + Queen + Nirvana} && 3+1+2 = 6\text{ €}, && 2+2+1 = 5\text{ kg}
\end{aligned}
$$

## FPTAS

1. $\theta = \varepsilon\cdot v_{\max} / n$
2. scale values: $v_i^* = \lfloor v_i/\theta \rfloor$ (weights and capacity unchanged)
3. run the value-indexed DP on the scaled instance
4. report the chosen items at their original values

Guarantee: $V_{\text{approx}} \ge (1-\varepsilon)\cdot V_{\text{opt}}$

$P \subseteq \text{FPTAS} \subseteq \text{PTAS} \subseteq \text{APX}$. Knapsack has an FPTAS. Max clique, set cover and max independent set are not in APX.

---

# Traps

1. Writing "by Ghouila-Houri". The lecture does not name it; write out the three conditions.
2. Checking every $2\times 2$ block instead of only the zero-free ones.
3. Forgetting "and $b$ integral" in the integrality statement.
4. Reading "integral polyhedron" as "all feasible points are integral" (false MC option, 2026 P1e). It means all vertices are integral; between two vertices lie fractional feasible points.
5. Choosing the DP index without comparing $W_{\max}$ and $V_{\max}$.
6. Giving the maximum value without the item set.
7. Calling knapsack DP polynomial.

# Recall list

$$
\begin{aligned}
&\text{TU} \iff \text{every square submatrix has } \det \in \{-1,0,+1\} \\
&\text{TU} + b \text{ integral} \implies \text{integral polyhedron} \implies \text{IP solvable as an LP} \\
&\text{incidence matrix of a bipartite or directed graph is TU} \\
&B[i,w] = \max\{B[i-1,w],\; v_i + B[i-1,w-w_i]\} \\
&O(nW) \text{ vs } O(nV)\text{: use the smaller}
\end{aligned}
$$

# Exercises and papers

```
TU  →  D8.1 Unimodularity [EXAM], then S8.1 TU + matroid dual [EXAM]
       paper: SS21 A5b
DP  →  D7.1 / T7.1 Cutting [EXAM]
       paper: SS23 E4 (14 pts)
```

`S8.1` covers TU and matroids together.

TU says when the LP relaxation is exact; a matroid says when greedy is exact ([08a](08a-matroids-lesson.md)).
