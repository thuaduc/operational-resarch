# Notation, and the minimum revised simplex you need

Reference card for the LP chapters. One example, every symbol listed against it.

Companion to [03a-sensitivity-lesson](03a-sensitivity-lesson.md), which uses the same example.

---

# The example

A workshop makes tables (`x₁`) and chairs (`x₂`).

```
table:  1 wood, 1 labour, earns €3
chair:  1 wood, 2 labour, earns €4
have:   4 wood, 6 labour
```

```
max  3x₁ + 4x₂
s.t.  x₁ +  x₂ ≤ 4        (wood)
      x₁ + 2x₂ ≤ 6        (labour)
      x₁, x₂ ≥ 0
```

Add a **slack** per row — the unused amount of that resource — to turn `≤` into `=`:

```
x₁ +  x₂ + s₁      = 4          s₁ = leftover wood
x₁ + 2x₂      + s₂ = 6          s₂ = leftover labour
```

**Optimum: `x* = (2, 2)`, `z = 14`.** Both resources exhausted, so `s₁ = s₂ = 0`.

```
m = 2 constraints        n = 4 variables (after slacks)
```

---

# Given 

```
        ⎡ x₁ ⎤          ⎡ 3 ⎤            ⎡ 4 ⎤
        ⎢ x₂ ⎥          ⎢ 4 ⎥            ⎣ 6 ⎦
   x =  ⎢ s₁ ⎥     c =  ⎢ 0 ⎥       b =
        ⎣ s₂ ⎦          ⎣ 0 ⎦
        n×1             n×1              m×1
      variables      objective        resources
```

```
              x₁  x₂  s₁  s₂
    wood    ⎡  1   1   1   0 ⎤
A =                              m×n     slack columns form an identity
    labour  ⎣  1   2   0   1 ⎦
```

**Columns of `A`** — `a_j` is the *recipe* for variable `j`:

```
a₁ = (1,1)ᵀ    a₂ = (1,2)ᵀ    a_{s₁} = (1,0)ᵀ    a_{s₂} = (0,1)ᵀ
 a table         a chair        = e₁               = e₂
```

Slacks always have `c_j = 0` — they earn nothing.

---

# The split: basic vs non-basic

Exactly `m = 2` variables may be non-zero. Those are **basic**; the rest are **non-basic** and equal 0.

```
BASIC     {x₁, x₂}    values 2, 2
NON-BASIC {s₁, s₂}    values 0, 0
```

Split `A` and `c` along that line:

```
     ⎡ 1  1 ⎤          ⎡ 1  0 ⎤          ⎡ 3 ⎤          ⎡ 0 ⎤
B =  ⎢      ⎥     N =  ⎢      ⎥    c_B = ⎣ 4 ⎦    c_N = ⎣ 0 ⎦
     ⎣ 1  2 ⎦          ⎣ 0  1 ⎦
      m×m               m×(n−m)          m×1            (n−m)×1
   basic columns     non-basic cols    basic costs
```

> **`c_B` is the thing to get right.** Read it off the tableau's Basis column, in that row order.
> Wrong `c_B` ⟹ every price downstream is wrong.

---

# Derived 

![[Pasted image 20260805132916.png]]

```
        ⎡  2  −1 ⎤
B⁻¹ =   ⎢        ⎥         det B = 2·1 − 1·1 = 1
        ⎣ −1   1 ⎦          2×2 inverse: swap diagonal, negate off-diagonal, ÷ det
```

| Symbol            | Formula          | Meaning                                       | Value here        |
| ----------------- | ---------------- | --------------------------------------------- | ----------------- |
| `b'` = `x_B`      | `B⁻¹b`           | **the plan** — amounts of the basic variables | `(2, 2)ᵀ`         |
| `N'`              | `B⁻¹N`           | tableau body under non-basic columns          | `[[2,−1],[−1,1]]` |
| `y` = `π`         | `c_Bᵀ B⁻¹`       | **shadow prices** / duals, one per constraint | `(2, 1)`          |
| `z`               | `c_Bᵀb'` = `yᵀb` | objective value                               | `14`              |
| `Row0_j` = `c'_j` | `yᵀa_j − c_j`    | reduced cost of column `j`                    | see below         |

The arithmetic, once:

```
b' = B⁻¹b   =  ( 2·4 − 1·6 ,  −1·4 + 1·6 )   =  (2, 2)
y  = c_BᵀB⁻¹ = ( 3·2 + 4·(−1) , 3·(−1) + 4·1 ) =  (2, 1)
z  = yᵀb     =  2·4 + 1·6                     =  14
```

