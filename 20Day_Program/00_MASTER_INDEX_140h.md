# SiC/GaN Power Semiconductor Process Theory
## 20-Day Intensive Program — Master Index (140 Hours)

> **과정명**: 전력반도체(SiC/GaN) 공정이론 — 4주 집중 과정
> **운영 형태**: **4주 × 주 5일 × 1일 7시간 = 140시간**
> **일일 구성**: 강의 4h + TCAD/공정 실습 2h + 사례·퀴즈·멘토링 1h
> **선수 조건**: 반도체 8대 공정 기초, 소자 물리 기초(또는 Day 2–3 수강)
> **문서 버전**: v3.0 (2026-11-02)
> **기존 문서**: `../00`–`../07` 8개 문서는 **Reference Library**로 유지

---

## 0. 이 프로그램의 설계 의도

기존 8개 문서는 **24시간(주 6시간)** 기준의 "핵심 이론 + 실습 참조"입니다.
이 프로그램은 이를 **140시간**으로 확장하여, 다음을 가능하게 합니다.

1. **매일 7시간**의 강의·실습·평가를 **20일**에 걸쳐 운영.
2. 이론 → 손계산 → TCAD → 공정 설계 → 산업 실무 → 캡스톤의 **반복 학습 루프**.
3. 수강생이 매일 **산출물**(계산 시트, TCAD 결과, 공정 표, 퀴즈)을 남김.
4. 강사가 **그대로 운영할 수 있는** 일일 시간표·질문·평가 기준 제공.

> **핵심 원칙**: "하루에 하나의 질문, 하나의 계산, 하나의 검증, 하나의 산출물."

---

## 1. 140시간 운영 개요

| 항목 | 값 |
|---|---|
| 총 기간 | 4주 |
| 주당 일수 | 5일 |
| 일당 시간 | 7시간 (09:00–17:30, 점심 1h·휴식 30min 별도) |
| 총 시간 | 20일 × 7h = **140h** |
| 강의 | 80h (4h/일) |
| 실습 | 40h (2h/일) |
| 사례·퀴즈·멘토링 | 20h (1h/일) |
| 평가 | 일일 퀴즈 20%, 실습 25%, 주차 시험 20%, 캡스톤 25%, 참여 10% |

### 1.1 일일 표준 시간표

| 시간 | 블록 | 내용 | 비고 |
|---|---|---|---|
| 09:00–10:30 | **Lecture 1** (1.5h) | 핵심 개념·유도·표 | 전일 복습 10분 포함 |
| 10:30–10:45 | Break | — | — |
| 10:45–12:15 | **Lecture 2** (1.5h) | 공정/소자 심화·사례 | 질문 10분 |
| 12:15–13:15 | Lunch | — | — |
| 13:15–15:15 | **Lab** (2h) | TCAD/계산/데이터 분석 | 2인 1조 권장 |
| 15:15–15:30 | Break | — | — |
| 15:30–16:30 | **Case/Industry** (1h) | 실제 제품·불량·표준 | 토론 |
| 16:30–17:30 | **Review** (1h) | 퀴즈 5문항·과제·멘토링 | 당일 산출물 제출 |

> **7시간의 정의**: 점심·휴식을 제외한 **순수 교육 시간**. 09:00–17:30 체류 시간은 8.5시간.

---

## 2. 20일 커리큘럼 한눈에 보기

