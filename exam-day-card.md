# Exam-day card — every decision rule, one page per slot

Rules only. No explanations — those are in the `theory/*a` lessons.
Read this on Thursday and again on Friday morning.

**Order to work the paper:** E1 → E3 → E5 → E6 → E4 → E2 → E7.
**Never leave a multiple-choice item blank.** All right = 2, one error = 1, blank = 0.
**Document every approach.** An undocumented correct answer scores zero.

---

# E1 — Multiple choice

```
all correct options, no wrong ones   →  2 pts
exactly one deviation                →  1 pt
more than one deviation, or blank    →  0 pts
⟹ ALWAYS GUESS
```

**Facts they trap on:**
```
P infeasible        →  D unbounded OR infeasible     (not "unbounded")
P unbounded         →  D infeasible
constraint slack    →  its dual variable = 0          (complementary slackness)
simplex cycles      ⟸  DEGENERACY only
cutting planes      →  NEVER remove integer solutions
KKT point           →  local optimum? NO
KKT + Slater        →  global optimum? YES
gradient descent    →  step size matters; convergence not automatic
TU submatrix dets   →  in {−1,0,+1}
```

---

# E2 — Sensitivity

## Read the solution off a tableau
```
1. all Row-0 entries ≥ 0?           →  optimal (max). Check FIRST.
2. Basis column ↔ RHS column        →  the basic variables and their values
3. everything not in Basis          →  = 0.  STATE THESE TOO
4. Row 0's RHS entry                →  z
5. sᵢ = 0 → constraint i BINDING    ;  sᵢ > 0 → slack, leftover = sᵢ
```
No Basis column? Find **unit columns**; the row of the `1` holds that variable's value.

## The formulas
```
c_B      = objective coefficients of the BASIC variables, in Basis-column order
yᵀ       = c_Bᵀ B⁻¹                    the shadow prices
Row0_j   = yᵀa_j − c_j                  optimal ⟺ all ≥ 0  (COURSE SIGN)
Row 0 under a basic column   = 0
Row 0 under slack sᵢ         = yᵢ      ← read shadow prices here
b'       = B⁻¹b                         the plan; feasible ⟺ b' ≥ 0
z        = c_Bᵀb' = yᵀb
```

## Which half breaks
```
change b (a resource)  →  PLAN moves, Row 0 fixed   →  only FEASIBILITY can break
change c (a price)     →  ROW 0 moves, plan fixed   →  only OPTIMALITY can break
```

## RHS ranging — `bᵢ → bᵢ + δ`
```
take the COLUMN of slack sᵢ  (= B⁻¹eᵢ, already in the tableau)
x_B(δ) = b' + δ·(that column) ≥ 0   →  one inequality per row  →  intersect
z(δ) = z + yᵢ·δ                      valid ONLY inside the range
at each endpoint: name the basic variable that hits 0
```

## Cost ranging — `c_k → c_k + Δ`
```
BASIC x_k     →  take that variable's ROW; new Row0 = Row0 + Δ·(row) ≥ 0
                 two-sided range;  z(Δ) = z + Δ·x_k;  plan unchanged
NON-BASIC x_j →  only its own entry:  c'_j − Δ ≥ 0  ⟺  c_j ≤ yᵀa_j
                 one-sided;  z and plan both unchanged
```

## New column · degeneracy · multiple optima
```
c'_new = yᵀa_new − c_new      enters ⟺ c_new > yᵀa_new
                              min viable price with production cost k:  p ≥ yᵀa_new + k

BASIC variable with RHS = 0        →  DEGENERATE   (⟹ simplex can cycle)
NON-BASIC with Row 0 = 0           →  MULTIPLE OPTIMA
endpoints of an RHS range          →  exactly where degeneracy occurs
```

---

# E3 — Duality

## Derive the dual
```
1. max ↔ min
2. one dual VARIABLE per primal CONSTRAINT   (sign restrictions don't count)
3. one dual CONSTRAINT per primal VARIABLE
4. dual constraint j reads DOWN COLUMN j of A
5. swap c and b
```

