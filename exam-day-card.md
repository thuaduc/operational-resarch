
# §1 · LP basics and simplex

### Conversions

| From               | To                                               | Note                               |
| ------------------ | ------------------------------------------------ | ---------------------------------- |
| `aᵀx ≤ b`          | `aᵀx + s = b`, `s ≥ 0`                           | **slack** — is a basis column      |
| `aᵀx ≥ b`          | `aᵀx − s = b`, `s ≥ 0`                           | **surplus** — *not* a basis column |
| `aᵀx ≥ b`, `b < 0` | flip the row by `−1` **first**, then add a slack |                                    |
| `aᵀx = b`          | `aᵀx + w = b`, `w ≥ 0`                           | **artificial** — start basis only  |


---

# §2 · Reading a tableau

1. **All Row-0 entries `≥ 0`?** → optimal
2. **Basis column ↔ RHS column**, row by row → the basic variables and their values.
3. **Everything not in the Basis column = 0.** State these too.
4. **Row 0's RHS entry** → `z`.

| Where in Row 0 | What it is |
|---|---|
| under a **basic** column | always `0` |
| under **slack `sᵢ`** | `yᵢ` — the shadow price |

---

# §3 · Sensitivity

### Shadow prices

> **`yᵢ` = the marginal value of one more unit of resource `i`.**
> Raise `bᵢ` by 1 and the optimal objective improves by `yᵢ` — while the basis stays optimal.

It is a **derivative**, not an analogy: `z = yᵀb`, so `∂z/∂bᵢ = yᵢ`.

**The economic reading:** `yᵢ` is the most you would pay for one extra unit of resource `i`. Above that price, buying it loses money. That's the business answer the whole topic exists to give.

**Five names, one object.** The exam and the lectures use these interchangeably:

```
shadow price  ·  dual variable  ·  simplex multiplier  ·  opportunity cost  ·  π
```

It genuinely **is** the optimal dual solution from §4 — the same numbers you would get by solving
(D) directly.

### RHS ranging and degeneracy

**Degenerate BFS** = a **basic** variable equal to **zero** in the RHS column. Normally basic
means positive and non-basic means zero; degeneracy is when that split breaks. It's the boundary:
one more nudge and the variable goes negative and the basis loses feasibility.

**Perturbation** = replacing `bᵢ` by `bᵢ + δ` and asking how far `δ` can go. When you shift `bᵢ`,
every basic value moves by `δ ×` the entry in the **`sᵢ` column** — that column holds `B⁻¹`.

```
for every basic row:   RHS_row + δ · (sᵢ column entry) ≥ 0
```

Solve each for `δ`, intersect → the **range of `bᵢ` for which the basis stays optimal**.

> **The two endpoints of that range are exactly where the BFS becomes degenerate** — that's the
> same calculation, so one table answers both questions. The variable whose inequality **binds**
> is the one that hits zero, and it's the one that would **leave** the basis.

Uses only `B⁻¹` and `b` — **never `c`.** Zero prices change nothing here.

**Worked (SS25 E2d — water, constraint 1, so read the `s₁` column):**

```
      s₁      RHS
x₁ │  1/3  │  20        20 + δ/3   ≥ 0   ⟹   δ ≥  −60   ←  binds below
x₂ │ −1/6  │  10        10 − δ/6   ≥ 0   ⟹   δ ≤   60
s₃ │ −1/3  │  10        10 − δ/3   ≥ 0   ⟹   δ ≤   30   ←  binds above
s₄ │  1/12 │  15        15 + δ/12  ≥ 0   ⟹   δ ≥ −180

δ ∈ [−60, +30]      water from 40 to 130
δ = −60  → x₁ hits zero        δ = +30  → s₃ hits zero
```

Push `bᵢ` **past** the range and that variable leaves; pick the entering one by the **dual ratio
test** — in the leaving row, among entries `< 0`, minimise `|c'_j / row_j|` — then pivot once.

# §4 · Duality

### Deriving the dual

1. `max` ↔ `min`.
2. One dual **variable** per primal **constraint** — sign restrictions don't count.
3. One dual **constraint** per primal **variable**.
4. Dual constraint `j` reads **down column `j`** of `A`.
5. Swap `c` and `b`.

### SOB — classify, then translate

| | S (sensible) | O (odd) | B (bizarre) |
|---|---|---|---|
| **variable** | `x ≥ 0` | `x` free | `x ≤ 0` |
| **constraint in max** | `≤` | `=` | `≥` |
| **constraint in min** | `≥` | `=` | `≤` |

| Type | primal **constraint** → dual **variable** | primal **variable** → dual **constraint** |
|---|---|---|
| S | `y ≥ 0` | sensible direction |
| O | `y` free | `=` |
| B | `y ≤ 0` | bizarre direction |

**Sensible direction** = `≥` if the dual is a min, `≤` if it's a max. Bizarre is the opposite.

### Theorems

```
weak     cᵀx ≤ bᵀy      for ALL feasible x and y
strong   cᵀx* = bᵀy*    at optimality
```

> **Weak:** every feasible dual is a **ceiling** on every feasible primal. They never cross.
> **Strong:** at the optimum the ceiling is **reached** — both values are the same number.

So if you find an `x` and a `y` with `cᵀx = bᵀy`, **both are optimal.** One line, done.

| If the primal is | Then the dual is |
|---|---|
| unbounded | infeasible |
| infeasible | unbounded **or** infeasible |
| feasible, and so is D | both finite |

