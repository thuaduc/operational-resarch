# Branch and bound

Companion to [06-branch-and-bound-and-cuts](06-branch-and-bound-and-cuts.md).

Exam slot E5/P4, 13–16 points, on every past paper. Pure mechanics: no proofs, no modelling.

---

# Part 0: What the exam asks

**Graphical** (SS21 A4, SS23 E3, SS24 P3). A 2-variable IP with the LP relaxation and objective already plotted.

> SS24 P3: *"Note: In the following coordinate systems, the feasible set of the LP relaxation of (P) and the objective function are shown. You can use them to solve the subproblems graphically."*

The task: *"Solve the integer linear program (P) using the branch-and-bound method. Specify the optimal values for $x_1$ and $x_2$, as well as the optimal value of the objective function."*

**Reconstruction** (SS25 E5). The tree is drawn with blanked-out boxes, and you get a table of LP relaxations including decoys. Recover the nodes, give the optimum, state the FIFO and LIFO orders.

**Knapsack** (2026 P4, 15 cr). B&B on a knapsack, nodes solved by the greedy ratio rule, no plot. Also asked what an unbounded relaxation tells you (Part 2).

| Skill | Appears in | Priority |
|---|---|---|
| solving subproblems graphically | SS21, SS23, SS24 | highest |
| pruning rules, correct direction | all | highest |
| knapsack nodes by greedy ratio; unbounded relaxations | 2026 P4 | high |
| tree reconstruction from a table | SS25 | high |
| FIFO / LIFO order | SS25 | moderate |

You never run simplex inside this question: nodes are solved graphically or by the ratio rule. Bring a ruler.

---

# Part 1: Why B&B

An IP's feasible set is the lattice points in a polyhedron, so simplex does not apply directly, and enumeration is hopeless (50 binaries $= 2^{50}$ candidates).

1. **Bound:** solve the relaxation to learn how good a region could be.
2. **Branch:** split the problem, and discard a piece as soon as its bound shows it cannot contain anything better.

---

# Part 2: The bound

The **LP relaxation** drops integrality ($x \in \mathbb{N}_0$ becomes $x \ge 0$). It has more feasible points than the IP, so:

$$
\begin{aligned}
&\text{maximisation:} && \mathrm{OPT(IP)} \le Z_{LP} && \text{upper bound} \\
&\text{minimisation:} && \mathrm{OPT(IP)} \ge Z_{LP} && \text{lower bound}
\end{aligned}
$$

This is what makes pruning possible.

## Rounding

With integer objective coefficients and integer variables, the objective value is an integer, so for a max problem:

$$\mathrm{OPT(IP)} \le \lfloor Z_{LP} \rfloor$$

$Z_{LP} = 14.25$ gives $\mathrm{OPT(IP)} \le 14$.

## Unbounded relaxations

If a relaxation is unbounded, $Z_{LP} = +\infty$ gives no finite bound and cannot be used for pruning.

2026 P4a: relaxing a knapsack to free (sign-unrestricted) real variables makes it unbounded: push a variable with positive weight to $-\infty$ to create unlimited capacity, then push a high-value variable to $+\infty$. So $P_{\text{free}}$ is useless for B&B, while the standard relaxation $P_{\ge 0}$ stays bounded and gives a valid finite upper bound.

$P_{\text{free}}$ drops more than integrality (it also drops $x \ge 0$), so its unboundedness says nothing about the IP. Only when integrality alone is dropped (rational data) does an unbounded LP relaxation imply the IP is infeasible or unbounded.

---

# Part 3: Branching

For a fractional value $x_i = f$:

$$
\begin{aligned}
&\text{child 1:} && x_i \le \lfloor f \rfloor \\
&\text{child 2:} && x_i \ge \lceil f \rceil
\end{aligned}
$$

```
      x₁ = 2.25  is fractional
             │
      ┌──────┴──────┐
   x₁ ≤ 2        x₁ ≥ 3
```

No integer point is lost, because there is no integer between 2 and 3. The fractional optimum is excluded from both children, so each child's relaxation returns something different and no better.

