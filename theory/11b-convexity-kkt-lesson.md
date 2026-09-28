# Convexity, Slater and KKT

Companion to [11-nonlinear-and-convex-kkt](11-nonlinear-and-convex-kkt.md), second half. The unconstrained half is [11a](11a-nonlinear-unconstrained-lesson.md).

Exam slot E7b/P7. SS25's whole 20-point nonlinear question was constrained, and 2026 P7 (22 cr) was constrained too.

---

# Part 0: What the exam asks

**SS25 E7** *Minimal Circle Enclosure*:

1. State the definition of a convex function.
2. Prove that any norm $\lVert\cdot\rVert$ on $\mathbb{R}^d$ is convex.
3. Formulate the problem as a convex optimisation problem; show $f$ and $g_i$ are convex.
4. Show Slater's condition holds; give an explicit strictly feasible point.
5. State the KKT conditions.
6. Use KKT to show $\sum_i \lambda_i = 1$ and $c^* = \sum_i \lambda_i p_i$.
7. Suppose $\lambda_i > 0$. What does that imply about $p_i$?

**2026 P7** *Budget Allocation*: $\max\ \log(1+x_1) + \alpha \log(1+x_2)$ s.t. $x_1 + x_2 \le 1$, $x \ge 0$.

1. State Slater's condition.
2. Show the objective is concave (Hessian).
3. Show the feasible set is convex and Slater holds.
4. Show $\max\{x_1, x_2\}$ is convex directly from the definition.
5. Rewrite as a min, state the Lagrangian and all KKT conditions.
6. Show the budget constraint is active at every KKT point.
7. Case analysis over $\alpha$; justify why KKT points are global maxima.

Both follow the same arc: definition, convexity proof, formulation, constraint qualification, KKT, conclusion. Much of it is definitions and verification with little arithmetic.

---

# Part 1: Convex sets and convex functions

These are different ideas and you usually need both.

## Convex set

$$C \text{ convex} \iff \lambda x + (1-\lambda) y \in C \quad \forall x, y \in C,\ \forall \lambda \in [0,1]$$

The segment between any two points of the set stays in the set. $\lambda = 1$ gives $x$, $\lambda = 0$ gives $y$, $\lambda = \tfrac12$ the midpoint.

| Convex | Not convex |
|---|---|
| halfspace $\{x : a^Tx \le b\}$ | circle curve $x^2 + y^2 = r^2$ |
| hyperplane $\{x : a^Tx = b\}$ | ring / annulus |
| ball, closed disk | union of two disjoint disks |
| any intersection of convex sets | unions in general |

The circle curve is compact but not convex (a chord leaves the curve). The closed disk $x^2 + y^2 \le r^2$ is both.

## Convex function

$$f \text{ convex} \iff f(\lambda x_1 + (1-\lambda) x_2) \le \lambda f(x_1) + (1-\lambda) f(x_2) \quad \forall \lambda \in [0,1]$$

The chord between two points of the graph lies on or above the graph.

- **Strictly convex:** $<$ for $x_1 \ne x_2$, $\lambda \in (0,1)$. $x^2$ is; $\lvert x \rvert$ is convex but not strictly.
- **Concave:** reverse the inequality; equivalently $-f$ is convex.

## Why it matters

$$\text{convex objective} + \text{convex feasible set} \;\Longrightarrow\; \text{every local minimum is a global minimum}$$

---

# Part 2: Proving a function convex

## Route A: the definition

Use when $f$ is non-differentiable or abstract.

**Any norm is convex (SS25 E7b).** Let $x, y \in \mathbb{R}^d$, $\lambda \in [0,1]$.

$$
\begin{aligned}
\lVert \lambda x + (1-\lambda) y \rVert
&\le \lVert \lambda x \rVert + \lVert (1-\lambda) y \rVert && \text{triangle inequality} \\
&= \lvert\lambda\rvert \, \lVert x \rVert + \lvert 1-\lambda \rvert \, \lVert y \rVert && \text{homogeneity} \\
&= \lambda \lVert x \rVert + (1-\lambda) \lVert y \rVert && \lambda \ge 0,\ 1-\lambda \ge 0
\end{aligned}
$$

