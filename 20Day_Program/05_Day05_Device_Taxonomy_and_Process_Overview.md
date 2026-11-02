# Day 05 — 소자 Taxonomy와 공정 개요

> **Week 1 / Day 5** | **7시간** | 강의 4h + 실습 2h + 주차 시험 1h
> **선수**: Day 1–4
> **참조**: [`../01_Week1_Overview_and_WBG_Process_Fundamentals.md`](../01_Week1_Overview_and_WBG_Process_Fundamentals.md)

---

## 1. 오늘의 질문

> **"같은 1.2 kV라도 SBD/JBS/MOSFET/trench/SJ 중 무엇을 선택하며, 그 구조를 어떤 공정 순서로 만드는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | 소자 구조·I-V·trade-off |
| 10:45–12:15 | Lecture 2 | SiC/GaN 공정 flow, thermal budget |
| 13:15–15:15 | Lab | 1D TCAD sanity check |
| 15:30–16:30 | Week 1 Exam | 10문항, 30분 + 리뷰 |
| 16:30–17:30 | Review | 시험 해설, Week2 예고 |

---

## 3. 학습목표

1. SBD/JBS/MPS/PiN/MOSFET/Trench/SJ/HEMT/Cascode/IGBT를 비교한다.
2. Unipolar vs bipolar의 trade-off를 설명한다.
3. SiC DMOS/trench와 GaN HEMT의 공정 flow를 그린다.
4. Thermal budget 충돌 지점을 식별한다.
5. Mask 수·핵심 module·검증 포인트를 정리한다.
6. 1D TCAD로 $BV/R_{on}/V_{th}$ sanity check를 수행한다.

---

## 4. Lecture 1 — 소자 Taxonomy (1.5h)

### 4.1 Unipolar vs Bipolar

| 구분 | Unipolar | Bipolar |
|---|---|---|
| 전류 | 다수 캐리어 | 전자+정공 |
| 예 | SBD, MOSFET, HEMT | PiN, IGBT, thyristor |
| 장점 | 빠른 switching, 낮은 $Q_{rr}$ | conductivity modulation, high current |
| 단점 | $R_{on}$ vs BV trade-off | tail current, 느린 switching |

### 4.2 Diode 계열

| 소자 | 구조 | 장점 | 단점 | 응용 |
|---|---|---|---|---|
| SBD | metal/n-drift | no $Q_{rr}$, low $V_F$ | leakage, barrier lowering | PFC, freewheel |
| JBS | Schottky + p+ grid | low leakage, low $V_F$ | 공정 복잡 | 1.2 kV |
| MPS | Schottky + p+ merged | trade-off 제어 | 설계 복잡 | 고전력 |
| PiN | p+/n-/n+ | high BV, surge | $Q_{rr}$ | grid, rail |

### 4.3 MOSFET 계열

| 구조 | 채널 | 장점 | 단점 |
|---|---|---|---|
| Planar DMOS | 수평 | 공정 단순, robust | $R_{on,sp}$ 큼 |
| Trench | 수직 | 낮은 $R_{on,sp}$ | corner field, oxide 신뢰 |
| Double trench | 수직 + source trench | low $R_{on}$, shield | 공정 복잡 |
| SJ | charge balance | low $R_{on}$ at high BV | 공정 정밀도 |

### 4.4 GaN 계열

| 구조 | Normally | 특징 |
|---|---|---|
| HEMT | on (depletion) | 2DEG, high mobility |
| p-GaN gate | off | threshold control |
| recessed gate | off | $V_{th}$ 조절 |
| cascode | off | Si MOSFET + GaN HEMT |

### 4.5 IGBT

- BJT + MOSFET hybrid: high BV, conductivity modulation.
- 장점: 1.2–6.5 kV, high current, robust.
- 단점: tail current, switching loss.
- SiC IGBT는 10 kV+ 연구/일부 상용.

---

## 5. Lecture 2 — 공정 Flow와 Thermal Budget (1.5h)

### 5.1 SiC Planar DMOS flow

| # | Module | 핵심 | 온도 |
|---:|---|---|---|
| 1 | Substrate/epi | n+ 4H-SiC, n- drift | epi 1500–1600°C |
| 2 | JTE implant | Al, edge termination | RT–500°C |
| 3 | p-well implant | Al, box profile | RT–500°C |
| 4 | n+ source implant | N | RT–500°C |
| 5 | p+ contact implant | Al | RT–500°C |
| 6 | Activation | 1600–1700°C, cap | high |
| 7 | Sacrificial oxidation | defect removal | 1100–1300°C |
| 8 | Gate oxidation | dry O₂ + NO/N₂O | 1100–1300°C |
| 9 | Poly gate | depo/dope/pattern | 600–900°C |
| 10 | ILD | SiO₂ | 400–800°C |
| 11 | Ohmic contact | Ni anneal | 900–1000°C |
| 12 | Metal/passivation | Ti/Au, polyimide | <400°C |
| 13 | Backside drain | grind/metal | <400°C |

