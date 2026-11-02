# SiC/GaN Power Semiconductor Process Theory
## 4-Week Course: Index & Syllabus

> **과정명**: 전력반도체(SiC/GaN) 공정이론 — 4주 확장판
> **원 강좌**: 부산대-아카데미 「전력반도체 공정이론」 (2026-11-24 ~ 2026-12-01, 온라인, 12시간)
> **강사(원 강좌)**: 석오균 조교수, 부산대학교 전기전자공학부
> **확장 목적**: 12시간 이론 강좌를 4주(주 6시간 × 4주 = 24시간) 교육으로 재설계하고, 각 주차에 **TCAD 검증 실습**과 **산업 실무 관점**을 결합
> **문서 버전**: v2.0 (2026-11-02)
> **대상**: 재직자·대학원생 이상, 반도체 8대 공정 기초 보유자, TCAD 입문~중급

---

## 0. 문서 사용법

이 폴더는 **8개 문서**로 구성됩니다. 이전 단일 문서(v1.0)는 비교·복원용으로 [`_archive/`](./_archive/)에 보관했습니다.

| 파일 | 역할 | 분량(권장) |
|---|---|---|
| [`00_Course_Index_and_Syllabus.md`](./00_Course_Index_and_Syllabus.md) | 전체 안내·4주 syllabus·평가·자료 맵 | 30분 |
| [`01_Week1_Overview_and_WBG_Process_Fundamentals.md`](./01_Week1_Overview_and_WBG_Process_Fundamentals.md) | 1주차: 소재·소자·FOM·공정 개요 | 6시간 |
| [`02_Week2_SiC_Frontend_Processes.md`](./02_Week2_SiC_Frontend_Processes.md) | 2주차: SiC 전공정 6대 단위공정 | 6시간 |
| [`03_Week3_SiC_Contacts_Integration_and_Termination.md`](./03_Week3_SiC_Contacts_Integration_and_Termination.md) | 3주차: SiC 접촉·integration·edge termination | 6시간 |
| [`04_Week4_GaN_HEMT_Process_Reliability_and_Characterization.md`](./04_Week4_GaN_HEMT_Process_Reliability_and_Characterization.md) | 4주차: GaN HEMT·신뢰성·평가 | 6시간 |
| [`05_TCAD_Verification_Setup_and_Calibration.md`](./05_TCAD_Verification_Setup_and_Calibration.md) | TCAD 초기설정·덱·캘리브레이션 실무 | 참조 |
| [`06_Industry_Practice.md`](./06_Industry_Practice.md) | 양산·PCM·신뢰성·DTCO 실무 | 참조 |
| [`07_References_and_Professor_Publications.md`](./07_References_and_Professor_Publications.md) | 검증된 논문·표준·교재 | 참조 |

> **읽는 순서(강사용)**: 00 → 01 → 02 → 03 → 04 → 05 → 06 → 07
> **읽는 순서(수강생용)**: 00 → 해당 주차 파일 → 05의 해당 실습 → 06의 해당 실무 섹션

---

## 1. 원 강좌 → 4주 과정 매핑

원 강좌는 **2일 × 2세션 = 4블록(12시간)** 입니다. 이를 그대로 4주로 확장합니다.

| 원 강좌 블록 | 원 시간 | 4주 과정 | 확장 포인트 |
|---|---|---|---|
| D1-AM: 공정 개요 / WBG 특징 / SiC·GaN 구조·공정 차이 | 2h | **Week 1** (6h) | FOM 유도, 소자 taxonomy, 1D hand calculation, TCAD sanity check 추가 |
| D1-PM: SiC cleaning / 산화·계면 / gate oxide / implant / activation / etching | 4h | **Week 2** (6h) | 각 단위공정의 kinetics·장비·metrology·결함·TCAD 모델링 추가 |
| D2-AM: ohmic/Schottky / diode·MOSFET integration / edge termination·layout | 2h | **Week 3** (6h) | TLM 실측·시뮬, JTE 설계 최적화, BPD 신뢰성, PCM 추가 |
| D2-PM: GaN epi / isolation·recess·normally-off / ohmic·dielectric / passivation·FP / 결함·신뢰성 / integration·평가 | 4h | **Week 4** (6h) | 2DEG 정량 계산, trap 모델, current collapse, AEC-Q101 추가 |

---

