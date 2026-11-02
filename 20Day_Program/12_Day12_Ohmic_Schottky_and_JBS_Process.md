# Day 12 — Ohmic/Schottky 접촉과 JBS 공정

> **Week 3 / Day 12** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: Day 11
> **참조**: [`../03_Week3_SiC_Contacts_Integration_and_Termination.md`](../03_Week3_SiC_Contacts_Integration_and_Termination.md)

---

## 1. 오늘의 질문

> **"1.2 kV SiC 다이오드에서 Schottky의 낮은 $V_F$와 PiN의 낮은 leakage를 어떻게 JBS/MPS로 결합하는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | Schottky barrier, barrier lowering, JBS/MPS |
| 10:45–12:15 | Lecture 2 | Ni/Ti/Al 공정, p+ grid, annealing, edge |
| 13:15–15:15 | Lab | JBS TCAD: leakage/BV/$V_F$ |
| 15:30–16:30 | Case | 1.2 kV JBS 설계 리뷰 |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제 |

---

## 3. 학습목표

1. Schottky 전류-전압과 barrier lowering을 설명한다.
2. JBS와 MPS의 구조·동작·trade-off를 비교한다.
3. Ni, Ti, Pt Schottky의 장벽과 leakage를 비교한다.
4. p+ grid 설계가 전계·leakage·$V_F$에 미치는 영향을 설명한다.
5. Ohmic와 Schottky를 같은 공정 흐름에 통합한다.
6. TCAD로 JBS의 BV/leakage/$V_F$를 평가한다.

---

## 4. Lecture 1 — Schottky와 JBS (1.5h)

### 4.1 Schottky 전류

$$
J = A^*T^2\exp\left(-\frac{q\phi_B}{kT}\right)\left[\exp\left(\frac{qV}{nkT}\right)-1\right]
$$

- $A^*$: Richardson constant.
- $n$: ideality factor.
- Reverse current는 barrier에 지수적으로 의존.

### 4.2 Barrier lowering

$$
\Delta\phi = \sqrt{\frac{q\mathcal{E}}{4\pi\varepsilon_s}}
$$

- Reverse bias↑ → image force lowering↑ → leakage↑.
- Edge field crowding → premature leakage.

### 4.3 JBS 구조

- Schottky anode + p+ grid.
- p+ grid가 Schottky 표면 전계를 낮춤.
- Forward: Schottky가 먼저 turn-on → low $V_F$.
- Reverse: p+/n junction이 공핍 → Schottky 전계 완화 → low leakage.
- trade-off: p+ grid 면적↑ → $V_F$↑.

### 4.4 MPS

- Merged PiN-Schottky: Schottky + p+ island.
- 장점: surge current, low leakage.
- 단점: $Q_{rr}$/switching trade-off.

### 4.5 비교

| 소자 | $V_F$ | Leakage | $Q_{rr}$ | BV | 응용 |
|---|---|---|---|---|---|
| SBD | 낮음 | 높음 | ~0 | 중 | PFC |
| JBS | 중 | 낮음 | 작음 | 높음 | 1.2 kV |
| PiN | 높음 | 매우 낮음 | 큼 | 높음 | grid |

---

## 5. Lecture 2 — 공정 통합 (1.5h)

### 5.1 Schottky metal

| Metal | $\phi_B$ (n-SiC, eV) | 특징 |
|---|---:|---|
| Ti | ~0.6 | low barrier, high leakage |
| Ni | ~1.0 | balanced |
| Pt | ~1.3 | low leakage, high $V_F$ |

> Barrier는 공정/계면에 따라 달라짐. 교육용 대표값.

### 5.2 p+ grid 공정

1. p+ implant (Al), box profile.
2. Activation 1600–1700°C.
3. Sacrificial oxidation/clean.
4. Schottky metal depo/pattern.
5. Ohmic metal (Ni) for p+ contact.
6. Anneal (ohmic 먼저 or after).
7. Edge termination (JTE).
8. Passivation.

