# IP modelling

Companion to [05-ip-modeling](05-ip-modeling.md), which has the full catalogue of patterns.

Exam slot E4/P2, 21–35 points, the largest block on every paper. Parts 0–3 are the basics; Part 4 onward is exam material.

---

# Part 0: Vocabulary

$$
\begin{aligned}
\min\quad & 40x_1 + 25x_2 && \text{objective} \\
\text{s.t.}\quad & x_1 + x_2 \ge 3 && \text{constraints} \\
& 2x_1 + 5x_2 \le 12 \\
& x_1, x_2 \in \mathbb{N}_0 && \text{domains}
\end{aligned}
$$

- A **variable** is chosen by the solver ($x_1, x_2$).
- A **parameter** is given in the text (40, 25, 3, 12).
- A **feasible solution** satisfies every constraint; an **optimal solution** is the best feasible one.

Modelling means translating the text into these blocks. You are never asked to solve the model.

What makes it an integer program is the domain: variables in $\mathbb{N}_0$, $\mathbb{Z}$ or $\{0,1\}$.

---

# Part 1: Binary variables

$$y_i \in \{0,1\} \qquad y_i = 1: \text{build school } i; \quad y_i = 0: \text{do not}$$

Write $\in \{0,1\}$, not $0 \le y \le 1$, which would allow building 40% of a school.

## Sums of binaries count

With $y_1,\dots,y_5 \in \{0,1\}$ for five dorms, $y_1 + \dots + y_5$ is the number built.

| Requirement | Constraint |
|---|---|
| build at most 3 | $y_1+y_2+y_3+y_4+y_5 \le 3$ |
| build exactly 3 | $y_1+y_2+y_3+y_4+y_5 = 3$ |
| build at least 1 | $y_1+y_2+y_3+y_4+y_5 \ge 1$ |
| at most one of 2 and 3 | $y_2 + y_3 \le 1$ |

## Parameter × binary totals

With a cost $c_i$ per dorm, $\sum_i c_i y_i$ is the total cost of what is built.

| Requirement | Constraint |
|---|---|
| total cost at most €450k | $\sum_i c_i y_i \le 450$ |
| total capacity at least 600 | $\sum_i n_i y_i \ge 600$ |

---

# Part 2: Summation notation

## Two indices form a grid

$$x_{i,j} \in \{0,1\} \qquad = 1 \text{ if student } j \text{ is assigned to school } i$$

| | student 1 | student 2 |
|---|---|---|
| school 1 | $x_{1,1}$ | $x_{1,2}$ |
| school 2 | $x_{2,1}$ | $x_{2,2}$ |
| school 3 | $x_{3,1}$ | $x_{3,2}$ |

- $\sum_i x_{i,j}$: fix a student, sum down the column. How many schools student $j$ is assigned to.
- $\sum_j x_{i,j}$: fix a school, sum along the row. How many students school $i$ has.

## Free and bound indices

$$\sum_{i\in I} x_{i,j} = 1 \qquad \forall j \in J$$

- $i$ is bound (summed over).
- $j$ is free, so it needs $\forall j \in J$.

Every index that appears but is not summed over needs a $\forall$. A missing $\forall$ costs the point.

## What ∀ produces

$\forall$ makes one copy of the constraint per index value. With $I = \{1,2,3\}$, $J = \{1,2\}$:

$$
\begin{aligned}
& \sum_{i\in I} x_{i,j} = 1 \quad \forall j \in J \\
j=1:\quad & x_{1,1} + x_{2,1} + x_{3,1} = 1 \\
j=2:\quad & x_{1,2} + x_{2,2} + x_{3,2} = 1
\end{aligned}
$$
Each student is assigned to exactly one school.

$$
\begin{aligned}
& \sum_{j\in J} x_{i,j} \le 1 \quad \forall i \in I \\
i=1:\quad & x_{1,1} + x_{1,2} \le 1 \\
i=2:\quad & x_{2,1} + x_{2,2} \le 1 \\
i=3:\quad & x_{3,1} + x_{3,2} \le 1
\end{aligned}
$$
Each school gets at most one student.

## Rule

Sum over what you are counting; quantify over what you are counting it for.

"Each student must be assigned to exactly one school": count schools (sum over $i$) for each student ($\forall j$): $\sum_{i\in I} x_{i,j} = 1 \; \forall j \in J$.

You should always be able to say how many copies a constraint expands into.