Each child inherits all constraints from its ancestors.

---

# Part 4: Incumbent and pruning

## The incumbent $Z^*$

The value of the best integral solution found so far. It starts at $-\infty$ for max ($+\infty$ for min).

For max, $Z^*$ is a lower bound on the optimum, and the best open relaxation bound $Z'$ is an upper bound:

$$Z^* \le \mathrm{OPT(IP)} \le Z'$$

When they meet, you are done.

## Three ways to close a node

| # | Rule | Maximisation | Minimisation |
|---|---|---|---|
| 1 | integrality | LP solution integral: candidate; update $Z^*$ if $Z > Z^*$ | update if $Z < Z^*$ |
| 2 | infeasibility | relaxation has no feasible point | same |
| 3 | bound | $Z_{\text{node}} \le Z^*$ | $Z_{\text{node}} \ge Z^*$ |

Rule 3 for max: $Z_{\text{node}}$ is the best value possible anywhere in that subtree. If it does not beat $Z^*$, nothing below can.

For a min problem every comparison flips. Write MAX or MIN at the top of your answer.

Rule 1 also closes the node: an integral relaxation solution is the best that subtree can do.

Ties: $Z_{\text{node}} = Z^*$ prunes. Say so when you use it.

---

# Part 5: Solving a node on the plot

Every branch constraint is a vertical or horizontal line.

1. Start from the plotted LP relaxation.
2. Draw each branch constraint on the path to the node:
    - $x_1 \le 2$ → vertical line at $x_1 = 2$, keep the left side
    - $x_1 \ge 3$ → vertical line at $x_1 = 3$, keep the right side
    - $x_2 \le 1$ → horizontal line at $x_2 = 1$, keep below
    - $x_2 \ge 2$ → horizontal line at $x_2 = 2$, keep above
3. Constraints accumulate. Shade what satisfies all of them.
4. Empty region → prune by infeasibility.
5. Otherwise slide the objective line in the improving direction until it last touches the region. Read the vertex and compute $Z$.

The optimum is at a vertex, usually where a branch line meets an original constraint. Solve the $2 \times 2$ system if the picture is unclear.

## Which variable to branch on

Only SS25 states a rule (*"whenever a branching choice was ambiguous, he chose to branch on the first variable"*). Otherwise any choice is fine; state it and stay consistent.

---

# Part 6: A complete tree

$$
\begin{aligned}
\max\ & 3x_1 + 2x_2 \\
\text{s.t. } & 2.4x_1 + x_2 \le 9.15 \\
& x_1 + x_2 \le 6 \\
& x_1, x_2 \in \mathbb{N}_0
\end{aligned}
$$

**$P_0$ (root).** $x = (2.25, 3.75)$, $Z_0 = 14.25$. Fractional, $Z^* = -\infty$, and $\mathrm{OPT} \le 14$. Branch on $x_1$.

**$P_1$: $x_1 \le 2$.** $x = (2, 4)$, $Z = 14$. Integral: rule 1. $Z^* = 14$.

**$P_2$: $x_1 \ge 3$.** $x = (3, 1.95)$, $Z = 12.90$. $12.90 \le 14$: prune by bound.

No open nodes left:

$$x^* = (2, 4), \quad Z^* = 14$$

---

# Part 7: FIFO vs LIFO

Node selection decides which open node to process next. Only SS25 has asked about it.

| | FIFO | LIFO |
|---|---|---|
| also called | breadth-first | depth-first |
| structure | queue: take the oldest | stack: take the newest |
| behaviour | finish a level before going deeper | dive, then backtrack |
| first incumbent | may come late | found quickly |
| warm start | re-solve | reoptimise with dual simplex |

LIFO finds an integral solution early, so bound pruning starts sooner.

## Reading the order off a tree

1. Start at $P_0$.
2. FIFO: level by level, left to right.
3. LIFO: always continue with the most recently created node; when a node closes, go back to the deepest unexplored node.
4. Stop as soon as every remaining open node can be pruned. SS25 says "stopped as soon as the optimal solution was confirmed", so an early stop is expected.

