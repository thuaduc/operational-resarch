## The idea

You have long rolls of paper. Customers want short pieces. You want to use as few rolls as possible.

- A **pattern** is one way to cut one roll. Example: "one 4 and two 3s".
- The LP has **one variable per pattern**: how often do I use it?
- Problem: there can be **billions of patterns**. You can't write them all down.

**Column generation (CG):** start with a few patterns. Then keep asking:

> "Is there a pattern I don't have yet that would help?"

- **Yes** → add it, solve again.
- **No** → stop. Your answer is optimal.

**How do you know if a pattern helps? With prices.**
The LP gives every piece type a price `π` (the dual values).
A pattern helps if **its pieces are worth more than the roll costs** (1 roll):

```
Σ π_i · (pieces of type i in the pattern)  >  1   →  add it
```

Finding the most valuable pattern that fits on a roll = a **knapsack problem**.
This is called the **pricing problem**.

---

## Example

Roll length **10**. Pieces of length **4** and **3**.

**1. Start with simple patterns** (as many copies of one piece as fit):

| pattern | 4s | 3s | length used |
|---|---|---|---|
| A | 2 | 0 | 8 |
| B | 0 | 3 | 9 |

How to read it:
- **pattern**: the name of one way to cut a roll.
- **4s**: how many pieces of length 4 you get from one roll.
- **3s**: how many pieces of length 3 you get from one roll.
- **length used**: `4 · (4s) + 3 · (3s)`. Must be `≤ 10`, or it doesn't fit.

Row by row:
- **A** = cut a roll into two 4s: `4 + 4 = 8`. A third 4 would need 12 > 10, so 2 is the max. (2 left over as waste.)
- **B** = cut a roll into three 3s: `3 + 3 + 3 = 9`. A fourth 3 would need 12 > 10, so 3 is the max. (1 left over as waste.)

Why start like this? Each pattern makes only **one** piece type, so together they can always meet any demand. That gives a first feasible solution to begin from.

**2. Prices.** Each pattern costs 1 roll.
- A makes two 4s → one 4-piece is worth `π₄ = 1/2`
- B makes three 3s → one 3-piece is worth `π₃ = 1/3`

**3. Pricing: which pattern that fits in 10 is worth the most?**

| pattern | length | value |
|---|---|---|
| two 4s | 8 | 1/2 + 1/2 = 1 |
| three 3s | 9 | 1/3 · 3 = 1 |
| one 4 + one 3 | 7 | 1/2 + 1/3 = 5/6 |
| **one 4 + two 3s** | **10** | **1/2 + 2/3 = 7/6** |

**4. Decide.** `7/6 > 1` → the pattern "one 4 + two 3s" helps. Add it and solve again.
You stop when the best pattern is worth **≤ 1**.

(Reduced cost of the new pattern: `1 − 7/6 = −1/6 < 0`. Negative = improves. Same test.)

---

## Words you need

| word | meaning |
|---|---|
| **Master problem** | The full LP with *all* patterns. Too big. |
| **Restricted master (RMP)** | The LP with only the patterns you have so far. |
| **Column** | One pattern (a column of the LP). |
| **Prices `π`** | Dual values of the RMP: `πᵀ = c_Bᵀ B⁻¹`. One per piece type. |
| **Pricing problem** | Find the best new pattern. For cutting stock: a knapsack. |
| **Branch-and-price** | Branch-and-bound + CG. Needed for integer answers. |

**Master problem written out** (`a_ij` = pieces of type `i` in pattern `j`, `x_j` = uses of pattern `j`, `b_i` = demand):

```
min  Σ_j x_j                   (number of rolls)
s.t. Σ_j a_ij x_j ≥ b_i   ∀i    (meet the demand)
     x_j ≥ 0                   (CG solves the LP version)
```

**Careful:** CG gives the best **LP** answer (fractional). For integers you still need rounding or branch-and-price.

**Why patterns?** The pattern LP gives a much better bound than the simple "which piece on which roll" model.

---

## Exam steps

### One CG round
1. Solve the RMP.
2. Prices: `πᵀ = c_Bᵀ B⁻¹`.
3. Pricing: `z* = max { πᵀa − c_a : a is a valid pattern }`.
4. `z* ≤ 0` → **stop**, optimal. `z* > 0` → the new pattern **enters**.
5. Who leaves: ratio test, `argmin_k { b'_k / N'_k : N'_k > 0 }` with `b' = B⁻¹b`, `N' = B⁻¹a*`.

### "Formulate the pricing problem" — answer in 3 parts
1. **Variables:** `y_i` = number of pieces of type `i` in the new pattern.
2. **Valid pattern:** it fits on the roll: `Σ ℓ_i y_i ≤ L`, `y_i ∈ ℕ₀`.
3. **Objective:** `max Σ π_i y_i` (minus the pattern's cost, which is 1).

For cutting stock this is:

```
max  Σ_i π_i y_i
s.t. Σ_i ℓ_i y_i ≤ L
     y_i ∈ ℕ₀
```

Add the pattern **if the optimum is > 1**.

### When to use CG?
When there are **way more patterns than constraints**. If only a few patterns are possible, just write the whole LP.

---

## Formula box

| | |
|---|---|
| Prices | `πᵀ = c_Bᵀ B⁻¹` |
| Reduced cost | `c̄_j = c_j − πᵀa_j` — **negative = improves** |
| Pricing value | `πᵀa − c_a` — **positive = improves** |
| Stop | best pricing value `≤ 0` (cutting stock: knapsack optimum `≤ 1`) |
| Result | optimal for the **LP**, a lower bound for the integer problem |
