# eLSM-SU3: эталонный решатель для SU(3) расширенной линейной сигма-модели

Прозрачная численная реализация SU(3)-сектора работы

> Friesen et al., *Temperature Dependence of the Masses of Various Meson States: A
> Comparative Study in SU(3) and SU(4) extended Linear-Sigma Model*,
> [arXiv:2512.22198v2](https://arxiv.org/abs/2512.22198); Phys. Rev. C 114, 015207 (2026),
> [DOI 10.1103/ms99-5qnq](https://journals.aps.org/prc/abstract/10.1103/ms99-5qnq).

Цель — проверяемая базовая реализация: каждое соглашение, которое в статье допускает
неоднозначное толкование, явно зафиксировано в коде и задокументировано, а не угадано молча.

## Статус

| Цель | Результат |
|---|---|
| Вакуумный спектр из табл. I / V (π, η, η′, σ, a₀, K₀\*, f₀, ρ, a₁) | воспроизведён с точностью < 0.5 % |
| Киральная температура T_χ из табл. III (4 варианта) | в пределах одного шага сетки 2.5 МэВ (3 из 4), точно (1 из 4) |
| Масса каона, табл. I: 420.49 МэВ | **не воспроизводится**: модель даёт ≈ 495 МэВ (эксперимент: 495.6) |
| f_π, f_K из табл. I | не согласуются с массами из табл. I (модель: 89.7 / 105.1 МэВ) |
| Массы мезонов в среде по ур. (32) (рис. 2–4) | есть только общая процедура, к отдельным каналам пока не подключена |

Полные численные результаты: [`results/PAPER_REPRODUCTION.md`](results/PAPER_REPRODUCTION.md).

### Ключевые соглашения (см. [`docs/CHANGELOG.md`](docs/CHANGELOG.md))

- **Вакуум:** `sigma_x = 2 m_q / g = 0.10909 ГэВ` (m_q = 0.3 ГэВ, g = 5.5), как в разделе III
  статьи. `sigma_y` подгоняется под спектр табл. I (`fit_condensates_yukawa`). Обращение
  PCAC-соотношения ур. (37) (`fit_condensates_pcac`, sigma_x = 0.1263 ГэВ) оставлено только
  для диагностики: оно даёт m_π ≈ 436 МэВ.
- **Опечатки в табл. A1 исправлены** по гессиану лагранжиана:
  в `eta_NS` используется `c/sqrt(2) sigma_x`; аномальный член для K₀\* равен `+c/2 sigma_x`.
- **Тепловой кварковый вклад:** `-2T ∫ (log g⁺ + log g⁻)`. След по цвету уже содержится
  в логарифме с петлёй Полякова, поэтому множитель `2 nu_c` в ур. (27) считается опечаткой.
  Это согласуется с ур. (32) при `nu_c = 6` (покрыто тестами).
- **Море Дирака:** не учитывается (no-sea, как в цитируемой работе Schaefer & Wagner,
  arXiv:0808.1491). Для анализа чувствительности доступны два варианта с жёстким обрезанием.
- **Петля Полякова:** логарифмический потенциал Roessner–Ratti–Weise
  (a₀=3.51, a₁=−2.47, a₂=15.2, b₃=−1.75), T₀ = 0.27 ГэВ. При μ = 0 накладывается `phi = phi_bar`.

## Структура репозитория

```
src/elsm/          пакет
  parameters.py      SU3Parameters (h_x, h_y из ур. 15-16), PolyakovParameters
  potential.py       ур. (14) + градиент/гессиан, потенциал Полякова, кварковые члены, grand_potential
  meson_masses.py    древесные массы из табл. A1, Z-факторы (ур. A13), смешивание изоскаляров
  thermal_masses.py  тепловая поправка к массовой матрице по ур. (32), числа заполнения (ур. 33-36)
  vacuum_fit.py      калибровка вакуума: fit_condensates_yukawa (основная), fit_condensates_pcac
  solver.py          минимизация с несколькими стартовыми точками и продолжение по температуре
tests/             модульные тесты и тесты воспроизведения статьи
scripts/           reproduce_paper_tables.py, run_temperature_scan.py
verification/      независимые проверки формул табл. A1 и вакуума, использованного авторами
configs/su3.yaml   наборы параметров из табл. II (справочно; код его не читает)
data/              ручная оцифровка ветви sigma_x с рис. 1
results/           сгенерированные результаты
docs/              журнал изменений, внешняя перекрёстная проверка, полная история аудита
```

## Установка

```bash
python -m pip install -e ".[dev,plot]"
pytest
```

Требуется Python ≥ 3.10, numpy, scipy (matplotlib нужен только для `run_temperature_scan.py`).

## Использование

```python
from elsm import (SU3Parameters, PolyakovParameters, grand_potential,
                  fit_condensates_yukawa)

row = dict(m0_sq=-0.066564, lambda1=3.15, lambda2=36.253, c_anomaly=2.828)  # табл. II, c != 0
vac = fit_condensates_yukawa(**row)          # sigma_x = 0.10909, sigma_y = 0.10361 ГэВ
p = SU3Parameters.from_vacuum_condensates(
    **row, sigma_x=vac.sigma_x, sigma_y=vac.sigma_y, g_yukawa=5.5, Tc=0.270, include_FT=False)

omega = grand_potential(
    T=0.15, sigma_x=vac.sigma_x, sigma_y=vac.sigma_y, phi=0.2, phi_bar=0.2, p=p,
    polyakov_parameters=PolyakovParameters(T0=0.270), vacuum_prescription="omit")
```

`include_FT=True` включает замену из ур. (43): `m0² → m0² (1 − T²/Tc²)`.

### Воспроизведение статьи

```bash
python scripts/reproduce_paper_tables.py          # табл. I, V, III (~минуты); --fast для сетки 5 МэВ
python scripts/run_temperature_scan.py            # исследовательский скан конденсатов + график
python verification/derive.py                     # табл. A1 против гессиана лагранжиана
python verification/reconstruct.py                # какой вакуум использовали авторы
```

Результаты записываются в `results/`.

## Известные ограничения

- **Привязка ур. (32) к каналам.** Производные по каналам `d(m_f²)/dφ`, `d²(m_f²)/dφdφ`
  статья отсылает к своим ссылкам [59, 66]. Реализованы только направления
  `(sigma_x, sigma_y)` (`sigma_field_mass_derivatives`); полный вывод через юкавскую матрицу
  для каждого мезонного канала ещё предстоит сделать.
- **`run_temperature_scan.py`** минимизирует по `phi` и `phi_bar` независимо
  (`solve_mean_fields`). При μ = 0 это может выбрать асимметричную ветвь петли Полякова
  (см. `docs/AUDIT_HISTORY.md`), поэтому кривая отличается от сканов с `phi = phi_bar`,
  использованных для табл. III. Результат следует считать исследовательским.
- **Рис. 1.** Ручная оцифровка в `data/` (полувысота ≈ 0.201 ГэВ) совпадает с ветвью
  c = 0 + F(T), а не c ≠ 0 без F(T); подпись и легенда рисунка, по-видимому, противоречат друг
  другу. RMSE относительно растрового рисунка не заявляется.

## Дополнительная документация

- [`docs/CHANGELOG.md`](docs/CHANGELOG.md): исправление вакуума от 2026-10-18 и его обоснование.
- [`docs/EXTERNAL_CROSSCHECK.md`](docs/EXTERNAL_CROSSCHECK.md): сравнение с журнальной
  версией и с Tawfik et al., arXiv:2502.01644 (вероятные опечатки в уравнениях стационарности
  и Z-факторах этой работы).
- [`docs/AUDIT_HISTORY.md`](docs/AUDIT_HISTORY.md): полный хронологический журнал аудита
  (в основном устарел; сохранён для прослеживаемости).

## Литература

- Friesen et al., arXiv:2512.22198v2; Phys. Rev. C 114, 015207 (2026).
- Roessner, Ratti & Weise, Phys. Rev. D 75, 034007 (2007), DOI 10.1103/PhysRevD.75.034007.
- Schaefer & Wagner, Phys. Rev. D 79, 014018 (2009), arXiv:0808.1491.
- Parganlija, Giacosa & Rischke, Phys. Rev. D 82, 054024 (2010), DOI 10.1103/PhysRevD.82.054024.
- Tawfik, Diab & Hussein, Chin. Phys. C 43, 034103 (2019), DOI 10.1088/1674-1137/43/3/034103.
