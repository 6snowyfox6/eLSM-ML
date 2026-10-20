# Source audit notes (history)

> **Historical log.** Most sections below were written while the model used the PCAC
> vacuum (sigma_x = 0.1263 GeV) and are superseded by the final section
> "Resolution: vacuum convention" and by [`CHANGELOG.md`](CHANGELOG.md). Scripts and
> result files referenced here (`results/VALIDATION_STATUS.md`, `*_audit.csv`,
> `scripts/run_*_audit.py`, …) were removed from the repository on 2026-10-21; this
> file is kept only as a record of the reasoning.

## Confirmed directly from the supplied article

- Eq. (14): SU(3) tree-level scalar mean-field potential.
- Eqs. (15)–(16): stationarity equations defining `h_x` and `h_y` for chosen
  condensates.
- Eqs. (26)–(29): decomposition of the grand potential, Polyakov-modified
  quark logarithms, and quark mass/condensate conventions.
- Eqs. (32)–(36): finite-temperature correction to the mass-squared matrix,
  distribution functions `n_f^±`, and normalization factors `b_f^±`.
- Eq. (37): PCAC relations include channel-specific wave-function factors `Z_i`.
- Eq. (43): the optional replacement `m0^2 -> m0^2 (1 - T^2/Tc^2)`.
- Table II: SU(3), `c != 0` values used in `configs/su3.yaml`.
- Appendix B, Table A1: SU(3) vacuum mass formulas and wave-function-factor
  definitions in Eq. (A13).

## Cross-checked against standard external literature

The logarithmic Polyakov-loop form and coefficients
`a0=3.51`, `a1=-2.47`, `a2=15.2`, `b3=-1.75` are the standard
Roessner–Ratti–Weise parameterization, reproduced in PLSM literature. The
main article fixes `T0=0.27 GeV`.

## Still not source-resolved

1. Eq. (27) as printed appears dimensionally inconsistent in the zero-point
   term (`T` multiplies an integral of `E`, giving an extra energy dimension).
   No regularization/subtraction procedure is specified. The code therefore
   keeps vacuum energy separate and does not claim the thermal-only option is
   the exact full potential of the paper.
2. The PCAC/Z-factor relations and Table II do not by themselves establish an
   unambiguous, internally consistent numerical pair `(sigma_x,sigma_y)` in
   the supplied main text. No such pair is hard-coded.
3. Eq. (32) delegates SU(3) channel-specific derivatives to Refs. [59,66]. The
   general integral is implemented, but the supplied main text alone does not
   enumerate derivatives for each physical channel. No unsupported derivatives
   have been invented; channel-by-channel derivation from the full Yukawa mass
   matrix remains required before claiming full in-medium mass-curve reproduction.
4. Numerical agreement with the paper's Fig. 1 and meson-mass plots has not yet
   been demonstrated. Passing unit tests establishes code mechanics, not
   physics validation.


## Follow-up audit (2026-10-18)

- Added a second explicitly named cutoff vacuum term, `vacuum_quark_potential_unsubtracted`, alongside the massless-reference-subtracted version. Both require a positive user-supplied sharp three-momentum cutoff and both are labelled alternative prescriptions, not reconstructions of the ambiguous printed Eq. (27).
- `grand_potential` now accepts `vacuum_prescription="sharp_cutoff_unsubtracted"` as well as `omit` and `sharp_cutoff_subtracted`. This makes sensitivity to the subtraction convention testable instead of implicit.
- Added a regression test that checks finite values and the expected ordering between the two zero-point prescriptions.
- Eq. (32) remains a mathematically generic routine. It is deliberately not wired to all physical channels: the article points to Refs. [59,66] for SU(3) derivatives but does not reproduce their channel table in the main text. No full-channel result should be claimed until those source equations are retrieved and checked against the exact field normalization.
- Validation target still required: digitize the paper's SU(3) temperature curves and compare with a single source-consistent parameter set, with documented sensitivity to vacuum prescription, integration cutoff, momentum grid, minimizer starts and bounds.

## External source comparison (2026-10-18)

Added `EXTERNAL_CROSSCHECK.md` comparing the target preprint/journal version with
Tawfik et al., *Derivation of Meson Masses in SU(3) and SU(4) Extended Linear
Sigma Model at Finite Temperature* (arXiv:2502.01644; Particles 8 (2025) 9),
and the standard Parganlija–Giacosa–Rischke eLSM reference.

