# External cross-check: source comparison and equation audit

Date: 2026-10-18

## Sources compared

1. Friesen et al., *Temperature Dependence of the Masses of Various Meson States: A Comparative Study in the SU(3) and SU(4) extended linear-sigma models*, arXiv:2512.22198v2 (Dec 2025) and journal version, Phys. Rev. C 114, 015207 (2026), DOI: 10.1103/ms99-5qnq.
2. Tawfik et al., *Derivation of Meson Masses in SU(3) and SU(4) Extended Linear Sigma Model at Finite Temperature*, arXiv:2502.01644 / Particles 8 (2025) 9, DOI: 10.3390/particles8010009.
3. Parganlija, Giacosa & Rischke, *Vacuum properties of mesons in a linear sigma model with vector mesons and global chiral invariance*, Phys. Rev. D 82, 054024 (2010), DOI: 10.1103/PhysRevD.82.054024.

## Findings: likely transcription or convention issues

### A. Stationarity equations in the related 2025 paper

The potential printed in Eq. (1) of the related Tawfik et al. paper implies, by direct differentiation,

`h_x = m^2 sigma_x - C/sqrt(2) sigma_x sigma_y + ell_1 sigma_x sigma_y^2 + (2 ell_1 + ell_2)/2 sigma_x^3`

`h_y = m^2 sigma_y - C/(2 sqrt(2)) sigma_x^2 + ell_1 sigma_x^2 sigma_y + (ell_1 + ell_2) sigma_y^3`.

The text extraction of that paper's Eqs. (5)-(6) instead displays minus signs for the `ell_1` mixed terms, and Eq. (6) appears to use `sigma_x` rather than `sigma_y` in its first term. Those printed forms do not follow by differentiating its own displayed potential. The implementation in this repository follows the derivative of the potential (and the main target paper's Eqs. 15-16), not those apparent typos.

### B. Wave-function factors in the related 2025 paper

Its extracted Eqs. (34)-(36) show denominators without square roots for `Z_K`, `Z_etaS`, and `Z_K0star`; as printed, this also produces dimensionally incorrect expressions for dimensionless Z factors. The target paper's Appendix B Eq. (A13) and the conventional eLSM formulas use square roots. This repository uses the square-root form and tests positivity of each radicand.

### C. Eq. (32) is not a complete channel prescription by itself

Both the target paper and the related derivation paper express the thermal correction using derivatives `d(m_f^2)/d(phi_i,a)` and `d^2(m_f^2)/d(phi_i,a)d(phi_i,b)`, but this notation does not replace the explicit field-dependent Yukawa mass matrix and its normalization conventions. For diagonal condensate directions, the derivatives are determined directly from `m_u=m_d=g sigma_x/2`, `m_s=g sigma_y/sqrt(2)`. For charged/off-diagonal pseudoscalar and scalar directions, deriving eigenvalue derivatives requires the full matrix-level Yukawa coupling and careful treatment of degeneracies. We do not fabricate those entries from channel names alone.

### D. Published 2026 version versus preprint

The journal version is indexed as Phys. Rev. C 114, 015207 (published 27 July 2026). The accessible abstract and extracted Eq. (32) retain the same thermal-mass derivative structure and the same stated vacuum assumption. No public supplementary numerical arrays or a complete channel-by-channel derivative table were identified in the accessible material during this audit. Thus publication status does not resolve the implementation ambiguities above.

## What has been independently validated in code

- The analytic gradient of the tree-level potential agrees with central finite differences.
- The analytic Hessian agrees with finite differences of the analytic gradient.
- The diagonal constituent-mass first and second derivatives in the `(sigma_x, sigma_y)` directions agree with finite differences of `m_f^2`.
- Existing tests exercise the thermal integral's shape/symmetry and finite-valued behavior.

These are implementation-level checks, not proof that all channel-specific thermal corrections or the paper's plotted curves have been reproduced.

## References

- https://arxiv.org/abs/2512.22198
- https://journals.aps.org/prc/abstract/10.1103/ms99-5qnq
- https://arxiv.org/abs/2502.01644
- https://www.mdpi.com/2571-712X/8/1/9
- https://doi.org/10.1103/PhysRevD.82.054024
