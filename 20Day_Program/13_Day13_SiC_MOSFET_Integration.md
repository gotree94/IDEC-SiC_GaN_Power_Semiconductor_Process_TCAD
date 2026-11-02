# Day 13 — SiC MOSFET Integration: DMOS·Trench·SJ

> **Week 3 / Day 13** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: Day 12
> **참조**: [`../03_Week3_SiC_Contacts_Integration_and_Termination.md`](../03_Week3_SiC_Contacts_Integration_and_Termination.md)
> **논문**: [`../07_References_and_Professor_Publications.md`](../07_References_and_Professor_Publications.md) P1–P4

---

## 1. 오늘의 질문

> **"1.2 kV SiC MOSFET에서 planar DMOS, trench, double trench, SJ 중 무엇을 선택하며, mask와 공정 순서는 어떻게 다른가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | DMOS/trench/SJ 구조·저항 성분 |
| 10:45–12:15 | Lecture 2 | 통합 공정·mask·self-align·termination |
| 13:15–15:15 | Lab | Cross-section + mask flow 설계 |
| 15:30–16:30 | Case | 교수님 논문 P1–P4 설계 비교 |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제 |

---

## 3. 학습목표

1. DMOS/trench/double trench/SJ의 구조와 저항 성분을 비교한다.
2. $R_{on,sp}$를 channel/JFET/drift/substrate/contact로 분해한다.
3. Self-aligned 공정과 mask 수를 설계한다.
4. Body diode·termination·gate oxide를 통합한다.
5. 교수님 논문의 설계 아이디어를 공정 관점에서 해석한다.
6. 1.2 kV 소자의 integration plan을 작성한다.

---

## 4. Lecture 1 — 구조와 저항 (1.5h)

### 4.1 DMOS

- 수평 channel, source/body를 self-align.
- 장점: 공정 단순, gate oxide 신뢰성, robust.
- 단점: JFET 저항, $R_{on,sp}$ 큼.
- 핵심: channel length, JFET width, p-well doping.

### 4.2 Trench

- 수직 channel, gate가 trench 측벽.
- 장점: cell pitch↓, $R_{on,sp}$↓, JFET 저항 제거.
- 단점: bottom corner field, oxide 신뢰성, channel mobility.
- 핵심: trench depth/angle, corner rounding, bottom oxide.

### 4.3 Double Trench / Shield

- Gate trench + source trench(shield).
- 장점: bottom field 완화, $C_{gd}$ 감소, switching 개선.
- 단점: 공정 복잡, mask 증가.

### 4.4 Super Junction (SJ)

- p/n pillar charge balance.
- 장점: high BV + low $R_{on}$.
- 단점: charge imbalance, process precision, termination.

### 4.5 $R_{on,sp}$ 분해

$$
R_{on,sp} = R_{ch} + R_{JFET} + R_{drift} + R_{sub} + R_{contact}
$$

| 성분 | DMOS | Trench |
|---|---|---|
| Channel | 큼(planar) | 작음(수직) |
| JFET | 큼 | 없음 |
| Drift | BV에 의해 결정 | 동일 |
| Substrate | n+ | n+ |
| Contact | source/body | source/body |

> 1.2 kV에서는 drift가 지배적이지만, 650 V 이하에서는 channel/JFET가 중요.

---

## 5. Lecture 2 — 통합 공정 (1.5h)

### 5.1 DMOS mask flow (예)

| # | Mask | 공정 |
|---:|---|---|
| 1 | JTE | Al implant |
| 2 | p-well | Al implant |
| 3 | n+ source | N implant |
| 4 | p+ body | Al implant |
| 5 | Gate | poly pattern |
| 6 | Contact | oxide etch |
| 7 | Metal | source/drain metal |
| 8 | Passivation | pad open |

> 실제는 8–12 mask. Self-align으로 channel length를 정의.

### 5.2 Trench 추가 공정