---

# Part 3: A first model

> TUM is opening a new campus. Five dormitories can be built, with 100, 500, 400, 300 and 50 places. Construction costs are €100k, €400k, €360k, €275k, €75k. 600 students are expected. For each student who does not get a place, the city pays €950. Minimise total cost.

**Index set:**
$$I = \{1,2,3,4,5\} \qquad \text{dormitories}$$

**Parameters:**
$$
\begin{aligned}
n_i &= \text{capacity of dorm } i && n = (100, 500, 400, 300, 50) \\
c_i &= \text{cost of dorm } i && c = (100, 400, 360, 275, 75) \text{ in €k} \\
D &= 600 && \text{students expected} \\
p &= 950 && \text{penalty per unhoused student, € } (= 0.95 \text{ €k})
\end{aligned}
$$

**Variables:**
$$
\begin{aligned}
y_i &\in \{0,1\} && = 1 \text{ if dorm } i \text{ is built} \\
s &\in \mathbb{N}_0 && = \text{number of students who get a place}
\end{aligned}
$$
$s$ is needed because the cost depends on how many students are housed.

**Constraints:**
$$
\begin{aligned}
s &\le \sum_{i\in I} n_i y_i && \text{cannot house more than the capacity built} \\
s &\le 600 && \text{cannot house more students than exist}
\end{aligned}
$$

**Objective** (all money in €k):
$$\min \; \sum_{i\in I} c_i y_i + 0.95\cdot(600 - s)$$

**Model:**
$$
\begin{aligned}
\min\quad & \sum_{i\in I} c_i y_i + 0.95(600 - s) \\
\text{s.t.}\quad & s \le \sum_{i\in I} n_i y_i \\
& s \le 600 \\
& y_i \in \{0,1\} \quad \forall i \in I \\
& s \in \mathbb{N}_0
\end{aligned}
$$

Every answer has this shape: index sets, parameters, variables with domains, objective, constraints with quantifiers.

## Extra conditions (lecture slide 16)

| Requirement | Constraint | Reason |
|---|---|---|
| dorms 2 and 3 share a site, at most one | $y_2 + y_3 \le 1$ | sum of binaries |
| total cost at most €450k | $\sum_i c_i y_i \le 450$ | parameter × binary |
| at most 3 dorms | $\sum_i y_i \le 3$ | count |
| dorm 2 only if dorm 4 is built | $y_2 \le y_4$ | implication |
| dorm 4 only if all students are housed | $600y_4 \le s$ | if $y_4 = 1$ then $s \ge 600$ |

## Checking $y_2 \le y_4$ by cases

| $y_2$ | $y_4$ | $y_2 \le y_4$ | allowed |
|---|---|---|---|
| 0 | 0 | $0 \le 0$ | yes |
| 0 | 1 | $0 \le 1$ | yes |
| 1 | 0 | $1 \le 0$ | **no** |
| 1 | 1 | $1 \le 1$ | yes |

Only "2 without 4" is forbidden, which is the implication $y_2 \Rightarrow y_4$. Enumerating 0/1 cases is how to check any binary constraint.

## Logical operators (lecture slide 15)

| Logic | Constraint |
|---|---|
| $x_1 \lor \dots \lor x_n$ (at least one) | $x_1 + \dots + x_n \ge 1$ |
| $x_1 \land \dots \land x_n$ (all) | $x_1 \ge 1;\ x_2 \ge 1;\ \dots$ |
| $x_1 \Rightarrow x_2$ | $x_1 \le x_2$ |
| $x_1 \iff x_2$ | $x_1 = x_2$ |
| $\lnot A$ | $1 - A$ |

---

# Part 4: Why integer programs are hard

## Rounding does not work

$$
\begin{aligned}
\max\quad & x_1 + x_2 \\
\text{s.t.}\quad & 2x_2 \le 7 \\
& 7x_1 + 16x_2 \ge 56 \\
& 4x_1 + 3x_2 \le 20 \\
& x_1, x_2 \in \mathbb{N}_0
\end{aligned}
$$

The LP corners are $(0, 3.5)$, $(2.375, 3.5)$, $(3.54, 1.95)$. The only feasible integer point is $(2,3)$. Rounding the corners gives $(0,4)$, $(2,4)$, $(4,2)$, all infeasible, and nothing in the LP solution points to $(2,3)$.