This is the definition of convexity. The last line needs its reason stated; omitting it is the usual lost mark.

**$\max(x_1, x_2)$ is convex (2026 P7d).** The question says "directly from the definition", so citing the max rule from Route C does not earn the marks.

Let $x, y \in \mathbb{R}^2$, $\lambda \in [0,1]$, $z = \lambda x + (1-\lambda) y$. For each coordinate $j \in \{1,2\}$:

$$z_j = \lambda x_j + (1-\lambda) y_j \le \lambda \max(x_1,x_2) + (1-\lambda) \max(y_1,y_2)$$

because $x_j \le \max(x_1,x_2)$, $y_j \le \max(y_1,y_2)$ and $\lambda, 1-\lambda \ge 0$. The right side does not depend on $j$, so taking the max over $j$ on the left keeps the inequality: $f(z) \le \lambda f(x) + (1-\lambda) f(y)$.

## Route B: the Hessian

On a convex domain:

| Hessian everywhere | Conclusion |
|---|---|
| $H_f(x) \succeq 0$ | $\iff$ $f$ convex |
| $H_f(x) \succ 0$ | $\Rightarrow$ $f$ strictly convex (converse false: $x^4$) |
| $H_f(x) \preceq 0$ | $\iff$ $f$ concave |

Same tests as [11a](11a-nonlinear-unconstrained-lesson.md) Part 5. State that the domain is convex.

2026 P7b: the Hessian of $\log(1+x_1) + \alpha\log(1+x_2)$ is

$$\nabla^2 f_\alpha = \begin{pmatrix} -\dfrac{1}{(1+x_1)^2} & 0 \\ 0 & -\dfrac{\alpha}{(1+x_2)^2} \end{pmatrix} \preceq 0$$

(diagonal, entries $\le 0$), so the function is concave.

## Route C: composition rules

Convexity is preserved by:

| Operation | Condition |
|---|---|
| $\alpha f$ | $\alpha \ge 0$ |
| $f + g$ | both convex |
| $f(Ax + b)$ | $f$ convex (affine substitution) |
| $\max\{f_1, \dots, f_m\}$ | all convex |
| $h \circ g$ | $h$ convex and non-decreasing, $g$ convex |

Every affine function $a^Tx + b$ is both convex and concave.

SS25 E7c in one line: $g_i(r, c_1, c_2) = \lVert M - p_i \rVert_2 - r$ is the sum of a norm of an affine function (convex by part b) and the affine function $-r$, hence convex.

## Route D: disproof

To show $f$ is not convex, give one triple $(x, y, \lambda)$ that violates the inequality.

---

# Part 3: Standard form

Convert every problem to:

$$
\begin{aligned}
\min\ & f(x) \\
\text{s.t.}\ & g_i(x) \le 0, && i = 1,\dots,m \\
& h_j(x) = 0, && j = 1,\dots,p
\end{aligned}
$$

- $g(x) \ge 0$ becomes $-g(x) \le 0$.
- $\max f$ becomes $\min -f$.

Write the conversion down; a sign error here carries through everything.

At a point $x^*$: $g_i(x^*) = 0$ means **active** (tight), $g_i(x^*) < 0$ means **inactive** (slack).

---

# Part 4: The Lagrangian

For a min problem:

$$L(x, \lambda, \mu) = f(x) + \sum_i \lambda_i g_i(x) + \sum_j \mu_j h_j(x), \qquad \lambda_i \ge 0,\ \mu_j \text{ free}$$

Why: violating $g_i \le 0$ means $g_i > 0$, and the term $\lambda_i g_i$ must penalise that in a minimisation, so $\lambda_i \ge 0$. An equality can be violated in either direction, so $\mu$ is free. This matches LP duality: $\le$ rows get sign-restricted duals, $=$ rows get free ones.

