<!-- solve-problem:6d75cfc3-4a41-49a1-a7ac-e09a248cb9f7 -->
# Dynamic optimality of splay trees

A [Tangle Solve problem](https://solve.tangle.tools/problems/6d75cfc3-4a41-49a1-a7ac-e09a248cb9f7).

## What is the problem?

A binary search tree (BST) maintains a set of keys {1,…,n} and serves an online sequence of accesses X = (x₁, x₂, …, x_m), where each access xᵢ must be performed by rotating it to the root (the standard BST access model; rotations and pointer movements cost O(1), only the nodes touched matter). The **offline optimal** BST algorithm OPT(X) is the minimum-cost BST algorithm that knows the entire sequence X in advance. A BST algorithm A is **dynamically optimal** (constant-competitive) if there is an absolute constant c such that for every n and every access sequence X, cost_A(X) ≤ c · cost_OPT(X) + O(n) (or + o(cost_OPT)).

**Conjecture (Sleator–Tarjan, 1985):** The splay tree — the simple algorithm that accesses x and then repeatedly performs zig/zig-zig/zig-zag rotations bringing x to the root — is dynamically optimal.

Formally: prove there exists an absolute constant c such that for all n and all access sequences, the total number of nodes touched by splay is at most c times the total touched by OPT. Note this is a conjecture about one specific algorithm, which is stronger than asking whether *some* online BST is constant-competitive (also open).

## Why does it matter?

Dynamic optimality is arguably the central open question in the theory of data structures: it asks whether a single, amnesia-free, pointer-machine-simple structure can match any BST algorithm including clairvoyant ones. A proof would settle the power of self-adjustment, yield a master theorem unifying all known BST bounds (working set, dynamic finger, static optimality, etc. all become corollaries), and the proof technique would likely transfer to other online problems with machine-learned/offline optima. BST access sequences also model execution traces and geometric searching, so the answer has implications well beyond trees.

## What's already known?

- **Sleator–Tarjan (1985):** Splay is O(log n)-competitive (trivially, versus OPT ≥ 1 per access) and achieves static optimality, the working-set bound, and (later, Cole et al. 1995 via the dynamic finger theorem) the dynamic finger bound.
- **Tango trees (Demaine, Harmon, Iacono, Pătrașcu, 2004):** First O(log log n)-competitive online BST — but not splay trees, and not constant-competitive. Refined by multi-splay trees (Wang, Derryberry, Sleator) and chain-splay (Derryberry–Sleator) which additionally achieve the working-set bound.
- **Geometric reformulation (DHIW 2004):** An access sequence is a point set; algorithms correspond to "arborally satisfied" supersets. Dynamic optimality ⇔ every point set has an O(1)-size arborally satisfied superset relative to optimal. This is the modern attack surface.
- **Fox (2011):** Constant-competitiveness of *any* greedy-like BST is equivalent to a purely combinatorial statement about avoiding arithmetic progressions in permutations — reducing the problem to additive combinatorics.
- **Reported 2026 preprint (Chmel et al., arXiv:2607.18498):** claims O(log log n · (log log log n)²)-competitiveness for splay itself. *(Verify — I cannot confirm this preprint exists or is correct; treat with skepticism and read it first if real.)*

The known barrier: all current techniques (potential functions, split/merge decompositions) prove bounds versus *static or finger-based cost functions*, not versus OPT. Competing with OPT requires reasoning about adaptivity, and no potential function is known that dominates the offline optimum.

## What would count as solving it?

Primary goal: **a proof that splay trees are c-competitive for some absolute constant c**: for all n, X: splay(X) ≤ c·OPT(X) + O(n + m). Acceptable partial but major results:
1. Any online BST algorithm with O(1)-competitive ratio (resolves the weaker "does a dynamically optimal BST exist" question).
2. A proof that splay achieves the **unified bound** (combining working set and dynamic finger; known only up to log factors — Bose, Douïeb, Dujmović, Howat, Sun 2013 give O(lg lg n) approximations) is not a solution but a milestone.
3. A refutation: an explicit family of access sequences where splay's ratio versus OPT diverges (disproves the conjecture).
4. Confirmation and verification of the claimed O(log log n polyloglog)-competitiveness would also be a landmark, though not constant.

## How to start

1. Read Sleator–Tarjan 1985 (the conjecture, potential-function machinery: weighted potential for static optimality, and the access lemma), then DHIPT 2004's tango-tree paper for the geometric/arborally-satisfied reformulation.
2. Master the geometric view: dynamic optimality ⇡ "greedy is optimal" — study the greedy algorithm of Lucas and of Demaine et al. (in the DHIW journal version) and the *independent-set* characterization of Greedy's cost.
3. Read Fox 2011 (SODA) carefully: it connects greedy's optimality to avoiding 3-term arithmetic progressions in "split" permutations — try to import upper-bound technology from the triangle-removal lemma literature (e.g., bounds on progression-free sets, Kelley–Meka) to bound Greedy's overhead.
4. Main obstacle: no technique lower-bounds OPT with an adaptive quantity that splay's potential function can match; all existing potentials are non-adaptive. A natural first attempt: strengthen the unified-bound analysis to handle interleaved finger/working-set structure with a multiplicative (not additive) combination.

## Key references

- Sleator, D., Tarjan, R. *Self-Adjusting Binary Search Trees* (JACM 1985). Defines splay trees, proves static optimality/working set, states the dynamic optimality conjecture. https://doi.org/10.1145/3828.3835
- Demaine, E., Harmon, D., Iacono, J., Pătrașcu, M. *Dynamic Optimality — Almost* (SICOMP 2007; conf. FOCS 2004). Gives tango trees (O(log log n)-competitive) and the geometric "arborally satisfied set" reformulation; also analyzes Greedy.
- Fox, K. *Upper Bounds for Maximally Greedy Binary Search Trees* (WADS 2011 / related SODA version). Connects greedy BST optimality to arithmetic-progression avoidance in permutations. *(verify exact venue)*
- Wang, C., Derryberry, J., Sleator, D. *O(log log n)-Competitive Multi-Splay Trees* (SODA 2006). Multi-splay trees: tango-style competitiveness plus working-set and scanning bounds.
- Bose, P., Douïeb, K., Dujmović, V., Howat, J., Sun, M. *Layered Working-Set Trees* / unified-bound results (2011–2013). Best known approximations to the unified bound. *(verify)*
- Chmel et al. *Splay Trees Are Almost Dynamically Optimal*, arXiv:2607.18498 (2026). Claims near-log-log competitiveness for splay — **verify existence and correctness before relying on it.**

## Warnings

- The 2026 preprint is from the census record only; I cannot independently verify it. Confirm it on arXiv and check the proof before building on it.
- The problem has two distinct versions: "splay specifically is optimal" vs. "some online BST is optimal." Don't conflate them in claimed results.
- Constant competitiveness must hold against OPT, which is itself only understood via lower bounds (Wilber's two bounds, 1986 — read these; they are how every known competitive ratio is proven). Any candidate proof needs an OPT lower-bound technique strong enough to dominate splay's cost.
- Common dead ends: potential functions proving static bounds don't adapt; naive unions of working-set and dynamic-finger arguments lose log factors. Also, the BST model matters — access cost is nodes touched, not rotations; model mismatches (e.g., allowing arbitrary restructuring) change the problem.

---
*`ALGO-000086` · unverified*

## Acceptance guidance

Primary goal: **a proof that splay trees are c-competitive for some absolute constant c**: for all n, X: splay(X) ≤ c·OPT(X) + O(n + m). Acceptable partial but major results:
1. Any online BST algorithm with O(1)-competitive ratio (resolves the weaker "does a dynamically optimal BST exist" question).
2. A proof that splay achieves the **unified bound** (combining working set and dynamic finger; known only up to log factors — Bose, Douïeb, Dujmović, Howat, Sun 2013 give O(lg lg n) approximations) is not a solution but a milestone.
3. A refutation: an explicit family of access sequences where splay's ratio versus OPT diverges (disproves the conjecture).
4. Confirmation and verification of the claimed O(log log n polyloglog)-competitiveness would also be a landmark, though not constant.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/6d75cfc3-4a41-49a1-a7ac-e09a248cb9f7?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000086.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000086)