---

# Part 8: Reconstructing an obscured tree (SS25)

1. $P_0$ is the row with "–" (no constraints added).
2. From the parent's fractional variable and value, the two edge labels are $x_i \le \lfloor f \rfloor$ and $x_i \ge \lceil f \rceil$.
3. A node's constraint set = parent's set + its own edge label. Find the table row with exactly that set.
4. Checks:
    - a child's $Z$ is never better than its parent's
    - contradictory constraints $\Rightarrow$ "Infeasible"
    - decoy rows have constraint sets that fit nowhere on the tree

Read the stated conventions. SS25: *"whenever a branching choice was ambiguous, he chose to branch on the first variable"* and *"he assigned greater-than-or-equal constraints to right-hand nodes"*.

---

# Part 9: Worked example, SS25 E5

Given: $P_0$: $x = (2.25, 3.75)$, $Z_0 = 14.25$, branch on $x_1$. $P_3$: $x = (3.4, 1)$, $Z_3 = 12.19$, branch on $x_1$. Edges $P_2 \to P_3$ is $x_2 \le 1$, $P_2 \to P_4$ is $x_2 \ge 2$.

| Node | Constraints | $x_1$ | $x_2$ | $Z$ |
|---|---|---|---|---|
| $P_A$ | – | 2.25 | 3.75 | 14.25 |
| $P_B$ | $x_1 \le 2$ | 2.00 | 4.00 | 14.00 |
| $P_C$ | $x_1 \ge 3$ | 3.00 | 1.95 | 12.90 |
| $P_H$ | $x_1 = 3,\ x_2 \le 1$ | 3.00 | 1.00 | 11.00 |
| $P_J$ | $x_1 \ge 3,\ x_2 \le 1$ | 3.40 | 1.00 | 12.19 |
| $P_L$ | $x_1 \ge 3,\ x_2 \ge 2$ | | | Infeasible |
| $P_M$ | $x_1 \ge 4,\ x_2 \le 1$ | | | Infeasible |

## (a) Missing edge constraints

$P_0$ branches on $x_1 = 2.25$:

$$
\begin{aligned}
C_1\ (P_0 \to P_1)&:\ x_1 \le 2 \\
C_2\ (P_0 \to P_2)&:\ x_1 \ge 3
\end{aligned}
$$

$P_3$ branches on $x_1 = 3.4$:

$$
\begin{aligned}
C_5\ (P_3 \to P_5)&:\ x_1 \le 3 && \text{(with } x_1 \ge 3 \text{ inherited, this is } x_1 = 3\text{)} \\
C_6\ (P_3 \to P_6)&:\ x_1 \ge 4
\end{aligned}
$$

## (b) Match the nodes

$$
\begin{aligned}
P_1 &= \{x_1 \le 2\} && \to P_B \\
P_2 &= \{x_1 \ge 3\} && \to P_C \\
P_3 &= \{x_1 \ge 3,\ x_2 \le 1\} && \to P_J \\
P_4 &= \{x_1 \ge 3,\ x_2 \ge 2\} && \to P_L && \text{(infeasible)} \\
P_5 &= \{x_1 \ge 3,\ x_2 \le 1,\ x_1 \le 3\} && \to P_H && \text{(written } x_1 = 3,\ x_2 \le 1\text{)} \\
P_6 &= \{x_1 \ge 3,\ x_2 \le 1,\ x_1 \ge 4\} && \to P_M && \text{(infeasible)}
\end{aligned}
$$

## (c) Optimum

$P_1$ gives $(2, 4)$, $Z = 14$, integral. Every other node is infeasible or bounded below 14:

$$x^* = (2, 4), \quad Z = 14$$

## (d) Exploration order

**FIFO:**
$$P_0,\ P_1,\ P_2,\ \text{STOP}$$

$P_1$ is integral with $Z = 14$, and $P_2$'s bound 12.90 covers its whole subtree, so the optimum is confirmed.