## 2. 강사 소개 (공개 정보 기준, 검증됨)

| 항목 | 내용 |
|---|---|
| 성명 | **석오균 (Ogyun Seok), Ph.D.** |
| 현 직위 | 부산대학교 전기전자공학부 **부교수** (2025–현재) |
| 연구실 | **PSLAB (전력반도체 연구실)**, 생물관 206호 |
| 연락처 | ogseok@pusan.ac.kr / +82-51-510-2371 |
| 홈페이지 | https://www.pnu-pslab.com |
| ORCID | [0000-0002-8585-5002](https://orcid.org/0000-0002-8585-5002) |
| 학위 | 서울대학교 전기·컴퓨터공학 박사 (2013) |
| 경력 | UIUC Postdoc (2013–2014) → 한국전기연구원(KERI) 선임연구원 (2014–2020) → 금오공과대 조교수 (2020–2024) → 부산대 조교수 (2024–2025) → 부산대 부교수 (2025–) |
| 연구분야 | SiC planar MOSFET, SiC P-i-N diode, SiC super-junction diode, GaN HEMT, edge termination, radiation hardness, TCAD, reliability |
| 학회 활동 | KIEE 편집이사(2020–), KIEE 전기재료 기술위원회 위원장(2024–), KISM Power Device Session Chair(2023–), APWS TPC(2024–) |
| 수상 | NST 우수신진연구자(2017), 과학기술정보통신부 장관 표창(2018), KIEE 전기물성·응용부문 특별상(2023) |
| 주요 과제(공개) | SiC 파워반도체 표준설계·공정개발, EV용 SiC MOSFET, 우주환경용 고신뢰성 SiC, 650 V 고효율 SiC MOSFET, PTC 히터용 Body Diode Turn-on Free SiC MOSFET, 통신위성 EPC용 SiC, 1.2 kV EV용 SiC MOSFET, 10 kV급 적층 성장, 화합물전력반도체 인력양성 |

> **강의 활용 포인트**: 교수님 연구실은 **"설계–공정–제작–측정–신뢰성" 전 과정**을 수행하며, 국내외 파운드리와 협업해 실제 칩을 제작합니다. 따라서 본 과정의 TCAD·신뢰성 파트는 교수님 연구 흐름(SiC MOSFET 구조 설계 → JTE/edge termination → 방사선·항복 신뢰성)과 직접 연결됩니다.

### 2.1 교수님 대표 논문 (본 과정에서 인용)

| 연도 | 제목 | 저널/학회 | DOI |
|---|---|---|---|
| 2025 | Channeling step shielding MOSFET design for improved short-circuit withstand time and UIS energy in 1200 V SiC devices | Semicond. Sci. Technol. | 10.1088/1361-6641/ae2b07 |
| 2026 | Radiation hardness of 1.2 kV SiC power devices with advanced edge termination structures under proton irradiation | J. Semicond. 47(1) 012501 | 10.1088/1674-4926/25040023 |
| 2025 | Improved Switching Characteristics of 1.2 kV P-Shielded Split Gate SiC MOSFETs | JEET 20(2) 1067–1075 | 10.1007/s42835-024-02132-y |
| 2025 | Designs of Charge-Balanced Edge Termination Structures for 3.3 kV SiC Power Devices Using PN Multi-Epitaxial Layers | Micromachines 16(1) 47 | 10.3390/mi16010047 |
| 2024 | The Effects of a Gate Bias Condition on 1.2 kV SiC MOSFETs during Irradiating Gamma-Radiation | Micromachines 15(4) 496 | 10.3390/mi15040496 |
| 2023 | Design of 1.2 kV SiC trench MOSFET using tilted ion implantation for suppression of electric field crowding at the bottom of the gate oxide | JJAP 62 011001 | 10.35848/1347-4065/acaadf |
| 2021 | Multi-floating-zone JTE for 4.5 kV SiC power devices with exponentially modulated dimensions | JJAP 60 | 10.35848/1347-4065/abe5c4 |
| 2020 | Gate Current and Snapback of 4H-SiC Thyristors on N+ Substrate for Power-Switching Applications | Electronics 9(2) 332 | 10.3390/electronics9020332 |
| 2019 | Effects of junction profiles in bottom protection p-well on electrical characteristics of 1.2 kV SiC trench-gate MOSFETs | EPJ Appl. Phys. | 10.1051/epjap/2020190269 |
| 2018 | (SiC power device process/termination related) | JJAP 57 06HC08 | 10.7567/JJAP.57.06HC08 |

> 전체 목록·검증 상태는 [`07_References_and_Professor_Publications.md`](./07_References_and_Professor_Publications.md) 참조.

---

## 3. 4주 Syllabus

### 3.1 주차별 개요

| 주차 | 주제 | 핵심 질문 | 강의 | 실습/토론 | 산출물 |
|---|---|---|---|---|---|
| **W1** | 소재·소자·공정의 큰 그림 | "왜 SiC/GaN인가? 어떤 소자를 어떤 공정으로 만드는가?" | 4h | 2h (FOM 계산 + 1D TCAD) | FOM 계산 리포트, 소자 구조 비교표 |
| **W2** | SiC 전공정 (Front-end) | "SiC에서 산화·주입·활성화·식각은 왜 Si과 다른가?" | 4h | 2h (SRIM + SProcess 1D) | 공정 window 표, 1D profile |
| **W3** | SiC 후공정·integration·termination | "접촉저항과 항복전압을 어떻게 설계·검증하는가?" | 4h | 2h (TLM 분석 + BV 시뮬) | TLM/ρc 리포트, JTE 최적화 결과 |
| **W4** | GaN HEMT·신뢰성·평가 | "2DEG를 만들고, normally-off를 구현하고, 신뢰성을 어떻게 보증하는가?" | 4h | 2h (2DEG 계산 + HEMT 시뮬) | HEMT 공정 flow, 신뢰성 매트릭스 |

### 3.2 주차별 학습목표(Learning Outcomes)

#### Week 1
- [ ] Si / 4H-SiC / GaN의 물성 차이를 **밴드갭–임계전계–이동도–열전도도** 축으로 설명할 수 있다.
- [ ] `R_on,sp = 4V_B²/(ε μ E_c³)`를 **유도**하고, 1.2 kV 기준 SiC/Si/GaN의 이상적 Ron을 계산할 수 있다.
- [ ] Baliga FOM의 의미와 **실제 소자가 이론값에 못 미치는 이유**를 설명할 수 있다.
- [ ] SBD/JBS/PiN/DMOS/Trench/SJ/HEMT/Cascode의 **구조–공정–특성**을 비교할 수 있다.
- [ ] WBG 공정의 **5가지 근본 차이**를 열거하고, 각각의 공정적 귀결을 설명할 수 있다.
- [ ] TCAD 1D sanity check(공핍폭, 산화막 전계, 항복전압)를 손으로 계산할 수 있다.

#### Week 2
- [ ] SiC 세정 시퀀스와 **sacrificial oxidation**의 목적을 설명할 수 있다.
- [ ] Deal-Grove 모델과 SiC 산화의 **orientation 의존성**을 설명할 수 있다.
- [ ] Dit/NIT의 물리적 기원과 **NO/N₂O nitridation**의 효과를 정량적으로 설명할 수 있다.
- [ ] Al/N/P implant의 **box profile** 설계와 **고온 활성화(1600–1800°C)** 의 필요성을 설명할 수 있다.
- [ ] 캡층(AlN/C/SiC)의 역할과 **surface morphology** 이슈를 설명할 수 있다.
- [ ] ICP 식각의 **가스 화학·마스크 선택비·profile 제어**를 설명할 수 있다.
- [ ] 각 단위공정을 **TCAD 모델**로 어떻게 표현하는지 설명할 수 있다.

#### Week 3
- [ ] Ohmic/Schottky 접촉의 **전류 수송 메커니즘**을 설명할 수 있다.
- [ ] TLM 데이터로 **ρc, R_sh, L_T**를 계산할 수 있다.
- [ ] Ni 실리사이드, Ti/Al p-ohmic의 **공정·계면 반응**을 설명할 수 있다.
- [ ] JTE/guard ring/field plate/mesa의 **전계 분산 원리**와 설계 변수를 비교할 수 있다.
- [ ] SiC DMOS/Trench/SJ의 **integration 순서와 thermal budget**을 설명할 수 있다.
- [ ] BPD/stacking fault와 **bipolar degradation**을 설명할 수 있다.
- [ ] TCAD로 **BV, 전계 분포, JTE dose window**를 검증할 수 있다.

#### Week 4
- [ ] AlGaN/GaN **polarization 2DEG**를 정량적으로 계산할 수 있다.
- [ ] MOCVD epi stack과 **buffer doping(C, Fe)** 의 역할을 설명할 수 있다.
- [ ] **normally-off** 구현 3방식(p-GaN, recess, cascode)을 비교할 수 있다.
- [ ] Ti/Al/Ni/Au ohmic, ALD Al₂O₃/SiNx gate dielectric, SiNx passivation의 **공정 조건**을 설명할 수 있다.
- [ ] **current collapse / dynamic Ron**의 원인과 field plate 설계를 설명할 수 있다.
- [ ] HTRB/HTGB/HTOL/H3TRB, AEC-Q101, JEDEC 기준을 매핑할 수 있다.
- [ ] TCAD에서 **polarization + trap + self-heating**을 설정할 수 있다.

---

## 4. 평가·과제 설계

| 평가 항목 | 비중 | 내용 | 평가 기준 |
|---|---|---|---|
| 주차 퀴즈 | 20% | 5문항 × 4주 (원 강좌 3/5 통과 기준 준용) | 개념 정확성 |
| TCAD 실습 리포트 | 30% | W1 1D, W2 공정, W3 BV, W4 HEMT | 재현성·물리적 타당성·캘리브레이션 근거 |
| 공정 flow 설계 | 20% | SiC MOSFET 또는 GaN HEMT 1개 | step/thermal budget/마스크 수/리스크 |
| 산업 실무 분석 | 15% | PCM·신뢰성·DOE 사례 분석 | 데이터 해석·한계 명시 |
| 최종 발표 | 15% | "1.2 kV SiC MOSFET 공정·TCAD 통합 설계" | 논리·정량성·발표 |

> **원 강좌 수료 조건**: 출석 100% + 퀴즈 5문제 중 3문제 이상. 4주 과정에서는 위 평가를 추가하되, 원 강좌 수료 기준은 그대로 유지합니다.

---

## 5. 선수지식 체크리스트

| 영역 | 필요한 수준 | 부족 시 보충 |
|---|---|---|
| 반도체 물리 | 밴드갭, 캐리어 농도, 페르미 준위, PN 접합, MOS C-V | Sze & Ng 1–4장 |
| 소자 물리 | MOSFET 동작, 문턱전압, 항복, 캐패시턴스 | Baliga *Fundamentals* 1–3장 |
| 공정 | 포토/에치/증착/산화/확산/주입/세정/금속 | 8대 공정 입문 강의 |
| 수학 | 미분방정식, 지수/로그, 단위 변환 | 공학수학 |
| TCAD | GUI 또는 스크립트 실행 경험(없어도 가능) | 05 파일의 "입문자 트랙" |

---

## 6. 4주 운영 캘린더(예시)

| 주 | 사전학습(1h) | 강의(4h) | 실습(2h) | 과제 |
|---|---|---|---|---|
| W1 | 물성표 암기 | 소재·FOM·소자·공정 개요 | FOM 계산 + 1D sanity check | FOM 리포트 |
| W2 | Deal-Grove 복습 | SiC 전공정 6개 | SRIM + SProcess 1D | 공정 window 표 |
| W3 | PN 접합/항복 복습 | 접촉·integration·termination | TLM + BV 시뮬 | TLM/ρc 리포트 |
| W4 | 이종접합/분극 예습 | GaN HEMT·신뢰성 | 2DEG + HEMT 시뮬 | HEMT flow + 신뢰성 매트릭스 |

---

## 7. 산업 실무 연결 지도

| 주차 | 강의 개념 | 산업 실무 연결 | 해당 파일 섹션 |
|---|---|---|---|
| W1 | FOM, 소자 구조 | 제품 spec → 소자 선택 → 공정 플랫폼 | 06 §3 |
| W2 | 산화/주입/활성화/식각 | inline metrology, SPC, recipe window | 06 §4 |
| W3 | 접촉/termination/integration | PCM, TLM monitor, BV test, yield | 06 §5 |
| W4 | GaN epi/HEMT/신뢰성 | AEC-Q101, HTRB/HTGB, dynamic Ron | 06 §6 |
| 공통 | TCAD | DTCO, corner model, DOE | 06 §7 |

---

## 8. 인용·검증 원칙

이 문서는 다음 원칙을 따릅니다.

1. **검증된 사실**은 DOI/URL과 함께 표기합니다.
2. **업계 일반 관행**은 "일반 관행"으로 명시하고, 특정 회사의 내부 자료로 오인되지 않게 합니다.
3. **대표값(typical)** 은 장비·레시피·문헌에 따라 달라질 수 있음을 매번 명시합니다.
4. 확인되지 않은 논문·수치는 **"미확인"** 으로 표기하거나 제외합니다.

> ⚠️ 삼성전자·SK하이닉스의 **내부 공정 조건·수율 데이터는 공개 자료가 아닙니다.** 06 파일은 공개 표준(JEDEC, AEC-Q101, IEC)과 공개 논문·백서를 기반으로 한 **일반 실무 프레임**이며, 특정 회사의 기밀을 포함하지 않습니다.

---

## 9. 원 강좌 행정 정보

| 항목 | 내용 |
|---|---|
| 캠퍼스 | 부산대-아카데미 |
| 구분 | 설계강좌 / 공통 / 중급 / 이론 |
| 원 강의 일자 | 2026-11-24(화), 2026-12-01(화) |
| 시간 | 10:00–12:00, 13:00–17:00 (일 6h) |
| 형태 | 온라인(Zoom) |
| 수강료 | 무료 |
| 수강 대상 | 재직자, 대학원생 이상 권장 |
| 수료 조건 | 출석 100% + 퀴즈 5문항 중 3문항 이상 |
| 신청·취소 | 재직자 2026-11-02 00:00 ~ 11-08 23:59 / 전체 11-09 00:00 ~ 11-22 23:59 |
| 안내 | 강의 하루 전 메일(Zoom URL 포함) |
| 페널티 | 전일 결석 시 8개월 수강 제한 + 기존 신청 전부 취소 |

---

## 10. 강사용 운영 노트

### 10.1 난이도 조절

- 원 강좌는 **"학사 최소, 석사·재직자 기준"** 으로 운영됩니다(수강 후기 기준).
- 4주 과정에서는 **1주차를 "공통 언어 맞추기"** 에 쓰고, 2–3주차에 SiC 심화, 4주차에 GaN을 배치하는 것이 안전합니다.
- 수강생 배경이 다양하면 **W1에 반도체 물리 복습 30분**을 반드시 넣으십시오.

### 10.2 실습 환경

- TCAD 라이선스가 없으면: **DEVSIM(무료) + SRIM(무료) + Python/ngspice** 로 대체 가능.
- 라이선스가 있으면: **Sentaurus SWB** 또는 **Silvaco DeckBuild** 로 W2–W4 실습을 통일.
- 실습은 **"정답 맞추기"가 아니라 "가정–모델–결과–검증"의 기록**을 평가합니다.

### 10.3 강의에서 강조할 3가지

1. **"공정 조건 → 물리 현상 → 소자 파라미터"의 인과 사슬**을 항상 말로 설명하기.
2. **숫자 감각**: 1.2 kV SiC epi 10 µm / 5×10¹⁵ cm⁻³, JTE dose 10¹³ cm⁻², Dit 10¹²–10¹³, ρc 10⁻⁵ Ω·cm².
3. **검증 습관**: 시뮬레이션 값은 항상 측정(C-V, I-V, TLM, SIMS)과 비교.

---

## 11. 다음 파일

- 1주차: [`01_Week1_Overview_and_WBG_Process_Fundamentals.md`](./01_Week1_Overview_and_WBG_Process_Fundamentals.md)
- 2주차: [`02_Week2_SiC_Frontend_Processes.md`](./02_Week2_SiC_Frontend_Processes.md)
- 3주차: [`03_Week3_SiC_Contacts_Integration_and_Termination.md`](./03_Week3_SiC_Contacts_Integration_and_Termination.md)
- 4주차: [`04_Week4_GaN_HEMT_Process_Reliability_and_Characterization.md`](./04_Week4_GaN_HEMT_Process_Reliability_and_Characterization.md)
- TCAD: [`05_TCAD_Verification_Setup_and_Calibration.md`](./05_TCAD_Verification_Setup_and_Calibration.md)
- 산업 실무: [`06_Industry_Practice.md`](./06_Industry_Practice.md)
- 참고문헌: [`07_References_and_Professor_Publications.md`](./07_References_and_Professor_Publications.md)