This comparison found apparent transcription errors in the related 2025 paper:
its printed stationarity equations do not differentiate its own potential as
written, and the extracted wave-function-factor equations omit square roots
from denominators that must have mass dimension squared. The implementation
retains the derivatives of the potential and the dimensionally consistent
square-root Z factors. Details and references are in `EXTERNAL_CROSSCHECK.md`.

Added finite-difference tests for the tree-level potential gradient/Hessian and
for the constituent quark mass derivatives in the diagonal `(sigma_x,sigma_y)`
directions. Test suite now passes 19 tests. These tests validate implemented
calculus but do not validate unimplemented off-diagonal physical meson channels.
The full Fig. 1 / temperature-curve comparison is still not complete: the
source-consistent vacuum prescription and complete field-level Yukawa matrix
must be settled before curve agreement can be claimed.

## Multiminimum / vacuum-sensitivity diagnostic (2026-10-18)

Added `scripts/audit_multiminima.py` and `results/multiminima_vacuum_sensitivity.csv`.
The audit runs four independent starts at T = 0.18, 0.195, 0.20, 0.205, 0.22,
0.25 GeV, and compares the thermal-only potential with massless-subtracted sharp
cutoffs 0.6, 0.8, and 1.0 GeV. It also solves a diagnostic constrained problem
with `phi == phi_bar` at mu=0.

Findings (diagnostic parameter set, not a validated reproduction):

- For the thermal-only potential, the lowest of the four starts is often an
  asymmetric Polyakov-loop solution. For example at T=0.20 GeV it reaches
  `(phi, phi_bar) ~= (0.588, 0.999)` or the swapped partner, while the best
  constrained symmetric solution has `(phi,phi_bar) ~= (0.567,0.567)` and a
  higher Omega. The potential is exchange-symmetric at mu=0, so the pair of
  asymmetric solutions is degenerate, but this is a warning that the unconstrained
  mean-field minimization is selecting a symmetry-breaking branch. We should not
  call the scan physical until the intended mu=0 Polyakov prescription is settled.
- Vacuum prescription strongly affects condensates. At T=0.20 GeV the best
  starts give sigma_x ~= 0.022 GeV for the thermal-only potential, ~=0.180 GeV
  for a 0.6 GeV subtracted cutoff, ~=0.236 GeV for a 0.8 GeV cutoff, and ~=0.290
  GeV for a 1.0 GeV cutoff. These alternatives are not source-equivalent schemes;
  this spread demonstrates that the unresolved zero-point term is a leading model
  choice, not a small numerical correction.
- Several Polyakov coordinates hit the imposed 0.999 bound. That means the result
  is partly controlled by the numerical guardrail and must not be treated as a
  converged unconstrained minimum without further physical-domain analysis.
- CSV rows preserve all starts rather than only the selected result. Reproduce with
  `PYTHONPATH=src python scripts/audit_multiminima.py`.

These checks resolve part of the earlier question: the jumps are not simply a
single-start accident. But they expose model/prescription sensitivity and an
unphysical-looking mu=0 asymmetric branch; they do not yet validate the paper's
Fig. 1. No figure-error metric is claimed until reference curves are digitized and
an apples-to-apples potential/parameter convention is established.

## Follow-up sensitivity audit (2026-10-18)

- Read the cited source [59] directly: B.-J. Schaefer and M. Wagner,
  Phys. Rev. D 79, 014018 (2009), arXiv:0808.1491. Its Sec. III explicitly
  states that the ultraviolet-divergent vacuum contribution to the quark
  grand potential is neglected (no-sea mean-field approximation). This is
  strong evidence that the source-consistent baseline for the target article
  is to omit the zero-point term, despite the dimensionally inconsistent
  prefactor printed for that term in Eq. (27). The omission is a source-based
  inference, not a correction confirmed by the target authors.
- At mu=0, the physical real mean-field branch is constrained by
  phi=phi_bar, as in standard PNJL/PQM mean-field treatments. The unconstrained
  minimization instead finds lower-Omega asymmetric solutions with one loop
  pinned to the artificial bound 0.999. This is a branch/domain pathology of
  the unconstrained numerical setup; do not treat those solutions as physical.
- With phi=phi_bar, four distinct starts converge to the same solution for
  T=0.18, 0.20, 0.22 and 0.25 GeV. Across 400/800/1600 momentum points and
  p_max=3/5/8 GeV, the largest observed changes at those temperatures are
  below 3e-6 GeV in sigma_x, below 4e-6 GeV in sigma_y, and below 1.7e-5
  GeV^4 in Omega. Thus the remaining sharp condensate change is not explained
  by the tested momentum-grid/cutoff settings.