| Day | Week | 주제 | 강의 핵심 | 실습 | 당일 산출물 |
|---:|---|---|---|---|---|
| 01 | W1 | 오리엔테이션·전력전자 지형 | 응용·손실·WBG 가치 | 손실 계산(Python/Excel) | 손실 비교 리포트 |
| 02 | W1 | 반도체 물리 I | 밴드·캐리어·수송 | 1D Poisson/C-V | C-V 해석 리포트 |
| 03 | W1 | PN 접합·항복 | 공핍·항복·edge | 1D diode BV | BV 설계 리포트 |
| 04 | W1 | WBG 물성·FOM | Si/SiC/GaN·FOM | FOM 계산기 | 소재 선택 리포트 |
| 05 | W1 | 소자 taxonomy·공정 개요 | 소자·flow·thermal budget | 1D TCAD sanity check | **Week 1 시험** |
| 06 | W2 | SiC 세정·표면 | RCA·sacrificial oxidation | 세정 시퀀스 설계 | 세정 spec |
| 07 | W2 | SiC 열산화 | Deal-Grove·면 의존성 | 산화 두께 fitting | 산화 recipe |
| 08 | W2 | Gate oxide·계면 | Dit/NIT·nitridation | C-V Dit 추출 | 계면 리포트 |
| 09 | W2 | 이온주입 | LSS·channeling·mask | SRIM/SProcess | implant recipe |
| 10 | W2 | 활성화·식각 | 1600–1800°C·ICP | SRP/식각 profile | **Week 2 시험** |
| 11 | W3 | 접촉 물리·TLM | Schottky/ohmic·TLM | TLM 데이터 분석 | ρc 리포트 |
| 12 | W3 | Ohmic/Schottky 공정 | Ni·Ti/Al·JBS/MPS | 접촉 시뮬 | 접촉 flow |
| 13 | W3 | SiC MOSFET integration | DMOS/trench/SJ·교수님 논문 | 공정 flow 설계 | integration plan |
| 14 | W3 | Edge termination | JTE/GR/FP·charge balance | JTE dose sweep | BV window |
| 15 | W3 | 신뢰성 | BPD/TDDB/BTI/UIS/방사선 | Weibull/HTRB 분석 | **Week 3 시험** |
| 16 | W4 | GaN 물성·분극 | wurtzite·2DEG·MOCVD | 2DEG 계산 | epi spec |
| 17 | W4 | GaN HEMT 공정 | isolation/ohmic/dielectric/FP | HEMT TCAD | HEMT flow |
| 18 | W4 | normally-off·신뢰성 | p-GaN/recess/cascode·dynamic Ron | trap 시뮬 | 신뢰성 matrix |
| 19 | W4 | TCAD·DOE·산업 실무 | calibration·PCM/SPC/PPAP | DOE/calibration | calibration report |
| 20 | W4 | 캡스톤·최종 평가 | 설계 리뷰·시험·정리 | 캡스톤 발표 | **최종 보고서** |

---

## 3. 주차별 학습목표

### Week 1 — 기초와 공통 언어 (Day 1–5)

- [ ] 전력변환 응용과 요구 spec을 **전압·전류·주파수·열** 축으로 정리한다.
- [ ] 도통/스위칭 손실을 계산하고, WBG가 유리한 조건을 설명한다.
- [ ] 밴드·캐리어·이동도·PN 접합·항복을 **수식과 그래프**로 설명한다.
- [ ] Si/SiC/GaN의 FOM과 **이상적 Ron**을 계산·비교한다.
- [ ] 주요 전력소자와 공정 flow를 **구조–공정–특성**으로 연결한다.
- [ ] 1D 손계산으로 TCAD 결과를 검증하는 습관을 만든다.

### Week 2 — SiC 전공정 (Day 6–10)

- [ ] SiC 세정·희생산화·표면 분석의 목적과 sequence를 설계한다.
- [ ] Deal-Grove와 SiC 산화의 **면 의존성**을 정량적으로 다룬다.
- [ ] Dit/NIT와 nitridation의 trade-off를 C-V로 해석한다.
- [ ] Al/N implant의 box profile과 mask를 설계한다.
- [ ] 1600–1800°C 활성화, cap, morphology, ICP 식각을 설명한다.
- [ ] SIMS/SRP/Hall/C-V를 TCAD 캘리브레이션에 연결한다.

### Week 3 — SiC 후공정·신뢰성 (Day 11–15)

- [ ] Schottky/ohmic 전류 수송과 ρc를 TLM으로 계산한다.
- [ ] Ni 실리사이드, Ti/Al p-ohmic, JBS/MPS 공정을 설명한다.
- [ ] DMOS/trench/SJ integration과 thermal budget을 비교한다.
- [ ] JTE/GR/FP/mesa의 전계 분산과 **dose window**를 설계한다.
- [ ] BPD/TDDB/BTI/UIS/방사선 신뢰성을 평가·해석한다.
- [ ] TCAD로 BV·E-field·oxide field를 검증한다.