`Row0_j = yᵀa_j − c_j` on all four columns:

```
x₁ :  (2·1 + 1·1) − 3  =  3 − 3  =  0        basic ⟹ always 0
x₂ :  (2·1 + 1·2) − 4  =  4 − 4  =  0        basic ⟹ always 0
s₁ :  (2·1 + 1·0) − 0  =  2      =  y₁       slack ⟹ = the shadow price
s₂ :  (2·0 + 1·1) − 0  =  1      =  y₂
```

```
b' ≥ 0        ⟺  FEASIBLE
all Row0 ≥ 0  ⟺  OPTIMAL   (max problem)
```

---

# The tableau, and what each region holds

```
 Basis │ x₁  x₂   s₁   s₂ │ RHS
───────┼──────────────────┼─────
 Row 0 │  0   0    2    1 │  14
───────┼──────────────────┼─────
   x₁  │  1   0    2   −1 │   2
   x₂  │  0   1   −1    1 │   2
```

| region | symbol |
|---|---|
| Basis column | which variables are basic, in row order |
| body under **basic** columns | identity `I` |
| body under **non-basic** columns | `N' = B⁻¹N` |
| RHS column, body rows | `b'` — the plan |
| RHS column, Row 0 | `z` |
| Row 0 under **basic** columns | always `0` |
| Row 0 under **slack `sᵢ`** | `yᵢ` — the shadow price |

**This is why you rarely compute `B⁻¹`.** It's already in the body, and `y` is already in Row 0.

---

# Revised simplex — the minimum

Ordinary simplex rewrites the whole tableau each pivot. Revised simplex keeps only `B⁻¹` and
computes the rest on demand. Same path, less bookkeeping.

```
1. PRICES         yᵀ = c_Bᵀ B⁻¹
2. REDUCED COSTS  Row0_j = yᵀa_j − c_j  for non-basic j     all ≥ 0 → STOP, optimal
3. ENTERING       most negative Row0_j
4. DIRECTION      d = B⁻¹a_j                                d ≤ 0  → STOP, unbounded
5. LEAVING        min { b'ᵢ / dᵢ : dᵢ > 0 }
6. UPDATE         swap in B and N, recompute B⁻¹, repeat
```

| | Endterm | Midterm |
|---|---|---|
| Steps 1–2 (the formulas) | **yes** — SS25 E2 cites them | yes |
| Reading `b'`, `N'`, `y` off a given tableau | **yes** — every E2 | yes |
| Steps 3–6 (executing an iteration) | **never**, 5 papers | **yes** — SS25 P4, 12 pts |

**Know 1–2 cold, understand 3–6, drill nothing.** Insurance if you want it: midterm-2025 P4,
12 minutes, `B⁻¹` supplied.

---

# Letters that clash between chapters

| Letter | LP / simplex | Elsewhere |
|---|---|---|
| `c` | objective coefficients | **max-flow: `c(e)` = arc CAPACITY** (`theory/09` writes `u(e)` for the same thing); min-cost flow: cost per unit |
| `b` | right-hand sides | network flow: `b(v)` = supply (`>0`) / demand (`<0`) at a node |
| `u` | — | network flow: arc capacity |
| `x` | decision variables | network flow: `f(e)` is the flow |
| `y` | duals / shadow prices | KKT: multipliers are `λ` (inequalities), `μ` (equalities) — same idea |
| `λ` | — | KKT multiplier **and** the weight in `λx + (1−λ)y`. Unrelated uses |
| `m`, `n` | constraints, variables | graphs: often nodes, edges |

The `c` clash bites: SS25 E6 says *"capacities `c(e) ∈ ℕ`"* while `theory/09` calls it `u(e)`.

---

# The eight lines to have cold

```
c_B     = objective coefficients of the BASIC variables, in Basis-column order
yᵀ      = c_Bᵀ B⁻¹                    shadow prices, one per constraint
Row0_j  = yᵀa_j − c_j                  optimal ⟺ all ≥ 0  (max)
Row 0 under a basic column = 0
Row 0 under slack sᵢ       = yᵢ
b'      = B⁻¹b                         the plan; feasible ⟺ b' ≥ 0
z       = c_Bᵀb' = yᵀb
c'_new  = yᵀa_new − c_new              enters iff c_new > yᵀa_new
```

Next: [03a-sensitivity-lesson](03a-sensitivity-lesson.md) for what to *do* with them.
