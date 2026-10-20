# Paper reproduction (SU(3))

Vacuum: sigma_x = 2 m_q/g = 0.109091 GeV (m_q=0.3, g=5.5); sigma_y fitted to Table I (pi, eta, eta', sigma). h_x, h_y from Eqs. (15)-(16). Thermal: -2T traced log, no-sea, phi=phi_bar.

## Tables I / V (MeV)

| row | sigma_y | pi | K | eta | eta' | sigma | a0 | K0* | f0 | rho | a1 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| c_finite | 0.10361 | 140.1 | 494.9 | 534.0 | 966.8 | 700.6 | 926.9 | 1034.3 | 1126.4 | 770.2 | 1070.0 |
| c_zero | 0.10357 | 139.9 | 494.5 | 139.9 | 644.6 | 600.5 | 821.4 | 1034.2 | 1144.3 | 770.2 | 1070.0 |
| paper c!=0 | | 139.85 | 420.49 | 531.9 | 965.44 | 700 | 926 | 1032 | 1124 | 770.11 | 1069.76 |
| paper c=0 | | 139.85 | 420.49 | 140.0 | 644.93 | 600 | | | | 770.11 | 1069.76 |

Kaon: the consistent model gives ~495 MeV (experiment 495.6); Table I's 420.49 is not reproduced by any consistent convention (Z_K=1 -> 407, Z_K0* used for K -> 410).

## Table III: T_chi = argmax |d sigma_x/dT| (GeV)

| row | F(T) | model | paper | T_d (Phi) |
|---|---|---|---|---|
| c_zero | False | 0.2150 | 0.2175 | 0.2075 |
| c_finite | False | 0.2300 | 0.2325 | 0.2075 |
| c_zero | True | 0.2050 | 0.2075 | 0.2025 |
| c_finite | True | 0.2175 | 0.2175 | 0.2050 |

Temperature grid step 2.5 MeV; paper values lie on a 2.5 MeV grid.