### 5.3 Thermal budget 순서

- p+ activation (high) → ohmic anneal (900–1000°C) → Schottky (low) → passivation.
- Schottky metal은 고온 anneal 후에 증착해 barrier 변화를 최소화.

### 5.4 Edge termination

- JBS edge는 JTE/guard ring 필요.
- p+ grid edge와 termination overlap 설계.
- Surface charge에 민감.

### 5.5 신뢰성

- Reverse leakage drift.
- Barrier height instability.
- Passivation interface.
- HTRB.

---

## 6. Lab — JBS TCAD (2h)

### 6.1 목표

- Schottky vs JBS의 reverse leakage/BV 비교.
- p+ grid pitch/width sweep.
- $V_F$–leakage trade-off curve.

### 6.2 구조

| 파라미터 | 값 |
|---|---|
| drift $N_D$ | $5\times10^{15}$ cm⁻³ |
| drift thickness | 10 µm |
| p+ grid width | 1–3 µm |
| pitch | 3–10 µm |
| Schottky barrier | 1.0 eV |
| temperature | 300 K, 400 K |

### 6.3 절차

1. Schottky only: $I$–$V$ forward/reverse.
2. JBS: p+ grid 추가.
3. pitch/width sweep.
4. $V_F$ @100 A/cm², leakage @1200 V 추출.
5. BV 추출.

### 6.4 제출물

- $I$–$V$ 비교 그래프.
- $V_F$/leakage/BV 표.
- "최적 pitch" 제안 + 근거.

### 6.5 체크포인트

- [ ] Barrier lowering 반영.
- [ ] p+ grid doping/activation.
- [ ] Leakage at 1200 V.
- [ ] $V_F$ at current density.

---

## 7. Case — 1.2 kV JBS 설계 (1h)

### 7.1 요구

- $V_F < 1.5$ V @100 A/cm².
- Leakage < 1 µA/cm² @1200 V.
- BV > 1400 V.
- $Q_{rr}$ 최소.

### 7.2 설계 선택

| 항목 | 선택 | 이유 |
|---|---|---|
| Schottky | Ni | barrier/leakage balance |
| p+ grid | 2 µm / 6 µm pitch | field shielding |
| termination | JTE | dose window |
| passivation | polyimide/SiN | surface stability |

### 7.3 리스크

- p+ grid alignment.
- JTE charge.
- Schottky barrier instability.

### 토론

1. p+ grid를 조밀하게 하면 $V_F$가 왜 증가하는가?
2. Ti Schottky를 쓰면 leakage가 커지는 이유는?
3. JBS가 PiN보다 switching에 유리한 이유는?

---

## 8. Quiz (5문항)

1. Schottky reverse current의 지배 인자는?
2. Image force barrier lowering 공식은?
3. JBS의 p+ grid 역할 2가지는?
4. Ni/Ti/Pt 중 leakage가 가장 낮은 것은?
5. Schottky metal을 ohmic anneal 후에 증착하는 이유는?

**정답**: 1) $\phi_B$, $T$, $\mathcal{E}$. 2) $\sqrt{q\mathcal{E}/(4\pi\varepsilon_s)}$. 3) 전계 완화, leakage 감소. 4) Pt. 5) barrier 변화/열화 방지.

---

## 9. Homework

- JBS pitch 4/6/8 µm에서 $V_F$/leakage를 비교.
- 1.2 kV JBS의 termination을 JTE로 설계.
- Day 13을 위해 **DMOS/trench** 예습.

---

## 10. 강사 노트

- JBS는 **"Schottky + p-n junction의 하이브리드"** 로 설명.
- $V_F$-leakage trade-off를 그래프로.
- Schottky metal은 **thermal budget 후** 원칙.
- 다음 날 MOSFET integration으로 확장.

---

*Day 12 / 20 — 140h program. Barrier/공정값은 교육용 대표값입니다.*