### Week 4 — GaN·TCAD·산업·캡스톤 (Day 16–20)

- [ ] polarization 2DEG를 계산하고 epi stack을 설계한다.
- [ ] GaN HEMT 공정(isolation/ohmic/dielectric/passivation/FP)을 설명한다.
- [ ] normally-off 3방식과 dynamic Ron/current collapse를 비교한다.
- [ ] TCAD calibration·DOE·corner·DTCO를 실무 흐름으로 정리한다.
- [ ] PCM/SPC/PPAP/8D/AEC-Q101을 제품화 흐름에 매핑한다.
- [ ] 1.2 kV SiC MOSFET 또는 650 V GaN HEMT **캡스톤 설계**를 완성한다.

---

## 4. 평가 체계

| 평가 | 비중 | 세부 | 기준 |
|---|---:|---|---|
| 일일 퀴즈 | 20% | 20일 × 5문항 | 60% 이상 통과, 오답 재학습 |
| 실습 리포트 | 25% | 20개 중 16개 이상 제출 | 재현성·물리 타당성·캘리브레이션 |
| 주차 시험 | 20% | 4회 × 5% | 70% 이상 |
| 캡스톤 | 25% | 보고서 + 발표 | rubric 참조 |
| 참여 | 10% | 출석·토론·멘토링 | 90% 출석 |

### 4.1 캡스톤 주제(택 1)

1. **1.2 kV SiC Planar DMOS** — BV 1400 V, Ron 15 mΩ, Vth 3 V, JTE 설계.
2. **1.2 kV SiC Trench MOSFET** — corner field 완화, tilted implant.
3. **650 V GaN HEMT** — p-GaN gate, dynamic Ron < 1.3, field plate.
4. **3.3 kV SiC JTE/charge-balanced termination** — BV window, surface charge tolerance.

### 4.2 캡스톤 산출물

- 10–15페이지 보고서: spec, 구조, 공정 flow, TCAD, DOE, risk, 결론.
- 15분 발표 + 10분 Q&A.
- **재현 가능한 TCAD deck/스크립트** 포함.

---

## 5. 선수지식·사전학습

| 영역 | 필요 수준 | 보충 |
|---|---|---|
| 반도체 물리 | 밴드, 캐리어, PN 접합 | Day 2–3 |
| 소자 | MOSFET, 항복, C-V | Day 3–5 |
| 공정 | 8대 공정 개요 | Day 5–6 |
| 수학 | 미분방정식, 지수/로그 | 사전 |
| TCAD | GUI/스크립트 경험(선택) | Day 2 실습 |

### 5.1 사전 학습(과정 시작 전 1주)

- Sze & Ng 1–3장 읽기.
- Baliga *Fundamentals* 1–2장.
- Python 기초(numpy, matplotlib).
- SRIM 설치(무료).

---

## 6. 도구·환경

| 용도 | 상용 | 무료 대체 |
|---|---|---|
| 공정/소자 TCAD | Sentaurus, Silvaco | DEVSIM, Python 1D |
| Implant | SProcess | SRIM |
| 회로 | Spectre, LTspice | ngspice |
| Layout | Virtuoso | KLayout |
| 데이터 | Excel, JMP | Python(pandas) |
| 시각화 | — | matplotlib, ParaView |

> **실습 원칙**: 라이선스 유무와 무관하게 **"가정–모델–결과–검증"** 기록을 평가합니다.

---

## 7. Reference Library (기존 8개 문서)