Lecture: rounding is fine for large quantities with low marginal cost, but fails for a few costly decisions, and rounding a binary decision is meaningless.

The IP feasible set is the lattice points inside a polyhedron. It has no corners or edges for simplex to walk along, hence branch and bound.

## Problem classes

| | Form | Variables |
|---|---|---|
| LP | $\max\{c^T x : Ax \le b,\ x \ge 0\}$ | continuous |
| MIP | $\max c^T x + h^T y$, $Ax + Gy \le b$, $x \ge 0$, $y \in \mathbb{N}_0$ | mixed |
| IP | all structural variables in $\mathbb{N}_0$ | integer |
| BIP | $\max c^T x$, $Ax \le b$, $x \in \{0,1\}$ | binary |

## LP relaxation and formulation strength

Dropping integrality ($x \in \{0,1\}$ becomes $0 \le x \le 1$) gives the LP relaxation. Its feasible region contains the IP's, so for a max problem:

$$z_{LP} \ge z_{IP} \qquad \text{upper bound (lower bound for min)}$$

Branch and bound relies on this bound, so the formulation affects solving time. Of two formulations with the same integer solutions, the one with the smaller relaxation region is **stronger**: tighter bound, more pruning.

$$
\begin{aligned}
\text{disaggregated:}\quad & x_{i,j} \le y_i && \forall i,j && \text{stronger} \\
\text{aggregated:}\quad & \sum_j x_{i,j} \le M\cdot y_i && \forall i && \text{weaker}
\end{aligned}
$$

Both allow the same integer solutions, but the aggregated form's relaxation allows $y_i = 0.01$ with one student assigned.

"Which formulation is stronger and why": equal integer feasible sets, smaller relaxation region.

## Complexity

- **P**: solvable in polynomial time.
- **NP**: a proposed solution can be verified in polynomial time.
- **Polynomial reduction**: instances of A convert to instances of B in polynomial time, and solutions convert back. Then B is at least as hard as A.
- **NP-hard**: every NP problem reduces to it. It need not be in NP.
- **NP-complete**: in NP and NP-hard.

Optimisation versions ("find the cheapest tour") are typically NP-hard; decision versions ("is there a tour under 100 km?") are NP-complete.

**Deciding whether an IP has any feasible solution is NP-hard.** SAT reduces to binary IP:

1. For each boolean $x_i$ introduce $v_i \in \{0,1\}$, $v_i = 1 \iff x_i$ TRUE.
2. Each clause $x_1 \lor \lnot x_2 \lor \dots \lor x_i$ becomes $v_1 + (1 - v_2) + \dots + v_i \ge 1$.
3. A satisfying assignment exists $\iff$ the IP is feasible.

Cook (1971): SAT is NP-complete. Karp (1972) added TSP, scheduling, colouring, knapsack, bin packing, set cover, integer programming.

**LP:** simplex is exponential in the worst case (Klee–Minty cubes, a path through all $2^n$ corners), but LP is in P: ellipsoid method (Khachiyan 1979, $O(n^4 L)$, impractical) and interior-point methods (Karmarkar 1984, $O(n^{3.5} L)$). Simplex remains the default because it supports sensitivity analysis and warm starts, which B&B needs.

LP is in P; IP is NP-hard.

## Standard problems

Exams ask "which known problem is this?" (SS24 P6b: vertex cover).

**Assignment:** $n$ jobs to $n$ machines, minimum cost.
$$\min \sum_i \sum_j c_{ij} x_{ij} \quad \text{s.t.} \quad \sum_i x_{ij} = 1 \ \forall j, \quad \sum_j x_{ij} = 1 \ \forall i, \quad x \in \{0,1\}$$
$n!$ candidate assignments, but solvable in polynomial time: its constraint matrix is totally unimodular, so the LP relaxation is integral.

**Generalized assignment (GAP):** with capacities it becomes NP-hard.
$$\min \sum_i \sum_j c_{ij} x_{ij} \quad \text{s.t.} \quad \sum_i x_{ij} = 1 \ \forall j, \quad \sum_j d_{ij} x_{ij} \le s_i \ \forall i, \quad x \in \{0,1\}$$

**0-1 knapsack:**
$$\max \sum_j p_j x_j \quad \text{s.t.} \quad \sum_j w_j x_j \le W, \quad x \in \{0,1\}$$
Variants: multiple ($m$ knapsacks), bounded (several copies per item), multiple-choice (exactly one item per class).

