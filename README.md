<!-- solve-problem:da78bfa3-0c51-43f2-9a43-2c3dd2217028 -->
# Integer factorization complexity

A [Tangle Solve problem](https://solve.tangle.tools/problems/da78bfa3-0c51-43f2-9a43-2c3dd2217028).

## What is the problem?

**Input:** an integer N (given in binary, so input size n = ⌈log₂ N⌉). **Task:** output the prime factorization N = p₁^e₁ ⋯ pₖ^eₖ.

Formally, the question is: is the decision problem "does N have a factor d with 1 < d < N?" (equivalently, is N composite — reducible both ways via primality testing, which is in P by AKS) solvable in deterministic polynomial time in n on a classical Turing machine?

A secondary, quantitative variant: can factoring be done asymptotically faster than the Number Field Sieve's running time L_N[1/3, c] = exp((c + o(1)) (ln N)^{1/3} (ln ln N)^{2/3}), with c ≈ 1.9 for the general NFS? E.g., is there an L_N[1/4] or L_N[o(1)] algorithm, or a genuinely new complexity class below the current barrier?

Assumptions/constraints: classical (no quantum) computation; worst-case over N; randomness is allowed as a weakening (BPP vs P — no randomized poly algorithm is known either).

## Why does it matter?

RSA and several deployed public-key protocols (and, via reductions, some Diffie–Hellman instantiations) rest directly on factoring hardness; a classical polynomial-time algorithm would break most of today's internet PKI. Complexity-theoretically, factoring sits in NP ∩ coNP (under the standard primality-in-P result), so proving it NP-hard would collapse PH — its exact classification is a central data point for the P vs NP landscape. Progress below L[1/3] also recalibrates concrete key-size recommendations.

## What's already known?

- **Upper bounds (classical):** the Number Field Sieve (Lenstra, Lenstra, Manasse, Pollard; refined by Coppersmith and others) factors N in L_N[1/3, (64/9)^{1/3}] ≈ L_N[1/3, 1.923]. Earlier: quadratic sieve L[1/2], ECM (Lenstra) L_p[1/2] in the smallest factor p (subexponential only in p, useful for unbalanced factors). Practical records: 829-bit RSA-250 factored (2020, ~2700 core-years).
- **Quantum:** Shor's FOCS 1994 algorithm factors in polynomial time (O(n³)-ish gate counts), but requires a large fault-tolerant quantum computer — not built.
- **Randomized/partial progress:** Miller–Rabin makes primality deterministic-poly (AKS 2002); Pollard's rho is N^{1/4}; Coppersmith's lattice method finds factors exceeding N^{β²} given βN bits of the factor; Bernstein's circuits give sublinear-depth but not poly-time.
- **Lower bounds:** essentially none. Factoring is not known to be hard for any class, and is believed to be neither in P nor NP-hard. The L[1/3] barrier has stood since ~1990; the NFS bottleneck is finding smooth numbers in a relation-collection phase, and smoothness probability estimates (Canfield–Erdős–Pomerance) tightly govern the exponent.

## What would count as solving it?

1. **Full solution (positive):** a deterministic (or even randomized — that alone would be major) classical algorithm factoring every N in time poly(log N), with proof of correctness and running time.
2. **Full solution (negative):** a proof that no classical poly-time factoring algorithm exists — this requires (at minimum) P ≠ NP-type machinery and is far out of reach; not a realistic target.
3. **Partial but publishable:** any provable algorithm with running time L_N[α] with α < 1/3, or exp(o((ln N)^{1/3}(ln ln N)^{2/3})), for general N.
4. **Structural results:** e.g., showing factoring is/isn't complete for some class, or unconditional lower bounds in restricted models (circuits of bounded depth, monotone, etc.).

## How to start

1. Read Crandall & Pomerance, *Prime Numbers: A Computational Perspective*, chapters on sieves and ECM — the fastest way to internalize why L[1/3] is the barrier and where the smoothness bottleneck lies.
2. Read the original NFS paper (Lenstra–Lenstra–Manasse–Pollard, 1993) and Coppersmith's thesis-adjacent paper on NFS parameter optimization; then Coppersmith's 1996 "Finding a small integer root of a univariate modular equation" — the main modern tool for exploiting partial information about factors.
3. Understand why the L[1/3] exponent arises: relation collection needs y-smooth values of a degree-d polynomial, smoothness density from Canfield–Erdős–Pomerance, and two-parameter optimization. Any improvement must beat this tradeoff — e.g., higher-degree number fields, multiple polynomials, or non-smoothness-based approaches.
4. Main obstacle: decades of attempts (continued fractions → QS → NFS) all reduce to the same smoothness statistics; a genuinely new mechanism (analytic, algebraic, or complexity-theoretic) is needed, not parameter tuning.

## Key references

- P. W. Shor, "Algorithms for quantum computation: discrete log and factoring," FOCS 1994. Gives a polynomial-time quantum algorithm for factoring via period-finding of f(x)=aˣ mod N. https://doi.org/10.1109/SFCS.1994.365700
- A. K. Lenstra, H. W. Lenstra, M. S. Manasse, J. M. Pollard, "The number field sieve," Proc. STOC 1990 / book 1993. Introduces NFS, the L_N[1/3] general factoring algorithm. (The 1993 volume: Lecture Notes in Mathematics 1554, Springer.)
- R. Crandall, C. Pomerance, *Prime Numbers: A Computational Perspective*, 2nd ed., Springer 2005. Standard reference for all factoring algorithms and their analysis.
- M. Agrawal, N. Kayal, N. Saxena, "PRIMES is in P," Annals of Mathematics 2004. Deterministic polynomial-time primality — the coNP side of the factoring question. https://doi.org/10.4007/annals.2004.160.781
- D. Coppersmith, "Finding a small integer root of a univariate modular equation," EUROCRYPT 1996. Lattice-based method exploiting known bits of a factor; key partial-progress tool. https://doi.org/10.1007/3-540-68339-9_14
- E. R. Canfield, P. Erdős, C. Pomerance, "On a problem of Oppenheim concerning 'y-smooth' numbers," Math. Comp. 1983. The smoothness-density estimates underlying all L[1/3] analyses. https://doi.org/10.2307/2007635

## Warnings

- Factoring is in NP ∩ coNP; proving it NP-hard would imply PH collapses to Σ₂ — don't chase NP-hardness.
- Don't conflate the input measures: poly in N is trivial (trial division); the requirement is poly in log N.
- Randomized polynomial time is also open — an RP/BPP algorithm would be a landmark, not just a warm-up.
- Parameter tweaks within the NFS framework have been optimized extensively (Coppersmith–Odlyzko–Schroeppel); incremental "improvements" there are almost certainly already known. Verify any claimed constant improvements against the COP framework before claiming novelty.
- Heuristic assumptions: NFS analyses rely on unproved smoothness heuristics; a rigorously analyzed L[1/3] algorithm exists only for special forms (e.g., certain SNFS cases) — (verify details).

---
*`ALGO-000013` · unverified*

## Acceptance guidance

1. **Full solution (positive):** a deterministic (or even randomized — that alone would be major) classical algorithm factoring every N in time poly(log N), with proof of correctness and running time.
2. **Full solution (negative):** a proof that no classical poly-time factoring algorithm exists — this requires (at minimum) P ≠ NP-type machinery and is far out of reach; not a realistic target.
3. **Partial but publishable:** any provable algorithm with running time L_N[α] with α < 1/3, or exp(o((ln N)^{1/3}(ln ln N)^{2/3})), for general N.
4. **Structural results:** e.g., showing factoring is/isn't complete for some class, or unconditional lower bounds in restricted models (circuits of bounded depth, monotone, etc.).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/da78bfa3-0c51-43f2-9a43-2c3dd2217028?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000013.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000013)
