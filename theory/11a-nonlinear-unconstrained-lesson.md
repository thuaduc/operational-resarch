# Nonlinear optimisation, unconstrained

Companion to [11-nonlinear-and-convex-kkt](11-nonlinear-and-convex-kkt.md), first half. The constrained half (convexity, Slater, KKT) is in [11b](11b-convexity-kkt-lesson.md).

Exam slot E7a. The nonlinear block is worth 12–23 points; the unconstrained half appears on four of the last five papers.

---

# Part 0: What the exam asks

> **SS24 P7a:** *"Find all local extrema of $f(x,y) = x^2 + y^2 + e^{2x} - (8x + 6y) + 2xy$ and determine their type (max/min)."*
>
> **SS23 E7 (23 pts):** *"Let $\alpha > 0$ be a parameter. Determine all critical points of $f_\alpha(x,y) = -\tfrac14 x^4 - \tfrac14 y^4 + \alpha xy$ as a function of $\alpha$, and show in each case whether it is a local maximum, local minimum, or a saddle point."*

Three steps every time: find where the gradient vanishes, compute the Hessian there, classify. The SS23 version adds a parameter, so the answer becomes a case analysis.

---

# Part 1: The one-variable version

| Condition | Meaning |
|---|---|
| $f'(x) = 0$ | flat tangent, candidate for max or min |
| $f''(x) > 0$ | curves up: minimum |
| $f''(x) < 0$ | curves down: maximum |
| $f''(x) = 0$ | inconclusive |

In $n$ dimensions the gradient replaces $f'$, the Hessian replaces $f''$, and "positive" becomes "positive definite".

---

# Part 2: The gradient

$$\nabla f(x) = \left( \frac{\partial f}{\partial x_1}, \frac{\partial f}{\partial x_2}, \dots, \frac{\partial f}{\partial x_n} \right)^T$$

For $\partial f / \partial x$, differentiate in $x$ and treat every other variable as a constant.

For $f(x,y) = x^2 + y^2 + e^{2x} - 8x - 6y + 2xy$:

$$
\begin{aligned}
\frac{\partial f}{\partial x} &= 2x + 2e^{2x} - 8 + 2y \\
\frac{\partial f}{\partial y} &= 2y - 6 + 2x \\
\nabla f(x,y) &= \left( 2x - 8 + 2y + 2e^{2x},\; 2y - 6 + 2x \right)^T
\end{aligned}
$$

$\nabla f$ points in the direction of steepest ascent. At a peak or valley floor there is no uphill direction, so a **critical point** satisfies

$$\nabla f(x) = 0$$

Every local max and min is a critical point, but so is a saddle. That is why you need the Hessian.

---

# Part 3: Finding the critical points

$\nabla f = 0$ is $n$ equations in $n$ unknowns. Solving it is usually the hardest part; classification afterwards is mechanical. Standard tactic: solve one equation for one variable and substitute.

**SS24:**

$$
\begin{aligned}
2x - 8 + 2y + 2e^{2x} = 0 &\;\Longrightarrow\; x + y + e^{2x} = 4 \\
2y - 6 + 2x = 0 &\;\Longrightarrow\; x + y = 3
\end{aligned}
$$

Substituting $x + y = 3$ into the first: $e^{2x} = 1$, so $x = 0$, $y = 3$. One critical point: $(0, 3)$.

**SS23:**

$$\nabla f_\alpha = \left( -x^3 + \alpha y,\; -y^3 + \alpha x \right)^T = 0$$

From the first component $y = x^3/\alpha$. Substitute into the second:

$$-\left(\frac{x^3}{\alpha}\right)^3 + \alpha x = 0 \iff x\left( -\frac{x^8}{\alpha^3} + \alpha \right) = 0$$

So $x = 0$ or $x^8 = \alpha^4$, i.e. $x = \pm\sqrt{\alpha}$. Three critical points:

$$(0, 0), \qquad (\sqrt{\alpha}, \sqrt{\alpha}), \qquad (-\sqrt{\alpha}, -\sqrt{\alpha})$$

Factor instead of dividing, so the $x = 0$ branch is not lost. $\alpha > 0$ is what makes $\sqrt{\alpha}$ real.

---

# Part 4: The Hessian

$$
H_f(x) = \nabla^2 f(x) = \left( \frac{\partial^2 f}{\partial x_i \partial x_j} \right)_{ij},
\qquad
H_f = \begin{pmatrix} f_{xx} & f_{xy} \\ f_{yx} & f_{yy} \end{pmatrix}
$$

For $C^2$ functions $f_{xy} = f_{yx}$, so the Hessian is symmetric. Use that to check your arithmetic.