## SOB table
```
CLASSIFY
  variable:  x ≥ 0 → S      x free → O      x ≤ 0 → B
  constraint in MAX:  ≤ → S      = → O      ≥ → B
  constraint in MIN:  ≥ → S      = → O      ≤ → B

TRANSLATE  S→S, O→O, B→B
  primal CONSTRAINT → dual VARIABLE:    S: y ≥ 0    O: y free    B: y ≤ 0
  primal VARIABLE   → dual CONSTRAINT:  S: sensible  O: =         B: bizarre

  sensible direction = ≥ if the dual is a MIN,  ≤ if the dual is a MAX
```

## Theorems and outcomes
```
weak:    cᵀx ≤ bᵀy   for ALL feasible x, y
strong:  cᵀx* = bᵀy* at optimality

P unbounded   ⟹  D infeasible
P infeasible  ⟹  D unbounded OR infeasible
both feasible ⟹  both have finite optima
```

## Complementary slackness
```
primal CS:  ((Aᵀy)_j − c_j)·x_j = 0      for each variable j
dual CS:    ((Ax)_i − b_i)·y_i = 0       for each constraint i

slack constraint  ⟹  yᵢ = 0
non-zero variable ⟹  its dual constraint is TIGHT
```

## Show `x*` is NOT optimal (the 4 steps)
```
1. check x* is primal feasible          not feasible → done immediately
2. every SLACK constraint  → set its yᵢ = 0
3. every x_j ≠ 0           → set dual constraint j to EQUALITY; solve for y
4. test y against the dual's OTHER constraints and SIGN restrictions
      all satisfied →  x* IS optimal
      any violation →  x* is NOT optimal          ← this is the answer
```

---

# E4 — IP modelling

## The answer template (this is the rubric)
```
Introduce z ∈ {0,1}:  z = 1 ⟺ <MEANING IN A FULL SENTENCE>
    <linking constraint(s)>
Then:
    <the requirement, keyed on z>        ∀ i ∈ <EXPLICIT RANGE>
```
Three things score separately: **words · linking · ∀ with range.**
Every auxiliary needs a **domain line** (`z ∈ {0,1}`) — without it the model is broken.

## Indicator — both directions
```
want  t = 1 ⟺ X > K       (X integer)

(A)  X ≤ K + M·t          "X > K ⟹ t = 1"    forces t UP
(B)  X ≥ (K+1)·t          "t = 1 ⟹ X > K"    forces t DOWN

"indicates whether" / "iff"      →  BOTH
"if more than K, then …"         →  (A) + the consequence keyed on t
unsure                           →  BOTH
```
`M` is a **constant**; justify its size (`M = |J|`, `M = cᵢ`, `M = Σwⱼ`).
Continuous `X`: `t = 1 ⟺ X > K` is **impossible** (set not closed).

## Patterns
```
exactly one          Σ_{i∈I} x_{i,j} = 1              ∀j
at most k            Σ_i z_i ≤ k
capacity             Σ_{j∈J} x_{i,j} ≤ c_i            ∀i
only if built        x_{i,j} ≤ y_i         ∀i,j       ← DISAGGREGATED = stronger
implication A⇒B      A ≤ B
  generalised        Σ(premises) − (#premises−1) ≤ Σ(conclusions)
pairwise conflict    y_i + y_i' ≤ 1        for pairs violating a threshold
either-or            f₁ ≤ b₁ + Mz  ,  f₂ ≤ b₂ + M(1−z)
XOR                  build an indicator per block, mirror with z / (1−z)
rolling window       Σ_{t=k}^{k+6} x_t ≤ 5            ∀k ∈ {1,…,T−6}
ratio / %            move all left, clear denominators
product x_k·x_l      Y ≤ x_k , Y ≤ x_l , Y ≥ x_k + x_l − 1
                     rewarded → first two suffice;  penalised → third suffices
fixed charge         x ≤ M·y , x ≥ q·y                "nothing or at least q"
startup              y_t ≥ x_t − x_{t−1}              t ≥ 2
two objectives       min Σf_i y_i − Σs_{ij} x_{ij}    flip the sign of one
```

