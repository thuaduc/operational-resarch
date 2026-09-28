# Duality

Companion to [04-duality](04-duality.md).

Exam slot E3/P3, 6–19 points (usually about 15), on every past paper.

---

# Part 0: What the exam asks

SS25 E3, SS24 P2, SS23 E1 and 2026 P3 share the same parts:

- a) Derive the dual linear program (D) corresponding to (P).
- b) State the primal and the dual complementary slackness conditions.
- c) Show that the primal solution $x = (\dots)$ is (not) optimal.

(a) is a mechanical transformation, (b) is writing out a formula, (c) is one fixed procedure. None of it requires solving an LP.

---

# Part 1: Where the dual comes from

$$
\begin{aligned}
(P)\quad \max\quad & 3x_1 + 2x_2 \\
\text{s.t.}\quad & x_1 + x_2 \le 4 && \text{(constraint 1)} \\
& x_1 + 3x_2 \le 6 && \text{(constraint 2)} \\
& x_1, x_2 \ge 0
\end{aligned}
$$

How large can the objective be? Multiply constraint 1 by 3: $3x_1 + 3x_2 \le 12$. Since $x_2 \ge 0$, $3x_1 + 2x_2 \le 3x_1 + 3x_2 \le 12$. So the objective never exceeds 12.

## General recipe

Weight constraint 1 by $y_1 \ge 0$ and constraint 2 by $y_2 \ge 0$ and add:

$$(y_1 + y_2) \cdot x_1 + (y_1 + 3y_2) \cdot x_2 \le 4y_1 + 6y_2$$

The weights must be non-negative, since a negative multiplier flips an inequality. That is where $y \ge 0$ comes from.

If each coefficient on the left is at least the objective coefficient,

$$
\begin{aligned}
y_1 + y_2 &\ge 3 \\
y_1 + 3y_2 &\ge 2
\end{aligned}
$$

then, because $x \ge 0$,

$$3x_1 + 2x_2 \le (y_1+y_2)x_1 + (y_1+3y_2)x_2 \le 4y_1 + 6y_2$$

So $4y_1 + 6y_2$ is an upper bound for every such $y$.

## The dual is the best such bound

$$
\begin{aligned}
(D)\quad \min\quad & 4y_1 + 6y_2 \\
\text{s.t.}\quad & y_1 + y_2 \ge 3 \\
& y_1 + 3y_2 \ge 2 \\
& y_1, y_2 \ge 0
\end{aligned}
$$

| Feature of the dual | Reason |
|---|---|
| max becomes min | we want the tightest upper bound |
| one dual variable per primal constraint | each constraint gets a weight |
| one dual constraint per primal variable | each $x_j$'s coefficient must be dominated |
| $b$ becomes the objective | the bound is $\sum_i b_iy_i$ |
| $c$ becomes the right-hand side | each $c_j$ must be dominated |
| the matrix is transposed | dual constraint $j$ uses column $j$ of $A$ |
| $y \ge 0$ | negative weights would flip inequalities |
| dual constraints are $\ge$ | coefficients must dominate the objective |
| weak duality $c^Tx \le b^Ty$ | the chain above |

The dual of the dual is the primal.

---

# Part 2: Deriving the dual mechanically

1. Switch max and min.
2. One dual variable per primal constraint. Sign restrictions ($x \ge 0$) are not constraints.
3. One dual constraint per primal variable.
4. Transpose $A$: dual constraint $j$ comes from column $j$.
5. Swap $c$ and $b$.

## The transpose

| | $x_1$ | $x_2$ | $x_3$ | |
|---|---|---|---|---|
| (1) | 1 | 2 | 1 | $\le 10$ |
| (2) | 2 | $-1$ | 2 | $\ge 4$ |
| (3) | 1 | 1 | $-1$ | $= 3$ |

Read down each column:

$$
\begin{aligned}
\text{dual constraint 1 } (x_1 \text{ column}):&\quad 1y_1 + 2y_2 + 1y_3 \;?\; c_1 \\
\text{dual constraint 2 } (x_2 \text{ column}):&\quad 2y_1 - 1y_2 + 1y_3 \;?\; c_2 \\
\text{dual constraint 3 } (x_3 \text{ column}):&\quad 1y_1 + 2y_2 - 1y_3 \;?\; c_3
\end{aligned}
$$

Copying a primal row into a dual row is the classic error.

---

# Part 3: The SOB table (mixed constraint types)

SOB is taught in the central exercise (`central exercises/05-linear-programming-duality/ce-04-slides.pdf`, slides 4–5), not in the lecture. Show the classification and translation rather than writing "by SOB".

Classify each variable and constraint as Sensible, Odd or Bizarre:

