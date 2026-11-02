# Day 18 — GaN Normally-off·Dynamic Ron·신뢰성

> **Week 4 / Day 18** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: Day 17
> **참조**: [`../04_Week4_GaN_HEMT_Process_Reliability_and_Characterization.md`](../04_Week4_GaN_HEMT_Process_Reliability_and_Characterization.md)

---

## 1. 오늘의 질문

> **"GaN HEMT를 normally-off로 만들고, dynamic Ron을 억제하려면 어떤 구조·passivation·field plate가 필요한가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | p-GaN/recess/cascode, $V_{th}$ |
| 10:45–12:15 | Lecture 2 | Dynamic Ron/current collapse, 신뢰성 |
| 13:15–15:15 | Lab | Trap/dynamic Ron 시뮬 |
| 15:30–16:30 | Case | 650 V p-GaN HEMT |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제 |

---

## 3. 학습목표

1. p-GaN gate, recess gate, cascode의 원리를 비교한다.
2. Normally-off $V_{th}$ 설계 변수를 설명한다.
3. Dynamic Ron/current collapse의 trap 기원을 설명한다.
4. Passivation/field plate가 trapping에 미치는 영향을 정량화한다.
5. HTRB/HTGB/H3TRB/스위칭 시험을 제품 spec에 매핑한다.
6. 650 V normally-off HEMT를 설계한다.

---

## 4. Lecture 1 — Normally-off (1.5h)

### 4.1 p-GaN gate

- p-GaN (Mg)를 gate 아래에 형성.
- Gate metal/p-GaN junction이 depletion을 유도.
- $V_{th}$: p-GaN doping/thickness, gate metal work function.
- 장점: normally-off, relatively mature.
- 단점: Mg activation, gate reliability, hysteresis.

### 4.2 Recess gate

- AlGaN barrier를 etch해 2DEG를 국부적으로 차단.
- $V_{th}$: recess depth, dielectric.
- 장점: low hysteresis.
- 단점: etch damage, uniformity.

### 4.3 Cascode

- Low-voltage Si MOSFET + depletion-mode GaN HEMT.
- 장점: normally-off, robust gate.
- 단점: parasitic inductance, oscillation, size.

### 4.4 비교

| 방식 | $V_{th}$ | Gate reliability | 공정 난이도 | 비고 |
|---|---:|---|---|---|
| p-GaN | 1–2 V | 중 | 중 | 상용 |
| Recess | 1–3 V | 중 | 높음 | 연구/일부 |
| Cascode | 2–4 V | 높음(Si gate) | 낮음 | 시스템 복잡 |

---

## 5. Lecture 2 — Dynamic Ron과 신뢰성 (1.5h)

### 5.1 Current collapse

- Off-state stress 후 $R_{on}$ 증가.
- 원인: surface/buffer/dielectric traps에 전자 포획.
- 회복: 시간/온도/광에 의존.

### 5.2 Trap 위치

| 위치 | 원인 | 영향 |
|---|---|---|
| Surface | passivation/GaN interface | virtual gate |
| Barrier | AlGaN defects | $V_{th}$ shift |
| Buffer | C/Fe doping, dislocations | vertical leakage/dynamic Ron |
| Dielectric | SiN/Al₂O₃ traps | gate lag |

### 5.3 Field plate/passivation

- Field plate: gate edge field↓ → trapping↓.
- SiN passivation: surface trap↓.
- Al₂O₃: gate dielectric, but hygroscopic.
- Trade-off: $C_{gd}$↑, process complexity.

### 5.4 신뢰성 시험

| 시험 | 목적 | 주요 failure |
|---|---|---|
| HTRB | off-state high temp | leakage, dynamic Ron |
| HTGB | gate bias high temp | $V_{th}$ shift |
| H3TRB | humidity | corrosion/leakage |
| HTOL | operating life | wear-out |
| Switching | dynamic Ron | current collapse |
| AEC-Q101 | qualification | all |

### 5.5 수명 모델

- Arrhenius (temperature)
- Power-law (voltage)
- Weibull (failure distribution)

---

## 6. Lab — Trap/Dynamic Ron (2h)

### 6.1 목표

- Trap 농도/에너지에 따른 dynamic Ron 변화.
- Field plate/passivation 효과.
- 회복 시간 상수 추출.

### 6.2 파라미터

| 항목 | 값 |
|---|---|
| trap density | $10^{17}$–$10^{19}$ cm⁻³ |
| trap energy | 0.3–0.8 eV |
| capture cross section | $10^{-15}$–$10^{-13}$ cm² |
| stress | $V_{DS}=400$ V, 10 s |
| temperature | 25/75/125°C |

### 6.3 절차

1. Baseline $R_{on}$.
2. Off-state stress.
3. $R_{on}$ recovery vs time.
4. Fit exponential time constants.
5. Trap parameter sweep.

### 6.4 제출물

- $R_{on}$ recovery 그래프.
- Time constant 표.
- "field plate 효과" 1페이지.
- 설계 개선안.

### 6.5 체크포인트

- [ ] Stress 조건.
- [ ] $R_{on}$ normalization.
- [ ] Time constant.
- [ ] Temperature dependence.

---

## 7. Case — 650 V p-GaN HEMT (1h)

### 7.1 요구

- $V_{th}>1.5$ V.
- $R_{on}<100$ mΩ.
- Dynamic Ron<1.3.
- BV>650 V.
- AEC-Q101.

### 7.2 설계

| 항목 | 선택 |
|---|---|
| Gate | p-GaN |
| Passivation | SiN + Al₂O₃ |
| Field plate | source-connected |
| Buffer | C-doped |
| Substrate | GaN-on-Si |

### 7.3 리스크

- p-GaN gate reliability.
- Dynamic Ron.
- Thermal.

### 토론

1. p-GaN gate에서 $V_{th}$를 높이는 방법?
2. Cascode가 gate reliability에 유리한 이유?
3. Dynamic Ron을 줄이는 passivation 전략?

---

## 8. Quiz (5문항)

1. p-GaN gate의 원리는?
2. Cascode의 장단점?
3. Current collapse의 원인?
4. Field plate의 trade-off?
5. HTRB와 HTGB의 차이는?

**정답**: 1) p-GaN depletion. 2) robust gate/parasitic. 3) trap trapping. 4) BV↑/$C_{gd}$↑. 5) off-state vs gate bias.

---

## 9. Homework

- 650 V p-GaN HEMT의 $V_{th}$ 설계 표 작성.
- Dynamic Ron 측정 sequence 설계.
- Day 19를 위해 **TCAD calibration/DOE** 예습.

---

## 10. 강사 노트

- "Normally-off는 구조 문제, dynamic Ron은 trap 문제"로 구분.
- p-GaN gate는 **Mg activation**이 핵심.
- Field plate는 **만능이 아님**.
- 다음 날 TCAD/산업 실무로.

---

*Day 18 / 20 — 140h program. Trap/시험 조건은 교육용 대표값입니다.*