### Complementary slackness

```
primal CS   ((Aᵀy)_j − c_j) · x_j = 0     per variable
dual CS     ((Ax)_i − b_i) · y_i = 0      per constraint
```

**What it's for in the exam:** they give you an `x*` and ask "is it optimal?". CS is how you get
`y` **without solving the dual** — it turns the guess into equations:

| You see in `x*` | You write |
|---|---|
| constraint `i` has **slack** | `yᵢ = 0` |
| `x_j ≠ 0` | dual constraint `j` holds with **`=`** |

Those give you enough equations to solve for `y`. Then check `y` against the rest of the dual —
that's the recipe below.

### Showing `x*` is **not** optimal

1. Check `x*` is primal feasible — if not, you're already done.
2. Every **slack** constraint forces its `yᵢ = 0`.
3. Every `x_j ≠ 0` forces dual constraint `j` to **equality** — solve for `y`.
4. Test that `y` against the dual's **remaining constraints and sign restrictions**.

| Step 4 result | Conclusion |
|---|---|
| all satisfied | `x*` **is** optimal, `y` is the dual optimum |
| any violation | `x*` is **not** optimal ← the answer |

---

# §5 · IP modelling

### Indicator — `t = 1 ⟺ X > K`, `X` integer

```
(A)  X ≤ K + M·t        "X > K ⟹ t = 1"      forces t UP
(B)  X ≥ (K+1)·t        "t = 1 ⟹ X > K"      forces t DOWN
```

| Wording | Write |
|---|---|
| "indicates whether", "if and only if" | **both** |
| "if more than K, then …" | **(A)** plus the consequence keyed on `t` |
| unsure | **both** — never penalised |

`M` is a **constant** — justify its size (`M = |J|`, `M = cᵢ`, `M = Σwⱼ`).
For **continuous** `X`, `t = 1 ⟺ X > K` is impossible: the set isn't closed.

### Patterns

Every example below uses the **same story**, so you only have to learn the story once:
a company is deciding **which warehouses to build**, and **which warehouse serves which
customer**. Warehouses are `i ∈ I`, customers are `j ∈ J`. Two binary variables carry the
whole model:

```
y_i     ∈ {0,1}   = 1 iff warehouse i is BUILT
x_{i,j} ∈ {0,1}   = 1 iff warehouse i SERVES customer j
```

#### 1 · Counting — "how many?"

The easiest family. You are summing binaries, and a sum of binaries just *counts* how many are
switched on. So "exactly one", "at most three", "no more than `c_i`" all become one sum with a
`=` or `≤`. The only real decision is **which index you sum over** — sum over `i` to ask a
question about a customer, sum over `j` to ask about a warehouse.

```
every customer served by exactly one warehouse      Σ_i x_{i,j} = 1        ∀j
build at most 3 warehouses                          Σ_i y_i ≤ 3
warehouse i serves at most c_i customers            Σ_j x_{i,j} ≤ c_i      ∀i
only a BUILT warehouse can serve                    x_{i,j} ≤ y_i          ∀i,j
```

Read the first one aloud: *fix a customer `j`, add up all the warehouses serving them, that total
must be 1.* Because `j` is fixed inside the constraint but the requirement holds for everybody,
the `∀j` is what generates one such constraint per customer. Forgetting it is the classic lost mark.

The last line is the **linking constraint**, the single most important pattern in the chapter. It
says: if `y_i = 0` then `x_{i,j} ≤ 0`, so warehouse `i` serves nobody. If `y_i = 1` it says
`x_{i,j} ≤ 1`, which is no restriction at all. One inequality, and "you can't use what you didn't
build" is enforced. You could also write `Σ_j x_{i,j} ≤ |J|·y_i` — one constraint instead of
`|I|·|J|` — but that version is **weaker**: its LP relaxation allows fractional `y_i`, so branch
and bound has more work to do. When the exam says "give the stronger formulation", it wants the
disaggregated `x_{i,j} ≤ y_i`.

#### 2 · Logic — "if this, then that"

Here you are translating sentences with *if*, *and*, *or*, *not both*. The trick is always the
same: an implication `A ⇒ B` forbids exactly one combination, `A = 1` with `B = 0`. So write the
inequality that rules out that single row of the truth table.

```
build 1  ⟹  build 2                      y₁ ≤ y₂
build 1 AND 2  ⟹  build 3 or 4           y₁ + y₂ − 1 ≤ y₃ + y₄
1 and 2 are too close — at most one       y₁ + y₂ ≤ 1
bonus only if BOTH 1 and 2 are built      Y ≤ y₁ ,  Y ≤ y₂ ,  Y ≥ y₁ + y₂ − 1
```

`y₁ ≤ y₂` works because the only way to break it is `y₁ = 1, y₂ = 0` — precisely the forbidden
case. Everything else satisfies it.

The multi-premise version is the same idea scaled up. With two premises the left side reaches
`2 − 1 = 1` **only** when both are built, and that forces the right side to be at least 1, i.e.
at least one conclusion is chosen. If either premise is 0 the left side drops to `0` or `−1`, and
the constraint is satisfied for free. In general: `Σ(premises) − (#premises − 1) ≤ Σ(conclusions)`.