| | context | S | O | B |
|---|---|---|---|---|
| variable | any | $x \ge 0$ | $x$ free | $x \le 0$ |
| constraint | in a max problem | $\le$ | $=$ | $\ge$ |
| constraint | in a min problem | $\ge$ | $=$ | $\le$ |

Translate S $\to$ S, O $\to$ O, B $\to$ B:

| type | primal constraint $\to$ dual variable | primal variable $\to$ dual constraint |
|---|---|---|
| S | $y \ge 0$ | sensible direction |
| O | $y$ free | $=$ |
| B | $y \le 0$ | bizarre direction |

Sensible direction in the dual: $\ge$ if the dual is a min, $\le$ if it is a max.

Why a $\ge$ row in a max problem gives $y \le 0$: to use it in an upper bound you have to flip it, i.e. multiply by a negative weight.

---

# Part 4: The duality theorems

**Weak duality.** For any feasible $x$ of (P) and $y$ of (D):

$$c^Tx \le b^Ty$$

**Strong duality.** If either problem has a finite optimum, so does the other, and

$$c^Tx^* = b^Ty^*$$

## Outcome table

| | D finite | D unbounded | D infeasible |
|---|---|---|---|
| P finite | possible | impossible | impossible |
| P unbounded | impossible | impossible | possible |
| P infeasible | impossible | possible | possible |

- P unbounded $\Rightarrow$ D infeasible.
- P infeasible $\Rightarrow$ D unbounded or infeasible. Both infeasible is possible (MC trap, SS25 1a).

## Classifying without solving

*"Is (D) unbounded, infeasible, or does it have a finite optimum, without solving it?"*

1. Give one feasible point of (P) $\Rightarrow$ (D) is not unbounded (weak duality).
2. Give one feasible point of (D) $\Rightarrow$ (D) is not infeasible.
3. Both feasible $\Rightarrow$ (D) has a finite optimum (strong duality).

Often $x = 0$ or $y = 0$ is feasible.

---

# Part 5: Complementary slackness

## Derivation

At optimality the two ends of the chain are equal:
$$c^Tx^* \le (A^Ty^*)^Tx^* \le b^Ty^*$$

So both $\le$ are equalities.

First: the gap is $\sum_j ((A^Ty)_j - c_j) \cdot x_j$, a sum of non-negative terms, so each is zero:

$$((A^Ty^*)_j - c_j) \cdot x_j^* = 0 \quad \text{for each } j$$

Either $x_j = 0$ or dual constraint $j$ is tight.

Second, the same way:

$$((Ax^*)_i - b_i) \cdot y_i^* = 0 \quad \text{for each } i$$

Either $y_i = 0$ or primal constraint $i$ is tight.

In words:
- If a primal constraint is slack, its dual variable is zero.
- If a primal variable is non-zero, its dual constraint is tight.

Economically: a resource that is not fully used has shadow price zero.

## Part (b)

Write one condition per row. For SS25's (P):
| Primal CS conditions | Dual CS conditions |
|---|---|
| $(x_1 + 2x_2 + x_3 - 10) \cdot y_1 = 0$ | $(y_1 + 2y_2 + y_3 - 3) \cdot x_1 = 0$ |
| $(2x_1 - x_2 + 2x_3 - 4) \cdot y_2 = 0$ | $(2y_1 - y_2 + y_3 - 2) \cdot x_2 = 0$ |
| $(x_1 + x_2 - x_3 - 3) \cdot y_3 = 0$ | $(y_1 + 2y_2 - y_3 - 1) \cdot x_3 = 0$ |

Each is $(\text{constraint expression} - \text{RHS}) \times \text{matching variable} = 0$.

---

# Part 6: Part (c), the refutation procedure

Given $x^*$, show it is not optimal without solving the LP.

If $x^*$ were optimal, there would be a dual-feasible $y$ satisfying complementary slackness. Construct the $y$ that CS forces and show it is not dual-feasible.

1. **Step 1** Check $x^*$ is primal feasible. If not, you are done.
2. **Step 2** Each slack primal constraint forces its $y_i = 0$.
3. **Step 3** Each $x_j^* \ne 0$ forces dual constraint $j$ to hold with equality. List all of them, substitute the zeros from Step 2, and solve. Inconsistent system $\to$ no such $y$ $\to$ $x^*$ is not optimal.
4. **Step 4** Check the recovered $y$ against what is left: dual constraints not forced tight (as inequalities) and the sign restrictions.
   - all satisfied $\to$ $x^*$ is optimal and $y$ is the dual optimum
   - any violated $\to$ $x^*$ is not optimal

   A constraint forced tight in Step 3 must hold as an equality.

The contradiction usually appears in Step 4 as a violated sign restriction or an unused dual constraint, but an inconsistent Step 3 system is already a complete refutation.

