# Day 14 — Edge Termination·BV 설계

> **Week 3 / Day 14** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: Day 13
> **참조**: [`../03_Week3_SiC_Contacts_Integration_and_Termination.md`](../03_Week3_SiC_Contacts_Integration_and_Termination.md)
> **논문**: [`../07_References_and_Professor_Publications.md`](../07_References_and_Professor_Publications.md) P7–P8

---

## 1. 오늘의 질문

> **"Ideal BV의 50–80%에서 항복하는 planar junction을, JTE/GR/FP로 어떻게 90% 이상까지 끌어올리는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | Edge crowding, JTE/GR/FP/mesa 원리 |
| 10:45–12:15 | Lecture 2 | JTE dose window, multi-zone, charge balance |
| 13:15–15:15 | Lab | JTE dose sweep, surface charge sensitivity |
| 15:30–16:30 | Case | 3.3 kV/4.5 kV termination 논문 |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제 |

---

## 3. 학습목표

1. Planar junction edge crowding의 원인을 설명한다.
2. JTE/GR/FP/mesa/bevel의 전계 분산 원리를 비교한다.
3. JTE dose와 BV의 **window**를 계산·해석한다.
4. Surface charge가 BV에 미치는 영향을 정량화한다.
5. Multi-zone/floating-zone termination을 설명한다.
6. TCAD로 termination을 설계·검증한다.

---

## 4. Lecture 1 — Edge Crowding과 Termination (1.5h)

### 4.1 Edge crowding

- Planar junction의 곡률 → 가장자리 전계 집중.
- Surface charge, dielectric mismatch, implant tail이 악화.
- 결과: ideal BV의 50–80%에서 항복.
- 해결: junction을 **점진적으로** 확장.

### 4.2 Termination 비교

| 구조 | 원리 | 장점 | 단점 |
|---|---|---|---|
| JTE | implanted region으로 surface field 완화 | 공정 단순, 면적 효율 | dose/charge 민감 |
| Guard ring | floating p+ ring으로 전위 분배 | robust | 면적 소모 |
| Field plate | metal/dielectric 위 전계 분산 | MOS 호환 | oxide 신뢰 |
| Mesa | junction 노출 | 고전압 | passivation 중요 |
| Bevel | 경사 etch | 고전압 | 공정 어려움 |

### 4.3 JTE 동작

- JTE region은 완전 공핍되어 **charge balance** 형성.
- Optimal dose:

$$
Q_{JTE} \approx \frac{\varepsilon_s E_c}{q}
$$

4H-SiC에서 $Q_{opt} \sim 1\times10^{13}$ cm⁻² (대표).

- Dose too low: JTE가 일찍 공핍 → surface field 집중.
- Dose too high: JTE가 공핍되지 않음 → field crowding at edge.

### 4.4 JTE length

- $L_{JTE} > W_{dep}$ at target BV.
- 너무 길면 면적/on-resistance 손해.
- 1.2 kV: 30–60 µm; 3.3 kV: 100–200 µm (대표).

---

## 5. Lecture 2 — 설계·신뢰성 (1.5h)

### 5.1 Dose window

- BV vs dose는 **종 모양**.
- Window는 dose ±10–20%, surface charge에 따라 이동.
- 목표: process variation 내에서 BV margin 확보.

### 5.2 Multi-zone JTE

- 여러 dose/length zone.
- 장점: 넓은 window, surface charge tolerance.
- 단점: mask/공정 복잡.

### 5.3 Floating zone / multi-floating-zone

- Floating p+ ring을 여러 개.
- 전위 분배로 surface field 완화.
- P8: exponentially modulated multi-floating-zone JTE (4.5 kV).

### 5.4 Charge-balanced termination

- P7: PN multi-epitaxial charge-balanced structure (3.3 kV).
- 목표: ideal 1D BV에 가깝게.
- 공정: epi/mask 정밀도 중요.

### 5.5 Surface charge

| Charge | 영향 | 대응 |
|---|---|---|
| Positive | JTE 공핍 변화 | passivation |
| Negative | field crowding | dielectric |
| Mobile ion | drift | clean/passivation |
| Humidity | instability | hermetic |

---

## 6. Lab — JTE Dose Sweep (2h)

### 6.1 목표

- JTE dose vs BV curve 작성.
- Window와 surface charge sensitivity 평가.
- 1.2 kV/3.3 kV 설계 선택.

### 6.2 파라미터

| 항목 | 값 |
|---|---|
| drift $N_D$ | $5\times10^{15}$ / $2\times10^{15}$ cm⁻³ |
| JTE dose | $0.5$–$2.0\times10^{13}$ cm⁻² |
| JTE length | 30–120 µm |
| surface charge | $0, \pm5\times10^{12}$ cm⁻² |

### 6.3 절차

1. Dose sweep → BV curve.
2. Peak BV와 window 추출.
3. Surface charge ± 변화.
4. JTE length sweep.
5. 최적 설계 선택.

### 6.4 제출물

- BV vs dose 그래프.
- Window 표.
- Surface charge tolerance.
- "설계 + margin" 1페이지.

### 6.5 체크포인트

- [ ] Dose 단위(cm⁻²).
- [ ] Peak BV 위치.
- [ ] Window 폭.
- [ ] Surface charge 효과.

---

## 7. Case — 3.3 kV/4.5 kV Termination (1h)

### 7.1 P7: charge-balanced PN multi-epi

- 3.3 kV.
- Charge balance로 ideal BV에 근접.
- 공정: multi-epi/mask 정밀도.

### 7.2 P8: multi-floating-zone JTE

- 4.5 kV.
- Exponentially modulated dimensions.
- 장점: surface field 분산.

### 7.3 토론

1. JTE dose가 최적보다 높으면 BV가 왜 감소하는가?
2. Surface charge가 positive일 때 BV는 어떻게 변하는가?
3. Multi-zone JTE가 single-zone보다 robust한 이유는?

---

## 8. Quiz (5문항)

1. Edge crowding의 원인 2가지는?
2. JTE optimal dose 공식은?
3. Dose가 너무 낮으면?
4. Guard ring의 원리는?
5. Multi-zone JTE의 장점은?

**정답**: 1) curvature, surface charge. 2) $\varepsilon_s E_c/q$. 3) 조기 공핍/field 집중. 4) 전위 분배. 5. 넓은 dose window/tolerance.

---

## 9. Homework

- 1.2 kV JTE dose window를 TCAD로 재현.
- 3.3 kV termination length를 계산.
- Day 15를 위해 **신뢰성** 예습.

---

## 10. 강사 노트

- "BV는 termination이 결정" 강조.
- Dose window를 **그래프**로 보여줌.
- Surface charge는 **passivation**과 연결.
- 다음 날 신뢰성으로 마무리.

---

*Day 14 / 20 — 140h program. JTE 수치는 교육용 대표값입니다.*
