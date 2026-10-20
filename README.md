# eLSM-SU3: reference solver for the SU(3) extended Linear Sigma Model

A transparent numerical implementation of the SU(3) sector of

> Friesen et al., *Temperature Dependence of the Masses of Various Meson States: A
> Comparative Study in SU(3) and SU(4) extended Linear-Sigma Model*,
> [arXiv:2512.22198v2](https://arxiv.org/abs/2512.22198); Phys. Rev. C 114, 015207 (2026),
> [DOI 10.1103/ms99-5qnq](https://journals.aps.org/prc/abstract/10.1103/ms99-5qnq).

The goal is an auditable baseline: every convention that the paper leaves ambiguous is
made explicit in code and documented, rather than silently guessed.

## Status

| Target | Result |
|---|---|
| Table I / V vacuum spectrum (π, η, η′, σ, a₀, K₀\*, f₀, ρ, a₁) | reproduced to < 0.5 % |
| Table III chiral temperature T_χ (4 variants) | within one 2.5 MeV grid step (3 of 4), exact (1 of 4) |
| Kaon mass, Table I 420.49 MeV | **not reproduced**: the model gives ≈ 495 MeV (experiment: 495.6) |
| f_π, f_K from Table I | inconsistent with the Table I masses (model: 89.7 / 105.1 MeV) |
| Eq. (32) in-medium meson masses (Figs. 2–4) | generic routine only, not yet wired to individual channels |

Full numbers: [`results/PAPER_REPRODUCTION.md`](results/PAPER_REPRODUCTION.md).

### Key conventions (see [`docs/CHANGELOG.md`](docs/CHANGELOG.md))

- **Vacuum:** `sigma_x = 2 m_q / g = 0.10909 GeV` (m_q = 0.3 GeV, g = 5.5), as in the paper's
  Sec. III. `sigma_y` is fitted to the Table I spectrum (`fit_condensates_yukawa`). The
  PCAC inversion of Eq. (37) (`fit_condensates_pcac`, sigma_x = 0.1263 GeV) is kept for
  diagnostics only: it gives m_π ≈ 436 MeV.
- **Table A1 misprints corrected** against the Lagrangian Hessian:
  `eta_NS` uses `c/sqrt(2) sigma_x`; the K₀\* anomaly term is `+c/2 sigma_x`.
- **Quark thermal term:** `-2T ∫ (log g⁺ + log g⁻)`. The color trace is already inside
  the Polyakov logarithm, so the printed `2 nu_c` prefactor of Eq. (27) is treated as a
  misprint. This is consistent with Eq. (32) with `nu_c = 6` (tested).
- **Dirac sea:** omitted (no-sea, as in the cited Schaefer & Wagner, arXiv:0808.1491).
  Two sharp-cutoff alternatives are available as explicit options for sensitivity studies.
- **Polyakov loop:** Roessner–Ratti–Weise logarithmic potential
  (a₀=3.51, a₁=−2.47, a₂=15.2, b₃=−1.75), T₀ = 0.27 GeV. At μ = 0, `phi = phi_bar` is imposed.

## Repository layout

```
src/elsm/          the package
  parameters.py      SU3Parameters (h_x, h_y from Eqs. 15-16), PolyakovParameters
  potential.py       Eq. (14) + gradient/Hessian, Polyakov potential, quark terms, grand_potential
  meson_masses.py    Table A1 tree-level masses, Z factors (Eq. A13), isoscalar mixing
  thermal_masses.py  Eq. (32) thermal mass-matrix correction, occupations (Eqs. 33-36)
  vacuum_fit.py      vacuum calibration: fit_condensates_yukawa (primary), fit_condensates_pcac
  solver.py          multistart minimisation and temperature continuation
tests/             unit and paper-reproduction tests
scripts/           reproduce_paper_tables.py, run_temperature_scan.py
verification/      standalone checks of the Table A1 formulas and of the authors' vacuum
configs/su3.yaml   Table II parameter sets (reference only; not read by the code)
data/              manual digitisation of the Fig. 1 sigma_x branch
results/           generated outputs
docs/              changelog, external cross-check, full audit history
```

## Installation

```bash
python -m pip install -e ".[dev,plot]"
pytest
```

Requires Python ≥ 3.10, numpy, scipy (matplotlib only for `run_temperature_scan.py`).

## Usage

```python
from elsm import (SU3Parameters, PolyakovParameters, grand_potential,
                  fit_condensates_yukawa)

row = dict(m0_sq=-0.066564, lambda1=3.15, lambda2=36.253, c_anomaly=2.828)  # Table II, c != 0
vac = fit_condensates_yukawa(**row)          # sigma_x = 0.10909, sigma_y = 0.10361 GeV
p = SU3Parameters.from_vacuum_condensates(
    **row, sigma_x=vac.sigma_x, sigma_y=vac.sigma_y, g_yukawa=5.5, Tc=0.270, include_FT=False)

omega = grand_potential(
    T=0.15, sigma_x=vac.sigma_x, sigma_y=vac.sigma_y, phi=0.2, phi_bar=0.2, p=p,
    polyakov_parameters=PolyakovParameters(T0=0.270), vacuum_prescription="omit")
```

`include_FT=True` switches on the Eq. (43) replacement `m0² → m0² (1 − T²/Tc²)`.

### Reproducing the paper

```bash
python scripts/reproduce_paper_tables.py          # Tables I, V, III (~minutes); --fast for 5 MeV grid
python scripts/run_temperature_scan.py            # exploratory condensate scan + plot
python verification/derive.py                     # Table A1 vs Lagrangian Hessian
python verification/reconstruct.py                # which vacuum the authors used
```

Outputs are written to `results/`.

## Known limitations

- **Eq. (32) channel map.** The paper delegates the channel-specific derivatives
  `d(m_f²)/dφ`, `d²(m_f²)/dφdφ` to its Refs. [59, 66]. Only the `(sigma_x, sigma_y)`
  directions are provided (`sigma_field_mass_derivatives`); the full Yukawa-matrix
  derivation per meson channel is still to be done.
- **`run_temperature_scan.py`** minimises over `phi` and `phi_bar` independently
  (`solve_mean_fields`). At μ = 0 this can select an asymmetric Polyakov-loop branch
  (see `docs/AUDIT_HISTORY.md`), so its curve differs from the `phi = phi_bar` scans used for
  Table III. Treat it as exploratory.
- **Fig. 1.** The manual trace in `data/` (half-height ≈ 0.201 GeV) matches the c = 0 + F(T)
  branch rather than c ≠ 0 without F(T); the figure's caption/legend appear inconsistent.
  No RMSE against the raster figure is claimed.

## Further documentation

- [`docs/CHANGELOG.md`](docs/CHANGELOG.md): the 2026-10-18 vacuum fix and its evidence.
- [`docs/EXTERNAL_CROSSCHECK.md`](docs/EXTERNAL_CROSSCHECK.md): comparison with the journal
  version and with Tawfik et al., arXiv:2502.01644 (apparent misprints in that paper's
  stationarity equations and Z factors).
- [`docs/AUDIT_HISTORY.md`](docs/AUDIT_HISTORY.md): the full chronological audit log
  (mostly superseded; kept for traceability).

## References

- Friesen et al., arXiv:2512.22198v2; Phys. Rev. C 114, 015207 (2026).
- Roessner, Ratti & Weise, Phys. Rev. D 75, 034007 (2007), DOI 10.1103/PhysRevD.75.034007.
- Schaefer & Wagner, Phys. Rev. D 79, 014018 (2009), arXiv:0808.1491.
- Parganlija, Giacosa & Rischke, Phys. Rev. D 82, 054024 (2010), DOI 10.1103/PhysRevD.82.054024.
- Tawfik, Diab & Hussein, Chin. Phys. C 43, 034103 (2019), DOI 10.1088/1674-1137/43/3/034103.
