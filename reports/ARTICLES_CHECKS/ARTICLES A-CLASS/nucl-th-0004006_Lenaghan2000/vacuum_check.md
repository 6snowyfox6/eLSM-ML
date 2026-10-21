# Vacuum check: Lenaghan, Rischke, Schaffner-Bielich, PRD 62, 085008 (2000), arXiv:nucl-th/0004006

U(3)xU(3) LSM, no quarks, no vector mesons (Z_i=1). Vacuum is tree level; finite T is Hartree (not supported by solver). Condensates from PCAC Eq. (16). eta/eta', sigma/f0 compared as lower/upper eigenvalues.

## A_c_finite
Source: Sec. III A, Eqs. (21)-(25), p. 9

sigma_x = 0.092400 GeV, sigma_y = 0.094469 GeV
h_x: solver 1.759331e-03, paper 1.759551e-03 GeV^3, rel -1.25e-04
h_y: solver 3.807038e-02, paper 3.807059e-02 GeV^3, rel -5.38e-06

| channel | paper, MeV | solver, MeV | rel. diff |
|---|---|---|---|
| pi | 137.995 | 137.987 | -5.88e-05 |
| K | 495.999 | 495.997 | -3.70e-06 |
| eta | 539.008 | 539.005 | -5.48e-06 |
| eta_prime | 963.046 | 963.045 | -1.20e-06 |
| sigma | 600.000 | 599.993 | -1.11e-05 |
| f0 | 1221.113 | 1221.113 | +2.09e-07 |
| a0 | 1028.707 | 1028.706 | -8.12e-07 |
| K0star | 1124.315 | 1124.316 | +4.62e-07 |

Max |rel. diff| over nonzero channels: 5.88e-05

## B_c_zero
Source: Sec. III B, Eqs. (26)-(28), p. 9. Text says m_sigma = 600 MeV

sigma_x = 0.092400 GeV, sigma_y = 0.094469 GeV
h_x: solver 1.759086e-03, paper 1.759551e-03 GeV^3, rel -2.64e-04
h_y: solver 3.806978e-02, paper 3.807059e-02 GeV^3, rel -2.11e-05

| channel | paper, MeV | solver, MeV | rel. diff |
|---|---|---|---|
| pi | 137.995 | 137.977 | -1.28e-04 |
| K | 495.999 | 495.992 | -1.35e-05 |
| eta | 137.995 | 137.977 | -1.28e-04 |
| eta_prime | 634.818 | 634.811 | -1.10e-05 |
| sigma | 600.000 | 799.990 | +3.33e-01 |
| f0 | 1341.367 | 1341.359 | -5.76e-06 |
| a0 | 850.387 | 850.380 | -7.78e-06 |
| K0star | 1124.315 | 1124.309 | -5.30e-06 |

Max |rel. diff| over nonzero channels: 3.33e-01