**SS24:** differentiate $\nabla f = \left( 2x - 8 + 2y + 2e^{2x},\; 2y - 6 + 2x \right)$ again:

$$H_f = \begin{pmatrix} 2 + 4e^{2x} & 2 \\ 2 & 2 \end{pmatrix}$$

**SS23:** from $\nabla f_\alpha = \left( -x^3 + \alpha y,\; -y^3 + \alpha x \right)$:

$$H_f = \begin{pmatrix} -3x^2 & \alpha \\ \alpha & -3y^2 \end{pmatrix}$$

SS23's Hessian depends on $x$ and $y$, so evaluate it at each critical point separately.

---

# Part 5: Definiteness

| Hessian at $x^*$ | Curvature | $x^*$ is a |
|---|---|---|
| positive definite | up in every direction | **local minimum** |
| negative definite | down in every direction | **local maximum** |
| indefinite | up in some, down in others | **saddle point** |
| semidefinite (an eigenvalue is 0) | flat in some direction | **inconclusive** |

For symmetric $A$:

| | Definition | Eigenvalues |
|---|---|---|
| positive definite | $x^TAx > 0$ for all $x \ne 0$ | all $> 0$ |
| negative definite | $x^TAx < 0$ for all $x \ne 0$ | all $< 0$ |
| indefinite | | both signs |

## Test A: 2×2 shortcut (default)

For $H = \begin{pmatrix} a & b \\ b & d \end{pmatrix}$ with $\det H = ad - b^2$ and $\operatorname{tr} H = a + d$:

| Condition | Definiteness | Point |
|---|---|---|
| $\det > 0$, $\operatorname{tr} > 0$ | positive definite | **minimum** |
| $\det > 0$, $\operatorname{tr} < 0$ | negative definite | **maximum** |
| $\det < 0$ | indefinite | **saddle** |
| $\det = 0$ | inconclusive | use Test C |

Reason: $\lambda_1 \lambda_2 = \det$ and $\lambda_1 + \lambda_2 = \operatorname{tr}$. $\det > 0$ means same sign, and the trace says which. $\det < 0$ means opposite signs. $\det = 0$ means an eigenvalue is zero.

## Test B: leading principal minors (any size)

$D_k$ = determinant of the top-left $k \times k$ submatrix. For 2×2: $D_1 = a$, $D_2 = \det H$.

| Pattern | Result |
|---|---|
| all $D_k > 0$ | positive definite |
| $D_1 < 0, D_2 > 0, D_3 < 0, \dots$ (alternating, starting negative) | negative definite |
| no $D_k = 0$, neither pattern | indefinite |
| some $D_k = 0$ | test does not apply: use Test C |

Check the negative pattern on $H = -I$: $D_1 = -1$, $D_2 = 1$.

## Test C: eigenvalues (fallback)

Solve $\det(H - \lambda I) = 0$ and read the signs of the roots.

You will need it: SS23's origin has $H = \begin{pmatrix} 0 & \alpha \\ \alpha & 0 \end{pmatrix}$, so $D_1 = 0$ and Test B does not apply.

---

# Part 6: The convexity shortcut

If the Hessian is positive definite everywhere, not just at the critical point:

$$H_f \succ 0 \text{ everywhere} \;\Longrightarrow\; f \text{ strictly convex} \;\Longrightarrow\; \text{at most one minimum, no maxima}$$

and any critical point is the global minimum.

SS24's official solution starts this way:

> *"Since $\operatorname{Tr}(\nabla^2 f) > 0$ and $\det(\nabla^2 f) > 0$ it follows that $\nabla^2 f$ is positive definite. Hence $f$ is strictly convex. Consequently, if a minimum exists it is unique. No maxima exist."*

Check: $\operatorname{tr} = 4 + 4e^{2x} > 0$ and $\det = 2(2 + 4e^{2x}) - 4 = 8e^{2x} > 0$ for every $(x, y)$.

Do this check first. "No maxima exist" is part of the answer to "find all local extrema".

The converse fails: $f(x) = x^4$ is strictly convex but $f''(0) = 0$. So $H \succ 0 \Rightarrow$ strictly convex, not $\Leftrightarrow$.

**MC trap (2026 P1g):** strictly convex does not imply a minimum exists. $f(x) = e^x$ is strictly convex with no minimiser ($\inf f = 0$ is never attained). Strict convexity gives uniqueness if a minimum exists; existence needs a separate argument (compact domain, or a critical point you actually found). The true statement also tested there: a stationary point of a convex $f$ is a global minimum.

---

# Part 7: Worked example, SS24 P7a

> Find all local extrema of $f(x,y) = x^2 + y^2 + e^{2x} - (8x + 6y) + 2xy$ and determine their type.

**1. Gradient.**

