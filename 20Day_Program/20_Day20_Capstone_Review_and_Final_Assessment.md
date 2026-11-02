# Day 20 — 캡스톤 리뷰·최종 평가·과정 정리

> **Week 4 / Day 20** | **7시간** | 강의 2h + 발표/실습 3h + 최종 시험 2h
> **선수**: Day 1–19
> **참조**: [`../24_Capstone_Project_Brief.md`](./24_Capstone_Project_Brief.md)

---

## 1. 오늘의 질문

> **"내 설계가 spec을 만족하는가? 근거는 측정·TCAD·문헌으로 재현 가능한가? 다음 단계는 무엇인가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:00 | Lecture | Design review 기준, 발표법 |
| 10:00–12:00 | Capstone | 15분 발표 + 10분 Q&A (4–5팀) |
| 13:00–15:00 | Capstone | 계속 발표/피드백 |
| 15:15–17:15 | Final Exam | 20문항, 2시간 |
| 17:15–17:30 | Wrap-up | 수료/다음 단계 |

---

## 3. 학습목표

1. 설계 리뷰에서 spec–근거–risk를 명확히 제시한다.
2. TCAD/측정/문헌을 **재현 가능한 형태**로 설명한다.
3. 공정-소자-신뢰성-제품화를 하나의 story로 연결한다.
4. 동료/강사의 질문에 방어 가능한 답을 한다.
5. 최종 시험으로 140시간 학습을 통합한다.
6. 후속 학습/연구/취업 로드맵을 정한다.

---

## 4. Lecture — Design Review 기준 (1h)

### 4.1 발표 구조 (15분)

| 시간 | 내용 |
|---:|---|
| 1–2 min | 문제 정의/spec |
| 2–5 min | 구조/소재 선택 |
| 5–9 min | 공정 flow/TCAD |
| 9–12 min | 결과/DOE/risk |
| 12–15 min | 결론/next step |

### 4.2 체크리스트

- [ ] Spec이 수치로 명시되었는가?
- [ ] 구조가 spec을 만족하는 근거가 있는가?
- [ ] 공정 순서가 thermal budget과 compatible한가?
- [ ] TCAD calibration이 되었는가?
- [ ] 측정/문헌과 비교했는가?
- [ ] Risk와 mitigation이 있는가?
- [ ] 재현 가능한 deck/데이터가 있는가?

### 4.3 자주 하는 실수

1. "SiC가 좋다"만 강조하고 **수치 근거 없음**.
2. TCAD만 제시하고 **측정 비교 없음**.
3. 공정 순서에서 **thermal budget 충돌**.
4. Termination을 **빠뜨림**.
5. 신뢰성/제품화 **고려 없음**.

---

## 5. Lecture — 140시간의 연결 (1h)

### 5.1 주차별 핵심

| Week | 핵심 | 캡스톤 반영 |
|---|---|---|
| 1 | 왜/무엇 | spec, FOM, 구조 |
| 2 | SiC 전공정 | implant/oxidation/activation |
| 3 | 후공정/신뢰성 | contact/termination/reliability |
| 4 | GaN/TCAD/산업 | calibration/DOE/제품화 |

### 5.2 다음 단계

| 진로 | 추천 |
|---|---|
| 공정 엔지니어 | DOE/SPC, 장비, recipe |
| 소자 엔지니어 | TCAD, layout, characterization |
| 신뢰성 | AEC-Q101, failure analysis |
| 연구 | 논문 재현, TCAD calibration |
| 시스템 | gate driver, thermal, EMI |

### 5.3 추천 자료

- Sze & Ng, *Physics of Semiconductor Devices*.
- Baliga, *Fundamentals of Power Semiconductor Devices*.
- Kimoto & Cooper, *Fundamentals of SiC Technology*.
- 교수님 논문 P1–P13 (DOI는 [`../07_References...`](../07_References_and_Professor_Publications.md)).

---

## 6. Capstone 발표 (3h)

### 6.1 평가 rubric (100점)

| 항목 | 배점 | 기준 |
|---|---:|---|
| Spec/문제 정의 | 10 | 수치/응용 명확 |
| 구조/소재 선택 | 15 | 근거/대안 비교 |
| 공정 flow | 20 | 순서/thermal budget |
| TCAD/검증 | 25 | calibration/측정 비교 |
| 신뢰성/risk | 15 | 시험/대응 |
| 발표/Q&A | 15 | 논리/방어 |

### 6.2 발표 시 유의

- 슬라이드 12–15장.
- 그래프 축/단위 명시.
- "가정"과 "결과"를 구분.
- 질문에는 **모르면 모른다고** 답하고, 확인 방법을 제시.

---

## 7. Final Exam (2h, 20문항)

### 7.1 계산 (8문항)

1. 800 V/200 A, $D=0.5$, $R_{on}=20$ mΩ → 도통 손실.
2. $V=800$ V, $I=200$ A, $t_{sw}=30$ ns, $f=10$ kHz → 스위칭 손실.
3. $N_D=5\times10^{15}$ cm⁻³ 4H-SiC abrupt junction의 ideal BV (대표 $E_c=2.5$ MV/cm, $\varepsilon_r=9.7$).
4. 위 구조의 depletion width at breakdown.
5. Baliga FOM 비율(SiC/Si) 계산(대표값).
6. TLM: $R_s=1$ kΩ/□, $L_T=1$ µm → $\rho_c$.
7. JTE optimal dose ($\varepsilon_s E_c/q$) 근사.
8. 2DEG $n_s$: $\sigma=1.3\times10^{13}$ cm⁻², barrier 보정 무시.

### 7.2 개념 (12문항)

9. DMOS vs trench의 $R_{on}$ 성분 차이.
10. SiC oxidation이 느린 이유.
11. Dit 추출법 2가지.
12. Implant channeling 방지법.
13. Activation cap의 역할.
14. Ni/n-SiC ohmic 메커니즘.
15. JBS p+ grid 역할.
16. JTE dose too high/low 결과.
17. BPD degradation 원인.
18. p-GaN gate 원리.
19. Dynamic Ron 원인.
20. TCAD calibration 순서.

### 7.3 정답 요약

1) 16 W. 2) 24 W. 3) $\varepsilon E_c^2/(2qN_D)$ ≈ 1.3 kV. 4) $W=2BV/E_c$ ≈ 10.4 µm. 5) ~300–400. 6) $\rho_c=R_sL_T^2=10^{-6}$ Ω·cm². 7) ~$1\times10^{13}$ cm⁻². 8) ~$8\times10^{12}$ cm⁻²(보정 전).

---

## 8. Wrap-up

### 8.1 수료 기준

- 출석 90% 이상.
- 퀴즈 60% 이상.
- 캡스톤 70% 이상.
- 최종 시험 70% 이상.

### 8.2 포트폴리오

- 캡스톤 보고서.
- TCAD deck/스크립트.
- 실습 리포트 16개.
- 최종 시험 답안.

### 8.3 마지막 메시지

> "좋은 공정 엔지니어는 **숫자로 말하고, 데이터로 방어하며, 실패를 기록**합니다. 140시간은 시작일 뿐입니다."

---

*Day 20 / 20 — 140h program. 최종 평가는 교육용 예시입니다.*
