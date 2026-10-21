# Lenaghan, Rischke, Schaffner-Bielich 2000 — arXiv:nucl-th/0004006 (PRD 62, 085008)

**Class:** A for the vacuum (same U(3)xU(3) mesonic potential, tree level); B for finite T (Hartree/CJT — not in solver).
**What was run:** `tools/vacuum_check.py` on `params.yaml` (Sec. III A, III B, p. 9) with Z_i = 1, condensates from PCAC Eq. (16).
Details: `vacuum_check.md`. Solver untouched.

## Results
| set | max rel. diff | verdict |
|---|---|---|
| A (c != 0) | 6e-5 (all 8 channels, h_x, h_y) | match |
| B (c = 0)  | 1e-4 for 7 channels; sigma 800.0 vs printed 600 | paper misprint |

## Findings
1. **Independent confirmation of the two Table A1 fixes** (2026-10-18) for arXiv:2512.22198: the kappa/K0*
   mass with anomaly term +c/2·sigma_x (1124.316 vs 1124.315) and the eta_NS mixing c/sqrt2·sigma_x
   (eta 539.005 / eta' 963.045 vs 539.008 / 963.046) agree to 1e-6 with an older, independent paper.
2. **Misprint in Lenaghan Sec. III B:** text says m_sigma = 600 MeV, but the printed lambda1 = -4.550,
   m^2 = -(503.551 MeV)^2 and m_f0 = 1341.367 MeV all correspond consistently to m_sigma = 800 MeV
   (solver: 799.99; m_sigma = 600 would need lambda1 ~ -17). Not a solver error.
3. At c = 0 the paper calls the pion-degenerate isoscalar "eta'", the solver orders by eigenvalue — naming only.

## Feature candidates (not implemented, per instructions)
- Hartree (CJT) finite-T approximation for the purely mesonic LSM (Figs. of T-dependent masses/condensates).
- Vacuum "Z_i = 1 / no vector mesons" mode as a first-class option (currently done via explicit Z dict).
