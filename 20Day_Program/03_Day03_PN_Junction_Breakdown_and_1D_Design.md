# Day 03 — PN 접합·항복·1D 설계

> **Week 1 / Day 3** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: Day 2
> **참조**: [`../01_Week1_Overview_and_WBG_Process_Fundamentals.md`](../01_Week1_Overview_and_WBG_Process_Fundamentals.md)

---

## 1. 오늘의 질문

> **"1.2 kV를 버티는 drift layer는 어떻게 설계하며, 왜 실제 소자는 ideal BV보다 일찍 항복하는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | PN 접합 전기장, 공핍, C-V |
| 10:45–12:15 | Lecture 2 | Avalanche breakdown, BV vs doping, edge effect |
| 13:15–15:15 | Lab | 1D diode BV/E-field 계산 |
| 15:30–16:30 | Case | 1.2 kV SiC SBD/JBS와 termination |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제 |

---

## 3. 학습목표

1. PN 접합의 $V_{bi}$, $W$, $E_{max}$, $C_j$를 계산한다.
2. Avalanche breakdown과 ionization integral을 설명한다.
3. BV와 drift doping/thickness의 trade-off를 정량화한다.
4. Planar junction의 **edge crowding**을 설명한다.
5. JTE/GR/FP/mesa가 BV를 개선하는 원리를 개략 설명한다.
6. 1D TCAD로 1.2 kV diode를 설계·검증한다.

---

## 4. Lecture 1 — PN 접합 전기장 (1.5h)

### 4.1 Built-in potential

$$
V_{bi} = \frac{kT}{q}\ln\left(\frac{N_A N_D}{n_i^2}\right)
$$

SiC는 $n_i$가 매우 작아 $V_{bi}$가 큽니다(대표 2.5–3 V). Si는 약 0.8–0.9 V.

### 4.2 공핍 근사

$$
W = \sqrt{\frac{2\varepsilon}{q}\left(\frac{1}{N_A}+\frac{1}{N_D}\right)(V_{bi}-V)}
$$

일방 접합($N_A \gg N_D$):

$$
W \approx \sqrt{\frac{2\varepsilon (V_{bi}-V)}{qN_D}}
$$

최대 전계:

$$
E_{max} = \sqrt{\frac{2qN_D}{\varepsilon}(V_{bi}-V)}
$$

### 4.3 접합 커패시턴스

$$
C_j = \frac{\varepsilon}{W} = \frac{C_{j0}}{\sqrt{1-V/V_{bi}}}
$$

$1/C_j^2$ vs $V$ 기울기로 doping을 추출합니다.

### 4.4 Abrupt vs linearly graded

| 유형 | $C_j$ vs $V$ | $E(x)$ | 대표 |
|---|---|---|---|
| Abrupt | $C \propto (V_{bi}-V)^{-1/2}$ | 삼각형 | epitaxial drift |
| Linearly graded | $C \propto (V_{bi}-V)^{-1/3}$ | 포물선 | diffusion junction |

> **실무**: SiC drift/epi는 abrupt에 가깝고, implant junction은 grading이 있습니다.

---

## 5. Lecture 2 — 항복과 Edge Effect (1.5h)

### 5.1 항복 메커니즘

| 메커니즘 | 조건 | 특징 |
|---|---|---|
| Avalanche | 넓은 공핍, 고전계 | 온도↑ → BV↑ (positive TC) |
| Zener/tunneling | 고도핑, 얇은 junction | 온도↑ → BV↓ |
| Punch-through | drift가 얇음 | BV가 drift 두께에 포화 |
| Surface breakdown | 표면 전계 집중 | 습도·charge에 민감 |

### 5.2 Ionization integral

$$
I = \int_0^W \alpha(\mathcal{E})\,dx = 1
$$

에서 breakdown. $\alpha$는 전계에 지수적으로 증가합니다.

### 5.3 BV vs doping (abrupt, one-sided)

$$
BV \approx \frac{\varepsilon E_c^2}{2qN_D}
$$

| $N_D$ (cm⁻³) | 4H-SiC ideal BV (V) | drift 두께 (µm, 대표) |
|---:|---:|---:|
| $1\times10^{16}$ | ~600 | ~5 |
| $5\times10^{15}$ | ~1,200 | ~10 |
| $2\times10^{15}$ | ~3,300 | ~30 |
| $5\times10^{14}$ | ~10,000 | ~100 |

