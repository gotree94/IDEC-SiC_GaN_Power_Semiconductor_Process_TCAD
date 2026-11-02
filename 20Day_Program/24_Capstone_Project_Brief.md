# 24 — Capstone Project Brief

> **대상**: Day 20 | **비중**: 25% | **팀**: 2–3인
> **참조**: [`20_Day20_Capstone_Review_and_Final_Assessment.md`](./20_Day20_Capstone_Review_and_Final_Assessment.md)

---

## 1. 목적

캡스톤은 140시간 동안 배운 **소자 물리–공정–TCAD–신뢰성–제품화**를 하나의 설계로 통합합니다.

> **"Spec을 정하고, 구조/공정을 설계하고, TCAD/문헌으로 검증하며, risk를 방어하라."**

---

## 2. 주제 (택 1)

### Option A — 1.2 kV SiC Planar DMOS

| 항목 | 목표 |
|---|---|
| BV | >1,400 V |
| $R_{on,sp}$ | <15 mΩ·cm² (교육 목표) |
| $V_{th}$ | 2.5–3.5 V |
| Termination | JTE |
| 신뢰성 | HTRB/HTGB/UIS |

**핵심**: JFET 저항, channel length, JTE dose window.

### Option B — 1.2 kV SiC Trench MOSFET

| 항목 | 목표 |
|---|---|
| BV | >1,400 V |
| $R_{on,sp}$ | <10 mΩ·cm² |
| $V_{th}$ | 2.5–3.5 V |
| 구조 | trench + corner rounding |
| 신뢰성 | TDDB/BTI |

**핵심**: bottom corner field, gate oxide, channel mobility.

### Option C — 650 V GaN HEMT

| 항목 | 목표 |
|---|---|
| BV | >650 V |
| $R_{on}$ | <100 mΩ |
| $V_{th}$ | >1.5 V (normally-off) |
| Dynamic Ron | <1.3 |
| 신뢰성 | HTRB/HTGB/H3TRB |

**핵심**: p-GaN/recess, passivation, field plate.

### Option D — 3.3 kV SiC Termination

| 항목 | 목표 |
|---|---|
| BV | >3,800 V |
| Termination | JTE/multi-zone/floating zone |
| Surface charge | ±$5\times10^{12}$ cm⁻² tolerance |
| 신뢰성 | HTRB |

**핵심**: charge balance, dose window, passivation.

---

## 3. 팀 구성·역할

| 역할 | 책임 |
|---|---|
| Leader | 일정/통합/발표 |
| Device | 구조/TCAD/calibration |
| Process | flow/mask/thermal budget |
| Reliability | 시험/risk (3인 팀) |

> 2인 팀은 Device+Process, Reliability는 공동.

---

## 4. 일정 (Week 4)

| 시점 | 마일스톤 | 산출물 |
|---|---|---|
| Day 16 | 주제/팀 확정 | 1페이지 proposal |
| Day 17 | 구조/공정 초안 | cross-section + flow |
| Day 18 | TCAD 1차 | $V_{th}/R_{on}/BV$ |
| Day 19 | Calibration/DOE | 비교표 + risk |
| Day 20 | 최종 발표 | 보고서 + slide |

> Day 16–19 오후 실습 시간을 캡스톤에 30–60분 할애.

---

## 5. 산출물

### 5.1 보고서 (10–15페이지)

1. **Spec**: 응용/전압/전류/주파수/온도.
2. **구조**: cross-section, 치수, doping.
3. **공정 flow**: mask/thermal budget.
4. **TCAD**: model/mesh/calibration.
5. **결과**: $V_{th}$, $R_{on}$, BV, $C_{gd}$, leakage.
6. **DOE/민감도**: 핵심 변수 2–3개.
7. **신뢰성**: 시험/예상 failure.
8. **Risk/mitigation**.
9. **결론/next step**.
10. **참고문헌** (DOI).

### 5.2 발표 (15분 + 10분 Q&A)

- 12–15장.
- 그래프 축/단위.
- "가정"과 "결과" 구분.
- backup slide 3–5장.

### 5.3 재현 자료

- TCAD deck/script.
- Excel/Python 계산.
- Raw data/plot.

---

## 6. Rubric (100점)

| 항목 | 배점 | 우수(90–100) | 보통(70–89) | 미흡(<70) |
|---|---:|---|---|---|
| Spec/문제 | 10 | 수치/응용 명확 | 일부 모호 | 없음 |
| 구조/소재 | 15 | 근거/대안 비교 | 기본 설명 | 근거 부족 |
| 공정 flow | 20 | thermal budget 완벽 | 순서 대체로 타당 | 충돌 |
| TCAD/검증 | 25 | calibration/측정 비교 | 일부 비교 | 미검증 |
| 신뢰성/risk | 15 | 시험/대응 구체 | 일반론 | 없음 |
| 발표/Q&A | 15 | 논리/방어 | 보통 | 방어 곤란 |

**합격**: 총 70점 이상 + 각 항목 50% 이상.

---

## 7. 설계 리뷰 체크리스트

- [ ] Spec이 수치로 정의됨.
- [ ] 구조가 spec을 만족하는 근거.
- [ ] 공정 순서가 thermal budget과 compatible.
- [ ] $V_{th}$/ $R_{on}$/BV 계산.
- [ ] TCAD calibration.
- [ ] 측정/문헌 비교.
- [ ] Termination 포함.
- [ ] 신뢰성 시험 매핑.
- [ ] Risk/mitigation.
- [ ] 재현 가능한 deck.

---

## 8. 자주 하는 실수

1. "SiC가 좋다"만 강조 → **수치 근거 없음**.
2. TCAD만 제시 → **측정 비교 없음**.
3. Activation/gate oxide **순서 오류**.
4. Termination **누락**.
5. $R_{on}$에 channel/contact **누락**.
6. BV를 **1D ideal**로만 제시.
7. 신뢰성 **일반론**.
8. 발표 **시간 초과**.

---

## 9. Risk 예시

| Risk | 영향 | Mitigation |
|---|---|---|
| JTE dose variation | BV↓ | window/DOE |
| Channel mobility low | $R_{on}$↑ | NO anneal |
| Trench corner field | TDDB | rounding |
| p-GaN gate | $V_{th}$ drift | gate stack |
| Dynamic Ron | efficiency | passivation/FP |
| Thermal | $T_j$↑ | substrate/package |

---

## 10. 제출

- LMS/지정 폴더: `capstone/teamXX/`.
- 마감: Day 20 09:00.
- 파일: `report.pdf`, `slides.pdf`, `deck/`, `data/`.
- 지각: 10%/일 감점.

---

## 11. FAQ

**Q. TCAD 라이선스가 없으면?**
A. Python 1D/DEVSIM + 해석식으로 대체. 평가는 물리/검증 중심.

**Q. 논문을 그대로 재현해도 되나?**
A. 가능. 단, **공정 가능성/risk**를 추가해야 함.

**Q. 측정 데이터가 없으면?**
A. 문헌/데이터시트와 비교. "가정"을 명시.

**Q. 2인 팀은?**
A. Reliability를 공동으로, 발표 시간 12분.

---

*24 / 24 — Capstone Project Brief. 20일 프로그램 부록.*
