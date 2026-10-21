# Parganlija, Kovacs, Wolf, Giacosa, Rischke — PRD 87, 014011 (2013), arXiv:1208.0585

**Class:** A (vacuum). The reference Nf=3 eLSM fit on which arXiv:2512.22198 builds.
**What was run:** `runs/parganlija2012_prd87_vacuum.py` — Table III parameters into solver Table A1 + A13
(Z factors from the model's own vector masses). Raw output: `vacuum_check.md`.

## Convention mapping (verified against the paper's Eqs. 15-34)
- solver m0_sq <- C1 = m0^2 + lambda1(phiN^2+phiS^2), solver lambda1 <- 0 (exact for pi, K, a0, K0*; Eq. 49 is lambda1=0).
- solver vector c1 <- C2 = m1^2 + h1/2(phiN^2+phiS^2), delta_x = delta_N = 0, delta_y = delta_S.
- U(1)_A: PRD 87 uses c1 (detPhi - detPhi^dag)^2; it does not contribute to pi, K, a0, K0*, scalars or vectors
  at tree level, so solver c_anomaly = 0 is exact there. eta/eta' NOT checked (term not in solver).

## Results (13 observables)
| | max rel. diff |
|---|---|
| f_pi, f_K, rho, K*, phi, a1, f1(1420), a0, K0*, f0^L, f0^H (Eq. 49) | <= 3.7e-4 |
| pion | 6.5e-3 (pseudo-Goldstone cancellation; C1, phiN printed with 4 digits) |
| kaon | 1.1e-3 |

## Verdict
Match. The whole vacuum sector of the solver (A1 masses incl. vector/axial-vector, A13 Z factors, PCAC
f_pi = phiN/Z_pi, f_K = (phiN + sqrt2 phiS)/(2 Z_K)) reproduces the reference eLSM fit.

## Feature candidate (not implemented)
- Alternative U(1)_A term c1 (detPhi - detPhi^dag)^2 (eLSM papers of the Frankfurt group) for eta/eta'.