## Traps
```
missing ∀ range, or wrong range (J\{21},  t ∈ {4,…,15})
unnamed auxiliary variable
M treated as a variable
one direction of an indicator when "iff" was stated
a product left non-linear
merging two sub-questions into one answer
```

---

# E5 — Branch and Bound

```
LP relaxation:  max → z_LP is an UPPER bound   ;  min → LOWER bound
integer c and x  →  max: OPT ≤ ⌊z_LP⌋

branch on FRACTIONAL x_i = f:     x_i ≤ ⌊f⌋   |   x_i ≥ ⌈f⌉
  → loses NO integer point (none lies strictly between)
  → children INHERIT all ancestor constraints
```

## Three pruning rules
```
                        MAXIMISE                MINIMISE
1. integrality   LP integral; update Z* if Z > Z*      if Z < Z*
2. infeasible    LP relaxation has no feasible point   same
3. bound         Z_node ≤ Z*                           Z_node ≥ Z*
```
**Write MAX or MIN at the top of your answer.** Ties (`Z_node = Z*`) still prune.

## Solving a node graphically
```
branch constraint = a VERTICAL or HORIZONTAL line
  x₁ ≤ 2 → keep LEFT     x₁ ≥ 3 → keep RIGHT
  x₂ ≤ 1 → keep BELOW    x₂ ≥ 2 → keep ABOVE
constraints ACCUMULATE down the path
empty region → prune by infeasibility, no arithmetic
otherwise slide the objective line to the last touching vertex
```

## Node order
```
FIFO = queue  = breadth-first   take the OLDEST open node
LIFO = stack  = depth-first     take the NEWEST open node
stop as soon as the incumbent is confirmed
state which child you push first — the answer depends on it
```

## Per node, record
```
node │ constraint added │ vertex │ Z │ which rule closed it
```

---

# E6 — Combinatorial

## Matroids
```
(1) ∅ ∈ ℐ
(2) B ∈ ℐ, A ⊆ B  ⟹  A ∈ ℐ                          hereditary
(3) A,B ∈ ℐ, |A| < |B|  ⟹  ∃x ∈ B\A : A∪{x} ∈ ℐ      exchange
        SMALLER set grows;  element comes from the LARGER

PROVE:    all three bullets, in order
DISPROVE: 1. is ∅ ∈ ℐ?      2. hereditary?     3. exchange?
          for exchange you must refute EVERY x ∈ B\A  → keep it to 2–3 elements

basis = MAXIMAL independent set;  all bases equicardinal
r(B)  = max{|A| : A ⊆ B, A ∈ ℐ};  connected graphic: r(E) = |V| − 1
greedy: increasing → MIN basis  ;  decreasing → MAX basis
```

## Total unimodularity
```
TU ⟺ every square submatrix has det ∈ {−1,0,+1}   (⟹ every entry in {−1,0,+1})
TU + b integral ⟹ integral polyhedron ⟹ IP solvable in polynomial time

PROVE by recognition:  incidence matrix of a BIPARTITE or DIRECTED graph is TU
                       consecutive-ones (interval matrix) is TU
PROVE by conditions:   1. entries in {−1,0,+1}
                       2. ≤ 2 non-zeros per COLUMN
                       3. split rows into M₁,M₂ — same sign → different parts,
                          opposite signs → same part
                       (DO NOT write "Ghouila-Houri" — unnamed in the lecture)
DISPROVE:  find a ZERO-FREE 2×2 with |det| ≥ 2
           (any 2×2 containing a 0 always has det ∈ {−1,0,+1})
closure:   A TU ⟹ −A, Aᵀ, A⁻¹, [A,I] all TU
```

## Knapsack DP
```
B[i,w] = best value from items 1..i with capacity w
  w_i ≤ w :  B[i,w] = max{ B[i−1,w] ,  v_i + B[i−1, w−w_i] }
  else    :  B[i,w] = B[i−1,w]
row 0 and column 0 are all zeros;  answer = B[n,W]

index by weight O(n·W_max)  or  by value O(n·V_max)  →  RUN THE SMALLER, say why
backtrack: B[i,w] = B[i−1,w] ⟹ item i NOT taken; else taken, w := w − w_i
pseudopolynomial, not polynomial
FPTAS: θ = ε·v_max/n,  v_i* = ⌊v_i/θ⌋,  value-DP,  report original values
       V_approx ≥ (1−ε)·V_opt        P ⊆ FPTAS ⊆ PTAS ⊆ APX
```