$\lambda$ is a shadow price. For a min problem $\lambda_i^* = -\partial f^* / \partial b_i$: relaxing constraint $i$'s right-hand side by $\Delta$ lowers $f^*$ by about $\lambda_i^* \Delta$. In a max LP, relaxing a resource raises $z^*$ by $y_i^* \Delta$. Same price, opposite sign.

For a max problem the convention is $L = f - \sum_i \lambda_i g_i - \sum_j \mu_j h_j$. Easier: convert to a min first.

SS25 wrote $L = f + \sum_i \lambda_i g_i - \mu r$: the $-\mu r$ is the constraint $r \ge 0$ written as $-r \le 0$ with $\mu \ge 0$.

---

# Part 5: The four KKT blocks

$$
\begin{aligned}
&\textbf{1. Stationarity} && \nabla f(x^*) + \sum_i \lambda_i \nabla g_i(x^*) + \sum_j \mu_j \nabla h_j(x^*) = 0 \quad (\nabla_x L = 0) \\
&\textbf{2. Primal feasibility} && g_i(x^*) \le 0 \ \ \forall i, \qquad h_j(x^*) = 0 \ \ \forall j \\
&\textbf{3. Dual feasibility} && \lambda_i \ge 0 \ \ \forall i \qquad (\mu_j \text{ free}) \\
&\textbf{4. Complementary slackness} && \lambda_i \, g_i(x^*) = 0 \ \ \forall i
\end{aligned}
$$

Each block is scored separately when the exam says "state the KKT conditions".

**Stationarity** generalises $\nabla f = 0$: the gradient of $f$ may be non-zero, but the constraint gradients must balance it exactly.

**Complementary slackness:**

$$\lambda_i \, g_i(x^*) = 0 \iff \lambda_i = 0 \ \text{ or } \ g_i(x^*) = 0$$

An inactive constraint has multiplier zero; a non-zero multiplier means the constraint is active. Same as in LP.

---

# Part 6: Solving a KKT system

Each inequality gives a fork: either $\lambda_i = 0$ (inactive) or $g_i(x^*) = 0$ (active). With $m$ inequalities that is up to $2^m$ cases, most of which die quickly.

1. Convert to standard form.
2. Write the Lagrangian.
3. Write all four blocks.
4. List the cases from complementary slackness.
5. Solve each case.
6. Discard a case if $\lambda_i < 0$ (violates dual feasibility) or $g_i(x^*) > 0$ (violates primal feasibility).
7. The survivors are the KKT points.

State the reason for each discarded case.

## Worked example

$$\min\ (x-2)^2 + (y-2)^2 \quad \text{s.t.} \quad x + y - 2 \le 0$$

$L = (x-2)^2 + (y-2)^2 + \lambda(x + y - 2)$.

- Stationarity: $2(x-2) + \lambda = 0$, $2(y-2) + \lambda = 0$
- Primal: $x + y - 2 \le 0$
- Dual: $\lambda \ge 0$
- CS: $\lambda(x + y - 2) = 0$

**Case 1 ($\lambda = 0$).** $\nabla f = 0$ gives $(2,2)$, but $g = 2 > 0$. Discard: primal infeasible.

**Case 2 ($x + y = 2$).** Stationarity gives $x = y = 2 - \lambda/2$. Into the constraint: $4 - \lambda = 2$, so $\lambda = 2 \ge 0$. Keep: point $(1,1)$, $\lambda^* = 2$, $f = 2$.

Globality: $f$ is convex ($H = 2I$), $g$ is affine, and $\bar x = (0,0)$ has $g(\bar x) = -2 < 0$, so Slater holds and $(1,1)$ is the global minimum.

Shadow-price check: relaxing to $x + y \le 2 + \Delta$ gives $f^* = (2-\Delta)^2/2$, so $\partial f^*/\partial\Delta = -2 = -\lambda^*$ at $\Delta = 0$.