- Vacuum-term sensitivity was quantified as a diagnostic only. With the
  source-motivated no-sea baseline at T=0.20 GeV, sigma_x=0.03275 GeV;
  adding an explicit, non-source-derived sharp-cutoff-subtracted vacuum term
  changes sigma_x to 0.19019, 0.24095, and 0.29021 GeV for cutoffs 0.6, 0.8,
  and 1.0 GeV. Those cutoff variants are not alternative reproductions of the
  paper; they demonstrate why the vacuum prescription must not be guessed.
- The supplied Fig. 1 raster was inspected. Curves overlap, and the red
  branches cannot be assigned robustly by the current automatic pixel trace.
  No RMSE or maximum-error statistic is claimed until a curve-by-curve trace
  is manually verified. The computed symmetric thermal-only curve is saved in
  results/symmetric_thermal_only_curve.csv for the next comparison pass.
- The cited Schaefer–Wagner paper's Eq. (8) uses the standard thermal coefficient
  `nu_c*T` with `nu_c=2*N_c=6`, while the target paper's Eq. (27) appears to print
  `-2*nu_c*T`; this is a possible factor-of-two transcription/convention issue.
  The implementation retains the cited no-sea coefficient rather than silently
  doubling the thermal term. See `results/VALIDATION_STATUS.md`.
- With the current PCAC-fit inputs and this source-motivated baseline, the
  maximum discrete slope of normalized `sigma_x(T)` is near 0.19 GeV, compared
  with 0.2325 GeV in target Table III for SU(3), c != 0, without F(T). This
  ~43 MeV discrepancy survives the tested integration-grid variation, so the
  current fit is not yet validated against the paper.

## Critical normalization correction (2026-10-18, follow-up)

A code audit found that `quark_thermal_potential` multiplied the Polyakov-modified
quark logarithm by `2*N_c`. This double-counted color: the traced logarithm already
contains the color structure through its `3*phi`, `3*phi_bar`, and cubic terms.
The standard prefactor outside this logarithm is 2 for spin. The implementation
now uses `-2*T*integral`; the legacy `n_c` keyword remains accepted for API
compatibility but does not rescale this traced-log expression. A regression test
checks that changing that legacy argument does not change the result.

This correction materially changes the condensate curve. Therefore all earlier
numerical statements about the 43 MeV discrepancy, as well as earlier sensitivity
CSV files generated before this correction, must be treated as superseded and
not used as validation evidence until regenerated. A quick coarse-grid diagnostic
with the corrected prefactor gives a maximum-slope temperature around 0.258 GeV
for the table Yukawa coupling g=5.5 and T0=0.27 GeV (grid spacing 8.3 MeV); this
is only a coarse diagnostic, not a final T_chi estimate. The target paper reports
0.2325 GeV for SU(3), c != 0, without F(T). The remaining mismatch is therefore
roughly 26 MeV at this coarse resolution and needs a finer, fully regenerated
scan plus resolution and source-parameter checks.

## Corrected fine scan and parameter consistency (2026-10-18)

`results/fine_parameter_scan.csv` contains four 41-point scans from 0.20 to
0.28 GeV with step 2 MeV. With the corrected traced-log prefactor, the
maximum-slope temperatures are 0.258 GeV (g=5.5, T0=0.27), 0.248 GeV
(g=5.5, T0=0.22), and 0.244 GeV (g=5.5, T0=0.20). The PCAC-derived Yukawa
coupling g=4.7502 with T0=0.27 gives a maximum at 0.276 GeV, the scan's upper
boundary, so that result is censored and not a trustworthy peak estimate.
These scans use 100 momentum points and p_max=2.5 GeV; they are parameter
sensitivity diagnostics, not final converged validation runs.

The paper states g=5.5 and g=2*m_q/sigma_x with m_q=0.300 GeV, which implies
sigma_x=0.10909 GeV. However, the PCAC/Z-factor inputs listed in this project
produce sigma_x=0.12631 GeV and g=4.7502 at the same constituent mass. This is
an unresolved internal parameter-consistency issue in the source/fit mapping.
Do not tune T0 or g merely to force T_chi=0.2325 GeV. Fig. 1 was inspected from
the supplied PDF; the SU(3) branches overlap too closely for an unverified
pixel trace to support an honest RMSE yet.


