# SiC/GaN Power Semiconductor Process Theory
## Curriculum · Theory · TCAD Design & Analysis Guide
### (부산대-아카데미 「전력반도체 공정이론」 자기학습 정리본)

> **문서 목적**
> 부산대-아카데미 「전력반도체 공정이론」 강좌(2026-11-24 ~ 2026-12-01, 온라인, 석오균 조교수)의
> 공개된 강의목표·강의개요·일자별 세부목차를 분석하여
> ① 12시간 커리큘럼을 재구성하고
> ② 각 세션의 이론을 정리하며
> ③ 관련 설계/해석(TCAD) 프로그램의 종류·초기 설정·분석 절차를 실무 관점에서 정리한 자기학습용 문서입니다.
>
> **작성 기준**: 공개된 강좌 정보 + 4H-SiC / GaN 전력소자 공정 표준 문헌·업계 관행
> **주의**: 구체 수치(온도, 두께, 도즈 등)는 장비·레시피·문헌에 따라 달라지는 **대표값**이며, 실제 공정/시뮬레이션에서는 반드시 캘리브레이션이 필요합니다.

---

## 목차

1. [강좌 Fact Sheet](#1-강좌-fact-sheet)
2. [강의 목표 해부](#2-강의-목표-해부)
3. [12시간 커리큘럼 재구성](#3-12시간-커리큘럼-재구성)
4. [핵심 이론 정리](#4-핵심-이론-정리)
5. [설계·해석 프로그램(TCAD) 지형도](#5-설계해석-프로그램tcad-지형도)
6. [초기 설정(Initial Setup) 상세](#6-초기-설정initial-setup-상세)
7. [분석 절차(Analysis Workflow) 상세](#7-분석-절차analysis-workflow-상세)
8. [수료·행정 체크리스트](#8-수료행정-체크리스트)
9. [퀴즈 대비 핵심 문답 20선](#9-퀴즈-대비-핵심-문답-20선)
10. [참고문헌·추천 학습자료](#10-참고문헌추천-학습자료)
11. [부록: 용어집·공정요약카드·자가점검표](#11-부록)

---

## 1. 강좌 Fact Sheet

| 항목 | 내용 |
|---|---|
| 캠퍼스 | 부산대-아카데미 |
| 구분 | 설계강좌 / 공통 / 중급 / 이론 |
| 강의 제목 | 전력반도체 공정이론 |
| 강의 일자 | 2026-11-24(화) ~ 2026-12-01(화), **매주 화요일 총 2회** |
| 강의 형태 | 온라인 (Zoom) |
| 강사 | 석오균 조교수, 부산대학교 |
| 신청 현황 | 10 / 100명 |
| 수강료 | 일반 무료 / 학생 무료 |
| 수강 대상 | 재직자, 대학원생 이상 권장 |
| 선수과목 | 없음 |
| 수료 조건 | **출석 100% + 퀴즈 5문제 중 3문제 이상 통과** |
| 신청·취소 기간 | 재직자: 2026-11-02 00:00 ~ 2026-11-08 23:59<br>전체: 2026-11-09 00:00 ~ 2026-11-22 23:59 |
| 강의 안내 | 강의 정보(Zoom URL 포함) 및 주의사항은 **매 강의 하루 전 메일** 안내 |
| 페널티 | 1개 교육에 대해 **전일 결석** 시 향후 **8개월간 수강 신청 자동 제한** 및 기존 수강신청 전면 취소 |

### 1.1 한 줄 요약

> **"SiC/GaN이라는 wide-bandgap 소재를 '왜' 쓰는지(물성·FOM)에서 출발하여, SiC는 산화·이온주입·활성화·식각·접촉·edge termination·MOSFET integration까지, GaN은 epi·2DEG·isolation·normally-off·ohmic·passivation·field plate까지 — 즉 '소자 구조 → 공정 흐름 → 공정 이슈 → 신뢰성'의 인과 사슬을 12시간에 압축한 이론 강좌."**

### 1.2 이 강좌가 다루지 않는 것(추정)

공개 목차 기준으로 **실습(hands-on) 비중은 낮고**, TCAD 툴의 버튼 조작보다는 **공정 물리와 integration 사고방식**에 초점이 있습니다.
따라서 본 문서 5~7장(TCAD)은 강좌 범위를 **자기주도로 확장**하기 위한 보충 자료로 읽으면 됩니다.

---

## 2. 강의 목표 해부

### 2.1 원문 목표 문장 분해

> "SiC 및 GaN 기반 전력반도체소자의 핵심 공정이론을 이해하고, wide-bandgap 전력반도체 공정에서 요구되는 **표면처리, 식각, 절연막 형성, 이온주입, 금속 접촉, passivation 및 공정 integration** 기술을 학습한다. 이를 통해 **SiC MOSFET/diode 및 GaN HEMT 전력소자의 제작 공정 흐름과 주요 공정 이슈**를 이해한다."

이 문장은 4개의 역량으로 분해됩니다.

| # | 역량 | 해당 키워드 | 확인 방법(수료 후 스스로 답할 수 있어야 하는 질문) |
|---|---|---|---|
| C1 | **소재 선택의 공학적 근거** | wide-bandgap, FOM | "Si 대비 SiC/GaN이 같은 항복전압에서 왜 1/300~1/1000의 on-resistance를 갖는가?" |
| C2 | **단위 공정(unit process) 물리** | 표면처리, 식각, 절연막, 이온주입, 금속접촉, passivation | "SiC에서 Al 이온주입 후 왜 1600~1700°C 활성화가 필요한가?" |
| C3 | **공정 integration 사고** | integration, 공정 흐름 | "SiC DMOS에서 gate oxide와 self-aligned implant, JTE를 어떤 순서로 쌓는가?" |
| C4 | **공정 이슈 → 소자 특성 → 신뢰성 연결** | 공정 이슈, 결함, 신뢰성 | "계면 트랩(Dit)이 채널 이동도와 Vth 안정성에 어떻게 나타나는가?" |

### 2.2 수강 대상·선수지식 해석

- **"재직자, 대학원생 이상 권장"** → 반도체 8대 공정(포토/에치/증착/산화/확산/이온주입/금속/세정)의 기본 용어를 이미 안다고 가정합니다.
- **"선수과목 없음"** → 반도체물리(밴드갭, 캐리어, PN 접합, MOS)를 모르면 따라가기 어려울 수 있으므로, 아래 4.1~4.2를 사전 학습으로 권장합니다.
- **"중급/이론"** → 수식 유도보다 **공정 조건 ↔ 물리 현상 ↔ 소자 파라미터**의 정성적 인과관계를 묻는 강좌로 이해하는 것이 효율적입니다.

### 2.3 기대 성과(Learning Outcome)

강좌 종료 시점에 다음을 **말로 설명할 수 있으면** 목표 달성입니다.

1. Si / 4H-SiC / GaN의 물성표를 보고 **Baliga FOM**을 계산·해석할 수 있다.
2. SiC 공정이 Si 공정과 근본적으로 다른 **5가지 이유**를 설명할 수 있다.
3. SiC 열산화의 **orientation 의존성**과 **계면 트랩** 문제, 그리고 **nitridation** 해법을 설명할 수 있다.
4. SiC **이온주입 → 고온 활성화 → 캡층** 공정의 순서와 이유를 설명할 수 있다.
5. SiC **ohmic/Schottky 접촉**의 형성 원리와 **TLM** 측정법을 설명할 수 있다.
6. SiC **edge termination**이 항복전압에 미치는 영향을 설명할 수 있다.
7. GaN HEMT의 **2DEG 생성 원리(분극)**와 **normally-off 구현 3가지 방식**을 비교할 수 있다.
8. GaN **passivation / field plate**가 **current collapse**를 억제하는 원리를 설명할 수 있다.
9. SiC/GaN의 **대표 결함**과 **신뢰성 시험 항목(HTGB, HTRB, HTOL, H3TRB, dynamic Ron)**을 매핑할 수 있다.
10. 공정 파라미터를 TCAD로 **DOE/민감도 분석**하는 절차를 설계할 수 있다.

---

## 3. 12시간 커리큘럼 재구성

### 3.1 전체 타임라인 (4개 블록 = 12시간)

| 블록 | 일자 | 시간 | 원문 주제 | 재구성된 학습 모듈 | 핵심 개념 | 권장 산출물 |
|---|---|---|---|---|---|---|
| **D1-AM** | 11/24(화) | 10:00–12:00 (2h) | 전력반도체소자 공정 개요 / WBG 공정 특징 / SiC·GaN 구조·공정 차이 | **M1. 소재·소자·공정의 큰 그림** | FOM, 항복전압-저항 트레이드오프, 수직 vs 수평 소자, SiC/GaN 공정 차이 | 물성 비교표 + FOM 계산 |
| **D1-PM** | 11/24(화) | 13:00–17:00 (4h) | SiC cleaning·표면전처리 / 열산화·계면 / gate oxide·계면개선 / ion implantation / activation·고온열처리 / mesa·trench etching | **M2. SiC 전공정(前端) 6대 단위공정** | Deal-Grove, Dit/NIT, nitridation, box profile, 캡층 활성화, ICP 식각 | SiC 공정 흐름도 1장 |
| **D2-AM** | 12/01(화) | 10:00–12:00 (2h) | ohmic/Schottky contact / diode·MOSFET integration / edge termination·layout | **M3. SiC 후공정 + 소자 integration** | ρc, TLM, 실리사이드, 일함수-배리어, JTE/guard ring/field plate, DMOS/trench | SiC MOSFET 공정 step 표 |
| **D2-PM** | 12/01(화) | 13:00–17:00 (4h) | GaN epi·heterostructure / isolation·gate recess·normally-off / ohmic·gate dielectric / passivation·field plate / SiC·GaN 결함·신뢰성 / integration·특성평가 | **M4. GaN HEMT 전공정 + 신뢰성 + 평가** | 분극/2DEG, MOCVD, p-GaN gate, Ti/Al ohmic, ALD Al₂O₃, SiNx passivation, current collapse, HTGB/HTRB | GaN HEMT 공정 흐름도 + 신뢰성 매트릭스 |

### 3.2 지식 의존성 그래프

```
[반도체물리 기초]
      │
      ▼
[M1] 소재·FOM ──────────────┐
      │                     │
      ▼                     ▼
[M2] SiC 전공정 ──────► [M3] SiC 후공정·integration
      │                     │
      │                     ▼
      │              [SiC diode/MOSFET]
      │
      ▼
[M4] GaN epi/전공정 ──► [GaN HEMT]
      │
      ▼
[공통] 결함·신뢰성·특성평가
```

> **학습 전략**: M2와 M4는 "재료가 다르면 같은 공정도 왜 달라지는가"를 대칭으로 비교하면 기억이 오래갑니다.
> 예) *산화*: SiC는 열산화 가능(단, 계면 나쁨) / GaN은 열산화 불가 → 절연막은 증착(ALD/PECVD).
> 예) *도핑*: SiC는 확산 불가 → implant+고온활성화 / GaN은 epi 도핑(Si, C, Mg)이 주력.

### 3.3 블록별 세부 학습 포인트

#### D1-AM · M1. 소재·소자·공정의 큰 그림

| 세부 주제 | 반드시 이해할 것 | 흔한 오해 |
|---|---|---|
| 전력반도체 공정 개요 | 수직형(전류가 기판 관통) vs 수평형(표면 근처) 구조, 기판/에피/후공정의 역할 | "SiC는 무조건 수직형" (X — 600V급은 수평도 존재) |
| WBG 공정 특징 | 높은 결합에너지 → 낮은 확산계수, 화학적 불활성, 높은 이온화 에너지 | "Si 공정을 그대로 쓰면 된다" (X) |
| SiC vs GaN 구조·공정 차이 | SiC = indirect, 산화막 있음, 수직 소자 유리 / GaN = direct, 이종접합·분극, HEMT(수평) 유리 | "GaN도 열산화로 게이트 산화막을 만든다" (X) |

#### D1-PM · M2. SiC 전공정 6대 단위공정

1. **Wafer cleaning & 표면 전처리** — SPM/SC-1/SC-2/HF, sacrificial oxidation, C-rich residue 제거
2. **열산화 & SiC/SiO₂ 계면** — Si-face/C-face 산화속도 차, C cluster, Dit, NIT
3. **Gate oxide & 계면 개선** — thermal vs deposited, NO/N₂O nitridation, 후처리
4. **Ion implantation** — Al(p)/N,P(n), box profile, high-T implant, 마스크
5. **Dopant activation & 고온 열처리** — 1600–1800°C, 캡층(AlN/C/SiC), surface morphology
6. **Mesa/trench etching** — ICP SF₆/O₂/Ar, hard mask, profile angle, trench corner

#### D2-AM · M3. SiC 후공정 + integration

1. **Ohmic contact** — n-SiC: Ni 실리사이드, p-SiC: Ti/Al, ρc와 TLM
2. **Schottky contact** — Ni/Ti/Pt/W, 배리어 높이, leakage
3. **Diode integration** — SBD / JBS / PiN
4. **MOSFET integration** — planar DMOS vs trench, self-aligned 공정
5. **Edge termination & layout** — JTE, guard ring, field plate, mesa

#### D2-PM · M4. GaN HEMT + 신뢰성 + 평가

1. **Epitaxy & heterostructure** — MOCVD, nucleation/buffer/channel/barrier/cap, 분극 2DEG
2. **Isolation / gate recess / normally-off** — mesa/ion isolation, recess, p-GaN gate, cascode
3. **Ohmic / gate dielectric** — Ti/Al/Ni/Au, recessed ohmic, ALD Al₂O₃, SiNx
4. **Passivation / field plate** — SiNx, surface state, current collapse, FP 설계
5. **결함·신뢰성** — SiC(BPD, stacking fault), GaN(TD, trap), HTGB/HTRB/HTOL/H3TRB
6. **Integration·특성평가** — TLM, C-V, I-V, Hall, SIMS, AFM, TEM, Raman/PL, EL

### 3.4 12시간 → 24시간 자기주도 확장 로드맵

| 주차 | 강좌 진행 | 자기주도 보충(권장 6h/주) |
|---|---|---|
| W1 (11/18–11/23) | 사전 학습 | 반도체물리 복습(PN 접합, MOS C-V), Si/SiC/GaN 물성표 암기, FOM 계산 연습 |
| W2 (11/24–11/30) | D1 수강 | SiC 공정 흐름도 직접 그리기, Deal-Grove 식으로 산화두께 계산, SRIM으로 Al implant profile 1회 실행 |
| W3 (12/01–12/07) | D2 수강 | GaN 2DEG 계산, TCAD로 SiC SBD 1개 / GaN HEMT 1개 시뮬레이션, 퀴즈 대비 문답 |
| W4 (12/08–12/14) | 복습 | 캘리브레이션 개념 정리, 논문 2편(ISPSD/IEDM) 읽고 공정 step 비교 |

---

## 4. 핵심 이론 정리

### 4.1 왜 SiC/GaN인가 — 물성과 FOM

#### 4.1.1 대표 물성 비교 (300 K, 대표값)

| 물성 | Si | 4H-SiC | GaN |
|---|---|---|---|
| 밴드갭 E_g (eV) | 1.12 | 3.26 | 3.4 |
| 임계전계 E_c (MV/cm) | 0.3 | 2.2–3.0 (약 2.5) | 3.3 |
| 전자 이동도 μ_n (cm²/V·s) | 1400 | 900–1000 (bulk) | 1000–2000 (2DEG) |
| 정공 이동도 μ_p (cm²/V·s) | 450 | 100–120 | ~30 |
| 비유전율 ε_r | 11.7 | 9.66 | 9.5 |
| 열전도도 λ (W/cm·K) | 1.5 | 3.7–4.9 | 1.3–1.7 |
| 포화속도 v_sat (10⁷ cm/s) | 1.0 | 2.0 | 2.5 |
| 결정 구조 | Diamond | 4H (hex) | Wurtzite |

#### 4.1.2 이상적 단극 소자의 on-resistance

이상적인 단극(unipolar) 소자의 비온저항은

```
R_on,sp(ideal) = 4 · V_B² / (ε · μ · E_c³)
```

- `V_B`: 항복전압, `ε = ε_r ε_0`, `μ`: 이동도, `E_c`: 임계전계
- 같은 `V_B`에서 `E_c³`에 반비례 → WBG 소재가 압도적으로 유리

**Baliga Figure of Merit (BFOM)**

```
BFOM = ε · μ · E_c³        (Si 대비 상대값으로 표기)
```

| 소재 | BFOM (Si = 1) | 이상적 R_on,sp 비 (Si = 1) | 비고 |
|---|---|---|---|
| Si | 1 | 1 | 기준 |
| 4H-SiC | **~300–400** | ~1/300–1/400 | 상용화 성숙 |
| GaN | **~800–1200** | ~1/800–1/1000 | HEMT/2DEG 기준, 이론값 |

> **실무 포인트**: 이론 FOM은 "소재의 상한"입니다. 실제 소자는 **채널 이동도(계면 산란), 접촉저항, edge termination 효율, 열저항** 때문에 이론값에 도달하지 못합니다. 강좌의 후반부(SiC 계면, GaN passivation)는 결국 **"이론 FOM과 실제 성능의 격차를 줄이는 공정"** 이야기입니다.

#### 4.1.3 전압 등급별 소자 선택 가이드(일반론)

| 항복전압 | Si | SiC | GaN |
|---|---|---|---|
| < 100 V | MOSFET | (경쟁력 낮음) | GaN HEMT (고주파) |
| 100–650 V | IGBT/SJ-MOSFET | SBD/MOSFET (600–650 V) | GaN HEMT (650 V) |
| 650–1200 V | IGBT | **SiC MOSFET/SBD 주력** | GaN HEMT(일부) |
| 1.2–3.3 kV | IGBT | SiC MOSFET | (연구 단계) |
| > 3.3 kV | IGBT/IEGT | SiC, Ga₂O₃(연구) | — |

---

### 4.2 WBG 공정의 5가지 근본 차이 (강좌 전체를 관통하는 뼈대)

| # | 차이 | 물리적 이유 | 공정적 결과 | 강좌 연결 |
|---|---|---|---|---|
| 1 | **도펀트 확산이 거의 없음** | 높은 결합에너지, 낮은 확산계수 | 열확산으로 junction 형성 불가 → **이온주입 + 고온 활성화** 필수 | D1-PM ion implantation/activation |
| 2 | **화학적 불활성** | 강한 Si–C/Ga–N 결합 | 습식식각 난이도↑, **건식식각(ICP)** 의존, 세정제 한계 | D1-PM cleaning/etching |
| 3 | **산화막 계면 품질** | SiC만 열산화 가능, C 잔류 | **Dit/NIT↑ → 채널 이동도↓, Vth 불안정** → nitridation | D1-PM oxidation/gate oxide |
| 4 | **불완전 이온화** | 도펀트 이온화에너지가 kT보다 큼 | 실온에서 캐리어 농도 < 도펀트 농도 → **TCAD/소자 설계 시 필수 고려** | 전 블록 |
| 5 | **이종접합·분극(GaN)** | Wurtzite 구조의 spontaneous/piezoelectric polarization | **2DEG 형성**, 도핑 없이 고이동도 채널 | D2-PM GaN epi |

---

### 4.3 SiC Wafer Cleaning & 표면 전처리

#### 4.3.1 왜 어려운가

- SiC는 **화학적으로 매우 안정** → Si처럼 SC-1/SC-2만으로 유기물·금속·파티클을 완전히 제거하기 어려움
- 표면에 **탄소 잔류물(C-rich layer)**, **산화막**, **스크래치/step bunching**이 남기 쉬움
- 계면 트랩과 누설전류의 **주요 원인**이 표면/계면 오염

#### 4.3.2 표준 세정 시퀀스(대표 예)

| 순서 | 용액 | 목적 | 주의 |
|---|---|---|---|
| 1 | SPM (H₂SO₄:H₂O₂) 또는 piranha | 유기물 제거 | 강산, 안전 |
| 2 | SC-1 (NH₄OH:H₂O₂:H₂O) | 파티클 제거 | SiC 표면은 Si보다 거칠어질 수 있음 |
| 3 | SC-2 (HCl:H₂O₂:H₂O) | 금속 이온 제거 | — |
| 4 | HF (또는 BOE) | 자연산화막 제거 | SiC는 HF 반응이 느림 |
| 5 | **Sacrificial oxidation + HF** | 표면 손상층/탄소 제거 | 산화→식각 반복으로 표면 품질 개선 |
| 6 | 초순수 rinse + N₂ dry | 건조 | water mark 방지 |

#### 4.3.3 표면 전처리 핵심

- **Sacrificial oxidation**: 얇게 열산화 후 HF로 제거 → 표면 결함·오염을 산화막과 함께 걷어냄
- **H-termination / OH-termination**: 후속 원자층 증착(ALD) 전 표면 화학 제어
- **In-situ dry cleaning**(H₂ anneal, HCl) → 산화막 형성 전 계면 청정화
- **Step bunching 제어**: 고온 H₂ anneal 조건에 따라 표면 morphology가 달라짐 → 게이트 산화막 균일도에 직접 영향

> **시뮬레이션 관점**: 세정은 TCAD로 직접 모델링하기 어렵습니다. 대신 **"표면 재결합 속도(surface recombination velocity)"**, **"계면 트랩 밀도(Dit)"**, **"접촉저항"** 같은 **경계조건 파라미터**로 반영합니다.

---

### 4.4 SiC 열산화 & SiC/SiO₂ 계면

#### 4.4.1 Deal-Grove 모델(일반 산화)

```
x_ox² + A·x_ox = B·(t + τ)
```

- `x_ox`: 산화막 두께
- `B`: 포물선 속도 상수(확산 지배, 두꺼운 산화막)
- `B/A`: 선형 속도 상수(계면 반응 지배, 얇은 산화막)
- `τ`: 초기 산화막 보정 시간

SiC도 Deal-Grove 형태를 따르지만 **선형 구간이 매우 짧고**, 결정면에 따라 속도가 크게 다릅니다.

#### 4.4.2 SiC 산화의 특징

| 특징 | 내용 | 공정적 의미 |
|---|---|---|
| **결정면 의존성** | C-face (000-1)가 Si-face (0001)보다 산화속도가 수 배 빠름 | 웨이퍼 off-axis(보통 4°), trench sidewall/bottom의 산화막 두께 불균일 |
| **SiC 소모량** | SiO₂ 1 nm 성장 시 SiC 약 **0.46 nm** 소모 (Si은 ~0.44 nm) | 계면 위치 설계 시 고려 |
| **C 잔류** | 산화 중 탄소가 계면에 축적 → **C cluster, Dit** | 계면 트랩의 주원인 |
| **계면 트랩** | Dit ~ 10¹²–10¹³ cm⁻²eV⁻¹ (E_c 부근) | 채널 이동도 저하, Vth 불안정, SS 증가 |
| **NIT** | Near-Interface Traps (산화막 내 1–2 nm) | 전자 포획 → BTI, hysteresis |

#### 4.4.3 계면 개선: Nitridation

- **NO 또는 N₂O 분위기, 1175–1300°C** 후열처리
- N이 계면의 C 관련 결함을 passivation → **Dit 감소**, **채널 이동도 향상**
- 대표 효과: as-oxidized μ_ch ~ 1–10 cm²/V·s → nitridation 후 **20–40 cm²/V·s** (소자/공정에 따라 상이)
- 부작용: 산화막 내 고정전하 증가, 신뢰성(TDDB) trade-off → **최적화 필요**

#### 4.4.4 Gate oxide 형성 방식 비교

| 방식 | 장점 | 단점 | 사용 |
|---|---|---|---|
| **Thermal oxidation (SiC)** | 계면 품질 상대적 우수, 얇은 EOT 가능 | C 잔류, Dit, 긴 고온 공정 | SiC MOSFET 주류 |
| **Deposited SiO₂ (TEOS/LTO)** | 두께 제어 용이, 저온 | 계면 품질, 밀도 | 캡/절연 |
| **ALD Al₂O₃** | 높은 밴드갭, 낮은 누설, 우수한 계면 | SiC와 격자/열팽창 mismatch, 신뢰성 | GaN MOS, 연구 |
| **ALD HfO₂ / high-k** | 높은 k, EOT 감소 | 결정화, 계면층, 신뢰성 | 연구/특수 |

---

### 4.5 SiC 이온주입 (Ion Implantation)

#### 4.5.1 도펀트 선택

| 도펀트 | 전도형 | 이온화에너지(대표) | 비고 |
|---|---|---|---|
| **Al** | p-type | E_a ~ 0.19–0.24 eV | 가장 일반적, 고온 활성화 필요 |
| **B** | p-type | E_a ~ 0.3 eV | 깊은 준위, 활성화 어려움 |
| **N** | n-type | E_a ~ 0.05–0.1 eV | 상대적으로 낮은 활성화 온도 |
| **P** | n-type | E_a ~ 0.06–0.1 eV | N 대체, 깊은 implantation |

#### 4.5.2 공정 특징

- **Box profile**: 여러 에너지의 다중 주입으로 균일한 농도 프로파일 형성
  - 예) n⁺ source: 30/60/120 keV × 3단계 → 0.1–0.3 µm box
- **High-temperature implantation (300–600°C)**: 손상 회복, 비정질화 억제, 채널링 감소
- **Channeling**: <0001> 방향 정렬 주입 시 깊은 tail → off-axis(4°) 또는 tilt 주입
- **마스크**: photoresist만으로는 부족(고에너지/고도즈) → **SiO₂ 1–3 µm, poly-Si, Ni, Al** 하드마스크
- **도즈 범위**: 10¹²–10¹⁶ cm⁻² (source/drain은 10¹⁹–10²⁰ cm⁻³ 목표)

#### 4.5.3 프로파일 해석

- **Gaussian 근사**: `N(x) = (Φ/(√(2π)ΔR_p)) · exp(-(x-R_p)²/(2ΔR_p²))`
  - `Φ`: 도즈, `R_p`: projected range, `ΔR_p`: straggle
- **Monte Carlo**: 결정 채널링·다중산란·손상까지 고려 (Sentaurus MC, Crystal-TRIM)
- **SRIM/TRIM**: 빠른 1D range/straggle 확인용 (무료)

> **TCAD 팁**: SiC는 **채널링과 손상**이 중요하므로 analytic Gaussian보다 **Monte Carlo implant 모델**이 실제와 잘 맞습니다. 다만 계산 비용이 크므로, 1D 캘리브레이션 후 analytic 모델로 2D/3D에 적용하는 워크플로가 실용적입니다.

---

### 4.6 Dopant Activation & 고온 열처리

#### 4.6.1 왜 1600–1800°C인가

- SiC는 **도펀트 확산이 거의 없고**, 주입 손상 회복과 **격자 자리 치환(activation)** 에 높은 에너지가 필요
- Al: **1600–1700°C**, N/P: **1600–1800°C** (대표값)
- 활성화율: Al ~50–90%, N ~80–95% (조건 의존)

#### 4.6.2 캡층(Cap layer)이 필요한 이유

- 고온에서 SiC 표면이 **Si 승화/분해** → 표면 거칠어짐, step bunching
- 해결: **AlN, carbon(C), poly-Si, SiC** 캡층
  - AlN: 우수한 보호, 이후 제거 필요
  - Carbon: 저렴·우수, 제거(O₂ plasma) 필요
  - SiC cap: epitaxial 재성장

#### 4.6.3 열처리 장비/조건

| 항목 | 대표값 |
|---|---|
| 장비 | RTA (halogen lamp), 고온 furnace, microwave anneal(연구) |
| 온도 | 1600–1800°C |
| 시간 | 30 s – 5 min (RTA), 수십 분(furnace) |
| 분위기 | Ar, N₂, SiH₄(과압) |
| Susceptor | Graphite, TaC-coated graphite |
| 승온/냉온 | 50–100°C/s (RTA), 급냉 |

#### 4.6.4 주요 이슈

- **Surface roughness / step bunching** → 후속 산화막 균일도, 리소그래피
- **Dopant out-diffusion** (미미하지만 표면 결함)
- **Activation efficiency**: 도즈가 너무 높으면 clustering/Precipitation → 활성화율 포화
- **Residual damage**: TEM/Raman으로 확인

---

### 4.7 SiC Mesa / Trench Etching

#### 4.7.1 건식식각 기본

- **ICP-RIE** (Inductively Coupled Plasma) 주류
- 가스: **SF₆ (F 공급) + O₂ (sidewall passivation) + Ar (physical sputtering)**
- SiC는 결합에너지가 커서 **높은 bias power / 높은 밀도 플라즈마** 필요
- 식각 속도: 0.3–1 µm/min (장비/조건 의존)
- 선택비: SiC/SiO₂, SiC/Ni

#### 4.7.2 마스크

| 마스크 | 선택비 | 비고 |
|---|---|---|
| Photoresist | 낮음 | 얕은 식각만 |
| SiO₂ | 중간 | 일반적 |
| **Ni** | 높음 | 깊은 trench, hard mask |
| Al | 중간 | — |

#### 4.7.3 형상 제어

- **Sidewall angle**: 80–90° (trench MOSFET은 수직에 가깝게)
- **Trench corner rounding**: 전계 집중 완화 → **둥근 corner**가 항복에 유리
- **RIE lag**: 좁은 패턴이 느리게 식각 → 균일도 문제
- **Microtrenching / notching**: 하부 corner 과식각 → 공정 최적화 필요
- **Etch damage**: 표면 결함 → 후속 산화/계면 특성 악화 → **sacrificial oxidation**으로 제거

#### 4.7.4 도핑 의존 식각속도

- p/n 도핑 농도에 따라 식각속도가 달라질 수 있음 → **endpoint 검출** 및 **공정 편차** 관리 중요

---

### 4.8 SiC Ohmic / Schottky Contact

#### 4.8.1 접촉저항의 정의

```
ρ_c = R_c · A_c   [Ω·cm²]
R_T = (R_sh / W) · (L + 2L_T),   L_T = √(ρ_c / R_sh)
```

- `R_T`: TLM 패턴 간 총저항, `R_sh`: sheet resistance, `L`: 전극 간격, `L_T`: transfer length
- **TLM(Transmission Line Method)** 으로 `R_T` vs `L` plot → 절편에서 `2R_c`, 기울기에서 `R_sh`, `L_T` → `ρ_c`

#### 4.8.2 Ohmic contact

| 기판 | 금속 스택 | 열처리 | ρ_c (대표) | 비고 |
|---|---|---|---|---|
| n-SiC | **Ni** (30–100 nm) | 950–1050°C, 30–120 s, RTA | 1×10⁻⁶–1×10⁻⁵ Ω·cm² | Ni₂Si 실리사이드 형성 |
| n-SiC | Ti/Al, Ti/Ni/Ag | 900–1000°C | 10⁻⁵–10⁻⁶ | Al 확산, 계면 |
| p-SiC | **Ti/Al** | 900–1000°C | 10⁻⁵–10⁻⁴ | 높은 일함수 금속 필요 |
| p-SiC | Ni, Pt | — | 상대적으로 열등 | — |

**원리**: 강하게 도핑된 표면(>10¹⁹ cm⁻³)에서 **터널링**이 지배 → 배리어 높이보다 **도핑 농도와 계면 상태**가 ρc를 결정.

#### 4.8.3 Schottky contact

| 금속 | n-SiC 배리어 높이(대표) | 특징 |
|---|---|---|
| Ti | ~1.0–1.1 eV | 낮은 배리어, 누설↑ |
| Ni | ~1.3–1.5 eV | 표준, 안정 |
| Pt | ~1.4–1.6 eV | 높은 배리어, 고온 안정 |
| W | ~1.3 eV | 고온 공정 |

- **이상계수(ideality factor) n**: 1.0–1.2 (양호), 결함/계면 상태 시 증가
- **누설전류**: 배리어 낮아짐, edge field 집중, 결함 → **edge termination**과 **표면 passivation**이 중요
- **열적 안정성**: 고온 동작 시 금속/계면 반응 → 배리어 변화

---

### 4.9 Edge Termination & Layout

| 방식 | 원리 | 장점 | 단점 |
|---|---|---|---|
| **Mesa** | 물리적 식각으로 표면 전계 완화 | 단순 | 식각 손상, 신뢰성 |
| **JTE (Junction Termination Extension)** | 저농도 이온주입으로 공핍 확장 | 높은 효율, 표준 | 도즈 최적화 민감 |
| **Guard Ring** | 다중 floating ring으로 전계 분산 | 설계 유연 | 면적 증가 |
| **Field Plate** | 절연막 위 금속으로 전계 분산 | 효과적, SiC MOSFET에 흔함 | 절연막 신뢰성 |
| **Bevel** | 경사 식각 | 고전압 | 공정 복잡 |

**설계 고려사항**
- **JTE 도즈 최적화**: 너무 낮으면 공핍 부족, 너무 높으면 표면 전계 집중 → **시뮬레이션으로 최적 도즈 탐색**
- **Field plate 길이/절연막 두께**: 전계 분산 vs 절연막 breakdown
- **Layout**: corner rounding, termination 폭, bonding pad 배치, 열 분산

---

### 4.10 SiC Diode / MOSFET Integration

#### 4.10.1 Diode 종류

| 소자 | 구조 | 특징 |
|---|---|---|
| **SBD (Schottky Barrier Diode)** | 금속/n-SiC | 낮은 Vf, 빠른 스위칭, 누설↑ |
| **JBS (Junction Barrier Schottky)** | p⁺ grid + Schottky | 누설↓, 항복↑ |
| **PiN** | p⁺/n⁻/n⁺ | 높은 항복, 느린 스위칭 |

#### 4.10.2 SiC MOSFET 구조

| 구조 | 특징 | 장단점 |
|---|---|---|
| **Planar DMOS** | 수평 채널, self-aligned | 공정 성숙, 채널 이동도 낮음, JFET 저항 |
| **Trench MOSFET** | 수직 채널 | 채널 밀도↑, Ron↓, trench corner 전계 집중 |
| **Double Trench** | gate trench + source trench | 게이트 산화막 전계 완화, 신뢰성↑ |

#### 4.10.3 대표 공정 흐름 (Planar DMOS, 개념도)

| Step | 공정 | 핵심 파라미터 | 이슈 |
|---|---|---|---|
| 1 | n⁺ substrate + n⁻ epi | epi 두께/농도 (BV 결정) | epi 결함 |
| 2 | JTE implant (Al) | 도즈/에너지 | termination 최적화 |
| 3 | P-body implant (Al) | 도즈/에너지 | 채널 농도 |
| 4 | N⁺ source implant (N/P) | 고도즈 | self-align |
| 5 | P⁺ contact implant | 고도즈 | 접촉저항 |
| 6 | Activation anneal | 1600–1700°C, 캡층 | 표면 morphology |
| 7 | Gate oxidation | 두께, NO anneal | Dit, TDDB |
| 8 | Poly-Si gate | 도핑, patterning | 일함수 |
| 9 | Interlayer dielectric | SiO₂/SiN | — |
| 10 | Ohmic contact | Ni (n), Ti/Al (p) | ρc |
| 11 | Passivation | SiN/SiO₂ | 신뢰성 |
| 12 | Pad/후공정 | — | — |

> **Integration 핵심**: 각 단계의 **열예산(thermal budget)** 이 뒤 단계에 영향을 줍니다. 예) 고온 활성화 후 형성된 계면/도핑 프로파일이 gate oxidation에 영향, contact anneal이 passivation에 영향.

---

### 4.11 GaN HEMT Epitaxy & Heterostructure

#### 4.11.1 2DEG 생성 원리

- AlGaN/GaN 이종접합에서 **spontaneous polarization + piezoelectric polarization** 차이 → 계면에 **양전하** → 전자 accumulation → **2DEG**
- 도펀트 없이 **10¹³ cm⁻²** 수준의 2DEG 형성
- 2DEG 밀도(근사):

```
n_s ≈ σ_pol/q − (ε/(q·d)) · (φ_b + E_F − ΔE_c)
```

- `σ_pol`: 분극 전하, `d`: AlGaN 두께, `φ_b`: gate barrier, `ΔE_c`: conduction band offset

#### 4.11.2 MOCVD 성장 구조 (대표)

| Layer | 재료 | 두께(대표) | 역할 |
|---|---|---|---|
| Substrate | Si, SiC, sapphire | — | 지지, 열전도 |
| Nucleation | AlN | 20–200 nm | 격자 mismatch 완화 |
| Buffer | AlGaN/GaN superlattice, C-doped GaN | 1–5 µm | 누설 억제, stress 관리 |
| Channel | GaN | 0.5–2 µm | 2DEG 채널 |
| Barrier | AlGaN (Al 20–30%) | 15–30 nm | 2DEG 형성 |
| Cap | GaN/SiN | 1–5 nm | 표면 보호 |

#### 4.11.3 주요 이슈

- **Lattice mismatch / thermal mismatch** → threading dislocation, wafer bow
- **C doping** (buffer): 누설 억제 vs **current collapse** trade-off
- **Al 조성/두께**: 2DEG 밀도와 Vth 결정
- **Surface donor state**: 표면 상태가 2DEG에 영향

---

### 4.12 GaN Isolation / Gate Recess / Normally-off

#### 4.12.1 Isolation

| 방식 | 내용 | 비고 |
|---|---|---|
| **Mesa isolation** | ICP (Cl₂/BCl₃) 식각 | 단순, step coverage 이슈 |
| **Ion isolation** | N, Ar, F implant | 평탄, 손상/누설 |
| **Implant + anneal** | 손상 회복 | 공정 복잡 |

#### 4.12.2 Normally-off 구현

| 방식 | 원리 | Vth | 장단점 |
|---|---|---|---|
| **p-GaN gate** | gate 아래 p-GaN으로 2DEG depletion | +1–2 V | 상용 주류, gate 신뢰성 |
| **Recessed MIS** | AlGaN 식각 후 절연막/gate | +1–3 V | Vth 제어, 계면/신뢰성 |
| **Cascode** | depletion HEMT + Si MOSFET | — | 단순, 시스템 복잡 |
| **Fluorine implant** | F로 depletion | + | 안정성 이슈 |

---

### 4.13 GaN Ohmic / Gate Dielectric

#### 4.13.1 Ohmic

| 금속 스택 | 열처리 | ρ_c (대표) | 비고 |
|---|---|---|---|
| **Ti/Al/Ni/Au** | 800–900°C, 30 s, N₂ | 10⁻⁶–10⁻⁵ Ω·cm² | 표준 |
| Ti/Al/Ti/Au | 800–900°C | 유사 | Au-free 호환 |
| Recessed ohmic | AlGaN 식각 후 증착 | 더 낮음 | contact resistance↓ |

- **Ti**가 AlGaN과 반응 → **TiN** 형성 → 낮은 배리어/터널링
- **Au**: 산화 방지, 하지만 CMOS 호환성 문제 → **Au-free** 스택 연구

#### 4.13.2 Gate dielectric

| 재료 | 증착 | 특징 |
|---|---|---|
| **ALD Al₂O₃** | ALD | 높은 밴드갭(~7 eV), 낮은 누설, 우수한 계면 |
| **PECVD SiNx** | PECVD | passivation 겸용, 계면 상태 |
| **SiO₂** | PECVD/ALD | 낮은 k, 누설 |
| **High-k (HfO₂ 등)** | ALD | EOT↓, 신뢰성 연구 |

---

### 4.14 GaN Passivation & Field Plate

#### 4.14.1 Current Collapse / Dynamic Ron

- **원인**: 표면/버퍼 트랩에 전자가 포획 → 2DEG 부분 depletion → on-resistance 증가
- **대책**: **SiNx passivation**, **field plate**, buffer doping 최적화, 표면 처리

#### 4.14.2 Passivation

| 재료 | 두께(대표) | 특징 |
|---|---|---|
| **PECVD SiNx** | 100–200 nm | 표준, 계면 상태 제어 |
| SiO₂/SiNx stack | — | 추가 보호 |
| ALD Al₂O₃ | 10–50 nm | 우수한 계면, 얇음 |

#### 4.14.3 Field Plate

- **source-connected field plate**: gate-drain 사이 전계 분산 → **peak field 감소** → 항복/신뢰성 향상
- 설계 변수: FP 길이(0.5–3 µm), 절연막 두께(100–500 nm), FP와 gate 간격
- **trade-off**: FP 길이↑ → Cgd↑ → 스위칭 손실↑

---

### 4.15 공정 결함 & 신뢰성

#### 4.15.1 SiC 대표 결함

| 결함 | 설명 | 영향 |
|---|---|---|
| **BPD (Basal Plane Dislocation)** | 기판/epi 결함 | **Bipolar degradation** (PiN/MOSFET body diode) |
| **Stacking Fault** | stacking order 결함 | Ron 증가, 특성 열화 |
| **Triangular / Carrot defect** | epi 성장 결함 | 소자 불량, 누설 |
| **Micropipe** | 관통 hole | 치명적 불량 (최근 감소) |
| **Interface trap (Dit)** | SiO₂/SiC 계면 | 이동도, Vth, 신뢰성 |

#### 4.15.2 GaN 대표 결함

| 결함 | 설명 | 영향 |
|---|---|---|
| **Threading Dislocation (TD)** | 격자 mismatch | 누설, 이동도, 신뢰성 |
| **Point defect / trap** | C, N vacancy, impurity | current collapse, dynamic Ron |
| **Surface trap** | passivation 계면 | Vth drift, hysteresis |
| **Crack** | thermal mismatch | 불량 |

#### 4.15.3 신뢰성 시험 매트릭스

| 시험 | 조건(대표) | 평가 항목 |
|---|---|---|
| **HTGB** (High Temperature Gate Bias) | 150–175°C, Vgs stress | Vth shift, gate leakage |
| **HTRB** (High Temperature Reverse Bias) | 150–175°C, 80% BV | 누설, 항복 |
| **HTOL** (High Temperature Operating Life) | 125–175°C, 동작 | 수명 |
| **H3TRB** (High Humidity High Temp Reverse Bias) | 85°C/85% RH | 습도 신뢰성 |
| **TDDB** | gate stress | oxide lifetime |
| **Dynamic Ron** | switching stress | current collapse |
| **Thermal cycling** | ΔT | 패키지/접합 |

---

### 4.16 특성평가 (Characterization)

| 기법 | 측정 대상 | 활용 |
|---|---|---|
| **TLM** | 접촉저항 ρc, sheet resistance | ohmic 최적화 |
| **C-V** | 산화막 두께, doping, Vfb, Dit | gate oxide/계면 |
| **I-V** | Vf, Ron, 누설, 항복 | 소자 성능 |
| **Hall** | 이동도, sheet carrier density | epi/2DEG |
| **SIMS** | 도펀트 프로파일 | implant/활성화 |
| **AFM** | 표면 거칠기 | 산화/식각/anneal |
| **TEM** | 계면/결함 | 계면 구조 |
| **Raman / PL** | 결정 품질, stress, 결함 | epi/공정 손상 |
| **EL (Electroluminescence)** | 결함 발광 | 소자 결함 위치 |

---

## 5. 설계·해석 프로그램(TCAD) 지형도

### 5.1 전체 지형도

```
[공정 시뮬레이션]                [소자 시뮬레이션]              [회로/시스템]
Sentaurus SProcess               Sentaurus SDevice              LTspice / ngspice
Silvaco Victory Process          Silvaco Victory Device/Atlas   PLECS / SIMetrix
Cogenda VisualTCAD               Cogenda Genius                 Saber
       │                               │
       └──────────► 구조(.tdr/.str) ────┘
                                       │
                              [열/신뢰성/패키지]
                              COMSOL, Ansys Icepak, FloTHERM
                                       │
                              [레이아웃/설계]
                              Cadence Virtuoso, L-Edit, KLayout
```

### 5.2 상용 TCAD 비교

| 구분 | **Synopsys Sentaurus** | **Silvaco Victory/Atlas** | **Cogenda VisualTCAD** |
|---|---|---|---|
| 공정 | SProcess | Victory Process, Athena | VictoryProcess 유사 |
| 소자 | SDevice | Victory Device, Atlas | Genius |
| GUI/스크립트 | SWB, SDE, SVisual | DeckBuild, TonyPlot | VisualTCAD GUI |
| DOE | Sentaurus Workbench | Victory DoE | 지원 |
| 강점 | 업계 표준, 정확도, 다물리 | 사용 편의, 가격, 교육용 | GUI 친화, 상대적 저가 |
| 약점 | 라이선스 비용, 학습곡선 | 모델 정확도 편차 | 생태계/문헌 |
| SiC/GaN 지원 | 우수 (parameter set) | 우수 | 양호 |

### 5.3 Sentaurus 구성요소

| 도구 | 역할 |
|---|---|
| **SWB (Sentaurus Workbench)** | 프로젝트/실험 관리, DOE, 파라미터 스윕, 결과 비교 |
| **SDE (Sentaurus Device Editor)** | 구조 생성(geometry, doping, mesh) |
| **SProcess** | 공정 시뮬레이션(산화, 확산, implant, etch, deposit) |
| **SDevice** | 소자 물리 시뮬레이션 |
| **SVisual / Inspect** | 시각화, curve 추출 |
| **SPDB (Sentaurus Parameter Database)** | 재료/모델 파라미터 DB |

### 5.4 Silvaco 구성요소

| 도구 | 역할 |
|---|---|
| **DeckBuild** | 통합 실행 환경(deck 편집/실행) |
| **Victory Process** | 공정 시뮬레이션 |
| **Victory Device / Atlas** | 소자 시뮬레이션 |
| **TonyPlot** | 시각화 |
| **Victory DoE** | DOE/최적화 |
| **Athena** | 전통 공정 시뮬레이터 |

### 5.5 무료/오픈소스 대안

| 도구 | 용도 | 비고 |
|---|---|---|
| **DEVSIM** | 오픈소스 소자 시뮬레이터 | Python/C++ 기반, 교육용 |
| **Charon (Sandia)** | 오픈소스 TCAD | 반도체 소자 |
| **ngspice / LTspice** | 회로 시뮬레이션 | 소자 compact model |
| **KLayout** | 레이아웃 뷰어/편집 | GDS 처리 |
| **SRIM/TRIM** | 이온주입 range/straggle | 무료 |
| **COMSOL (Semiconductor Module)** | 다물리/열/전계 | 상용, 유연 |

### 5.6 무엇을 언제 쓰는가 (의사결정 트리)

```
"공정 조건이 소자에 미치는 영향을 보고 싶다"
   ├─ 도핑/산화/식각 profile → SProcess / Victory Process
   ├─ 전기 특성(I-V, C-V, BV) → SDevice / Victory Device
   ├─ 공정-소자 연동 → SProcess → SDevice (구조 전달)
   ├─ 최적화/민감도 → SWB / Victory DoE
   └─ 회로/스위칭 → LTspice + compact model
```

---

## 6. 초기 설정(Initial Setup) 상세

> 아래는 **Sentaurus**와 **Silvaco**를 기준으로 한 실무 절차입니다. 다른 툴도 개념은 동일합니다.

### 6.1 환경·라이선스·디렉터리

1. **라이선스 확인**: FlexLM/서버, 환경변수(`LM_LICENSE_FILE`, `SNPSLMD_LICENSE_FILE`)
2. **환경변수**: `PATH`, `STROOT`(Sentaurus), `SILVACO`(Silvaco)
3. **작업 디렉터리 구조** (권장):

```
project/
├── 00_docs/          # 요구사항, 회의록
├── 01_process/       # SProcess / Victory Process deck
├── 02_device/        # SDevice / Victory Device deck
├── 03_structures/    # .tdr / .str
├── 04_results/       # I-V, C-V, log
├── 05_calibration/   # 측정 vs 시뮬 비교
├── 06_doe/           # DOE 결과
└── 07_reports/       # 최종 리포트
```

4. **버전 관리**: deck, parameter, 결과 요약을 Git으로 관리(대용량 .tdr는 LFS 또는 별도)

### 6.2 재료 파라미터 DB 구축 (가장 중요)

TCAD 정확도의 80%는 **재료 파라미터**에서 결정됩니다. 최소한 아래를 명시적으로 관리하세요.

#### 6.2.1 4H-SiC 기본 파라미터 (대표값)

| 파라미터 | 값 | 비고 |
|---|---|---|
| E_g | 3.26 eV | 온도 의존성 |
| χ (electron affinity) | 3.7–4.0 eV | 문헌 편차 |
| ε_r | 9.66 | anisotropy |
| N_c (300K) | ~1.7×10¹⁹ cm⁻³ | effective DOS |
| N_v (300K) | ~2.5×10¹⁹ cm⁻³ | |
| μ_n (bulk) | 900–1000 cm²/V·s | doping dependence |
| μ_p | 100–120 cm²/V·s | |
| E_c | 2.2–3.0 MV/cm | doping/orientation |
| v_sat | 2×10⁷ cm/s | |
| Al acceptor E_a | 0.19–0.24 eV | **incomplete ionization** |
| N donor E_a | 0.05–0.1 eV | **incomplete ionization** |
| Impact ionization | Okuto-Crowell /  anisotropy | **필수** |

#### 6.2.2 GaN 기본 파라미터 (대표값)

| 파라미터 | 값 | 비고 |
|---|---|---|
| E_g | 3.4 eV | |
| χ | 4.1 eV | |
| ε_r | 9.5 | |
| μ_n (2DEG) | 1500–2000 cm²/V·s | |
| μ_p | ~30 cm²/V·s | |
| E_c | 3.3 MV/cm | |
| v_sat | 2.5×10⁷ cm/s | |
| Polarization | spontaneous + piezoelectric | **필수** |
| AlGaN bandgap | 3.4 + 2.8x (x=Al fraction) | 근사 |

> **주의**: SiC/GaN은 **anisotropy**(결정 방향에 따른 이동도/항복/열전도)가 큽니다. 2D 단면 시뮬레이션에서는 **c-axis 방향**을 명시하고, 필요 시 anisotropic 모델을 켜야 합니다.

### 6.3 메쉬 전략

| 영역 | 메쉬 크기(권장) | 이유 |
|---|---|---|
| Bulk 기판 | 0.5–2 µm | 계산량 절감 |
| Epi layer | 50–200 nm | 도핑 gradient |
| Junction/공핍 영역 | 5–20 nm | 전계/캐리어 정확도 |
| Gate oxide | **0.2–0.5 nm** | 터널링/신뢰성 |
| SiC/SiO₂ interface | 0.2–0.5 nm | Dit/계면 |
| AlGaN/GaN interface | 0.2–0.5 nm | 2DEG |
| Trench corner | 1–5 nm | 전계 집중 |
| Contact edge | 5–10 nm | 전류 집중 |

**메쉬 원칙**
- **도핑 농도 변화율**이 큰 곳에 집중
- **전계 집중/터널링/계면**에 집중
- 너무 조밀하면 **수렴 실패/계산시간 폭증** → 단계적 refinement
- **mesh adapt** (SDevice) 또는 **grading** 활용

### 6.4 물리 모델 선택 체크리스트

| 현상 | 모델 | SiC | GaN | 비고 |
|---|---|---|---|---|
| 재결합 | SRH, Auger | ✅ | ✅ | lifetime 설정 |
| 불완전 이온화 | Incomplete ionization | ✅ | △ | SiC 필수 |
| 도핑 의존 이동도 | Doping dependence | ✅ | ✅ | |
| 고전계 이동도 | High-field saturation | ✅ | ✅ | |
| 충돌 이온화 | Impact ionization | ✅ | ✅ | **항복 필수** |
| 밴드갭 narrowing | BGN | ✅ | ✅ | 고도핑 |
| 터널링 | Tunneling (band-to-band, trap-assisted) | ✅ | ✅ | 누설 |
| 계면 트랩 | Interface traps (Dit) | ✅ | ✅ | SiC/GaN 핵심 |
| 분극 | Polarization | — | ✅ | **2DEG 필수** |
| 이종접합 | Heterointerface | — | ✅ | |
| 열 | Thermodynamic / self-heating | ✅ | ✅ | Ron/열 |
| Poole-Frenkel | PF emission | △ | ✅ | 절연막 누설 |

### 6.5 경계조건·전극·접촉

| 항목 | 설정 |
|---|---|
| Ohmic | 저항성 접촉(ρc 지정) 또는 ideal |
| Schottky | barrier height 또는 work function |
| Gate | work function, 절연막 두께 |
| Substrate | back contact (수직 소자) |
| Passivation | fixed charge, interface trap |
| Thermal | 열저항/heat sink |

**접촉 설정 시 주의**
- **Schottky 배리어는 금속 일함수가 아니라 "실측 배리어"** 로 캘리브레이션하는 것이 일반적
- **ρc**는 TLM 실측값을 입력
- **fixed charge**는 C-V 평탄전압 shift로 캘리브레이션

### 6.6 수치해석 설정·수렴

| 항목 | 권장 |
|---|---|
| Solver | Newton (기본), Gummel (초기) |
| 초기 해 | equilibrium → bias ramp |
| Bias ramp | 작은 step (0.05–0.2 V), quasistationary |
| Damping | 자동/수동, 필요 시 감소 |
| Mesh adapt | 전계/캐리어 gradient 기준 |
| 항복 해석 | current-controlled sweep, compliance |
| Transient | 작은 time step, BDF |
| AC | small-signal, 주파수 스윕 |

**수렴 실패 시 체크**
1. 메쉬 품질(급격한 doping/gradient)
2. 초기 해(equilibrium) 문제
3. 물리 모델 충돌(예: incomplete ionization + high doping)
4. bias step 과대
5. 접촉/경계조건 오류

### 6.7 Sentaurus 입력 골격(개념)

**SProcess (예시 구조)**

```
# 4H-SiC wafer 정의
# - 기판 doping, epi 두께/농도
# - 초기 메쉬
# - 산화/증착/식각/implant/anneal
# - 최종 구조 저장
```

**SDevice (예시 구조)**

```
File { Grid = "..." ; Plot = "..." ; }
Electrode { name="source" ... ; name="gate" ... ; name="drain" ... ; }
Physics {
  Fermi
  Recombination(SRH Auger)
  IncompleteIonization
  Mobility(DopingDependence HighFieldSaturation)
  ImpactIonization
  BandgapNarrowing
  Tunneling
  InterfaceTraps(...)
}
Math { Newton; Iterations=...; RelErr=...; }
Solve {
  Poisson
  Coupled { Poisson Electron Hole }
  Quasistationary(...) { ... }
}
```

> 실제 문법은 버전별로 다르므로 **설치된 버전의 예제(Applications Library)** 를 기준으로 복사·수정하는 것이 가장 안전합니다.

### 6.8 Silvaco Deck 골격(개념)

**Victory Process / Athena**

```
go victoryprocess
# mesh, region, material
# deposit / etch / implant / diffuse / oxidize
# save structure
```

**Victory Device / Atlas**

```
go victorydevice
mesh ...
region ...
electrode ...
doping ...
material ...
models srh auger fldmob impact incomplete bgn tunnl
method newton
solve init
solve vdrain=...
log ...
save ...
tonyplot ...
```

### 6.9 재현성·문서화

- **deck + parameter + 결과 요약**을 한 세트로 버전 관리
- 각 시뮬레이션에 **가정/출처/캘리브레이션 근거**를 주석으로 기록
- **DOE 조건표**와 **결과 요약 CSV**를 함께 저장
- 결과는 **그래프 + 표 + 결론**으로 정리(재현 가능성 확보)

---

## 7. 분석 절차(Analysis Workflow) 상세

### 7.1 전체 7단계 파이프라인

```
1. 요구사항 정의 (BV, Ron, Vth, Id, 주파수)
      ↓
2. 1D 해석적 sanity check (FOM, depletion width, oxide field)
      ↓
3. 공정 시뮬레이션 (SProcess)
      ↓
4. 구조/도핑 검증 (SIMS, SRP, TEM 비교)
      ↓
5. 소자 시뮬레이션 (SDevice)
      ↓
6. 특성 추출 (C-V, I-V, BV, TLM, 스위칭)
      ↓
7. 측정 비교·캘리브레이션 → DOE/민감도 → 신뢰성 → 문서화
```

### 7.2 예제 A: 1.2 kV 4H-SiC SBD

#### 7.2.1 목표

| 항목 | 목표 |
|---|---|
| BV | ≥ 1.2 kV |
| Vf | ≤ 1.5 V @ 10 A |
| 누설 | ≤ 1 µA/cm² @ 1.2 kV |
| ρc | ≤ 1×10⁻⁵ Ω·cm² |

#### 7.2.2 공정 시뮬레이션

1. n⁺ substrate + n⁻ epi (10 µm, 5×10¹⁵ cm⁻³)
2. JTE implant (Al, 1×10¹³–1×10¹⁴ cm⁻², 0.5–1 µm)
3. Activation 1650°C, 30 min (AlN cap)
4. Field oxide / passivation
5. Schottky metal (Ni, 100 nm)
6. Anneal (400–500°C, 선택)

#### 7.2.3 소자 시뮬레이션

- **I-V forward**: Vf, Ron,sp, ideality factor
- **I-V reverse**: BV, leakage
- **전계 분포**: JTE 유무/도즈별 peak field
- **온도 의존성**: 25–175°C

#### 7.2.4 추출·검증

- BV @ 1 mA/cm²
- Ron,sp = ΔV/ΔJ
- 누설 vs reverse bias
- JTE 도즈 sweep → BV 최대점

### 7.3 예제 B: 1.2 kV 4H-SiC Planar DMOS

#### 7.3.1 구조 파라미터(대표)

| 항목 | 값 |
|---|---|
| Epi 두께/농도 | 10–12 µm / 5×10¹⁵ cm⁻³ |
| P-body | Al, 1×10¹⁷ cm⁻³, 0.5–1 µm |
| N⁺ source | 1×10¹⁹–10²⁰ cm⁻³ |
| P⁺ contact | 1×10¹⁹ cm⁻³ |
| Gate oxide | 40–50 nm |
| Channel length | 0.5–1 µm |
| JTE | Al, 1×10¹³–10¹⁴ cm⁻² |

#### 7.3.2 시뮬레이션 항목

| 항목 | 방법 | 주의 |
|---|---|---|
| Vth | Id-Vg linear extrapolation | interface trap 영향 |
| μ_ch | Id-Vg peak transconductance | Dit, Coulomb scattering |
| Ron,sp | Id-Vd @ Vgs=15–20 V | self-heating |
| BV | Id=1 mA/cm² | edge termination |
| Oxide field | peak E_ox | < 3 MV/cm |
| C-V | gate C-V | Cox, Vfb, Dit |
| Switching | Cgd/Cgs, Qg | field plate |
| Thermal | Rth, self-heating | 열모델 |

#### 7.3.3 캘리브레이션

- **C-V**: Cox(산화막 두께), Vfb(fixed charge), Dit(Terman/conductance)
- **Id-Vg**: Vth, SS, μ_ch
- **Id-Vd**: Ron
- **BV**: JTE 도즈/계면 전하
- **TLM**: ρc

### 7.4 예제 C: 650 V GaN HEMT

#### 7.4.1 구조

| Layer | 값 |
|---|---|
| Buffer | C-doped GaN, 3–5 µm |
| Channel | GaN, 1–2 µm |
| Barrier | Al₀.₂₅Ga₀.₇₅N, 20–25 nm |
| Cap | GaN, 1–2 nm |
| Passivation | SiNx, 100–200 nm |
| Field plate | source-connected, 1–3 µm |

#### 7.4.2 시뮬레이션 항목

| 항목 | 방법 | 주의 |
|---|---|---|
| 2DEG | polarization model | ns ~1×10¹³ cm⁻² |
| Vth | Id-Vg | normally-off |
| Ron | Id-Vd | dynamic Ron |
| BV | Id=1 mA/mm | field plate |
| C-V | gate C-V | AlGaN 두께 |
| Current collapse | transient/pulse | trap model |
| Thermal | self-heating | GaN 열전도 |

#### 7.4.3 GaN 특유의 주의점

- **Polarization charge**를 명시적으로 설정해야 2DEG가 형성됨
- **AlGaN/GaN heterointerface** 메쉬 조밀
- **Buffer trap (C-doped)** 모델 → dynamic Ron
- **Surface trap / passivation** → current collapse
- **Self-heating**이 SiC보다 심각(열전도 낮음)

### 7.5 C-V 분석

| 단계 | 내용 |
|---|---|
| 1 | MOS C-V 측정(주파수, 온도) |
| 2 | Accumulation capacitance → **Cox** → 산화막 두께 |
| 3 | Depletion slope → **doping profile** |
| 4 | **Vfb** → fixed charge |
| 5 | **Dit** → Terman method / conductance method / high-low frequency |
| 6 | Hysteresis → slow traps |

**주의**: SiC는 **incomplete ionization** 때문에 C-V 해석 시 도핑 보정이 필요합니다.

### 7.6 I-V / Ron / BV 분석

| 항목 | 정의 | 추출 |
|---|---|---|
| Vf | forward voltage @ 정격전류 | I-V |
| Ron,sp | differential resistance × area | ΔV/ΔJ |
| BV | breakdown voltage @ 기준전류 | reverse I-V |
| Leakage | reverse current density | reverse I-V |
| Ideality factor | n = (q/kT)·dV/dlnJ | forward I-V |
| SS | subthreshold swing | Id-Vg |
| Ion/Ioff | on/off ratio | Id-Vg |

### 7.7 TLM 분석

```
R_T(L) = (R_sh/W)·L + 2·R_c
기울기 = R_sh/W
절편 = 2·R_c
L_T = √(ρ_c/R_sh)
ρ_c = R_c·L_T·W  (또는 ρ_c = R_sh·L_T²)
```

- **선형성** 확인(R² > 0.99)
- **접촉 크기** 충분히 크게(전류 crowding 방지)
- **열처리 전/후** 비교

### 7.8 스위칭 / 게이트 전하

| 항목 | 의미 | 시뮬레이션 |
|---|---|---|
| Ciss/Coss/Crss | 입력/출력/역전달 capacitance | AC small-signal |
| Qg | gate charge | charge integration |
| Eon/Eoff | switching loss | transient + circuit |
| dv/dt, di/dt | 스위칭 속도 | transient |

### 7.9 신뢰성 시뮬레이션

| 시험 | 모델 | 접근 |
|---|---|---|
| TDDB | E-model, 1/E-model, Weibull | oxide field → lifetime |
| BTI | Vth shift vs stress time (power law) | trap model |
| HTRB | leakage/BV degradation | trap/impact |
| Dynamic Ron | trap emission/capture | transient |
| Thermal cycling | thermo-mechanical | FEM (COMSOL) |

### 7.10 캘리브레이션 절차

```
1. 측정 데이터 확보 (C-V, I-V, TLM, SIMS)
2. 1D 구조로 시작 (diode/MOS capacitor)
3. 산화막 두께/도핑 → C-V 맞춤
4. Dit/fixed charge → Vfb/SS 맞춤
5. 이동도 모델 → Id-Vg 맞춤
6. 접촉저항 → TLM 맞춤
7. 항복 모델 → BV 맞춤
8. 2D/3D로 확장, edge termination 캘리브레이션
9. DOE로 공정 window 확인
```

**캘리브레이션 원칙**
- **한 번에 하나의 파라미터**만 조정
- **물리적 타당성** 확인(문헌 범위)
- **과적합 금지**(측정 조건 여러 개로 검증)
- **불확실성** 명시

### 7.11 자주 하는 실수 12가지

| # | 실수 | 결과 | 해결 |
|---|---|---|---|
| 1 | 메쉬 부족 | BV 과대/과소, 수렴 실패 | junction/계면 집중 |
| 2 | Incomplete ionization 미적용 | Vth/Ron 오차 | SiC 필수 |
| 3 | Impact ionization 계수 부정확 | BV 오차 | 문헌/측정 캘리브레이션 |
| 4 | Oxide mesh 부족 | 터널링/누설 오차 | 0.2–0.5 nm |
| 5 | Doping gradient 급격 | 수렴 실패 | mesh grading |
| 6 | Schottky barrier를 work function으로만 설정 | I-V 오차 | 실측 배리어 사용 |
| 7 | Self-heating 무시 | Ron 과소 | thermal model |
| 8 | Interface trap 무시 | SS/Vth 오차 | Dit 입력 |
| 9 | GaN polarization 누락 | 2DEG 없음 | polarization model |
| 10 | 단위 혼동(cm⁻³ vs m⁻³) | 결과 1000배 오차 | 단위 체크 |
| 11 | 온도 미설정 | 특성 오차 | 300 K 명시 |
| 12 | 캘리브레이션 없이 결론 | 신뢰성 없음 | 측정 비교 |

---

## 8. 수료·행정 체크리스트

### 8.1 수료 조건

- [ ] **출석 100%** (2일, 4세션 전부)
- [ ] **퀴즈 5문제 중 3문제 이상** 통과
- [ ] 수료증 발급 조건 충족

### 8.2 신청·취소

- [ ] 재직자: 2026-11-02 00:00 ~ 2026-11-08 23:59
- [ ] 전체: 2026-11-09 00:00 ~ 2026-11-22 23:59
- [ ] 취소는 **홈페이지에서** 정상 처리
- [ ] **전일 결석 시 8개월 수강 제한** → 일정 관리 철저

### 8.3 강의 당일 준비

- [ ] 강의 하루 전 메일 확인(**Zoom URL**, 주의사항)
- [ ] 인터넷/마이크/카메라 테스트
- [ ] 강의 자료 사전 다운로드
- [ ] 질문 리스트 준비(공정 조건, TCAD 연계)

### 8.4 학습 기록

- [ ] 세션별 필기 → 본 문서에 추가
- [ ] 공정 흐름도 직접 작성
- [ ] TCAD 예제 1개 이상 실행
- [ ] 퀴즈 오답 정리

---

## 9. 퀴즈 대비 핵심 문답 20선

> 강좌 목표·세부목차 기반 **예상 문항**입니다. 실제 퀴즈와 다를 수 있습니다.

**Q1. Si 대비 SiC/GaN이 유리한 근본 이유는?**
A. 밴드갭이 커서 임계전계(E_c)가 3–10배 높고, 이상적 Ron,sp ∝ 1/(μE_c³)이므로 같은 항복전압에서 온저항을 수백 배 낮출 수 있다. 또한 열전도도가 높아 열 방출에 유리하다.

**Q2. Baliga FOM이란?**
A. ε·μ·E_c³로 정의되는 단극 소자 성능 지표. Si 대비 4H-SiC는 ~300–400, GaN은 ~800–1200 수준.

**Q3. SiC에서 열확산으로 junction을 만들 수 없는 이유는?**
A. SiC는 결합에너지가 크고 도펀트 확산계수가 매우 작아 실용적인 열확산이 어렵다. 따라서 이온주입 후 고온 활성화가 필수다.

**Q4. SiC 열산화에서 C-face와 Si-face의 차이는?**
A. C-face(000-1)가 Si-face(0001)보다 산화속도가 수 배 빠르다. 이로 인해 trench/off-axis 구조에서 산화막 두께 불균일이 발생한다.

**Q5. SiC/SiO₂ 계면의 주요 문제는?**
A. 산화 중 탄소 잔류로 Dit/NIT가 높아 채널 이동도 저하, Vth 불안정, SS 증가가 발생한다.

**Q6. Dit를 줄이는 대표 공정은?**
A. NO 또는 N₂O 분위기에서 1175–1300°C nitridation. N이 C 관련 결함을 passivation한다.

**Q7. SiC 이온주입에서 box profile을 만드는 방법은?**
A. 여러 에너지의 다중 주입을 겹쳐 균일한 농도 구간을 형성한다.

**Q8. SiC 고온 활성화 시 캡층이 필요한 이유는?**
A. 1600–1800°C에서 SiC 표면의 Si 승화/분해를 막아 표면 morphology를 보호하기 위해 AlN, C, poly-Si 등을 사용한다.

**Q9. SiC ohmic contact 형성 원리는?**
A. 고도핑 표면에서 터널링이 지배적이므로 배리어 높이보다 도핑 농도와 계면이 중요하다. n-SiC는 Ni 실리사이드, p-SiC는 Ti/Al이 대표적이다.

**Q10. TLM으로 ρc를 구하는 방법은?**
A. R_T vs L plot의 기울기로 R_sh, 절편으로 2R_c를 구하고, L_T=√(ρc/R_sh)로 ρc를 계산한다.

**Q11. SiC edge termination의 목적은?**
A. 표면/edge 전계 집중을 완화해 이론 항복전압에 가깝게 만드는 것. JTE, guard ring, field plate, mesa 등이 있다.

**Q12. SiC MOSFET planar vs trench의 차이는?**
A. Planar는 채널이 수평이고 공정이 성숙하지만 JFET 저항/채널 이동도 한계가 있다. Trench는 채널 밀도를 높여 Ron을 낮추지만 trench corner 전계 집중과 산화막 신뢰성 문제가 있다.

**Q13. GaN HEMT에서 2DEG가 형성되는 원리는?**
A. AlGaN/GaN 이종접합의 spontaneous/piezoelectric polarization 차이로 계면에 양전하가 생기고 전자가 accumulation되어 2DEG가 형성된다.

**Q14. GaN normally-off 구현 방법 3가지는?**
A. p-GaN gate, recessed MIS gate, cascode. (추가로 F implant 등)

**Q15. GaN ohmic contact의 대표 스택과 열처리는?**
A. Ti/Al/Ni/Au, 800–900°C RTA. Ti가 AlGaN과 반응해 TiN을 형성하여 낮은 접촉저항을 만든다.

**Q16. Current collapse / dynamic Ron의 원인과 대책은?**
A. 표면/버퍼 트랩에 의한 전자 포획으로 2DEG가 부분 depletion된다. SiNx passivation, field plate, buffer 최적화로 억제한다.

**Q17. Field plate의 역할과 trade-off는?**
A. gate-drain 전계를 분산해 peak field를 낮춘다. FP 길이가 길수록 항복/신뢰성은 좋아지지만 Cgd가 증가해 스위칭 손실이 커진다.

**Q18. SiC BPD가 문제되는 이유는?**
A. BPD가 stacking fault로 확장되면 bipolar degradation(Ron 증가)을 유발한다. PiN/MOSFET body diode에서 특히 문제다.

**Q19. HTGB/HTRB/HTOL/H3TRB는 무엇인가?**
A. 각각 고온 게이트 바이어스, 고온 역바이어스, 고온 동작 수명, 고온고습 역바이어스 시험이다.

**Q20. TCAD 캘리브레이션의 기본 순서는?**
A. 1D 구조로 C-V(산화막/도핑) → Vfb/Dit → 이동도(Id-Vg) → TLM(ρc) → BV(항복 모델) → 2D/3D 확장 → DOE 순으로 맞춘다.

---

## 10. 참고문헌·추천 학습자료

### 10.1 교과서·핸드북

- B. J. Baliga, *Fundamentals of Power Semiconductor Devices*, Springer.
- B. J. Baliga, *Silicon Carbide Power Devices*, World Scientific.
- T. Kimoto & J. A. Cooper, *Fundamentals of Silicon Carbide Technology*, Wiley-IEEE.
- H. Morkoç, *Handbook of Nitride Semiconductors and Devices*.
- S. M. Sze & K. K. Ng, *Physics of Semiconductor Devices*.
- *Springer Handbook of Semiconductor Devices*.

### 10.2 주요 학회·저널

- ISPSD (International Symposium on Power Semiconductor Devices and ICs)
- IEDM, ECS Transactions (SiC/GaN 공정)
- IEEE Transactions on Electron Devices
- IEEE Transactions on Power Electronics
- Materials Science Forum (SiC 관련)
- Applied Physics Letters, Journal of Applied Physics

### 10.3 TCAD 문서

- Synopsys Sentaurus Device User Guide / SProcess User Guide
- Silvaco Atlas User's Manual / Victory Process Manual
- COMSOL Semiconductor Module User's Guide
- DEVSIM documentation (오픈소스)

### 10.4 온라인

- SEMI standards (wafer/공정 용어)
- IEEE Xplore, Google Scholar
- SRIM (http://www.srim.org)
- KLayout, ngspice 공식 문서

---

## 11. 부록

### 부록 A. 용어집

| 용어 | 설명 |
|---|---|
| **WBG** | Wide-Bandgap, 넓은 밴드갭 반도체(SiC, GaN 등) |
| **FOM** | Figure of Merit, 성능 지표 |
| **2DEG** | Two-Dimensional Electron Gas, 이종접합 계면의 전자 가스 |
| **Dit** | Interface trap density, 계면 트랩 밀도 |
| **NIT** | Near-Interface Traps, 계면 근처 산화막 트랩 |
| **BPD** | Basal Plane Dislocation, 기저면 전위 |
| **JTE** | Junction Termination Extension, 접합 종단 확장 |
| **TLM** | Transmission Line Method, 접촉저항 측정법 |
| **ρc** | Specific contact resistance, 비접촉저항 |
| **HTGB/HTRB** | High Temperature Gate/Reverse Bias 시험 |
| **TDDB** | Time-Dependent Dielectric Breakdown |
| **BTI** | Bias Temperature Instability |
| **MOCVD** | Metal-Organic Chemical Vapor Deposition |
| **ALD** | Atomic Layer Deposition |
| **ICP** | Inductively Coupled Plasma |
| **RTA** | Rapid Thermal Annealing |
| **SIMS** | Secondary Ion Mass Spectrometry |
| **AFM** | Atomic Force Microscopy |
| **EL** | Electroluminescence |

### 부록 B. 공정 흐름 요약 카드

#### B.1 SiC SBD

```
n⁺ substrate
 → n⁻ epi (10 µm)
 → JTE implant (Al)
 → activation (1650°C)
 → field oxide
 → Schottky metal (Ni)
 → passivation
 → pad
```

#### B.2 SiC Planar DMOS

```
n⁺ substrate
 → n⁻ epi
 → JTE implant
 → P-body implant
 → N⁺ source implant
 → P⁺ contact implant
 → activation (1650–1700°C)
 → gate oxidation + NO anneal
 → poly-Si gate
 → ILD
 → ohmic contact (Ni / Ti-Al)
 → passivation
 → pad
```

#### B.3 GaN HEMT

```
substrate (Si/SiC)
 → AlN nucleation
 → buffer (C-doped GaN)
 → GaN channel
 → AlGaN barrier
 → GaN cap
 → mesa isolation
 → ohmic (Ti/Al/Ni/Au, 800–900°C)
 → gate dielectric (ALD Al₂O₃)
 → gate metal
 → SiNx passivation
 → field plate
 → pad
```

### 부록 C. 자가점검표

| # | 점검 항목 | O/X |
|---|---|---|
| 1 | Si/SiC/GaN 물성표를 설명할 수 있다 | |
| 2 | Baliga FOM을 계산할 수 있다 | |
| 3 | WBG 공정의 5가지 차이를 말할 수 있다 | |
| 4 | SiC 세정 시퀀스를 설명할 수 있다 | |
| 5 | Deal-Grove 식과 SiC 산화의 orientation 의존성을 설명할 수 있다 | |
| 6 | Dit/NIT와 nitridation의 역할을 설명할 수 있다 | |
| 7 | SiC implant의 box profile/고온 활성화를 설명할 수 있다 | |
| 8 | SiC 식각의 hard mask와 profile 제어를 설명할 수 있다 | |
| 9 | Ohmic/Schottky contact 형성 원리를 설명할 수 있다 | |
| 10 | TLM으로 ρc를 계산할 수 있다 | |
| 11 | Edge termination 4가지를 비교할 수 있다 | |
| 12 | SiC MOSFET 공정 흐름을 그릴 수 있다 | |
| 13 | GaN 2DEG 형성 원리를 설명할 수 있다 | |
| 14 | GaN normally-off 3방식을 비교할 수 있다 | |
| 15 | Current collapse와 field plate를 설명할 수 있다 | |
| 16 | SiC/GaN 결함과 신뢰성 시험을 매핑할 수 있다 | |
| 17 | TCAD 공정-소자 연동 흐름을 설명할 수 있다 | |
| 18 | TCAD 초기 설정(재료/메쉬/모델)을 할 수 있다 | |
| 19 | C-V/I-V/TLM 분석 절차를 설명할 수 있다 | |
| 20 | 캘리브레이션 순서를 설명할 수 있다 | |

---

> **문서 끝**
> 본 문서는 공개된 강좌 정보와 일반적인 SiC/GaN 공정 문헌을 바탕으로 작성된 **자기학습용 정리본**입니다.
> 실제 강의 내용·퀴즈·공정 조건은 강사 및 기관 자료를 우선하세요.
