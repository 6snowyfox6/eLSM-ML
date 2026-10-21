# Phase-structure check: Tripathi, Rai, Tiwari, arXiv:2512.13398 (2025), Table II (QM parameters) and Table III (s-MFA rows QM, Log PQM; m_sigma = 500)

Standard mean field (no-sea) QM and Log PQM rows only; RQM/RPQM (on-shell renormalized vacuum term), PolyLog-glue and vector interactions = features. Log potential RRW (as solver), T0 = 187 MeV (paper Sec. II B). Paper: Phi, Phibar independent reals at the saddle point of the real part (Sec. II B). Independent-real MINIMIZATION is ill-posed (runs to the Phibar=0.999 bound even at mu=0, see result.md), so Phi=Phibar (PQM_equal) is used: exact at mu=0; at mu>0 differs from the saddle-point method by a few MeV in mu_CEP (cf. Mintz 2013 Table III). Physical point g = 6.5 (Rai 2023). Chiral-limit rows (m_pi = 0, f_pi = 90.67): g not stated -> run with g = 6.5 and with g = 2*300.3/90.67 = 6.624.

## mu = 0 transition

| set | mode | paper T_c | solver T (max -dsigma_x/dT, 0.25 MeV) | largest sigma_x jump, MeV |
|---|---|---|---|---|
| QM_ms500 | QM | 129.0 | 129.00 | 1.90 |
| LogPQM_ms500 | PQM_equal | 159.3 | 159.25 | 2.88 |
| QM_ms500_chiral_g6.5 | QM | 123.2 | 120.50 | 64.58 |
| QM_ms500_chiral_g6.624 | QM | 123.2 | 119.50 | 66.02 |
| LogPQM_ms500_chiral_g6.5 | PQM_equal | 155.7 | 154.00 | 66.95 |
| LogPQM_ms500_chiral_g6.624 | PQM_equal | 155.7 | 153.75 | 67.76 |

## CEP

| set | mode | paper (mu, T) | solver: last first-order T (1 MeV grid) | mu there | mu at T+1 |
|---|---|---|---|---|---|
| QM_ms500 | QM | (165.24, 97.52) | 97 | 166.6 | 164.3 |
| LogPQM_ms500 | ... | ... | 137 | 165.0 | — |

## CEP scan detail

| set | T | first order | mu_transition | jump h | jump h/8 |
|---|---|---|---|---|---|
| QM_ms500 | 85 | True | 192.77 | 38.64 | 34.14 |
| QM_ms500 | 86 | True | 190.82 | 36.57 | 32.80 |
| QM_ms500 | 87 | True | 188.87 | 36.44 | 31.39 |
| QM_ms500 | 88 | True | 186.91 | 35.39 | 29.86 |
| QM_ms500 | 89 | True | 184.57 | 33.10 | 28.53 |
| QM_ms500 | 90 | True | 182.62 | 33.13 | 26.87 |
| QM_ms500 | 91 | True | 180.27 | 32.09 | 25.37 |
| QM_ms500 | 92 | True | 178.32 | 28.39 | 23.36 |
| QM_ms500 | 93 | True | 175.98 | 29.35 | 21.76 |
| QM_ms500 | 94 | True | 173.63 | 28.75 | 19.95 |
| QM_ms500 | 95 | True | 171.29 | 26.84 | 17.95 |
| QM_ms500 | 96 | True | 168.95 | 23.33 | 15.81 |
| QM_ms500 | 97 | True | 166.60 | 24.58 | 13.50 |
| QM_ms500 | 98 | False | 164.26 | 24.04 | 10.32 |
| QM_ms500 | 99 | False | 161.52 | 22.35 | 8.25 |
| QM_ms500 | 100 | False | 159.18 | 18.48 | 7.04 |
| QM_ms500 | 101 | False | 156.45 | 16.71 | 5.56 |
| QM_ms500 | 102 | False | 153.71 | 18.13 | 4.23 |
| QM_ms500 | 103 | False | 151.37 | 17.99 | 3.70 |
| QM_ms500 | 104 | False | 148.63 | 17.20 | 3.18 |
| QM_ms500 | 105 | False | 145.51 | 16.07 | 2.85 |
| QM_ms500 | 106 | False | 142.77 | 14.81 | 2.54 |
| QM_ms500 | 107 | False | 139.65 | 13.61 | 2.24 |
| QM_ms500 | 108 | False | 136.91 | 12.60 | 2.00 |
| QM_ms500 | 109 | False | 133.79 | 11.82 | 1.82 |
| QM_ms500 | 110 | False | 130.27 | 11.20 | 1.66 |
| QM_ms500 | 111 | False | 127.15 | 10.68 | 1.52 |
| LogPQM_ms500 | 112 | True | 227.93 | 65.67 | 64.47 |
| LogPQM_ms500 | 113 | True | 226.37 | 65.22 | 64.00 |
| LogPQM_ms500 | 114 | True | 224.41 | 64.82 | 63.53 |
| LogPQM_ms500 | 115 | True | 222.85 | 64.27 | 63.01 |
| LogPQM_ms500 | 116 | True | 220.90 | 63.89 | 62.51 |
| LogPQM_ms500 | 117 | True | 218.95 | 63.18 | 61.98 |
| LogPQM_ms500 | 118 | True | 216.99 | 62.84 | 61.42 |
| LogPQM_ms500 | 119 | True | 214.65 | 62.39 | 60.87 |
| LogPQM_ms500 | 120 | True | 212.70 | 61.56 | 60.25 |
| LogPQM_ms500 | 121 | True | 210.35 | 61.16 | 59.65 |
| LogPQM_ms500 | 122 | True | 208.40 | 60.66 | 58.95 |
| LogPQM_ms500 | 123 | True | 206.05 | 60.07 | 58.26 |
| LogPQM_ms500 | 124 | True | 203.71 | 59.09 | 57.52 |
| LogPQM_ms500 | 125 | True | 200.98 | 58.55 | 56.80 |
| LogPQM_ms500 | 126 | True | 198.63 | 57.92 | 55.95 |
| LogPQM_ms500 | 127 | True | 195.90 | 57.22 | 55.11 |
| LogPQM_ms500 | 128 | True | 193.16 | 56.43 | 54.20 |
| LogPQM_ms500 | 129 | True | 190.43 | 55.56 | 53.21 |
| LogPQM_ms500 | 130 | True | 187.70 | 53.94 | 52.11 |
| LogPQM_ms500 | 131 | True | 184.57 | 52.98 | 51.04 |
| LogPQM_ms500 | 132 | True | 181.45 | 51.91 | 49.84 |
| LogPQM_ms500 | 133 | True | 178.32 | 50.70 | 48.52 |
| LogPQM_ms500 | 134 | True | 175.20 | 49.32 | 47.05 |
| LogPQM_ms500 | 135 | True | 171.68 | 48.78 | 45.57 |
| LogPQM_ms500 | 136 | True | 168.55 | 47.36 | 43.74 |
| LogPQM_ms500 | 137 | True | 165.04 | 45.81 | 41.88 |
