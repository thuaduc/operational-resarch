# Sensitivity analysis

Companion to [03-revised-simplex-and-sensitivity](03-revised-simplex-and-sensitivity.md).

Exam slot E2, 14–19 points. You are never asked to run simplex; you get a final tableau and are asked what happens when the data changes.

Parts 1–9 use one small example throughout. Part 10 is the exam question.

---

# Part 0: What sensitivity analysis is

After solving an LP, typical questions are:

- Is buying more of a resource worth it, and at what price per unit?
- If a product's price changes, does the plan change?
- Should a new product be introduced?

Sensitivity analysis answers these from the final tableau without re-solving.

---

# Part 1: The example

A workshop makes tables and chairs.

```
each table  uses 1 unit of wood, 1 hour of labour, earns €3
each chair  uses 1 unit of wood, 2 hours of labour, earns €4

available:  4 units of wood,  6 hours of labour
```

$$
\begin{aligned}
\max\quad & 3x_1 + 4x_2 \\
\text{s.t.}\quad & x_1 + x_2 \le 4 && \text{wood} \\
& x_1 + 2x_2 \le 6 && \text{labour} \\
& x_1, x_2 \ge 0
\end{aligned}
$$

Slack variables (unused amounts):
$$
\begin{aligned}
x_1 + x_2 + s_1 &= 4 \\
x_1 + 2x_2 + s_2 &= 6
\end{aligned}
$$

| plan | wood used | labour used | profit |
|---|---|---|---|
| (0,0) | 0 | 0 | 0 |
| (4,0) | 4 | 4 | 12 |
| (0,3) | 3 | 6 | 12 |
| **(2,2)** | **4** | **6** | **14** |

Optimum: 2 tables, 2 chairs, profit €14. Both resources are fully used ($s_1 = s_2 = 0$).

---

# Part 2: Where the tableau comes from

Basic variables (non-zero at the optimum): $x_1, x_2$. Non-basic: $s_1, s_2$.

$B$ is the matrix of basic columns (rows wood, labour; columns $x_1, x_2$):
$$
B = \begin{pmatrix} 1 & 1 \\ 1 & 2 \end{pmatrix}, \qquad
B^{-1} = \begin{pmatrix} 2 & -1 \\ -1 & 1 \end{pmatrix} \quad (\det B = 1)
$$

**Plan:** $b' = B^{-1}b = (2 \cdot 4 - 6, -4 + 6) = (2, 2)$.

**Prices:** $y^T = c_B^T B^{-1}$ with $c_B = (3, 4)$:

$$
\begin{aligned}
y_1 &= 3 \cdot 2 + 4 \cdot (-1) = 2 \\
y_2 &= 3 \cdot (-1) + 4 \cdot 1 = 1
\end{aligned}
$$

**Profit:** $z = y^Tb = 2 \cdot 4 + 1 \cdot 6 = 14$.

## Final tableau

```
 Basis │ x₁  x₂   s₁   s₂ │ RHS
───────┼──────────────────┼─────
 Row 0 │  0   0    2    1 │  14
───────┼──────────────────┼─────
   x₁  │  1   0    2   −1 │   2
   x₂  │  0   1   −1    1 │   2
```

- RHS column $(2, 2) = B^{-1}b$, the plan
- body under $s_1, s_2$ = $B^{-1}$ (the slack columns started as the identity)
- Row 0 under $s_1, s_2$ = $(2, 1) = y$
- Row 0 under $x_1, x_2$ = 0 (always 0 under basic columns)

---

# Part 3: Row 0 and the sign convention

$$\text{Row0}_j = y^Ta_j - c_j$$

For a max problem:
$$
\begin{aligned}
\text{optimal} &\iff \text{every Row-0 entry} \ge 0 \\
\text{worth bringing in} &\iff \text{Row-0 entry} < 0
\end{aligned}
$$

Row 0 here is $(0, 0, 2, 1)$: optimal.

- Under a basic column, Row 0 is 0.
- Under slack column $s_i$, Row 0 is $y_i$.