- Trench mask + etch.
- Sacrificial oxidation/corner rounding.
- Gate oxide (bottom thicker 목표).
- Poly recess/etchback.
- Shield implant(선택).

### 5.3 핵심 통합 이슈

| 이슈 | 영향 | 대응 |
|---|---|---|
| Channel length | $V_{th}$, $R_{on}$ | self-align, implant dose |
| JFET width | $R_{on}$, BV | layout |
| Body contact | latch-up, $R_{on}$ | p+ implant |
| Termination | BV | JTE/GR |
| Gate oxide | reliability | NO, thickness |
| Thermal budget | interface | 순서 최적화 |

### 5.4 교수님 논문 연결

| 논문 | 아이디어 | 공정 포인트 |
|---|---|---|
| P1 | channeling step shielding | SCWT/UIS 개선, implant/구조 |
| P2 | P-shielded split gate | $C_{gd}$/switching |
| P3 | tilted implant trench | bottom oxide field 완화 |
| P4 | bottom protection p-well | trench bottom field |

> 논문의 구조를 **mask/공정 순서**로 번역하는 연습.

---

## 6. Lab — Cross-section + Mask Flow (2h)

### 6.1 목표

- 1.2 kV DMOS와 trench의 cross-section 작성.
- Mask 수/순서 비교.
- $R_{on}$ 성분 분해.

### 6.2 절차

1. DMOS cross-section: p-well, n+ source, p+ body, JFET, JTE.
2. Trench cross-section: trench, gate oxide, poly, shield.
3. Mask table 작성.
4. $R_{on,sp}$ 성분 계산(가정값).
5. Sensitivity: channel mobility ±50%, JFET width ±20%.

### 6.3 제출물

- Cross-section 2개.
- Mask flow 표.
- $R_{on}$ 분해 표.
- "DMOS vs trench" 1페이지 비교.

### 6.4 체크포인트

- [ ] p-well/n+ overlap.
- [ ] Body contact.
- [ ] JTE 위치.
- [ ] Trench corner rounding.

---

## 7. Case — 1.2 kV DMOS vs Trench (1h)

### 7.1 DMOS

- 장점: robust, gate oxide, 단순.
- 단점: JFET 저항, cell pitch.
- 적합: 산업용, 신뢰성 우선.

### 7.2 Trench

- 장점: low $R_{on,sp}$, high current density.
- 단점: corner field, oxide reliability.
- 적합: EV, 고효율.

### 7.3 Double trench

- 장점: shield, low $C_{gd}$.
- 단점: 공정 복잡.
- 적합: 고주파 EV.

### 토론

1. DMOS에서 JFET width를 줄이면 BV는 왜 감소하는가?
2. Trench bottom corner를 rounding하는 방법 3가지?
3. P-shield가 switching에 유리한 이유는?

---

## 8. Quiz (5문항)

1. DMOS의 $R_{on}$ 성분 5가지는?
2. Trench가 DMOS보다 유리한 점은?
3. Self-align의 목적은?
4. Trench corner field가 문제되는 이유는?
5. P-shield의 역할은?

**정답**: 1) channel/JFET/drift/substrate/contact. 2) JFET 제거, cell pitch↓. 3) channel length 정의. 4) oxide breakdown. 5) bottom field 완화/$C_{gd}$ 감소.

---

## 9. Homework

- 1.2 kV DMOS mask flow를 10단계로 작성.
- Trench bottom oxide를 두껍게 만드는 방법 조사.
- Day 14를 위해 **JTE/termination** 예습.

---

## 10. 강사 노트

- Cross-section을 **손으로 그리게** 한다.
- Mask 수와 공정 순서를 **표로** 정리.
- 논문 구조를 그대로 베끼지 말고 **공정 가능성**을 토론.
- 다음 날 termination으로 BV 마무리.

---

*Day 13 / 20 — 140h program. 공정 순서는 교육용 예시입니다.*