**Multiple knapsack:**
$$\max \sum_i \sum_j p_j x_{ij} \quad \text{s.t.} \quad \sum_j w_j x_{ij} \le W_i \ \forall i, \quad \sum_i x_{ij} \le 1 \ \forall j$$
$\le 1$: items may be left out.

**Bin packing:**
$$\min \sum_i y_i \quad \text{s.t.} \quad \sum_i x_{ij} = 1 \ \forall j, \quad \sum_j d_j x_{ij} \le s\cdot y_i \ \forall i, \quad x, y \in \{0,1\}$$
$= 1$: everything is packed. Capacity $s$ is available only if bin $i$ is opened. Multidimensional version (server consolidation): $\sum_j u_{j,k,t} x_{ij} \le s_{i,k} y_i \ \forall i,k,t$.

**Set covering / partitioning / packing** ($a_{ij} = 1$ if set $j$ contains element $i$):
$$
\begin{aligned}
\text{covering:}\quad & \min c^T x,\ Ax \ge 1,\ x \text{ binary} && \text{every element at least once} \\
\text{partitioning:}\quad & \min c^T x,\ Ax = 1,\ x \text{ binary} && \text{exactly once} \\
\text{packing:}\quad & \max c^T x,\ Ax \le 1,\ x \text{ binary} && \text{at most once}
\end{aligned}
$$

---

# Part 5: Conditional constraints and big-M

> "If more than 200 students are assigned to zone A, then at least 300 must go to zone B."

A linear inequality is always active, so a condition has to be modelled with a binary switch.

$$X \le 200 + M\cdot t \qquad t \in \{0,1\}$$

- $t = 0$: $X \le 200$, the constraint applies.
- $t = 1$: $X \le 200 + M$. If $M$ is large enough, this restricts nothing.

## Choosing M

With 800 students, $X$ (students in zone A) is at most 800.

- $M = 800$: $t = 1$ gives $X \le 1000$, no restriction.
- $M = 50$: $t = 1$ gives $X \le 250$, which still cuts off legal solutions. Wrong.

$M$ must be at least the largest amount the switched-off constraint may need to allow (here $800 - 200 = 600$).

1. $M$ is a constant, not a variable. Do not list it among the variables.
2. Justify its size in one clause: "$M = 800$ works since there are only 800 students." SS24's solution: *"for M we can choose any fixed number greater or equal 4."*

Take $M$ from another quantity when possible: $M = \lvert J\rvert$, $M = c_i$, $M = \sum_j w_j$. When a variable is bounded by 1 (a fraction), $M = 1$ (2026 P2a: $x_i \le y_i$).

---

# Part 6: Indicator variables in both directions

Goal: a binary $t$ that is 1 exactly when $X > K$. That needs two inequalities:

$$
\begin{aligned}
\text{(A)}\quad & X \le K + M\cdot t && X > K \text{ forces } t = 1 \\
\text{(B)}\quad & X \ge (K+1)\cdot t && t = 1 \text{ forces } X > K
\end{aligned}
$$

**(A)**, with $K = 200$, $M = 800$, $X = 500$: $t = 0$ gives $500 \le 200$, violated, so $t = 1$.

**(B)** reads $X \ge 201\cdot t$: $t = 1$ is allowed only if $X \ge 201$; $t = 0$ gives $X \ge 0$.

## When one direction is enough

- (A) alone allows $t = 1$ with $X = 0$. Harmless only if $t = 1$ is costly in the objective.
- (B) alone allows $X = 800$ with $t = 0$. Harmless only if $t = 1$ is rewarded.

| Wording | Write |
|---|---|
| "indicates whether", "if and only if", $\iff$ | both (A) and (B) |
| "if more than K …, then …" | (A), plus the consequence keyed on $t$ |
| unsure | both; writing both is never penalised |

## The K+1

"More than 200" for an integer number of students means "at least 201". For continuous $X$, "$t = 1$ iff $X > K$" cannot be modelled exactly, because $\{X > K\}$ is not closed. The usual workaround uses a small $\varepsilon$: $X \ge (K + \varepsilon)\cdot t$.

## Threshold on a continuous variable (2026 P2j)

With $x \in [0,1]$ and threshold $\tau$, a switch $z$ with $z = 0$ below $\tau$ and $z = 1$ above:
$$
\begin{aligned}
x &\le \tau + z \\
x &\ge \tau\cdot z
\end{aligned}
$$
At exactly $x = \tau$ both values of $z$ are allowed.