---

# Part 4: Shadow prices

$y_i$ is the extra profit from one more unit of resource $i$:

$$
\begin{aligned}
y_1 &= 2 && \text{one more unit of wood is worth €2} \\
y_2 &= 1 && \text{one more hour of labour is worth €1}
\end{aligned}
$$

Check with 5 units of wood: $B^{-1}(5,6)^T = (4, 1)$, profit $3 \cdot 4 + 4 \cdot 1 = 16$. That is $14 + 2 = 14 + y_1$.

Since $z = y^Tb$, $\partial z / \partial b_i = y_i$. So you would pay up to €2 per extra unit of wood.

## Link to duality

$y$ is the optimal dual solution. Complementary slackness in tableau form:

$$
\begin{aligned}
&\text{resource not fully used } (s_i > 0, \text{ basic}) &&\Rightarrow\; y_i = 0 \\
&\text{resource fully used } (s_i = 0, \text{ non-basic}) &&\Rightarrow\; y_i \ge 0
\end{aligned}
$$

With 100 units of wood, $s_1$ would be basic and $y_1 = 0$: spare wood is worth nothing at the margin.

---

# Part 5: Which half of the tableau a change affects

| change | effect | what can break |
|---|---|---|
| a resource amount $b$ | the plan moves ($x_B = B^{-1}b$), Row 0 does not | only feasibility (a basic variable $< 0$) |
| a price $c$ | Row 0 moves, the plan does not | only optimality (a Row-0 entry $< 0$) |

Both ranging procedures push the change until the affected half breaks.

---

# Part 6: RHS ranging

The shadow price is valid only while the basis stays optimal.

1. Take the tableau column of slack $s_1$ ($= B^{-1}e_1$).
2. New plan: $x_B(\delta) = b' + \delta \cdot (\text{that column})$.
3. Require every entry $\ge 0$.
4. Intersect to get the $\delta$ range.

The $s_1$ column is $(2, -1)$:

$$
\begin{aligned}
x_1(\delta) &= 2 + 2\delta \ge 0 &&\Rightarrow\; \delta \ge -1 \\
x_2(\delta) &= 2 - \delta \ge 0 &&\Rightarrow\; \delta \le 2
\end{aligned}
$$

$\delta \in [-1, 2]$, so wood $\in [3, 6]$. Inside the range $z(\delta) = 14 + 2\delta$.

At the endpoints:

$$
\begin{aligned}
\delta = -1 \;(\text{wood } 3) &\;\to\; x_1 = 0 && \text{no tables} \\
\delta = +2 \;(\text{wood } 6) &\;\to\; x_2 = 0 && \text{no chairs}
\end{aligned}
$$

Check $\delta = 2$: $B^{-1}(6,6)^T = (6, 0)$, profit $18 = 14 + 2 \cdot 2$.

At the endpoints a basic variable is zero, so the basis is degenerate (SS25 E2d).

---

# Part 7: Cost ranging

## Case A: basic variable ($x_1$)

Changing a basic variable's price changes $y$ and therefore all of Row 0.

1. Take $x_1$'s tableau row under the non-basic columns: $(2, -1)$.
2. New Row 0 = old Row 0 $+ \Delta \cdot$ (that row).
3. Require $\ge 0$.

Table price $3 \to 3 + \Delta$:

$$
\begin{aligned}
\text{under } s_1:&\quad 2 + 2\Delta \ge 0 &&\Rightarrow\; \Delta \ge -1 \\
\text{under } s_2:&\quad 1 - \Delta \ge 0 &&\Rightarrow\; \Delta \le 1
\end{aligned}
$$

The table price can range over [€2, €4] with plan $(2, 2)$ optimal. Inside the range $z(\Delta) = 14 + \Delta \cdot x_1 = 14 + 2\Delta$.

Check at €5 ($\Delta = 2$): Row 0 under $s_2$ becomes $-1$, so the basis changes. Indeed 4 tables and no chairs give $5 \cdot 4 = 20 > 5 \cdot 2 + 4 \cdot 2 = 18$.

