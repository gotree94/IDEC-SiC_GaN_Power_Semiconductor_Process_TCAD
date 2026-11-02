# Day 15 — SiC 신뢰성·Qualification

> **Week 3 / Day 15** | **7시간** | 강의 4h + 실습 2h + 주차 시험 1h
> **선수**: Day 14
> **참조**: [`../03_Week3_SiC_Contacts_Integration_and_Termination.md`](../03_Week3_SiC_Contacts_Integration_and_Termination.md)
> **표준**: [`../06_Industry_Practice.md`](../06_Industry_Practice.md)

---

## 1. 오늘의 질문

> **"SiC MOSFET은 왜 Si보다 빨리, 혹은 다르게 열화되는가? 어떤 시험으로 제품 수명을 보증하는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | BPD/TDDB/BTI/UIS/SCWT 메커니즘 |
| 10:45–12:15 | Lecture 2 | AEC-Q101/JEDEC, 가속모델, screening |
| 13:15–15:15 | Lab | Weibull/HTRB 데이터 분석 |
| 15:30–16:30 | Week 3 Exam | 10문항, 30분 + 리뷰 |
| 16:30–17:30 | Review | 시험 해설, Week4 예고 |

---

## 3. 학습목표

1. SiC MOSFET의 주요 열화 메커니즘을 설명한다.
2. BPD/bipolar degradation과 stacking fault를 설명한다.
3. TDDB/BTI/UIS/SCWT의 시험법과 해석을 정리한다.
4. AEC-Q101/JEDEC/IEC 시험 항목을 제품 spec에 매핑한다.
5. Arrhenius/Weibull로 수명을 외삽한다.
6. 신뢰성 데이터를 설계·공정 개선에 연결한다.

---

## 4. Lecture 1 — 열화 메커니즘 (1.5h)

### 4.1 BPD/Bipolar degradation

- Basal Plane Dislocation (BPD) → stacking fault (SF) 확장.
- Forward bias에서 electron-hole recombination이 SF를 확장.
- 결과: $R_{on}$ 증가, $V_F$ 증가.
- 대응: BPD density 감소, epi 성장 최적화, buffer layer.

### 4.2 TDDB

- Gate oxide time-dependent dielectric breakdown.
- Weibull 분포:

$$
F(t) = 1-\exp\left[-\left(\frac{t}{\eta}\right)^\beta\right]
$$

- $\beta$: shape (failure mode).
- $\eta$: scale (63.2% failure time).
- 가속: voltage, temperature.

### 4.3 BTI

| 유형 | Stress | 영향 |
|---|---|---|
| PBTI | $V_{GS}>0$ | $V_{th}$↑ |
| NBTI | $V_{GS}<0$ | $V_{th}$ shift |

- SiC MOSFET은 PBTI가 중요.
- Recovery 존재 → 측정 timing 중요.

### 4.4 UIS/SCWT

| 시험 | 의미 | SiC 이슈 |
|---|---|---|
| UIS | avalanche energy | $E_{AS}$ 낮음 |
| SCWT | short-circuit withstand time | Si보다 짧음(2–5 µs) |
| RBSOA | reverse bias safe operating area | termination |

### 4.5 기타

| 시험 | 목적 |
|---|---|
| HTRB | reverse bias, high temp |
| HTGB | gate bias, high temp |
| H3TRB | humidity |
| TC/PC | thermal/power cycling |
| HAST | accelerated humidity |
| Radiation | SEE/SEB |

---

## 5. Lecture 2 — Qualification·가속모델 (1.5h)

### 5.1 AEC-Q101

- Discrete semiconductor stress test qualification.
- 주요 항목: HTGB, HTRB, H3TRB, TC, PC, UHAST, IOL, RSH.
- Grade 0–3: 온도 범위.

### 5.2 JEDEC/IEC

| 표준 | 내용 |
|---|---|
| JESD22 | package/test methods |
| JESD47 | stress-test-driven qualification |
| IEC 60747 | semiconductor devices |
| IEC 60749 | mechanical/climatic |

### 5.3 가속모델

Arrhenius:

$$
AF = \exp\left[\frac{E_a}{k}\left(\frac{1}{T_{use}}-\frac{1}{T_{stress}}\right)\right]
$$

Power-law (voltage):

$$
AF_V = \left(\frac{V_{stress}}{V_{use}}\right)^n
$$

Weibull lifetime:

$$
t_{63.2\%} = \eta
$$

### 5.4 Screening

- Gate stress, burn-in, thermal cycling.
- 목적: early failure 제거.
- SiC는 **gate oxide defect** screening 중요.

### 5.5 데이터 해석

- Failure mode 분리($\beta$).
- Outlier vs intrinsic.
- Field return 예측.
- Design margin 설정.

---

## 6. Lab — Weibull/HTRB 분석 (2h)

### 6.1 목표

- TDDB Weibull plot 작성.
- $\beta$, $\eta$ 추출.
- HTRB leakage drift 분석.
- 수명 외삽.

### 6.2 데이터(예)

| Stress time (h) | Failure count | Cumulative F (%) |
|---:|---:|---:|
| 10 | 1 | 5 |
| 20 | 2 | 15 |
| 40 | 4 | 35 |
| 80 | 6 | 60 |
| 160 | 8 | 85 |

### 6.3 절차

1. $F$를 Weibull scale로 변환: $\ln[-\ln(1-F)]$.
2. $\ln t$ vs Weibull plot.
3. 기울기 $\beta$, 절편 $\eta$.
4. Arrhenius로 use condition 외삽.
5. HTRB leakage drift rate 계산.

### 6.4 제출물

- Weibull plot.
- $\beta$, $\eta$ 표.
- Use-condition 수명 추정.
- "screening 개선" 제안.

### 6.5 체크포인트

- [ ] $F$ 계산.
- [ ] Weibull scale.
- [ ] $\beta$ 의미.
- [ ] 가속계수 적용.

---

## 7. Week 3 Exam (30분, 10문항)

1. TLM에서 $\rho_c$ 추출 방법?
2. Schottky barrier lowering 원인?
3. JBS p+ grid 역할?
4. DMOS vs trench 저항 성분 차이?
5. Trench corner field 문제?
6. JTE optimal dose?
7. JTE dose too high의 결과?
8. BPD degradation 원인?
9. TDDB Weibull $\beta$ 의미?
10. AEC-Q101의 목적?

**정답 요약**: 1) $R_{total}$ vs $d$ fit, $L_T$. 2) image force/field. 3) field shielding/leakage. 4) JFET vs vertical channel. 5) oxide breakdown. 6) $\varepsilon_s E_c/q$. 7) JTE not depleted/field crowding. 8) SF expansion. 9) failure mode shape. 10) discrete qualification.

---

## 8. Homework

- 1.2 kV SiC MOSFET의 HTRB/HTGB 조건을 AEC-Q101에서 찾아 정리.
- TDDB 데이터로 $\beta$를 계산.
- Week4를 위해 **GaN polarization** 예습.

---

## 9. 강사 노트

- SiC 신뢰성은 **"Si와 같은 시험, 다른 메커니즘"**.
- BPD는 **bipolar operation**에서 특히 중요.
- Weibull $\beta$ 해석을 강조.
- Week4 GaN로 전환하며 **polarization** 예고.

---

*Day 15 / 20 — 140h program. 신뢰성 데이터는 교육용 예시입니다.*