---

# Part 7: Answer template

The SS24 and SS25 official solutions use this shape:

$$
\begin{aligned}
& \text{We introduce } z \in \{0,1\}: \ z = 1 \iff \langle \text{meaning in a full sentence} \rangle \\
& \qquad \langle \text{linking constraints tying } z \text{ to the other variables} \rangle \\
& \text{Then:} \\
& \qquad \langle \text{the requirement, keyed on } z \rangle \quad \forall i \in \langle \text{explicit range} \rangle
\end{aligned}
$$

The meaning in words, the linking constraints, and the $\forall$ with an explicit range are marked separately. The prompt says every year:

> *"You may introduce additional variables. If you do so, please also state their intuitive meaning in words."*

| Kind | Who supplies it | Example |
|---|---|---|
| decision | given in the exam | $x_{i,j}$ = student $j$ to school $i$ |
| indicator | you | $t = 1$ if more than 100 students at school 1 |
| shorthand | you, for readability | $X_i = \sum_j x_{i,j}$ |

A sub-question worth more than 2 points usually needs an indicator.

---

# Part 8: Patterns, by exam frequency

Full catalogue: [05-ip-modeling](05-ip-modeling.md).

## On every paper

$$
\begin{aligned}
&\text{exactly one} && \sum_{i\in I} x_{i,j} = 1 && \forall j \in J \\
&\text{at most once} && \sum_{j\in J} x_{i,j} \le 1 && \forall i \in I \\
&\text{at most } k && \sum_i z_i \le k \\
&\text{capacity} && \sum_{j\in J} x_{i,j} \le c_i && \forall i \in I \\
&\text{only-if / linking} && x_{i,j} \le y_i && \forall i \in I,\ j \in J
\end{aligned}
$$

Use the disaggregated linking form by default.

Answer each sub-question separately. SS25 asked capacity in (b) and linking in (c); the combined $\sum_j x_{i,j} \le c_i\cdot y_i$ covers both but does not answer the part asked.

## Frequent

