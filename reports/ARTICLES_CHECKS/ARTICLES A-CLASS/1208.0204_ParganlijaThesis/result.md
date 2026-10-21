# D. Parganlija, PhD thesis "Quarkonium Phenomenology in Vacuum" (2012), arXiv:1208.0204

**Class:** A (vacuum; same model as PRD 87, two earlier Nf=3 fits). Nf=2 chapters skipped (SU(3) scope).
**What was run:** `tools/elsm_vector_check.py` on `params.yaml` — Fit I (Tables 8.4/8.5: a0(980), kappa scenario)
and Fit II (Tables 10.2/10.3: a0(1450), K0*(1430)). Raw output: `vacuum_check.md`.

## Results
| quantity | printed parameters | with delta_S inferred from m_phi |
|---|---|---|
| rho, a1, a0, K0*, Z_pi | <= 0.4% (both fits) | same |
| K*, phi, K1, f1S | off 1-24% | K*, K1, f1S <= 7e-4 (phi used as input) |
| Z_K | off 1.1% / 12% | 0.04% / 0.9% |
| K | 1.4% / 13% | 0.3% / 1.6% |
| pion | +10% / +26% | same (rounding, see below) |

## Findings
1. **Misprint of delta_S in Tables 8.4 and 10.2.** Printed 2292 and 4852 MeV^2; the values consistent with the
   thesis's own vector masses are (228.8 MeV)^2 and (484.4 MeV)^2, i.e. printed ~ 10*sqrt(delta_S).
   With them three independent strange (axial-)vector masses agree to <= 7e-4.
2. Pion/kaon deviations are rounding: Z_pi and lambda2 are printed with 3 significant digits; the induced
   uncertainty of m_pi^2/Z_pi^2 (~0.003-0.006 GeV^2) exceeds the deviation (0.002-0.004 GeV^2).
3. No solver error found.
