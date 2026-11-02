# Day 08 — Gate Oxide·계면·Nitridation

> **Week 2 / Day 8** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: Day 7
> **참조**: [`../02_Week2_SiC_Frontend_Processes.md`](../02_Week2_SiC_Frontend_Processes.md)

---

## 1. 오늘의 질문

> **"SiC MOS 계면의 Dit를 어떻게 측정·감소시키며, nitridation은 왜 mobility와 reliability의 trade-off를 만드는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | MOS C-V, Dit/NIT, 추출법 |
| 10:45–12:15 | Lecture 2 | NO/N₂O nitridation, TEOS/ALD, BTI |
| 13:15–15:15 | Lab | C-V stretch-out로 Dit 추출 |
| 15:30–16:30 | Case | $V_{th}$ instability와 hysteresis |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제 |

---

## 3. 학습목표

1. MOS C-V의 accumulation/depletion/inversion을 설명한다.
2. $C_{ox}$, $V_{FB}$, $V_{th}$를 계산한다.
3. Dit와 NIT를 구분하고 C-V stretch-out으로 추출한다.
4. NO/N₂O nitridation의 화학·전기적 효과를 설명한다.
5. Mobility vs Dit vs reliability trade-off를 논의한다.
6. BTI/hysteresis의 원인을 계면·oxide trap으로 설명한다.

---

## 4. Lecture 1 — MOS C-V와 계면 (1.5h)

### 4.1 MOS capacitor

$$
C_{ox} = \frac{\varepsilon_{ox}}{t_{ox}}
$$

$V_{FB}$:

$$
V_{FB} = \Phi_{MS} - \frac{Q_f}{C_{ox}} - \frac{1}{C_{ox}}\int_0^{t_{ox}}\frac{x}{t_{ox}}\rho(x)dx
$$

$V_{th}$ (p-type):

$$
V_{th} = V_{FB} + 2\phi_F + \frac{\sqrt{2\varepsilon_s qN_A(2\phi_F)}}{C_{ox}}
$$

### 4.2 C-V 영역

| 영역 | $V_G$ | capacitance |
|---|---|---|
| Accumulation | $V_G < V_{FB}$ (p형) | $C_{ox}$ |
| Depletion | $V_{FB}<V_G<V_{th}$ | $C_{ox}$ 직렬 $C_s$ |
| Inversion | $V_G>V_{th}$ | $C_{ox}$ (LF) / $C_{ox}$+$C_{dep}$ (HF) |

### 4.3 Dit vs NIT

| 구분 | 위치 | 특성 |
|---|---|---|
| Dit | SiC/SiO₂ 계면 | 에너지 분포, 빠른 응답 |
| NIT | oxide 내 | 느린 trapping, BTI |
| $Q_f$ | oxide fixed charge | $V_{FB}$ shift |

### 4.4 Dit 추출

**High-low frequency:**

$$
D_{it} = \frac{C_{ox}}{q^2}\left(\frac{C_{lf}}{C_{ox}-C_{lf}} - \frac{C_{hf}}{C_{ox}-C_{hf}}\right)
$$

**Terman:**

$$
D_{it} = \frac{C_{ox}}{q^2}\left|\frac{dV_G}{d\phi_s}\right| - \frac{C_s}{q^2}
$$

**Conductance method:** 주파수별 $G_p/\omega$ peak → $D_{it}$, time constant.

> SiC는 Dit가 $10^{12}$–$10^{13}$ cm⁻²eV⁻¹로 Si보다 1–2자리 높습니다.

---

## 5. Lecture 2 — Nitridation과 신뢰성 (1.5h)

### 5.1 NO/N₂O anneal

- 목적: C dangling bond passivation, Dit 감소.
- 온도: 1100–1300°C.
- N이 계면에 축적 → Si-O-N / Si-N 결합.
- 효과: Dit↓, $V_{th}$ shift 감소, mobility 개선(일부).
- 부작용: N-rich oxide는 BTI/leakage에 영향, mobility 제한.

### 5.2 TEOS/ALD

| 공정 | 장점 | 단점 |
|---|---|---|
| Thermal oxide | 계면 품질 | 고온, 느림 |
| TEOS LPCVD | uniformity, thick | as-deposited quality |
| ALD Al₂O₃ | high-k, low Dit? | hygroscopic, BTI |
| ALD SiO₂ | low temp | density/leakage |