**Implication between binaries.** $A \Rightarrow B$ is $A \le B$. With several premises:
$$
\begin{aligned}
& \text{"if W and R both hold, then P or C must hold"} \\
& \qquad W_t + R_t - 1 \le P_t + C_t \qquad \forall t \\
& \text{general:} \quad \textstyle\sum(\text{premises}) - (\#\text{premises} - 1) \le \sum(\text{conclusions})
\end{aligned}
$$
If $W = R = 1$ the left side is 1, forcing $P + C \ge 1$. If either premise is 0, nothing is forced.

**Pairwise conflict** (SS25 E4d, built schools at least 5 km apart):
$$
\begin{aligned}
& d \text{ known when modelling:} \\
& \qquad y_i + y_{i'} \le 1 && \forall i \ne i' \text{ with } d_{i,i'} < 5 \\
& d \text{ symbolic (official solution):} \\
& \qquad 1 + z_{i,i'} \ge y_i + y_{i'} && \forall i \ne i' && \text{(both built} \Rightarrow z = 1\text{)} \\
& \qquad d_{i,i'}\cdot z_{i,i'} \ge 5\cdot z_{i,i'} && \forall i \ne i'
\end{aligned}
$$

**At most one of two** (2026 P2f): $y_A + y_B \le 1$. **If A then B** (2026 P2g): $y_A \le y_B$. **At most k selected** (2026 P2h): $\sum_i y_i \le k$.

**Semi-continuous: zero or at least q** (2026 P2a + P2d):
$$
\begin{aligned}
x_i &\le y_i && (M = 1 \text{ since } x_i \le 1) \\
x_i &\ge q\cdot y_i
\end{aligned}
$$
Together: $x_i = 0$ or $x_i \ge q$.

**Adjacency in a sequence** (SS24 P4d, consecutive stages within 200 km):
$$
\begin{aligned}
& x_{i,j} + x_{i',j+1} \le 1 + y_{i,i'} && \forall i,i' \in I,\ j \in J\setminus\{21\} \\
& y_{i,i'}\cdot d_{i,i'} \le 200 && \forall i,i' \in I \\
& \text{alternative:} \\
& (x_{i,j} + x_{i',j+1} - 1)\cdot d_{i,i'} \le 200
\end{aligned}
$$
$J\setminus\{21\}$: stage 21 has no successor. Truncated ranges are marked, as are lookbacks (a 3-year history condition runs $\forall t \in \{4,\dots,15\}$).

**Either-or:**
$$
\begin{aligned}
f_1(x) &\le b_1 + M\cdot z \\
f_2(x) &\le b_2 + M\cdot(1 - z)
\end{aligned}
$$
Exactly one constraint is switched off, so at least one holds.

**Exactly one of two conditions** (SS24 P4e, "more than 3 mountain routes in either the first or the last 7 stages, not both"):
$$
\begin{aligned}
& \sum_i \sum_{j=1}^{7} m_i x_{i,j} \le 3 + M\cdot z && z = 1 \iff \text{first block exceeds 3} \\
& \sum_i \sum_{j=1}^{7} m_i x_{i,j} \ge 4z \\
& \sum_i \sum_{j=15}^{21} m_i x_{i,j} \le 3 + M(1-z) && \text{mirrored with } 1 - z \\
& \sum_i \sum_{j=15}^{21} m_i x_{i,j} \ge 4(1-z)
\end{aligned}
$$
The $z$ / $1-z$ mirror gives the exclusivity. A two-binary version with $z_1 + z_2 = 1$ also scores. $m_i$ is a 0/1 parameter, so $\sum m_i x_{i,j}$ counts mountain routes.

**Ratios.** Clear the denominator so all coefficients are constants:
$$
\begin{aligned}
& \text{"beer type w is at most 40\% of all barrels sold"} \\
& \qquad \sum_b w_b \le 0.4\cdot\sum_b (w_b + h_b + s_b + a_b) \\
& \Rightarrow\ 0.6 \sum_b w_b - 0.4 \sum_b (h_b + s_b + a_b) \le 0
\end{aligned}
$$

**Rolling window** ("at most 5 in any 7 consecutive days"):
$$\sum_{t=k}^{k+6} x_t \le 5 \qquad \forall k \in \{1,\dots,T-6\}$$
The window starts at $k$ and must stop at $T-6$. The $\forall k$ and its upper limit are the points.

**Product linearisation:**
$$
\begin{aligned}
&\text{binary × binary:} && Y \le x_k,\ Y \le x_l,\ Y \ge x_k + x_l - 1 \\
&\text{binary } z \text{ × continuous } a \in [0,U]: && w \le a,\ w \le U\cdot z,\ w \ge a - U(1-z),\ w \ge 0
\end{aligned}
$$
Binary case check: if either $x$ is 0, $Y = 0$; if both are 1, $Y \ge 1$. For $a \in [0,1]$ (2026 P2i): $w \le x$, $w \le z$, $w \ge x + z - 1$, $w \ge 0$.

If the objective rewards $Y$ (max), $Y \le x_k$ and $Y \le x_l$ suffice; if it penalises $Y$, $Y \ge x_k + x_l - 1$ suffices. Writing all three is safe; stating which direction the objective handles earns the understanding point.

A product of two continuous variables cannot be linearised exactly. That is why 2026 P2j first introduces a binary switch and then linearises $z\cdot x$ with P2i.

**Fixed charge / minimum lot size:**
$$
\begin{aligned}
& x \le M\cdot y,\ x \ge q\cdot y,\ y \in \{0,1\} && \text{produce nothing, or at least } q \\
& \text{objective: } \min f\cdot y + c\cdot x && f \text{ paid once if } x > 0
\end{aligned}
$$

**Startup detection:** $y_t \ge x_t - x_{t-1}$ for $t \ge 2$. Off at $t-1$ and on at $t$ forces $y_t = 1$. In a min problem with a positive setup cost this inequality alone suffices; say so.

## Objectives

**Weighted sum** (SS25 E4f):
$$
\begin{aligned}
& \text{"minimise construction cost while maximising preference"} \\
& \qquad \min \sum_i f_i y_i - \sum_i \sum_j s_{i,j} x_{i,j}
\end{aligned}
$$
Minimising the negative preference maximises it. Weights must be positive; their ratio sets the trade-off ("twice as important" $\Rightarrow$ weight 2 vs 1).

**Position-dependent coefficients** (SS24 P4a, towns pay $p_i$, double for the first or last stage):
$$\max \sum_{i\in I} p_i \cdot \left( 2\cdot x_{i,1} + \sum_{j=2}^{20} x_{i,j} + 2\cdot x_{i,21} \right)$$
The middle sum runs $j = 2..20$ because stages 1 and 21 are counted at the doubled rate.

---

# Part 9: Worked example, SS25 E4

> 15 sites, capacity $c_i$, cost $f_i$. 800 students, preference $s_{i,j}$. Given: $y_i$ = build site $i$, $x_{i,j}$ = assign student $j$ to school $i$.

**(a) Each student is assigned to exactly one school.** Column sum, free index $j$:
$$\sum_{i\in I} x_{i,j} = 1 \qquad \forall j \in J \qquad \text{(800 constraints)}$$

**(b) Capacity.** Row sum, free index $i$:
$$\sum_{j\in J} x_{i,j} \le c_i \qquad \forall i \in I \qquad \text{(15 constraints)}$$

**(c) Assign only to built schools.**
$$x_{i,j} \le y_i \qquad \forall i \in I,\ j \in J \qquad \text{(12,000 constraints)}$$
If $y_i = 0$, every $x_{i,j} = 0$.

**(d) Built schools at least 5 km apart.**

Introduce $z_{i,i'} \in \{0,1\}$: $z_{i,i'} = 1$ if schools $i$ and $i'$ are both built.
$$
\begin{aligned}
1 + z_{i,i'} &\ge y_i + y_{i'} && \forall i,i' \in I,\ i \ne i' \\
d_{i,i'}\cdot z_{i,i'} &\ge 5\cdot z_{i,i'} && \forall i,i' \in I,\ i \ne i'
\end{aligned}
$$
The second row reads $d \ge 5$ when $z = 1$, so two schools closer than 5 km cannot both be built.

**(e) If more than 200 students go to zone A, at least 300 go to zone B** ($A_i \in \{0,1\}$ marks zone-A sites):

Introduce $t \in \{0,1\}$: $t = 1$ if more than 200 students are assigned to zone-A schools.
$$
\begin{aligned}
& \sum_i \sum_j A_i x_{i,j} \le 200 + M\cdot t \\
& \sum_i \sum_j A_i x_{i,j} \ge 201\cdot t \\
& \text{Then:} \\
& \sum_i \sum_j (1-A_i) x_{i,j} \ge 300\cdot t
\end{aligned}
$$
$M = 800$ works, since there are only 800 students.

$(1 - A_i)$ selects zone-B sites. The last row imposes nothing when $t = 0$.

**(f) One objective:**
$$\min \sum_i f_i y_i - \sum_i \sum_j s_{i,j} x_{i,j}$$

---

# Part 10: Traps and practice

## Where points are lost

1. Missing or wrong $\forall$ range ($j \in J\setminus\{21\}$, $t \in \{4,\dots,15\}$).
2. Auxiliary variable without a meaning in words.
3. $M$ written as a variable, or its size not justified.
4. Only one direction of an indicator when the wording says "if and only if".
5. A product of variables left in the model.
6. Answering a different sub-question than the one asked.

## When stuck

1. Which index is free? That gives the $\forall$.
2. Row or column sum? Draw the grid.
3. Conditional? Introduce a binary, state its meaning, write both directions.
4. Check the 0/1 cases against the text.
5. Write something structured; partial credit is common on this question.

## Papers (one minute per point)

1. 2026 P2 *Glass Production* (21 min)
2. SS25 E4 *School Planning* (21 min)
3. SS24 P4 *Tour d'Allemagne* (22 min)
4. SS23 E2 *Ice cream production* (22 min)
5. SS21 A3 *Biergärten* (35 min)

## From blank paper

1. exactly-one assignment
2. capacity linked to a build decision
3. disaggregated "only if" linking
4. pairwise exclusion from a distance parameter
5. "if $A > k$ then $B \ge m$": both indicator directions plus the consequence
6. two goals in one objective, with the sign flip explained
7. linearising binary × continuous

## Facts used elsewhere

1. The LP relaxation of a max IP is an upper bound; same integer set with a smaller relaxation region means a stronger formulation.
2. Deciding IP feasibility is NP-hard; SAT reduces to binary IP.
3. TU + integral $b$ $\Rightarrow$ the LP relaxation's vertices are integral $\Rightarrow$ the IP is solvable as an LP. That is why assignment and network flow are easy and GAP and bin packing are not ([08](08-total-unimodularity-and-matroids.md)).