$$\nabla f = \left( 2x - 8 + 2y + 2e^{2x},\; 2y - 6 + 2x \right)^T$$

**2. Hessian, definiteness everywhere.**

$$H_f = \begin{pmatrix} 2 + 4e^{2x} & 2 \\ 2 & 2 \end{pmatrix}, \qquad \operatorname{tr} = 4 + 4e^{2x} > 0, \qquad \det = 8e^{2x} > 0$$

Positive definite everywhere, so $f$ is strictly convex: no maxima, and any critical point is the unique global minimum.

**3. Solve $\nabla f = 0$.**

$$x + y + e^{2x} = 4, \qquad x + y = 3$$

Subtract: $e^{2x} = 1$, so $x = 0$, $y = 3$.

**4. Conclude.** $(0, 3)$ is the unique global minimum. No local maxima exist.

---

# Part 8: Worked example, SS23 E7

> $\alpha > 0$. Determine all critical points of $f_\alpha(x,y) = -\tfrac14 x^4 - \tfrac14 y^4 + \alpha xy$ as a function of $\alpha$, and classify each.

**1. Critical points** (Part 3): $(0,0)$, $(\sqrt{\alpha}, \sqrt{\alpha})$, $(-\sqrt{\alpha}, -\sqrt{\alpha})$.

**2. Hessian.**

$$H_f = \begin{pmatrix} -3x^2 & \alpha \\ \alpha & -3y^2 \end{pmatrix}$$

**3. At $(\pm\sqrt{\alpha}, \pm\sqrt{\alpha})$.** Only $x^2$ and $y^2$ appear, so both points give

$$H_f = \begin{pmatrix} -3\alpha & \alpha \\ \alpha & -3\alpha \end{pmatrix}, \qquad D_1 = -3\alpha < 0, \qquad D_2 = 9\alpha^2 - \alpha^2 = 8\alpha^2 > 0$$

Alternating, starting negative: negative definite, so both are local maxima. (Test A agrees: $\det = 8\alpha^2 > 0$, $\operatorname{tr} = -6\alpha < 0$.)

**4. At $(0,0)$.**

$$H_f = \begin{pmatrix} 0 & \alpha \\ \alpha & 0 \end{pmatrix}, \qquad D_1 = 0 \;\Rightarrow\; \text{minors do not apply}$$

Eigenvalues: $\det(H - \lambda I) = \lambda^2 - \alpha^2 = 0$, so $\lambda = \pm\alpha$. One positive, one negative: indefinite, saddle point.

**5. Result.** For every $\alpha > 0$: local maxima at $(\pm\sqrt{\alpha}, \pm\sqrt{\alpha})$, saddle at the origin. State explicitly that no case split is needed because the signs do not change on $\alpha > 0$; that is the "as a function of $\alpha$" part.

---

# Part 9: Traps and practice

## Where points are lost

1. Stopping at $\nabla f = 0$ without classifying.
2. Reading a zero minor as "semidefinite". It means the test does not apply; use eigenvalues.
3. Writing "all $D_k < 0$" for negative definite. The signs alternate, starting negative.
4. Evaluating the Hessian at the wrong point, or not at all.
5. Losing a branch when solving $\nabla f = 0$ by dividing by something that can be zero.
6. Ignoring the parameter's stated range.
7. Not stating "no maxima exist" when $f$ is globally convex.

## The four steps

1. Compute $\nabla f$ and solve $\nabla f = 0$ for all critical points.
2. Compute $H_f$ symbolically.
3. At each point: det/trace (or minors); eigenvalues if a minor is 0.
4. Conclude min / max / saddle; globally convex means global and unique.

## Exercises (untimed)

1. `D10.2` *Unconstrained Optimization* `[EXAM]`: same task as SS24 P7a. Two or three functions.
2. `T10.1` *Gradient Descent vs. Newton's Method* `[CONCEPT]`: read only; MC material (SS25 1h).

Sheet 10: `exercises/11-nonlinear-convex-optimization/sheet-10-exercises.pdf`. CE-10: `central exercises/11-nonlinear-convex-optimization/ce-10-demo.pdf`.

## Papers (timed, one minute per point)

- SS24 P7a: convexity shortcut plus a substitution. Start here.
- SS21 A6 *Konvexe Funktionen* (13 pts).
- SS23 E7 (23 pts): parameterised, needs the eigenvalue fallback. Do it last.

## Link to the constrained half

The same Hessian test proves convexity: $H_f \succeq 0$ on a convex domain $\iff$ $f$ convex. See [11b](11b-convexity-kkt-lesson.md) and [11-nonlinear-and-convex-kkt](11-nonlinear-and-convex-kkt.md) Procedures 3–7.