## Removing cases early

Argue a constraint is active before enumerating.

- SS25 E7f: two distinct points force $r > 0$; CS $\mu r = 0$ then gives $\mu = 0$.
- 2026 P7f: stationarity gives $\lambda_1 = \dfrac{1}{1+x_1} + \lambda_2 > 0$, so CS forces $x_1 + x_2 = 1$.

After that, 2026 P7g needs only three cases (both positive, $(1,0)$, $(0,1)$), each giving a range of $\alpha$ where all multipliers are $\ge 0$.

---

# Part 7: Slater vs LICQ

KKT conditions are only meaningful under a constraint qualification, and which one holds decides how strong the conclusion is.

## Slater

**Requires:** $f$ and all $g_i$ convex, all $h_j$ affine, and some $\bar x$ with $g_i(\bar x) < 0$ for all $i$ (and $h_j(\bar x) = 0$).

**Gives:** a KKT point is a global optimum. No comparison needed.

Verify it by giving an explicit point.

- SS25 E7d: centroid $\bar c$ and $\bar r = \max_i \lVert \bar M - p_i \rVert_2 + 1$, so every $g_i \le -1 < 0$. The $+1$ makes the inequality strict.
- 2026 P7c: $\bar x = (\tfrac13, \tfrac13)$: $\tfrac13 + \tfrac13 < 1$, $\tfrac13 > 0$, $\tfrac13 > 0$.

## LICQ

**Requires:** $\{\nabla g_i(x^*) : i \text{ active}\} \cup \{\nabla h_j(x^*)\}$ linearly independent.

**Gives:** KKT points are candidates only. Evaluate $f$ at each and compare.

| | Slater | LICQ |
|---|---|---|
| Needs convexity? | yes | no |
| Checked where? | one strictly feasible point | at the candidate $x^*$ |
| Conclusion | KKT point is a global optimum | KKT points are candidates |
| Compare $f$ values? | no | yes |

MC (SS25 1h): "a KKT point is always a local optimum" is false. "For a convex problem, a KKT point is a global minimiser if Slater holds" is true.

---

# Part 8: Recipe

1. **Existence.** Feasible set compact (closed and bounded) and $f$ continuous $\Rightarrow$ a global min and max exist (Weierstrass).
2. **CQ check.** Convex problem with a strictly feasible point: Slater. Otherwise check LICQ at the candidate.
3. **Solve.** Lagrangian, four blocks, case split, discard infeasible cases.
4. **Compare.** Evaluate $f$ at every surviving candidate. Skip only if Slater held.

Compact examples: circle, closed disk, closed polytope, closed box. Not compact: $x + y \le 2$ alone (unbounded), $x^2 + y^2 < 1$ (open).

---

# Part 9: Worked example, SS25 E7

> $n$ distribution centres at $p_i = (x_i, y_i) \in \mathbb{R}^2$, at least two distinct. Find the centre $M = (c_1, c_2)$ and radius $r \ge 0$ of the smallest circle enclosing all of them.

**(a)** $f : \mathbb{R}^n \to \mathbb{R}$ is convex if for all $x, y$ and $\lambda \in [0,1]$: $f(\lambda x + (1-\lambda) y) \le \lambda f(x) + (1-\lambda) f(y)$.

**(b)** The norm proof from Part 2, Route A.

**(c)**

$$
\begin{aligned}
\min_{r, c_1, c_2}\ & r \\
\text{s.t.}\ & \lVert M - p_i \rVert_2 - r \le 0 \quad \forall i \\
& r \ge 0, \quad c_1, c_2 \in \mathbb{R}
\end{aligned}
$$

- $f = r$ is linear, hence convex.
- $g_i = \lVert M - p_i \rVert_2 - r$ is a sum of two convex functions by (b), hence convex.

**(d)** Centroid plus inflated radius:

