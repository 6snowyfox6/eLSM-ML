# Parganlija, Giacosa, Rischke, Kovacs, Wolf 2010 — arXiv:1011.6104 (AIP Conf. Proc. 1343, 2011)

**Class:** A (vacuum, vector/axial-vector sector + Z_K of the same Nf=3 eLSM). Conference proceedings, preliminary.
**What was run:** `runs/parganlija2010_zk_consistency.py` — Table 1 (9 rows: Z_pi, m_a1, m_K1, m_f1S).
Z_K by three routes using solver functions only: Eq. (6) = solver `wavefunction_factors` (A13), solved
self-consistently with sigma_y = Z_K f_K/sqrt2; Eqs. (8),(9) = inverted solver Table A1 vector formulas
(f1_S - omega_S, K1 - K*). Raw output: `zk_check.md`.

## Results
- Eq. (8) vs Eq. (9) (solver vector formulas): agree to <= 0.15% in all rows -> solver vector-sector formulas
  consistent with the paper's Eqs. (8),(9). a1 - rho difference reproduced exactly (Eqs. 10-11 vs solver).
- Eq. (6) (A13): systematically lower, spread 0.4% (Z_pi=1.1) ... 3.5% (Z_pi=1.9).
  Not explained by +-0.5 MeV rounding of the three printed masses (best case still 0.2-1.8%).
  Needed m_K1 shift for exact consistency: -19 MeV (Z_pi=1.1), -3 MeV (1.5), -1.4 MeV (1.9).

## Verdict
Partial match. Solver's A13 Z_K is literally the paper's Eq. (6), so the residual is an internal inconsistency
of the preliminary Table 1 (superseded by the full Parganlija et al. 2013 analysis), not a solver error.
Recheck against the 2013 paper when it comes up.

## Side observation about the solver (not changed)
`wavefunction_factors` raises if ANY of the four radicands is non-positive, even when only Z_K is needed
(here Z_etaS went negative at large trial sigma_y). Harmless for the solver's own use, but limits
root-bracketing in external checks.