## Case B: non-basic variable

Only its own Row-0 entry changes:
$$c'_j - \Delta \ge 0 \iff c_j \le y^Ta_j$$
One-sided; the plan and profit do not change inside the range.

| | basic | non-basic |
|---|---|---|
| what moves | all of Row 0, via the variable's row | one Row-0 entry |
| range | two-sided | one-sided |
| $z$ inside | $z + \Delta \cdot x_k$ | unchanged |
| plan inside | unchanged | unchanged |

---

# Part 8: A new product

A stool needs 1 wood and 1 labour and sells for $p$.

$$
\begin{aligned}
\text{opportunity cost} &= y^Ta_{\text{new}} = 2 \cdot 1 + 1 \cdot 1 = 3 \\
c'_{\text{new}} &= y^Ta_{\text{new}} - c_{\text{new}} = 3 - p \\
\text{enters} &\iff c'_{\text{new}} < 0 \iff p > 3
\end{aligned}
$$

If making one also costs $k$ (e.g. labour charges), the break-even price is $p \ge y^Ta_{\text{new}} + k$.

**Degeneracy:** a basic feasible solution is degenerate when a basic variable is zero in the RHS column. The endpoints of an RHS range are exactly such points.

---

# Part 9: The sensitivity chain

1. Shadow price of resource $i$?
   - Row 0 under $s_i$ [Part 4]
2. Range of $b_i$ where it is valid?
   - RHS ranging [Part 6]
3. Push $b_i$ past the range.
   - the basic variable that hits zero leaves
   - entering variable by the dual ratio test: in the leaving row, among entries $< 0$, pick the one minimising $\lvert c'_j / \text{row}_j \rvert$
   - one pivot
4. New shadow price and range?
   - read the new Row 0; repeat step 2 on the new basis

Check for step 4: in a max problem, reducing a resource can only raise its shadow price.

---

# Part 10: The exam question, SS25 E2

$$
\begin{aligned}
\max\quad & 0x_1 + 0x_2 \\
\text{s.t.}\quad & 4x_1 + 2x_2 \le 100 && \text{(water)} \\
& 1x_1 + 2x_2 \le 40 && \text{(sugar)} \\
& 1x_1 \le 30 && \text{(lemons)} \\
& 0.5x_2 \le 20 && \text{(tea leaves)}
\end{aligned}
$$

All profits are zero (the drinks are free), so this is a feasibility problem. With $c_B = 0$:

$$
y = c_B^TB^{-1} = 0, \qquad \text{Row 0} = 0
$$

Given tableau with basis $\{x_1, x_2, s_3, s_4\}$ and a wrong Row 0:

```
 Basis │ x₁  x₂    s₁     s₂   s₃  s₄ │ RHS
 Row 0 │  0  −1     2      1    0   0 │   0     wrong
   x₁  │  1   0   1/3   −1/3    0   0 │  20
   x₂  │  0   1  −1/6    2/3    0   0 │  10
   s₃  │  0   0  −1/3    1/3    1   0 │  10
   s₄  │  0   0  1/12   −1/3    0   1 │  15
```

**(a) Correct Row 0.** All costs are zero, so every entry $y^Ta_j - c_j = 0$:
```
 Row 0 │  0   0    0    0    0   0 │  0
```

**(b) Optimal?** Yes, every entry is $\ge 0$.

**(c) Range of the lemonade price $p_1$.** $c_B = (p_1, 0, 0, 0)$; $x_1$ is basic (Case A). Its row under the non-basic columns ($s_1, s_2$) is $(1/3, -1/3)$:

$$
\begin{aligned}
\text{under } s_1:&\quad p_1/3 \ge 0 &&\Rightarrow\; p_1 \ge 0 \\
\text{under } s_2:&\quad -p_1/3 \ge 0 &&\Rightarrow\; p_1 \le 0
\end{aligned}
$$

So $p_1 = 0$: any positive lemonade price changes the basis.