> **교육용 대표값**입니다. 실제는 termination, surface charge, lifetime에 따라 달라집니다.

### 5.4 Edge crowding

Planar junction에서 가장자리의 전계가 bulk보다 커집니다.

- 원인: junction curvature, implant tail, surface charge, dielectric mismatch.
- 결과: ideal BV의 50–80%에서 항복.
- 대응: JTE, guard ring, field plate, mesa, bevel.

### 5.5 Termination 미리보기

| 구조 | 원리 | 장점 | 단점 |
|---|---|---|---|
| JTE | implant로 표면 전계 완화 | 공정 단순 | dose/charge 민감 |
| Guard ring | 전위 분배 | robust | 면적 소모 |
| Field plate | dielectric 위 전계 분산 | MOS兼容 | oxide 신뢰성 |
| Mesa | etching으로 junction 노출 | 고전압 | passivation 중요 |

---

## 6. Lab — 1D Diode BV 설계 (2h)

### 6.1 목표

- 1.2 kV SiC diode의 drift doping/thickness를 설계.
- BV, $E_{max}$, depletion width를 추출.
- Ideal vs edge-terminated 차이를 비교.

### 6.2 절차

1. 1D 구조: p+ anode / n- drift / n+ substrate.
2. $N_D$ = $2\times10^{15}$, $5\times10^{15}$, $1\times10^{16}$ cm⁻³ sweep.
3. drift thickness = 8, 10, 12 µm sweep.
4. Reverse bias를 0→2000 V로 sweep.
5. $I_R$이 급증하는 전압을 BV로 정의.
6. $E(x)$ peak와 위치를 추출.

### 6.3 제출물

- BV vs $N_D$ 그래프.
- BV vs drift thickness 그래프.
- 1.2 kV 설계 표(선택한 $N_D$, $t$, BV margin).
- 해석: punch-through 여부.

### 6.4 체크포인트

- [ ] BV가 doping↑에 따라 감소.
- [ ] drift thickness가 부족하면 punch-through.
- [ ] $E_{max}$가 $E_c$에 도달.
- [ ] 1.2 kV 설계에 **20% margin** 확보.

---

## 7. Case — 1.2 kV SiC SBD/JBS (1h)

### 7.1 SBD

- Schottky barrier: Ti, Ni, Pt.
- 장점: no reverse recovery, low $V_F$.
- 단점: leakage, barrier lowering.

### 7.2 JBS

- p+ grid로 Schottky 전계 완화.
- trade-off: $V_F$ vs leakage.
- termination: JTE/guard ring 필수.

### 7.3 토론

1. 1.2 kV에서 drift 두께 8 µm면 왜 부족한가?
2. Edge에서 항복이 먼저 일어나면 어떤 측정 signature가 나타나는가?
3. JTE dose가 너무 높으면 BV가 왜 다시 감소하는가?

---

## 8. Quiz (5문항)

1. $V_{bi}$ 공식을 쓰시오.
2. 일방 abrupt junction의 $W$와 $E_{max}$ 관계는?
3. Avalanche BV의 온도 계수는?
4. Planar junction edge crowding의 원인 2가지는?
5. Punch-through가 발생하면 BV는 무엇에 의해 결정되는가?

**정답**: 1) $(kT/q)\ln(N_AN_D/n_i^2)$. 2) $E_{max}=2(V_{bi}-V)/W$. 3) positive. 4) curvature, surface charge. 5) drift thickness.

---

## 9. Homework

- 3.3 kV 설계를 위해 $N_D$와 $t$를 계산.
- 1D BV 코드를 termination 없이/있게 비교.
- Day 4를 위해 **FOM** 복습.

---

## 10. 강사 노트

- "Ideal BV는 상한값"임을 반복.
- Edge effect는 **실제 실패 사례**로 설명.
- 수강생이 $V_{bi}$를 과소평가하는 경우가 많음 → SiC는 2.5–3 V.
- 다음 날 FOM과 연결해 "왜 SiC가 유리한가"를 숫자로 마무리.

---

*Day 03 / 20 — 140h program. 설계 수치는 교육용 대표값입니다.*