### 5.3 Post-deposition anneal

- N₂/NO/N₂O, 900–1100°C.
- densification, trap reduction.
- 과도한 anneal → N penetration, mobility 저하.

### 5.4 BTI/hysteresis

| 현상 | 원인 | 측정 |
|---|---|---|
| Hysteresis | 느린 trap, mobile ion | 양방향 C-V |
| PBTI | electron trapping | positive gate stress |
| NBTI | hole trapping/interface states | negative gate stress |
| $V_{th}$ drift | NIT + Dit | stress-recovery |

> SiC MOSFET은 **PBTI**가 상대적으로 중요합니다. Gate oxide field와 온도가 수명을 결정합니다.

---

## 6. Lab — C-V Dit 추출 (2h)

### 6.1 목표

- $\Delta V$ stretch-out로 Dit 추정.
- Nitridation 전/후 비교.
- $V_{FB}$, $C_{ox}$, $V_{th}$ 추출.

### 6.2 데이터(예)

| $V_G$ (V) | $C/C_{ox}$ (no nitridation) | $C/C_{ox}$ (NO) |
|---:|---:|---:|
| -4 | 1.00 | 1.00 |
| -2 | 0.98 | 0.99 |
| 0 | 0.85 | 0.90 |
| 2 | 0.60 | 0.70 |
| 4 | 0.45 | 0.55 |
| 6 | 0.50 | 0.60 |

### 6.3 절차

1. $C_{ox}$를 accumulation에서 추출.
2. $V_{FB}$를 flatband capacitance로 추출.
3. Stretch-out $\Delta V$ 측정.
4. $D_{it} \approx C_{ox}\Delta V/(q\Delta\phi_s)$로 근사.
5. Nitridation 전/후 비교.

### 6.4 제출물

- C-V 그래프.
- $C_{ox}$, $V_{FB}$, $V_{th}$, Dit 표.
- "NO anneal 효과와 trade-off" 1페이지.

### 6.5 체크포인트

- [ ] $C_{ox}$ 단위(F/cm²).
- [ ] $V_{FB}$ 추출.
- [ ] Stretch-out 방향.
- [ ] Dit 단위(cm⁻²eV⁻¹).

---

## 7. Case — $V_{th}$ Instability (1h)

### 7.1 증상

- $V_{th}$가 stress 후 +0.5 V shift.
- Hysteresis 0.3 V.
- Temperature dependence.

### 7.2 원인

1. Oxide trap (NIT).
2. Interface trap (Dit).
3. Mobile ion (Na).
4. Carbon residue.

### 7.3 대응

- NO anneal 최적화.
- Gate oxide densification.
- Clean 강화.
- BTI stress test로 검증.

### 토론

1. Dit가 높으면 mobility가 왜 낮아지는가?
2. Nitridation이 PBTI를 악화시키는 이유는?
3. Hysteresis와 BTI를 어떻게 구분하는가?

---

## 8. Quiz (5문항)

1. $C_{ox}$ 공식은?
2. Dit와 NIT의 차이는?
3. High-low frequency Dit 공식의 의미는?
4. NO anneal의 목적은?
5. PBTI와 NBTI의 차이는?

**정답**: 1) $\varepsilon_{ox}/t_{ox}$. 2) 계면 vs oxide 내 trap. 3) LF/HF C 차이로 계면 trap 밀도 추출. 4) C dangling bond passivation/Dit 감소. 5) positive vs negative gate stress.

---

## 9. Homework

- 주어진 C-V에서 $C_{ox}$, $V_{FB}$, Dit를 계산.
- NO anneal 온도/시간이 Dit와 mobility에 미치는 영향 조사.
- Day 9를 위해 **LSS/SRIM** 예습.

---

## 10. 강사 노트

- Dit는 **에너지 분포**임을 강조(단일 값 아님).
- Nitridation은 "많을수록 좋다"가 아님.
- C-V stretch-out과 $V_{th}$ shift를 구분.
- 다음 날 implant로 도핑을 어떻게 넣는지 연결.

---

*Day 08 / 20 — 140h program. C-V 데이터는 교육용 예시입니다.*