**(d) Water capacity until the BFS is degenerate.** Water is constraint 1, so use the $s_1$ column:

$$
\begin{aligned}
x_1:&\quad 20 + \delta/3 \ge 0 &&\Rightarrow\; \delta \ge -60 \\
x_2:&\quad 10 - \delta/6 \ge 0 &&\Rightarrow\; \delta \le 60 \\
s_3:&\quad 10 - \delta/3 \ge 0 &&\Rightarrow\; \delta \le 30 \\
s_4:&\quad 15 + \delta/12 \ge 0 &&\Rightarrow\; \delta \ge -180
\end{aligned}
$$

$\delta \in [-60, 30]$, water between 40 and 130. At the endpoints:

$$
\begin{aligned}
\delta = -60 \;(\text{water } 40) &\;\to\; x_1 = 0 \\
\delta = +30 \;(\text{water } 130) &\;\to\; s_3 = 0
\end{aligned}
$$

**(e) New drink** with 3 L water, 1 kg lemon, price $p_S$. With $y = 0$:

$$c'_S = y^Ta_{\text{new}} - p_S = -p_S$$

Enters iff $p_S > 0$. With all prices zero, the resources have no opportunity cost.

---

# Part 11: Traps and practice

## Where points are lost

1. Wrong sign convention. Course: $\text{Row0} = y^Ta_j - c_j$, optimal $\iff$ all $\ge 0$.
2. Wrong slice: RHS ranging uses the slack's column; cost ranging for a basic variable uses its row.
3. Using $\Delta z = y_i\delta$ outside the validity range.
4. Treating a basic variable's cost range like a non-basic one.
5. Not naming the variable that hits zero at a range endpoint.
6. Running simplex when it is not asked.

## Formulas

$$
\begin{aligned}
y^T &= c_B^T B^{-1} && \text{shadow prices} \\
\text{Row0}_j &= y^Ta_j - c_j && \text{optimal} \iff \text{all} \ge 0 \text{ (max)} \\
\text{Row 0 under slack } s_i &= y_i \\
x_B(\delta) &= b' + \delta \cdot (\text{column of } s_i) \ge 0 && \text{RHS ranging} \\
z(\delta) &= z + y_i \cdot \delta \\
c'_{\text{new}} &= y^Ta_{\text{new}} - c_{\text{new}} && \text{enters iff } c_{\text{new}} > y^Ta_{\text{new}}
\end{aligned}
$$

## Self-test on the workshop tableau

```
 Basis │ x₁  x₂   s₁   s₂ │ RHS
 Row 0 │  0   0    2    1 │  14
   x₁  │  1   0    2   −1 │   2
   x₂  │  0   1   −1    1 │   2
```

1. What is one extra hour of labour worth?
2. Over what range of labour does that hold? ($s_2$ column $(-1, 1)$)
3. A new product needs 2 wood and 1 labour. Minimum price to be worth making?
4. How far can the chair price move before the plan changes? ($x_2$ row $(-1, 1)$)

Answers: 1) $y_2 = 1$. 2) labour $\in [4, 8]$. 3) $2 \cdot 2 + 1 \cdot 1 = 5$. 4) chair price $\in [3, 6]$.

## Exercises (untimed)

1. `T3.3` *Sensitivity* `[DRILL]`: ranging only. Start here.
2. `T3.2` *Lemonade Production* `[EXAM]`: closest match to SS25 E2.
3. `D3.2` *Waldgeist Distillery* `[EXAM]`: the full chain.
4. `S3.5` *PopCo* `[EXAM]`: second run of the chain.
5. `D3.1` / `T3.1` / `S3.2` *Revised Simplex* `[CONCEPT]`: you need the Row 0 formula, not the algorithm.

## Papers (timed)

- SS25 E2 (16): the wrong-Row-0 format.
- SS21 A1 *Backmischung* (16): includes RHS ranging.
- Midterm SS25 P5 and SS26 P4: the midterms have more sensitivity than the endterms.