The last line **linearises a product.** `Y` is standing in for `y₁·y₂`, which is not linear and so
not allowed. The first two inequalities push `Y` down to 0 if either factor is 0; the third pushes
it up to 1 when both are 1. Which ones you actually need depends on the objective: if `Y` is
**rewarded** (the solver wants it large) the two upper bounds are enough, since it will climb on
its own. If `Y` is **penalised**, only the lower bound stops it collapsing to 0. Writing all three
is never wrong, so when in doubt write all three.

#### 3 · Big-M — "this constraint only applies sometimes"

Sometimes a limit should be enforced only when a switch is on. You can't write "if" in an LP, so
you add `M·z` to the right-hand side: when `z = 1` the bound becomes so large that the constraint
cannot bind, and it is effectively switched **off**. When `z = 0` it is back in force.

```
serve region A OR region B                f₁ ≤ b₁ + Mz  ,  f₂ ≤ b₂ + M(1−z)
produce NOTHING, or at least q            x ≤ M·y  ,  x ≥ q·y
t = 1 iff i serves more than K            Σ_j x_{i,j} ≤ K + M·t
                                          Σ_j x_{i,j} ≥ (K+1)·t
```

Notice the mirroring `z` / `(1−z)` in the either–or pair: exactly one of the two is relaxed, never
both, and that's what makes it "or".

Fixed charge is worth reading twice — it's the same shape doing something different. `x ≤ M·y`
says you can only produce if you paid to open (`y = 1`), and `x ≥ q·y` says that if you *did* open,
you must produce at least the minimum batch `q`. Together: **nothing, or at least `q`** — a gap in
the middle that a plain LP cannot express.

`M` must be a **constant you can justify**, never a variable, and never "a very big number" left
unexplained. Pick the smallest value that can't bind: `M = |J|` for a count over customers,
`M = c_i` for a capacity, `M = Σ_j w_j` for a total weight. A tighter `M` gives a tighter
relaxation, which is a real modelling mark.

#### 4 · Time — "per period, and across periods"

When the index is time, constraints usually talk about a **window** of periods rather than a
single one, so the `∀k` range has to stop early enough that the window still fits.

```
at most 5 of any 7 consecutive days       Σ_{t=k}^{k+6} x_t ≤ 5   ∀k ∈ {1,…,T−6}
pay a start-up when a machine turns on    s_t ≥ x_t − x_{t−1}     t ≥ 2
```

The rolling window is one constraint per starting day `k`, each covering `k` through `k+6`. The
range stops at `T−6` because `k = T−5` would need a day `T+1` that doesn't exist — that off-by-one
is exactly what's being tested.

Start-up detects a **change**, not a state. `x_t − x_{t−1}` equals 1 only on the period where the
machine goes from off to on, and that's the only period where `s_t` is forced up to 1. If it's
running in both periods the difference is 0, so no second charge. It starts at `t ≥ 2` because
`x₀` doesn't exist.

#### 5 · Ratios — clear the denominator

A percentage looks like a fraction, and a fraction of variables is not linear. So never leave it
divided: multiply out and collect everything on one side.

```
at least 40% of output is type 1          x₁ ≥ 0.4(x₁ + x₂)   →   0.6x₁ − 0.4x₂ ≥ 0
```

Write the fraction form first if it helps you think, but the answer must be the expanded one.

### A full model, in the exam's format

*"Build at most 3 warehouses, each serving ≤ 10 customers, everyone served, minimise cost."*

Put the pieces together in the order the marker reads them: **variables with their meaning in
words and their domain**, then the **objective**, then the **constraints, each with its `∀` and a
short label**. Nothing here is new — it's patterns 1 and 3 from above, assembled.

```
y_i     ∈ {0,1}   = 1 iff warehouse i is built
x_{i,j} ∈ {0,1}   = 1 iff warehouse i serves customer j

min  Σ_i f_i y_i  +  Σ_i Σ_j c_{ij} x_{ij}

s.t. Σ_i x_{i,j} = 1          ∀ j ∈ J        everyone served, exactly once
     Σ_j x_{i,j} ≤ 10 y_i     ∀ i ∈ I        capacity, and zero if not built
     x_{i,j} ≤ y_i            ∀ i ∈ I, j ∈ J  the strong linking
     Σ_i y_i ≤ 3                             at most three
```


---

# §6 · Branch and bound

### Bounds and branching

| Problem | Relaxation gives | Integer `c` and `x` |
|---|---|---|
| max | an **upper** bound | `OPT ≤ ⌊z_LP⌋` |
| min | a **lower** bound | `OPT ≥ ⌈z_LP⌉` |

Branch on a **fractional** `x_i = f` into `x_i ≤ ⌊f⌋` and `x_i ≥ ⌈f⌉`. No integer point is lost —
none lies strictly between. Children **inherit** every ancestor constraint.

### The three pruning rules

| | Maximise | Minimise |
|---|---|---|
| **integrality** | LP integral; update `Z*` if `Z > Z*` | update if `Z < Z*` |
| **infeasibility** | relaxation has no feasible point | same |
| **bound** | `Z_node ≤ Z*` | `Z_node ≥ Z*` |

Write **MAX** or **MIN** at the top of your answer. A tie (`Z_node = Z*`) still prunes.

### Solving a node on the plot

| Branch constraint | Line | Keep |
|---|---|---|
| `x₁ ≤ 2` | vertical at 2 | left |
| `x₁ ≥ 3` | vertical at 3 | right |
| `x₂ ≤ 1` | horizontal at 1 | below |
| `x₂ ≥ 2` | horizontal at 2 | above |

### Node order