| 기존 문서 | 20일 과정에서의 역할 |
|---|---|
| [`../00_Course_Index_and_Syllabus.md`](../00_Course_Index_and_Syllabus.md) | 원 강좌 행정·교수 소개·4주 개요 |
| [`../01_Week1_Overview_and_WBG_Process_Fundamentals.md`](../01_Week1_Overview_and_WBG_Process_Fundamentals.md) | Day 1–5 심화 참조 |
| [`../02_Week2_SiC_Frontend_Processes.md`](../02_Week2_SiC_Frontend_Processes.md) | Day 6–10 심화 참조 |
| [`../03_Week3_SiC_Contacts_Integration_and_Termination.md`](../03_Week3_SiC_Contacts_Integration_and_Termination.md) | Day 11–15 심화 참조 |
| [`../04_Week4_GaN_HEMT_Process_Reliability_and_Characterization.md`](../04_Week4_GaN_HEMT_Process_Reliability_and_Characterization.md) | Day 16–18 심화 참조 |
| [`../05_TCAD_Verification_Setup_and_Calibration.md`](../05_TCAD_Verification_Setup_and_Calibration.md) | 전 Day 실습 참조 |
| [`../06_Industry_Practice.md`](../06_Industry_Practice.md) | Day 1, 5, 15, 19, 20 |
| [`../07_References_and_Professor_Publications.md`](../07_References_and_Professor_Publications.md) | 전 Day 논문·표준 |

> 기존 문서는 **삭제하지 않고** Reference Library로 유지합니다. 데일리 파일은 "그날 필요한 만큼" 요약 + 참조 링크를 제공합니다.

---

## 8. 데일리 파일 목록

| Day | 파일 |
|---:|---|
| 01 | [`01_Day01_Orientation_Power_Electronics_and_Course_Roadmap.md`](./01_Day01_Orientation_Power_Electronics_and_Course_Roadmap.md) |
| 02 | [`02_Day02_Semiconductor_Physics_Equilibrium_and_Transport.md`](./02_Day02_Semiconductor_Physics_Equilibrium_and_Transport.md) |
| 03 | [`03_Day03_PN_Junction_Breakdown_and_1D_Design.md`](./03_Day03_PN_Junction_Breakdown_and_1D_Design.md) |
| 04 | [`04_Day04_WBG_Materials_FOM_and_Thermal.md`](./04_Day04_WBG_Materials_FOM_and_Thermal.md) |
| 05 | [`05_Day05_Device_Taxonomy_and_Process_Overview.md`](./05_Day05_Device_Taxonomy_and_Process_Overview.md) |
| 06 | [`06_Day06_SiC_Cleaning_and_Surface_Preparation.md`](./06_Day06_SiC_Cleaning_and_Surface_Preparation.md) |
| 07 | [`07_Day07_SiC_Thermal_Oxidation_Kinetics.md`](./07_Day07_SiC_Thermal_Oxidation_Kinetics.md) |
| 08 | [`08_Day08_Gate_Oxide_Interface_and_Nitridation.md`](./08_Day08_Gate_Oxide_Interface_and_Nitridation.md) |
| 09 | [`09_Day09_SiC_Ion_Implantation_and_Masking.md`](./09_Day09_SiC_Ion_Implantation_and_Masking.md) |
| 10 | [`10_Day10_High_Temperature_Activation_and_Etching.md`](./10_Day10_High_Temperature_Activation_and_Etching.md) |
| 11 | [`11_Day11_Contact_Physics_and_TLM.md`](./11_Day11_Contact_Physics_and_TLM.md) |
| 12 | [`12_Day12_Ohmic_Schottky_and_JBS_Process.md`](./12_Day12_Ohmic_Schottky_and_JBS_Process.md) |
| 13 | [`13_Day13_SiC_MOSFET_Integration.md`](./13_Day13_SiC_MOSFET_Integration.md) |
| 14 | [`14_Day14_Edge_Termination_and_BV_Design.md`](./14_Day14_Edge_Termination_and_BV_Design.md) |
| 15 | [`15_Day15_SiC_Reliability_and_Qualification.md`](./15_Day15_SiC_Reliability_and_Qualification.md) |
| 16 | [`16_Day16_GaN_Material_Polarization_and_Epi.md`](./16_Day16_GaN_Material_Polarization_and_Epi.md) |
| 17 | [`17_Day17_GaN_HEMT_Process_Integration.md`](./17_Day17_GaN_HEMT_Process_Integration.md) |
| 18 | [`18_Day18_GaN_Normally_Off_Dynamic_Ron_and_Reliability.md`](./18_Day18_GaN_Normally_Off_Dynamic_Ron_and_Reliability.md) |
| 19 | [`19_Day19_TCAD_Calibration_DOE_and_Industry_Practice.md`](./19_Day19_TCAD_Calibration_DOE_and_Industry_Practice.md) |
| 20 | [`20_Day20_Capstone_Review_and_Final_Assessment.md`](./20_Day20_Capstone_Review_and_Final_Assessment.md) |