**LIFO:**
$$P_0,\ P_2,\ P_4,\ P_3,\ P_6,\ P_5,\ P_1$$

From $P_0$ push $P_1, P_2$; take $P_2$ (newest). $P_2$ fractional: push $P_3, P_4$; take $P_4$ (infeasible). Take $P_3$, fractional: push $P_5, P_6$; take $P_6$ (infeasible). Take $P_5$: integral, $Z = 11$, $Z^* = 11$. Take $P_1$: integral, $Z = 14$, $Z^* = 14$. Stack empty.

Here LIFO visits every node and FIFO only three, because the good solution sits on the left branch.

---

# Part 10: Gomory cuts

2026 P5 (15 cr) was a full Gomory question: derive a fractional cut from the tableau row of a fractional basic variable, show all intermediate steps, and verify that the current LP optimum violates the cut.

- A **valid inequality** holds for every integer-feasible point.
- A **cutting plane** is a valid inequality violated by the current fractional LP optimum.
- The **ideal formulation** is the convex hull of the integer feasible points; all its vertices are integral.
- Cuts and B&B combine (branch and cut).

MC trap: "cutting planes remove integer solutions" is false.

The procedure, with the 2026 P5 tableau worked through, is in [06](06-branch-and-bound-and-cuts.md) §Procedures. Key points: split each coefficient into floor and fractional part with $f \in [0, 1)$ ($\lfloor -5/4 \rfloor = -2$, so $f = 3/4$); at the LP optimum the nonbasic variables are 0, so the cut reads $f_0 \le 0$, which is false; then write the relaxation with the cut added.

---

# Part 11: Traps and practice

## Where points are lost

1. Pruning in the wrong direction. SS24 P3 is a minimisation.
2. Forgetting that branch constraints accumulate.
3. Branching on a variable that is already integral.
4. Not stating which variable you branched on.
5. Missing the early stop in the SS25 format.
6. Ignoring the stated convention for which child goes right.
7. Not updating the incumbent.
8. Negative fractional parts in a Gomory cut.

## Answer format

For every node:
```
node │ constraint added │ vertex │ Z │ rule that closed it
```

## Before the papers

From blank paper:
- the three pruning rules, for max and for min
- why $x_i \le \lfloor f \rfloor \mid x_i \ge \lceil f \rceil$ loses no integer point
- FIFO and LIFO order on a 7-node tree

## Exercises (untimed)

1. `D6.1` *Branch-and-Bound* `[EXAM]`: a full worked tree. Start here.
2. `T6.1` *Knapsack Branch-and-Bound* `[EXAM]`: nodes solved by the ratio rule (2026 P4 format).
3. `S6.1` *Branch-and-Bound* `[EXAM]`: third repetition.
4. `T6.3` *Staff Scheduling* `[DRILL]`: an IP model; IP modelling revision.
5. `T6.2`, `S6.2`, `D6.2`, `D6.3`: Gomory cuts. Do at least two from a raw tableau, including one with a negative coefficient ($\lfloor -1/4 \rfloor = -1$, $f = 3/4$), and practise writing the cut in the original variables and appending the slack row for dual simplex.

Sheet 6: `exercises/07-integer-programming-solution-methods/sheet-06-exercises.pdf` (S6.x in the second half).

## Papers (timed, one minute per point)

- SS23 E3 (16): max, plot provided.
- SS24 P3 (16): minimisation, plot provided. Do it right after SS23 to feel the rule flip.
- SS21 A4 (13): max, graphical.
- SS25 E5 (14): reconstruction, FIFO/LIFO.
- 2026 P4 (15) and P5 (15): knapsack B&B, Gomory cut.

## Connections

- LP duality also gives bounds: any dual-feasible solution bounds the relaxation ([04a](04a-duality-lesson.md)).
- A stronger formulation gives a tighter bound and a smaller tree ([05a](05a-ip-modeling-lesson.md) Part 4).
- A totally unimodular constraint matrix makes the relaxation integral, so B&B ends at the root ([08](08-total-unimodularity-and-matroids.md)).