|          | Structure             | Takes                    |
| -------- | --------------------- | ------------------------ |
| **FIFO** | queue — breadth-first | the **oldest** open node |
| **LIFO** | stack — depth-first   | the **newest** open node |

Stop as soon as the incumbent is confirmed. **State which child you push first** — the answer
depends on it.

For every node record: **node · constraint added · vertex · `Z` · which rule closed it.**

---

# §7 · Total unimodularity

`A` is **TU** ⟺ every square submatrix has determinant in `{−1, 0, +1}`. Hence every entry is in `{−1, 0, +1}`.

> **TU + integral `b` ⟹ integral polyhedron ⟹ the IP is solvable in polynomial time.**

### Proving it

| Route | What to say |
|---|---|
| **recognition** | incidence matrix of a **bipartite** or **directed** graph is TU — usually the whole answer |
| | consecutive-ones (interval matrix) is TU |
| **three conditions** | 1. entries in `{−1,0,+1}` · 2. at most 2 non-zeros per **column** · 3. rows split into `M₁, M₂`: same sign → different parts, opposite signs → same part |

**Do not write "Ghouila-Houri"** — the lecture never names it. Reproduce the three conditions.

### Disproving it

Find a **zero-free `2×2`** with `|det| ≥ 2`. Any `2×2` containing a zero always has
`det ∈ {−1,0,+1}`, so only zero-free blocks can break it.

**Closure:** `A` TU ⟹ `−A`, `Aᵀ`, `A⁻¹`, `[A, I]` are all TU.

---

# §8 · Matroids

### What it is, and what it's for

A matroid is a **finite pile of things**, plus a rule saying which **selections** from that pile count as *allowed*. Nothing more.

It exists because of one theorem:

> **Greedy — sort by weight, take anything that doesn't break the rule — finds the optimum for every weight function, if and only if the system is a matroid.**

That's why Kruskal's minimum spanning tree works. Everything below is the definition of
"greedy-friendly".

### The vocabulary

| Symbol  | Name                          | What it means                                   |
| ------- | ----------------------------- | ----------------------------------------------- |
| `E`     | **ground set**                | the finite pile you're choosing from            |
| `I`     | the **independent sets**      | the collection of allowed selections, `I ⊆ 2^E` |
| `A ∈ I` | `A` is **independent**        | this particular selection is allowed            |
| basis   | a **maximal** independent set | a member of `I` that cannot be extended         |

**"Independent" carries no meaning of its own.** It's whatever the problem declares allowed —
no cycle, no two adjacent nodes, fits inside a fixed set. Read the definition they give you.

### An example — read this before the axioms

The **graphic matroid**. Take a triangle. Let `E` be its edges, and call a set of edges
**independent** if it contains **no cycle**.

```
       a          E = { ab, ac, bc }
      / \
     b───c        independent ⟺ no cycle
```

| | |
|---|---|
| independent sets `I` | `∅`, `{ab}`, `{ac}`, `{bc}`, `{ab,ac}`, `{ab,bc}`, `{ac,bc}` |
| **not** independent | `{ab,ac,bc}` — that's the triangle, a cycle |
| **bases** | the three 2-edge sets — you can't add a third edge without closing the cycle |

Note the bases all have size **2**. That's not luck — see *Bases* below.

### The axioms

`(E, I)` is a **matroid** when all three hold:

```
(1)  ∅ ∈ I

(2)  B ∈ I  and  A ⊆ B   ⟹   A ∈ I

(3)  A, B ∈ I  with |A| < |B|   ⟹   ∃ x ∈ B \ A  with  A ∪ {x} ∈ I
```

Axioms (1) and (2) alone give an **independence system**. Adding (3) makes it a **matroid**.
### Bases

**A basis is a MAXIMAL independent set** — independent, and you cannot add anything to it and
stay independent.

> **A basis is not extra structure — it is one of the members of `I`.**
> ```
> bases ⊆ I        every basis is independent
>                  most independent sets are NOT bases
> ```
> The matroid *is* the pair `(E, I)`. Bases are derived: scan `I` for the sets that can't grow.
> Conversely, because `I` is hereditary, the bases determine `I` — it's every subset of every
> basis. Same object, described from the other end.

On the triangle: `{ab,ac}`, `{ab,bc}`, `{ac,bc}` are bases. `∅` and `{ab}` are independent but
**not** bases — they can still grow.

**Maximal is not maximum**, and the exam uses both:

| Word | Means | Where |
|---|---|---|
| **maximal** | cannot be extended | the **definition** — this is what SS23 E5a wants |
| **maximum** | largest possible size | the **theorem** below |

Writing "the largest independent set" as the definition loses the mark.

> **All bases of a matroid have the same size.** *(so in a matroid, maximal ⟹ maximum)*

---

# §9 · Knapsack DP

### What it's for

```
max Σ v_j x_j    s.t.  Σ w_j x_j ≤ W ,  x ∈ {0,1}
```

Pick items to maximise value under a weight budget. Checking all `2ⁿ` subsets is hopeless, so
you **build the answer up from smaller versions of the same problem** — that's dynamic
programming, and it works here because of **Bellman's principle**: every part of an optimal
solution is itself optimal for its subproblem.

### The table

```
B[i, w] = the best value using ONLY items 1..i, with capacity w
```

Rows = how many items you're allowed to consider. Columns = how much room you have.
**Row 0 and column 0 are all zeros** — no items, or no room, means no value.

### The recurrence — it's one decision per cell