### 8.1 부록

| 파일 | 역할 |
|---|---|
| [`21_TCAD_Lab_Manual.md`](./21_TCAD_Lab_Manual.md) | 20일 실습 통합 매뉴얼 |
| [`22_Assessment_Bank.md`](./22_Assessment_Bank.md) | 일일 퀴즈·주차 시험·최종 시험 |
| [`23_Instructor_Operation_Guide.md`](./23_Instructor_Operation_Guide.md) | 강사 운영·시간·FAQ |
| [`24_Capstone_Project_Brief.md`](./24_Capstone_Project_Brief.md) | 캡스톤 요구사항·rubric |

---

## 9. 강사 운영 원칙

1. **매일 09:00에 5문항 복습 퀴즈**로 전날 내용을 활성화.
2. **강의 4h는 "개념 60% + 계산/그림 40%"** 로 구성.
3. **실습 2h는 반드시 산출물 제출**(스크린샷 + 수치 + 해석).
4. **사례 1h는 "실제 제품/불량/표준"** 중 하나를 골라 토론.
5. **17:00–17:30은 멘토링**으로 개인 질문·캡스톤 진행.
6. **매주 금요일 30분 주차 시험** + 30분 리뷰.

### 9.1 난이도 조절

- 수강생 배경이 다양하면 **Day 1–3을 "공통 언어"** 로 운영하고, 실습 난이도를 낮춥니다.
- TCAD 경험이 없으면 **Day 2부터 Python 1D**로 시작해 Day 5에 TCAD GUI로 전환.
- 경험이 많으면 **Day 9부터 DOE/자동화**를 추가.

### 9.2 결석·보충

- 1일 결석 시: 해당 데일리 파일 + 실습 데이터로 자습, 30분 멘토링.
- 2일 이상 결석 시: 주차 시험 전 보충 세션 1회.
- **원 강좌 수료 기준(출석 100% + 퀴즈 3/5)** 은 별도로 유지.

---

## 10. 데이터·인용 원칙

1. **공정 조건은 대표값**이며 장비·레시피에 따라 달라짐을 명시.
2. **회사 내부 자료**는 사용하지 않음. 공개 표준(JEDEC/AEC/IEC)과 공개 논문만 인용.
3. TCAD 결과는 **측정 캘리브레이션 전 절대값 신뢰 금지**.
4. 교수님 논문은 DOI와 함께 [`../07_References_and_Professor_Publications.md`](../07_References_and_Professor_Publications.md)에 정리.
5. 확인되지 않은 논문·수치는 **"미확인"** 또는 제외.

---

## 11. 수료 후 역량(Exit Outcomes)

수료생은 다음을 **혼자 수행**할 수 있어야 합니다.

- 1.2 kV SiC MOSFET 또는 650 V GaN HEMT의 **공정 flow를 설계**하고 thermal budget을 검토한다.
- TCAD로 **BV/Ron/Vth/E-field**를 계산하고 측정과 비교한다.
- TLM/C-V/I-V 데이터로 **ρc, Dit, Vth, Ron**을 추출한다.
- JTE/FP/termination을 **dose window와 tolerance** 관점에서 설계한다.
- 신뢰성 시험(HTRB/HTGB/H3TRB/TC/PC)을 **제품 spec에 매핑**한다.
- PCM/SPC/DOE/8D를 사용해 **공정 이상과 불량을 분석**한다.

---

## 12. 시작하기

1. [`01_Day01_Orientation_Power_Electronics_and_Course_Roadmap.md`](./01_Day01_Orientation_Power_Electronics_and_Course_Roadmap.md)를 연다.
2. 사전 학습 체크리스트를 확인한다.
3. Python/Excel, SRIM, TCAD(또는 DEVSIM)를 설치한다.
4. 매일 **17:30까지 산출물 1개**를 제출한다.

---

*© 2026 SiC/GaN Process Course Notes. 140-hour intensive program. 공정 조건은 대표값입니다.*