## High-resolution follow-up (2026-10-18)

A wider temperature scan for the PCAC-derived Yukawa coupling was run with 45 points on T=0.12--0.34 GeV (ΔT=5 MeV), 240 momentum points up to 4 GeV, warm starts plus two alternative initial conditions, and the thermal-only/no-sea prescription. It gives the largest sampled |d sigma_x/dT| at T=0.275 GeV. This is an interior point, but the result is not yet a full convergence claim: momentum-grid and minimizer-start convergence still need a controlled comparison. Data are in `results/fine_parameter_scan.csv`; the script is `scripts/run_fine_parameter_scan.py`.

The paper's parameter discussion (PDF pp. 11--12) says g is fixed from g=2 m_q/sigma_x with m_q=0.3 GeV, while Table II lists g=5.5. The PCAC relations using the paper's stated f_pi, f_K, m_a1, m_K1, and g1 yield sigma_x=0.12631 GeV, hence g=4.7502 from that relation. Conversely, g=5.5 and m_q=0.3 imply sigma_x=0.10909 GeV. These are not the same vacuum point. The paper does not tabulate h_x,h_y directly in Table II; it states they are defined by the global minimum of the mesonic potential. Our h_x,h_y therefore follow from Eq. (15)--(16) evaluated at the PCAC-derived condensates and Table II scalar-sector parameters, not from an independently reported pair in the paper. This is a reproducibility ambiguity, not proof of an error by the authors.

Figure 1 is not quantitatively digitized yet. We deliberately do not report a curve-fit error until the plotted branches can be identified and manually checked. The current numerical scan remains a diagnostic, not a validated reproduction.

## Extended scan / PCAC consistency update (2026-10-18)

An extended temperature scan was run for `g=4.750203647` (the value implied by
`2*m_q/sigma_x` using the PCAC-derived `sigma_x`) and for the tabulated `g=5.5`,
with `T0=0.270 GeV`, over 0.120–0.340 GeV (56 points, 4 MeV spacing), using
180 radial momentum points up to 3 GeV, thermal-only/no-sea prescription, and
`phi=phibar`. Output: `results/extended_convergence_scan.csv`.

- PCAC-implied g: maximum discrete `|d sigma_x/dT|` at 0.276 GeV.
- Tabulated g=5.5: maximum discrete `|d sigma_x/dT|` at 0.260 GeV.
- Neither maximum is at a scan boundary. These are still conditional on the
  stated thermal-only prescription and the chosen mean-field/Polyakov setup.
  The scan spacing is 4 MeV; do not report more precision than this without a
  local refinement and integration-convergence check.

The PCAC condensates `sigma_x=0.1263103742 GeV`, `sigma_y=0.0977103971 GeV`
combined with the Table-II scalar couplings reproduce the stationarity sources
from Eqs. (15)-(16): `h_x=0.01358729156 GeV^3`, `h_y=0.01921262401 GeV^3`.
Thus the source terms are not the inconsistency in this implementation. The
remaining mismatch is the simultaneous use of `g=5.5`, `m_q=0.300 GeV`, and
`g=2*m_q/sigma_x`: the latter two with the PCAC-derived `sigma_x` imply
`g=4.7502`, while `g=5.5` implies `sigma_x=0.10909 GeV` for the same `m_q`.

No RMSE against Fig. 1 is claimed: a verified digitization of individual
curves has not yet been completed. Do not treat this as a full reproduction.

## Wider scan and exact source-equation audit (2026-10-18)

A wider scan was run for the PCAC-derived Yukawa coupling `g=4.750203647` with
`T0=0.270 GeV`, `T=0.100..0.360 GeV` in steps of 0.004 GeV, 180 momentum
points up to 3 GeV, thermal-only/no-sea prescription, and the symmetric branch
`phi=phi_bar`. The largest sampled absolute slope occurs at `T=0.276 GeV`
(slope about `-0.861` in GeV/GeV). This is an interior maximum on this grid, but
is not a convergence-certified physical result: the branch is obtained with a
coarse temperature grid and a finite number of optimizer starts. Output:
`results/wide_convergence_pcac_g.csv`; reproducible driver:
`scripts/run_wide_pcac_scan.py`.