For item `i` at capacity `w`, you either take it or you don't:

```
w_i ≤ w :   B[i,w] = max{  B[i−1, w]  ,   v_i + B[i−1, w − w_i]  }
                            ↑                ↑
                        SKIP item i      TAKE item i, and spend w_i of the room

w_i > w :   B[i,w] = B[i−1, w]          it doesn't fit at all
```

### A worked table

Three items, capacity `W = 5`:

| item | value | weight |
|---|---|---|
| 1 | 3 | 2 |
| 2 | 4 | 3 |
| 3 | 5 | 4 |

`V_max = 12`, `W_max = 5`. Five is smaller, so **index by weight**.

```
capacity:        0   1   2   3   4   5
 0 items         0   0   0   0   0   0
 + item 1        0   0   3   3   3   3
 + item 2        0   0   3   4   4   7
 + item 3        0   0   3   4   5   7
```

Two cells, to see the recurrence run:

```
item 2 (v=4, w=3) at capacity 5:
    max{ B[1,5] = 3 ,  4 + B[1,2] = 4+3 = 7 }  =  7      ← take it

item 3 (v=5, w=4) at capacity 5:
    max{ B[2,5] = 7 ,  5 + B[2,1] = 5+0 = 5 }  =  7      ← skip it
```

**Answer: `B[3,5] = 7`.**

### Backtracking — which items?

Start at `(n, W)` and walk back up:

```
B[i,w] = B[i−1,w]   →  item i was NOT taken;  move to (i−1, w)
otherwise           →  item i WAS taken;      move to (i−1, w − w_i)
```

On the table above, from `(3,5)`:

```
B[3,5]=7 = B[2,5]=7   →  item 3 not taken   →  (2,5)
B[2,5]=7 ≠ B[1,5]=3   →  item 2 TAKEN       →  (1, 5−3) = (1,2)
B[1,2]=3 ≠ B[0,2]=0   →  item 1 TAKEN       →  (0,0)  stop
```

**Items 1 and 2** — value `3+4 = 7`, weight `2+3 = 5` ✓ exactly the budget.

### Complexity — asked directly

| Index by | Cost |
|---|---|
| weight | `O(n · W_max)` |
| value | `O(n · V_max)` |

**Run whichever bound is smaller, and say why.** This is a marked step.

**Pseudopolynomial, not polynomial** — `W` is a *number*, and writing it down takes only
`log W` bits, so `O(nW)` is exponential in the input *length*. That's why knapsack stays NP-hard.

### FPTAS, if asked

```
1. θ = ε·v_max / n
2. scale the values:  v_i* = ⌊v_i / θ⌋       weights and capacity unchanged
3. run the VALUE-indexed DP on the scaled instance
4. report the chosen items at their ORIGINAL values

guarantee:  V_approx ≥ (1−ε)·V_opt
hierarchy:  P ⊆ FPTAS ⊆ PTAS ⊆ APX
```

Max clique, set cover and max independent set are **not in APX at all** — no constant-factor
approximation exists.

---

# §10 · Network flow

### What it's for

Water through pipes, traffic through roads, data through a network. **How much can you push from one point to another**, given that each link has a limit?

### The setup

```
N = (V, E, u, s, t)      u(e) = capacity of arc e
                         s = SOURCE (produces)   t = SINK (absorbs)
```

A **flow** `f` assigns a number to every arc, obeying two rules:

```
CAPACITY       0 ≤ f(e) ≤ u(e)                        can't overfill a pipe
CONSERVATION   in = out at every node EXCEPT s and t  nothing created or stored
```

```
val(f) = Σ_j f(s,j)     total leaving s — equals the total arriving at t
```

### Cuts, and why capacity is forward-only

A **cut** splits the nodes into two teams with `s` on one side, `t` on the other:

```
S = [X, V\X]        s ∈ X ,  t ∉ X
cap(S) = Σ u(i,j)   over  i ∈ X, j ∉ X       ← FORWARD ARCS ONLY
```

**Why forward only:** capacity measures what *could* cross toward `t`. An arc pointing the wrong
way cannot carry anything that direction, so it imposes no limit. Counting it inflates the cut.

### This is duality again

```
val(f) ≤ cap(S)          for EVERY flow and EVERY cut     ← weak duality
max val(f) = min cap(S)                                    ← strong duality
```

Everything reaching `t` must cross the cut somewhere, so no flow can beat any cut.

> **Find a flow and a cut with the same number and you've proved both optimal.** That's your
> verification step, and it costs one line.

### The residual network — and why backward arcs exist

Given a current flow, build what moves are still available:

```
FORWARD  arc (i,j)   capacity  u(e) − f(e)     spare room
BACKWARD arc (j,i)   capacity  f(e)            flow you could UNDO
```

Arcs of residual capacity 0 are dropped.

**Why the backward arc.** Five arcs, all capacity 1:

```
s→a   s→b   a→t   b→t   a→b
```

Push `s→a→b→t` first. Now `s→a`, `a→b`, `b→t` are full. Looking only at unused arcs: `s→b` is
free but `b→t` is full; `a→t` is free but `s→a` is full. **Stuck at value 1** — yet the true
maximum is **2** (`s→a→t` plus `s→b→t`).

The residual network has the backward arc `b→a` with capacity 1. So this path exists:

```
s→b (forward)   b→a (BACKWARD, undoing a→b)   a→t (forward)
```

Augmenting along it gives value 2. **The backward arc is the algorithm's undo button** — without
it, greedy path-finding is not correct.

