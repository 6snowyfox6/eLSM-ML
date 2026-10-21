# Röder, Ruppert, Rischke 2003 — arXiv:nucl-th/0301085 (PRD 68, 016003)

**Class:** A for the U(3)xU(3) vacuum (Table IV, p. 16); B for finite T (Hartree). O(4), U(2), U(4) parts skipped (SU(3) scope).
**What was run:** `tools/vacuum_check.py` on `params.yaml`, four parameter sets, Z_i = 1. Details: `vacuum_check.md`.

## Results
| set | max rel. diff | verdict |
|---|---|---|
| physical, c != 0 | 9e-4 (pion), others <= 3e-4 | match within printed rounding |
| physical, c = 0  | 2e-3 (pion), others <= 6e-4 | match within printed rounding |
| chiral limit, c != 0 | 3e-4 massive states; Goldstones 5.9 MeV instead of 0 | rounding artefact |
| chiral limit, c = 0  | 1e-4 massive states; Goldstone m^2 ~ -1e-6 GeV^2 (NaN) | rounding artefact |

## Notes
- Parameters are printed with 2 decimals (e.g. lambda1 = -24.13); the pseudo-Goldstone pion amplifies this
  (m_pi^2 ~ h_x/sigma_x is a near cancellation), hence the largest deviations sit in the pion.
- Chiral-limit sets: the solver's stationarity gives |h| ~ 1e-6 GeV^3 instead of 0, again from rounding;
  Goldstone masses ~ sqrt(h/sigma) ~ few MeV. Expected, not a defect.
- Second independent confirmation of the K0* and eta/eta' formulas (mesonic sector of Table A1).

## Feature candidates (not implemented)
- Same as Lenaghan 2000: Hartree finite-T for the mesonic LSM; Z_i = 1 mode.
- U(4) parameter set (Table V) — out of SU(3) scope; relevant only if SU(4) is added.