### 5.2 SiC Trench MOSFET flow

- Trench etch (ICP), depth 1–2 µm.
- Sacrificial oxidation + corner rounding.
- Gate oxide (thicker bottom ideal).
- Poly recess.
- Shield/implant optional.
- Challenges: bottom corner field, channel mobility, oxide reliability.

### 5.3 GaN HEMT flow

| # | Module | 핵심 | 온도 |
|---:|---|---|---|
| 1 | Substrate | Si/SiC/sapphire | — |
| 2 | Buffer | AlN/AlGaN superlattice | MOCVD 1000–1100°C |
| 3 | Channel | GaN | MOCVD |
| 4 | Barrier | AlGaN | MOCVD |
| 5 | Isolation | implant/mesa | RT |
| 6 | Ohmic | Ti/Al/Ni/Au anneal | 800–900°C |
| 7 | Dielectric | SiN/SiO₂/Al₂O₃ | 300–600°C |
| 8 | Gate | metal | <400°C |
| 9 | Passivation | SiN | 300–400°C |
| 10 | Field plate/interconnect | Cu/Au | <400°C |

### 5.4 Thermal budget 충돌

| 충돌 | 이유 | 해결 |
|---|---|---|
| SiC activation vs gate oxide | 1600°C가 oxide를 손상 | activation 먼저, gate oxide 나중 |
| Ohmic anneal vs gate | 900–1000°C가 gate 저하 | gate 전 ohmic 또는 저온 ohmic |
| GaN epi vs gate | epi 1000°C | gate는 epi 후 저온 |
| Passivation vs hydrogen | H가 trap passivation/activation | annealing 순서 제어 |

> **원칙**: "고온 공정을 앞으로, 저온·계면 공정을 뒤로." 단, 계면 품질을 위해 sacrificial oxidation을 추가.

---

## 6. Lab — 1D TCAD Sanity Check (2h)

### 6.1 목표

- 1.2 kV SiC DMOS의 1D slice로 $BV$, $R_{on,sp}$, $V_{th}$를 계산.
- 손계산과 TCAD를 비교.
- Week 1 내용을 하나의 구조로 통합.

### 6.2 절차

1. drift: $N_D=5\times10^{15}$ cm⁻³, $t=10$ µm.
2. p-well: $N_A=10^{17}$ cm⁻³, depth 0.5 µm.
3. gate oxide: 50 nm.
4. $V_{th}$: flatband + 2$\phi_F$ + oxide drop.
5. $R_{on,sp}$: drift + channel + substrate.
6. $BV$: reverse sweep, $E_c$ check.

### 6.3 제출물

- 1D 구조 그림.
- $V_{th}$, $R_{on,sp}$, $BV$ 표.
- TCAD vs 손계산 오차와 원인.

### 6.4 체크포인트

- [ ] $V_{th}$에 oxide thickness 반영.
- [ ] $R_{on}$에 channel mobility 반영.
- [ ] $BV$에 termination 미포함(1D 한계).
- [ ] 단위 일관성.

---

## 7. Week 1 Exam (30분, 10문항)

1. 400 V/30 A, $D=0.5$, $R_{on}=30$ mΩ의 도통 손실?
2. $V=400$ V, $I=30$ A, $t_{sw}=40$ ns, $f=20$ kHz의 스위칭 손실?
3. $V_{bi}$ 공식?
4. 일방 abrupt junction의 $W$ 공식?
5. Avalanche BV의 온도 계수?
6. Baliga FOM 공식?
7. 4H-SiC의 이방성이란?
8. Planar DMOS vs Trench의 장단점?
9. SiC 공정에서 activation을 gate oxide 전에 하는 이유?
10. GaN HEMT가 normally-on인 이유?

**정답 요약**: 1) 13.5 W. 2) 4.8 W. 3) $(kT/q)\ln(N_AN_D/n_i^2)$. 4) $\sqrt{2\varepsilon(V_{bi}-V)(1/N_A+1/N_D)/q}$. 5) positive. 6) $\varepsilon\mu E_c^3$. 7) 결정축별 물성 차이. 8) planar 단순/robust, trench low $R_{on}$/corner field. 9) 1600–1700°C가 gate를 손상. 10) polarization 2DEG가 zero bias에서 존재.

---

## 8. Homework

- SiC DMOS와 trench의 mask 수를 비교.
- GaN HEMT flow에서 "저온 공정"만 모아 timeline 작성.
- Week 2를 위해 **세정·산화** 예습.

---

## 9. 강사 노트

- Week1 시험은 **계산 5문항 + 개념 5문항**으로 균형.
- TCAD 1D와 손계산 차이를 **오차 원인**으로 토론.
- "소자 선택은 FOM + 신뢰성 + 공정 난이도 + 가격"임을 강조.
- Week2 시작 전 **세정/산화** 안전 교육 10분.

---

*Day 05 / 20 — 140h program. 공정 온도는 교육용 대표값입니다.*