### Ford–Fulkerson

```
1. Build the residual network.
2. Find ANY s–t path in it. None left → the flow is MAXIMAL, stop.
3. κ = the smallest residual capacity on that path (the bottleneck).
4. f += κ on forward arcs, f −= κ on backward arcs. Repeat from 1.
```

**Edmonds–Karp** = the same, always taking a *shortest* path (BFS).
`FF: O(|E|·U)` pseudopolynomial · `EK: O(V·E²)` polynomial.

### Reading the minimum cut for free

```
1. Take the FINAL residual network (no augmenting path remains).
2. X = every node reachable from s.
3. S = [X, V\X] is a minimum cut.
4. CHECK cap(S) = val(f).
```

It works because `t` is unreachable (so it's a genuine cut) and every arc leaving `X` must be
saturated (or it would still be in the residual network).


### Modelling transformations

| Problem | Fix |
|---|---|
| several sources / sinks | add a super-source with arcs to each `sᵢ`; likewise a super-sink |
| a **node** has a capacity | split `v` into `v_in → v_out`, joined by an arc of that capacity |
| undirected edge | replace with two opposite directed arcs |

---

# §11 · TSP

### What it's for

Visit every city once, return home, minimize distance. The canonical hard combinatorial problem
— and the chapter is really about **what to do when a problem is too hard to solve exactly**.

### Euler vs Hamilton — the cheapest marks here

```
EULERIAN    every EDGE exactly once      ←  easy   (check degrees + connectivity)
HAMILTONIAN every NODE exactly once      ←  HARD   (NP-complete)
```

> **Euler = edges. Hamilton = nodes.** Sounds alike, opposite difficulty.

**TSP** = a minimum-weight Hamiltonian cycle.

```
symmetric   c_ij = c_ji      (n−1)!/2 tours
asymmetric  c_ij ≠ c_ji      (n−1)!   tours
metric      c_ik ≤ c_ij + c_jk    ← the TRIANGLE INEQUALITY; both ratios below need it
```

### The IP, in two stages

**Stage 1 — degree constraints.** Each city entered once, left once:

```
min Σᵢ Σⱼ c_ij x_ij
s.t. Σ_{i≠j} x_ij = 1  ∀j        Σ_{j≠i} x_ij = 1  ∀i        x_ij ∈ {0,1}
```

**Stage 2 — why that isn't enough.** Those constraints are *exactly the assignment problem*, and
they permit **subtours**: with six cities you could get two disjoint triangles. Every city is
entered once and left once, and yet it isn't a tour.

```
   1───2           4───5
    \ /             \ /            two valid-looking cycles, one invalid "tour"
     3               6
```

So you must add subtour elimination — **two ways, and the exam wants the trade-off:**

| | Constraint | Count | Relaxation |
|---|---|---|---|
| **SEC** | `Σ_{i,j∈U} x_ij ≤ \|U\|−1` for all `U` with `2 ≤ \|U\| ≤ n−1` | exponential | **tight** |
| **MTZ** | `u_i − u_j + 1 ≤ (n−1)(1−x_ij)`, `u₁ = 1`, `2 ≤ u_i ≤ n` | `O(n²)` | **weak** |

SEC says *any group of `k` cities holds at most `k−1` tour edges*, so it can't close its own
cycle. MTZ gives each city a position label and forces the labels to increase along the tour.

Use the lecture's `(n−1)`, not the textbook's `n`.

### Approximation — the two algorithms differ in one step

| | Ratio | Procedure |
|---|---|---|
| **MST-doubling** | **2** | MST → **double every edge** → all degrees even → Euler tour → shortcut |
| **Christofides** | **3/2** | MST → **min-weight matching on the ODD-degree vertices** → degrees even → Euler tour → shortcut |
| nearest neighbour | none | go to the nearest unvisited city; no guarantee at all |

Both end the same way — make all degrees even, walk an Euler tour, shortcut past repeats. They
differ only in **how** the degrees are fixed: doubling everything, versus matching only the odd
vertices. Matching is cheaper, hence 3/2 instead of 2.

> ⚠ The papers and `ce-09-demo` mislabel MST-doubling as "Christofides' 2-approximation".
> **Execute what is described**, and add one line noting the correct names.

### Breaking a heuristic — SS23 E6's shape

```
1. Build a SMALL instance satisfying EVERY stated assumption. Check them explicitly.
2. Run the heuristic; record its tour and cost.
3. Exhibit a BETTER tour.
4. Write the sentence: the heuristic's tour costs more, so it is not optimal.
```

The lever is always the same: **greedy commits to a cheap early edge and pays for it later.**

### Classifying a problem

| Wording | Class |
|---|---|
| pair `n` things one-to-one | assignment |
| one budget, maximise value | knapsack |
| minimise the number of containers | bin packing |
| cover every element, minimise cost | set covering |
| visit every node once and return | TSP |
| pick nodes so every **edge** is touched | **vertex cover** ← SS24 P6 |

---

# §12 · Nonlinear, unconstrained

### It's the one-variable test, in `n` dimensions

```
f'(x) = 0    →  flat tangent  →  candidate      f''> 0 → min ·  f''< 0 → max
```

The **gradient** replaces `f'`, the **Hessian** replaces `f''`, and "positive" becomes
"positive definite". That's the whole translation.

```
∇f(x) = (∂f/∂x₁, …, ∂f/∂xₙ)ᵀ        H_f = (∂²f/∂xᵢ∂xⱼ)   — always symmetric
```

`∇f` points uphill. At a peak or a valley floor there's no uphill direction, so **`∇f = 0`** is
the condition for a **critical point**. But saddles have it too — hence the Hessian.

### The three shapes

```
   MINIMUM              MAXIMUM              SADDLE
   f = x² + y²          f = −x² − y²         f = x² − y²
   a bowl               a dome               a Pringle
   curves UP every way  curves DOWN every    UP one way, DOWN another
```

### The procedure

1. Solve `∇f = 0` for **all** critical points — **factor, don't divide**. `x·(…) = 0` has two
   branches and dividing by `x` loses one.
2. Compute `H_f` symbolically.
3. **Substitute each critical point separately** — the Hessian usually still contains `x, y`,
   and different points can classify differently.

### Test A · 2×2 · `H = [[a,b],[b,d]]`, `det = ad − b²`, `tr = a + d`

| `det` | `tr` | Definiteness | Point is a |
|---|---|---|---|
| `> 0` | `> 0` | positive definite | **minimum** |
| `> 0` | `< 0` | negative definite | **maximum** |
| `< 0` | — | indefinite | **saddle** |
| `= 0` | — | — | inconclusive → Test C |

**Why:** `λ₁λ₂ = det` and `λ₁ + λ₂ = tr`. So `det > 0` means the eigenvalues share a sign and the
trace says which; `det < 0` means opposite signs — up one way, down another — *a saddle*.

### Test B · leading principal minors · any size

`D_k` = determinant of the top-left `k×k` block.

| Pattern | Verdict |
|---|---|
| all `D_k > 0` | positive definite → **minimum** |
| `D₁ < 0, D₂ > 0, D₃ < 0, …` — alternating, **starting negative** | negative definite → **maximum** |
| no `D_k = 0`, but neither pattern | indefinite → **saddle** |
| any `D_k = 0` | test **inapplicable** → Test C |

Check the alternating pattern against `H = −I`: `D₁ = −1 < 0`, `D₂ = +1 > 0` ✓

### Test C · eigenvalues · the fallback

Solve `det(H − λI) = 0`.

| Roots | Point is a |
|---|---|
| all `λ > 0` | **minimum** |
| all `λ < 0` | **maximum** |
| mixed signs | **saddle** |
| some `λ = 0` | genuinely inconclusive |

**A zero minor does not mean semidefinite** — it means the minor test *cannot decide*. SS23 E7
puts `H = [[0, α], [α, 0]]` at the origin precisely to force this:
`det(H − λI) = λ² − α² ⟹ λ = ±α ⟹` indefinite ⟹ **saddle**.

### Worked — both outcomes from one function

`f(x,y) = x³ − 3x + y²`

```
∇f = (3x² − 3, 2y) = 0   →   x² = 1, y = 0   →   (1,0) and (−1,0)

           ⎡ 6x   0 ⎤
H_f  =     ⎢        ⎥
           ⎣  0   2 ⎦

at (1,0):   H = [[6,0],[0,2]]    det = 12 > 0,  tr = 8 > 0   →  MINIMUM
at (−1,0):  H = [[−6,0],[0,2]]   det = −12 < 0                →  SADDLE
```

Same formula, different verdicts — because you substituted each point separately.

> **Shortcut worth doing FIRST:** if `H ≻ 0` **everywhere** (not just at the critical point),
> then `f` is strictly convex ⟹ **no maxima exist**, and any critical point is the unique global
> minimum. SS24 P7a opens with exactly this. Note `H ≻ 0 ⟹ strictly convex` but **not**
> conversely — `x⁴` is strictly convex with `f''(0) = 0`.

---

# §13 · Convexity

### Why anyone cares

```
convex objective  +  convex feasible set   ⟹   every LOCAL minimum is GLOBAL
```

For a general nonlinear problem you find candidates and hope. For a convex one, finding a local
solution **finishes** the problem. Everything else in this section is machinery for exploiting it.

### Two different things called convex

```
convex SET        λx + (1−λ)y ∈ C                     ∀ x,y ∈ C,  λ ∈ [0,1]
                  "the whole straight line between any two points stays inside"

convex FUNCTION   f(λx + (1−λ)y) ≤ λf(x) + (1−λ)f(y)
                  "the chord between two points on the graph lies ABOVE the graph"
```

`λx + (1−λ)y` as `λ` runs `0 → 1` traces exactly the segment: `λ=1` gives `x`, `λ=0` gives `y`,
`λ=½` the midpoint.

| Object | Compact | Convex |
|---|---|---|
| circle **curve** `x²+y²=r²` | ✓ | **✗** — the chord cuts through the middle, which isn't on the curve |
| closed **disk** `x²+y²≤r²` | ✓ | ✓ |
| halfplane `x+y ≤ 2` | **✗** unbounded | ✓ |
| intersection of convex sets | | ✓ always |
| union of convex sets | | ✗ generally |

**Strictly convex** — `<` for `x ≠ y`. `x²` is; `|x|` is convex but *not* strictly.
**Concave** — reverse the inequality; equivalently `−f` is convex. **Affine** `aᵀx + b` is both.

### Compactness, for the existence step

```
CLOSED    contains its own boundary        [0,1] yes · (0,1) no
BOUNDED   fits in a finite box             [0,1] yes · [0,∞) no
COMPACT   closed AND bounded               (Heine–Borel, in ℝⁿ)
```

Each condition blocks a different failure. On `(0,1)`, `max x` climbs toward 1 and never reaches
it — *not closed*. On `[0,∞)`, `max x` runs away — *not bounded*. On `[0,1]` it's attained.

> **Weierstrass:** non-empty compact set + continuous `f` ⟹ a global min **and** max are
> **attained**. This is step 1 of §14's recipe.

Note "not open" ≠ "closed": `[0,1)` is neither, `ℝⁿ` is both.

### Four routes to proving convexity

| Route | Use when |
|---|---|
| **definition** | `f` is abstract or non-differentiable — a norm |
| **Hessian** `H_f ⪰ 0` on a **convex domain** | `f` is differentiable — say the domain is convex |
| **composition rules** | it's built from known pieces |
| **counterexample** — one triple `(x,y,λ)` | you need to **dis**prove |

```
preserved by:  αf (α ≥ 0) ·  f + g ·  f(Ax + b) ·  max{f₁,…,fₘ} ·  h∘g  (h convex, non-decreasing)
```

### The norm proof — SS25 E7b, three lines

```
‖λx + (1−λ)y‖  ≤  ‖λx‖ + ‖(1−λ)y‖         triangle inequality
               =  |λ|·‖x‖ + |1−λ|·‖y‖      homogeneity
               =  λ‖x‖ + (1−λ)‖y‖          because λ ∈ [0,1], both are ≥ 0   ← SAY THIS
```

That last step is doing real work. Skipping the justification is the usual mark lost.

---

# §14 · KKT

### What it's for

Unconstrained, you set `∇f = 0`. **Constrained, you may be blocked by a wall** — the gradient
needn't vanish, it only has to be balanced by the constraints pushing back. KKT is the
bookkeeping for that.

### Standard form — convert first, always

```
min f(x)   s.t.   g_i(x) ≤ 0 ,   h_j(x) = 0

g(x) ≥ 0   becomes   −g(x) ≤ 0            max f   becomes   min −f
```

A sign error here propagates through everything. Write the conversion down.

```
ACTIVE   (tight, binding)   g_i(x*) = 0     the constraint is pressing
INACTIVE (slack)            g_i(x*) < 0     there's room to spare
```

### The Lagrangian

```
L(x, λ, μ)  =  f(x)  +  Σᵢ λᵢ gᵢ(x)  +  Σⱼ μⱼ hⱼ(x)        (MIN problem)

λᵢ   inequality multipliers   —   λᵢ ≥ 0        SIGN-CONSTRAINED
μⱼ   equality multipliers     —   free in sign
```

**Why `λ ≥ 0` but `μ` free:** violating `gᵢ ≤ 0` means `gᵢ > 0`, and the penalty `λᵢgᵢ` must
*increase* the objective you're minimising — so `λᵢ` can't be negative. An equality can be
violated in either direction, so its multiplier needs both signs.

This is the SOB pattern from §4: `≤` rows get sign-restricted duals, `=` rows get free ones.
**`λ` is a shadow price** — same reading as `y` in §3.

### The four blocks

| | Condition |
|---|---|
| **stationarity** | `∇f + Σλᵢ∇gᵢ + Σμⱼ∇hⱼ = 0` |
| **primal feasibility** | `gᵢ ≤ 0`, `hⱼ = 0` |
| **dual feasibility** | `λᵢ ≥ 0` |
| **complementary slackness** | `λᵢ · gᵢ(x*) = 0` ⟹ `λᵢ = 0` **or** `gᵢ = 0` |

Complementary slackness is §4's statement again: **an inactive constraint has multiplier zero;
a non-zero multiplier means the constraint is active.**

### Solving — the case split

Complementary slackness is what makes the system solvable. Each inequality forks:

```
for each i:   either  λᵢ = 0      (constraint inactive)
              or      gᵢ(x*) = 0  (constraint active)
```

```
DISCARD a case — and say why — if   λᵢ < 0    dual infeasible
                                    gᵢ > 0    primal infeasible
```

**Kill cases early.** Look for a fact in the problem statement forcing a constraint active or
inactive. SS25 E7: *at least two distinct points exist ⟹ `r > 0` ⟹ `μr = 0` ⟹ `μ = 0`* — one
sentence removes a whole branch.

### Constraint qualifications — this decides your conclusion

| | Requires | Gives you |
|---|---|---|
| **Slater** | `f, gᵢ` convex; `hⱼ` affine; an `x̄` with **every** `gᵢ(x̄) < 0` *strictly* | a KKT point **IS a global optimum** — stop, no comparison |
| **LICQ** | active `∇gᵢ` together with all `∇hⱼ` linearly independent | **candidates only** — evaluate `f` at each and compare |

**Verify Slater by exhibiting the point.** SS25 E7 uses the centroid with the radius inflated by
`+1`, so every `gᵢ ≤ −1 < 0`. The `+1` is what manufactures strictness.

> **MC trap:** *"a KKT point is always a local optimum"* — **false**.
> *"for a convex problem a KKT point is a global minimiser if Slater holds"* — **true**.

### The four-step recipe

1. **Existence** — feasible set compact + `f` continuous ⟹ global min and max exist
   (Weierstrass, §13). Cheap, and frequently forgotten.
2. **CQ** — Slater? otherwise LICQ.
3. **Solve** — Lagrangian → four blocks → case split → discard with reasons.
4. **Compare** — evaluate `f` at the survivors. **Skip only if Slater held.**