## Network flow
```
0 ≤ f(e) ≤ u(e)          conservation: in = out at every node except s, t
val(f) = Σ_j f(s,j)

cut S = [X, V\X], s ∈ X, t ∉ X
cap(S) = Σ u(i,j) over i ∈ X, j ∉ X       ← FORWARD ARCS ONLY

residual: forward u(e) − f(e)   ,   backward f(e)      ← backward = the undo button

FORD-FULKERSON: find any s–t path in the residual net; κ = min residual on it;
                f += κ forward, f −= κ backward; repeat; none left → maximal
MIN CUT: X = nodes reachable from s in the FINAL residual network
VERIFY: cap(S) = val(f)   ← free correctness check, do it every time

val(f) ≤ cap(S) always  ;  max-flow = min-cut at optimum
node capacity → split v into v_in → v_out with that capacity
```

## TSP
```
EULER = every EDGE once (polynomial)    HAMILTON = every NODE once (NP-complete)
TSP = min-weight Hamiltonian cycle;  symmetric (n−1)!/2  ,  asymmetric (n−1)!

degree constraints alone = the ASSIGNMENT problem → permits SUBTOURS
SEC:  Σ_{i,j∈U} x_ij ≤ |U|−1    ∀U, 2≤|U|≤n−1     exponential, TIGHT
MTZ:  u_i − u_j + 1 ≤ (n−1)(1−x_ij)                polynomial, WEAK
      u₁ = 1,  2 ≤ u_i ≤ n                          use (n−1), not n

MST-DOUBLING = 2      double MST edges → Euler tour → shortcut
CHRISTOFIDES = 3/2    MST → min-weight matching on ODD-degree vertices → Euler → shortcut
both need the TRIANGLE INEQUALITY
nearest neighbour: NO guarantee
⚠ papers mislabel MST-doubling as "Christofides" — EXECUTE WHAT IS DESCRIBED
```

## Classify a problem
```
pair n things one-to-one              → assignment
one budget, maximise value            → knapsack
minimise number of containers         → bin packing
cover every element, minimise cost    → set covering
visit every node once, return         → TSP
pick nodes so every EDGE is touched   → vertex cover
```

---

# E7 — Nonlinear

## Unconstrained
```
1. ∇f = 0  →  ALL critical points     (factor, don't divide — x·(…)=0 has two branches)
2. H_f symbolically
3. PLUG IN each critical point separately — verdicts can differ
```

### 2×2 test — the workhorse
```
H = [[a,b],[b,d]]      det = ad − b²     tr = a + d

det > 0, tr > 0   →  positive definite   →  MINIMUM
det > 0, tr < 0   →  negative definite   →  MAXIMUM
det < 0           →  indefinite          →  SADDLE
det = 0           →  inconclusive        →  eigenvalues
                                   (λ₁λ₂ = det ,  λ₁+λ₂ = tr)
```

### Leading principal minors — any size
```
all D_k > 0                          →  PD → MIN
D₁ < 0, D₂ > 0, D₃ < 0, …            →  ND → MAX     (alternating, starts NEGATIVE)
no D_k = 0 but neither pattern       →  indefinite → SADDLE
any D_k = 0                          →  test INAPPLICABLE → eigenvalues
```

### Eigenvalues — the fallback
```
solve  det(H − λI) = 0

all λ > 0    →  MIN
all λ < 0    →  MAX
mixed signs  →  SADDLE
some λ = 0   →  genuinely inconclusive
```

### Shortcut
```
H ≻ 0 EVERYWHERE  ⟹  strictly convex  ⟹  NO maxima, unique GLOBAL minimum
(state this before finding the critical point — it's part of the answer)
H ≻ 0 ⟹ strictly convex, but NOT conversely (x⁴)
```