Given a dual point instead: swap the roles. Check dual feasibility; slack dual constraints force $x_j = 0$; non-zero $y_i$ force primal row $i$ tight; solve; check primal feasibility.

---

# Part 7: Worked example, SS25 E3

$$
\begin{aligned}
(P)\quad \text{Maximize}\quad & 3x_1 + 2x_2 + x_3 \\
\text{subject to}\quad & x_1 + 2x_2 + x_3 \le 10 \\
& 2x_1 - x_2 + 2x_3 \ge 4 \\
& x_1 + x_2 - x_3 = 3 \\
& x_1, x_2, x_3 \ge 0
\end{aligned}
$$

## (a) Dual

Max problem, so use the max row of the SOB table:

| primal | type | dual |
|---|---|---|
| constraint 1: $\le$ | S | $y_1 \ge 0$ |
| constraint 2: $\ge$ | B | $y_2 \le 0$ |
| constraint 3: $=$ | O | $y_3$ free |
| $x_1, x_2, x_3 \ge 0$ | S | dual constraints $\ge$ (dual is a min) |

$$
\begin{aligned}
(D)\quad \text{Minimize}\quad & 10y_1 + 4y_2 + 3y_3 \\
\text{subject to}\quad & y_1 + 2y_2 + y_3 \ge 3 && x_1 \text{ column} \\
& 2y_1 - y_2 + y_3 \ge 2 && x_2 \text{ column} \\
& y_1 + 2y_2 - y_3 \ge 1 && x_3 \text{ column} \\
& y_1 \ge 0,\; y_2 \le 0,\; y_3 \in \mathbb{R}
\end{aligned}
$$

## (b) CS conditions

As in Part 5.

## (c) Show $x = (2, 2, 1)^T$ is not optimal

**Step 1: primal feasibility.**
$$
\begin{aligned}
2 + 2 \cdot 2 + 1 &= 7 \le 10 && \text{slack} \\
2 \cdot 2 - 2 + 2 \cdot 1 &= 4 \ge 4 && \text{tight} \\
2 + 2 - 1 &= 3 = 3 \\
2, 2, 1 &\ge 0
\end{aligned}
$$
Feasible.

**Step 2.** Constraint 1 is slack, so $y_1 = 0$.

**Step 3.** All three $x_j$ are non-zero, so all three dual constraints are tight. With $y_1 = 0$:

$$
\begin{aligned}
2y_2 + y_3 &= 3 \\
-y_2 + y_3 &= 2 \\
2y_2 - y_3 &= 1
\end{aligned}
$$

The last two give $y_2 = 3$, $y_3 = 5$, but then the first reads $2 \cdot 3 + 5 = 11 \ne 3$. Inconsistent.

(Equivalently: the last two equations give $y = (0, 3, 5)^T$, and $y_2 = 3 > 0$ violates $y_2 \le 0$.)

> No dual-feasible $y$ satisfies complementary slackness with $x = (2, 2, 1)^T$, so $x$ is not optimal.

---

# Part 8: Traps and practice

## Where points are lost

1. Giving sign restrictions a dual variable.
2. Reading rows instead of columns.
3. Wrong sign for a $\ge$ row in a max problem (it is $y \le 0$).
4. Writing "P infeasible $\Rightarrow$ D unbounded" (it is "unbounded or infeasible").
5. Solving the LP in part (c).
6. Stopping without a contradiction. Write the tight equation for every non-zero $x_j$; if the system is consistent, the answer is in Step 4.

## From blank paper, before the papers

- the SOB table
- both CS statements, in words and formulas
- the $3 \times 3$ outcome table

## Exercises (untimed)

1. `D4.1` *Dual Problem* `[DRILL]`: mechanical dualisation, 2–3 times.
2. `T4.2` *Duality* `[DRILL]`: mixed constraint types and free variables.
3. `T4.1` *Duality & Complementary Slackness* `[EXAM]`: the exam's format.
4. `S4.2` *Duality & Complementary Slackness* `[EXAM]`: second repetition.
5. `D4.2` *Primal-Dual* `[EXAM]`: extra practice.
6. `T4.3` Rock-Paper-Scissors, `D4.3` Transportation `[CONCEPT]`: read only.

Sheet 4: `exercises/05-linear-programming-duality/sheet-04-exercises.pdf` (S4.x in the second half).

## Papers (timed, one minute per point)

2026 P3 (15) · SS25 E3 (15) · SS24 P2 (16) · SS23 E1 (19)

## Links

- $y^T = c_B^T B^{-1}$: the optimal duals are the shadow prices, found in Row 0 of the final tableau under the slack columns ([03](03-revised-simplex-and-sensitivity.md)).
- In branch and bound, any dual-feasible solution of a node's LP relaxation bounds that node ([06a](06a-branch-and-bound-lesson.md)).