$$\bar c_1 = \frac1n \sum_i x_i, \qquad \bar c_2 = \frac1n \sum_i y_i, \qquad \bar r = \max_i \lVert \bar M - p_i \rVert_2 + 1$$

Then $g_i \le -1 < 0$ for all $i$, so Slater holds and any KKT point is a global optimum.

**(e)** With $L = r + \sum_i \lambda_i g_i - \mu r$, $\lambda_i, \mu \ge 0$:

- Stationarity: $\nabla L = 0$
- Primal feasibility: $g_i \le 0$, $-r \le 0$
- Complementary slackness: $\lambda_i g_i = 0$, $\mu r = 0$
- Dual feasibility: $\lambda_i \ge 0$, $\mu \ge 0$

**(f)** Two distinct points force $r > 0$, so $\mu r = 0$ gives $\mu = 0$.

Stationarity in $r$:

$$\frac{\partial L}{\partial r} = 1 - \sum_i \lambda_i - \mu = 0 \;\Longrightarrow\; \sum_i \lambda_i = 1$$

Stationarity in $c_1$:

$$\frac{\partial L}{\partial c_1} = \sum_i \lambda_i \frac{c_1 - x_i}{\lVert p_i - M \rVert_2} = 0$$

By CS, every term with $\lambda_i > 0$ has $\lVert p_i - M \rVert_2 = r$, so

$$\sum_i \lambda_i \frac{c_1 - x_i}{r} = 0 \;\Longrightarrow\; c_1 \sum_i \lambda_i = \sum_i \lambda_i x_i \;\Longrightarrow\; c_1^* = \sum_i \lambda_i x_i$$

Same for $c_2$. The optimal centre is a convex combination of the points.

**(g)** $\lambda_i > 0$ forces $g_i = 0$, i.e. $\lVert p_i - M \rVert_2 = r$: $p_i$ lies on the boundary circle.

---

# Part 10: Traps and practice

## Where points are lost

1. Not converting to standard form, then a sign error everywhere.
2. Swapping the sign rules: $\lambda \ge 0$, $\mu$ free.
3. Skipping the definition or convexity proof. They are 4–6 points on SS25.
4. In the norm proof, not saying why $\lvert\lambda\rvert = \lambda$.
5. Confusing convex sets with convex functions.
6. Claiming Slater without giving a point.
7. Comparing $f$ values after Slater (unnecessary), or not comparing after LICQ (required).
8. Not stating why each discarded case fails.
9. Asserting an optimum exists without compactness and continuity.

## Two statements to know

| | Needs convexity | Conclusion |
|---|---|---|
| Slater | yes | KKT point is a global optimum; no comparison |
| LICQ | no | KKT points are candidates; compare |

## Exercises (untimed)

1. `T11.1` *Convex Functions – Examples* `[DRILL]`: decide convexity for a list.
2. `T11.2` *Convex functions* `[DRILL]`: find $r$ so that $e^{2x} + xy + y^2$ is convex on the square $Q_r$. This is a Hessian exercise, not a from-definition proof. For the definition proof, redo the norm and max proofs in Part 2.
3. `D10.1` *Topological Properties and Convex Sets* `[CONCEPT]`: sets vs functions, compactness. Skim.
4. `T10.2` *Lagrange multipliers* `[DRILL]`: Lagrangian with equalities only.
5. `D10.3` *KKT-conditions* `[EXAM]`: inequalities and the case split. The core exercise.
6. `T10.3` *Non-linear optimization – KKT conditions* `[EXAM]`: second repetition.
7. Skip `T11.3` *Hedge Algorithm* and CE-11 (online optimisation, not examined).

Sheets 10 and 11: `exercises/11-nonlinear-convex-optimization/`. CE-10: `central exercises/11-nonlinear-convex-optimization/`.

## Papers (timed, one minute per point)

- 2026 P7, all parts.
- SS25 E7 *Minimal Circle Enclosure*, all parts.
- SS24 P7b: KKT with the single constraint $x + y - 2 \le 0$, Slater verified.