## Convexity
```
CONVEX SET       λx + (1−λ)y ∈ C     ∀x,y ∈ C, λ ∈ [0,1]
CONVEX FUNCTION  f(λx+(1−λ)y) ≤ λf(x) + (1−λ)f(y)

circle CURVE x²+y²=r²  → compact, NOT convex
closed DISK  x²+y²≤r²  → compact AND convex
halfplane              → convex, NOT compact (unbounded)

convex f + convex feasible set ⟹ every local min is GLOBAL

PROVE: (a) definition   (b) H_f ⪰ 0 on a convex domain
       (c) rules: αf (α≥0), f+g, f(Ax+b), max{f_i}, h∘g (h convex non-decreasing)
       (d) disprove with ONE triple (x,y,λ)

norm is convex:  ‖λx+(1−λ)y‖ ≤ ‖λx‖+‖(1−λ)y‖ = |λ|‖x‖+|1−λ|‖y‖ = λ‖x‖+(1−λ)‖y‖
                 last step needs λ ∈ [0,1]  ← say it
```

## KKT
```
STANDARD FORM   min f(x)  s.t.  g_i(x) ≤ 0 ,  h_j(x) = 0
  g ≥ 0 becomes −g ≤ 0    ;    max f becomes min −f

L = f + Σ λ_i g_i + Σ μ_j h_j        λ_i ≥ 0  (inequalities)
                                     μ_j FREE (equalities)

1. STATIONARITY   ∇f + Σλ_i∇g_i + Σμ_j∇h_j = 0
2. PRIMAL FEAS.   g_i ≤ 0 ,  h_j = 0
3. DUAL FEAS.     λ_i ≥ 0
4. COMPL. SLACK.  λ_i · g_i = 0        ⟹ λ_i = 0 OR g_i = 0

CASE SPLIT on (4); discard a case if λ_i < 0 or g_i > 0, and SAY WHY
kill cases early: argue a constraint is active/inactive from the problem statement
```

## Constraint qualifications
```
SLATER   f, g convex; h affine; ∃x̄ with g_i(x̄) < 0 STRICTLY
         ⟹  a KKT point IS a GLOBAL optimum. No comparison needed.
         verify by EXHIBITING the point (e.g. centroid, radius + 1)

LICQ     active ∇g_i together with all ∇h_j are linearly independent
         ⟹  KKT gives CANDIDATES only. COMPARE f values.
```

## Four-step recipe
```
1. EXISTENCE  feasible set compact (closed AND bounded, Heine–Borel) + f continuous
              ⟹ global min and max EXIST (Weierstrass)
2. CQ         Slater? else LICQ?
3. SOLVE      Lagrangian → four blocks → case split → discard
4. COMPARE    evaluate f at survivors — SKIP only if Slater held
```

---

# Corrections to carry in

```
Christofides = 3/2 (odd-degree matching)   MST-doubling = 2
   papers mislabel doubling as Christofides → execute what is DESCRIBED

TU sufficient condition is UNNAMED in the lecture → reproduce the three conditions

Row 0 sign is the COURSE convention: Row0 = yᵀa_j − c_j, optimal ⟺ all ≥ 0
   (textbooks use the negation — state your convention)

midterm SS26 P2d's sample solution is wrong: the cone it gives for unbounded c
   is where the LP is BOUNDED
```

---

# Timing

```
E1  Multiple choice   ~15 pts / 12 min   best pts/min, never blank
E3  Duality           ~15 pts / 15 min   most procedural, banks points early
E5  Branch & Bound    ~14 pts / 14 min
E6  Combinatorial     ~14 pts / 14 min
E4  IP modelling      ~22 pts / 25 min   largest; do it while sharp
E2  Simplex/sens      ~16 pts / 18 min
E7  Nonlinear         ~20 pts / 22 min   LAST — convexity parts first, then KKT
                             ~20 min spare → E4 and E7
```

Allowed: non-programmable calculator, analog dictionary, ruler. No cheat sheet.
Partial credit is real on E4 and E7 — **write the setup even if you can't finish.**