The supplied paper's Eq. (27) was rechecked directly from the PDF text. As
printed, it has `-2 nu_c T` in front of the thermal logarithms and defines
`nu_c=2 N_c`; this is a factor `2 N_c` outside a logarithm whose `g_f^+/-`
expressions already contain the Polyakov color trace. Its vacuum integral also
appears with a factor `T` and without a visible regulator/subtraction, which is
dimensionally problematic for a grand-potential density. The implementation
therefore does NOT literally reproduce Eq. (27): it uses the standard traced-log
thermal normalization `-2 T` and omits the UV-divergent vacuum term in line with
the cited no-sea treatment. This is a defensible convention choice, but the
source discrepancy is unresolved and could materially affect the condensate
curve and `T_chi`.

Figure 1: axis mapping for the SU(3) panel is approximately x=158..475 px for
T=0..0.5 GeV and y=440..170 px for normalized condensate 0..1 in the supplied
page raster (1105x1430). The red solid curves represent finite anomaly `c=2.828`
without F(T), but multiple branches and colors overlap near the crossover. No
RMSE is reported until the sigma_x and sigma_y branches are manually traced and
checked against the raster. This is intentional; approximate pixel extraction
must not be presented as precise digitization.

## 2026-10-18 — Eq. (27) thermal prefactor experiment

Added `thermal_normalization` to `quark_thermal_potential` and `grand_potential`, default 1.0. Regression test checks that value 6 multiplies the thermal contribution by exactly six. `scripts/compare_thermal_normalizations.py` runs both cases with identical parameters and numerical settings; outputs are in `results/thermal_normalization_comparison.csv` and `results/thermal_normalization_summary.csv`. Standard traced-log convention yields sampled T_chi=0.276 GeV for the PCAC-derived g; literal Eq. (27) prefactor yields 0.160 GeV. Do not select a convention merely because it is closer to the paper's 0.2325 GeV.

## 2026-10-18 — dense-grid stability and visual Fig. 1 check

The standard traced-log branch was rescanned at 1 MeV (240 momentum points) and 0.5 MeV (360 momentum points) resolution near the crossover. Both locate max |d sigma_x/dT| at 0.276 GeV; magnitudes 0.862118 and 0.862246, respectively (~0.015% difference). See `results/dense_grid_convergence.png` and CSVs. Visual reading of the red solid finite-c/no-F(T) curve in Fig. 1 suggests its steepest fall around 0.23--0.25 GeV; because red curves overlap in the raster, no exact pixel RMSE is reported. Internal numerical convergence does not establish reproduction: the model's Tchi remains ~0.276 GeV vs Table III's 0.2325 GeV.

## Fig. 1 digitization attempt

A source-raster pixel candidate extractor was added. It exports red pixels with calibrated (T, normalized ordinate) coordinates but does not force a unique curve trace because overlapping red branches make that inference non-identifiable in parts of the plot. No RMSE or digitized single-curve fit is claimed. Visual target transition is earlier than both existing model variants; do not describe the replication as complete.

### Figure 1 manual branch trace
A manually anchored trace for the steep red solid sigma_x branch was added under `results/fig1_sigmax_manual_anchors.csv`; its interpolation is stored separately. It places the half-height at roughly 0.201 GeV, while current model curves reach half-height around 0.265 GeV (g=5.5) or 0.287 GeV (PCAC-derived g). Approximate anchor RMSEs are recorded in `results/VALIDATION_STATUS.md` and plotted in `results/fig1_sigmax_shape_comparison.png`. These are diagnostic, not rigorous fit statistics, because the source is a low-resolution raster with overlapping branches.


## 2026-10-18 — Resolution: vacuum convention (supersedes the PCAC-based notes above)

The PCAC inversion (sigma_x=0.1263) is not the authors' vacuum. Table I's own vector masses fix
sigma_x=0.1090 = 2 m_q/g, so the "g=5.5 vs 4.75 tension" is an artefact of the PCAC route, and
Eq. (27)'s literal 2*nu_c prefactor is a misprint (Eq. 32 with nu_c=6 is exactly the Hessian of
the -2T potential; test added). Corrected Table A1 entries: eta_NS = Z_pi Z_etaS c/sqrt(2) sigma_x,
K0* anomaly term +c/2 sigma_x. With these, Tables I, III, V are reproduced; see CHANGELOG_FIX.md.
The manual Fig. 1 trace (half-height ~0.201 GeV) matches the c=0 + F(T) branch (0.2025 GeV),
not c!=0 without F(T); the caption/legend of Fig. 1 are inconsistent.
