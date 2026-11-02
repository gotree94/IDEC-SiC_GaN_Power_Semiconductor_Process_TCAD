# Day 17 — GaN HEMT 공정 Integration

> **Week 4 / Day 17** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: Day 16
> **참조**: [`../04_Week4_GaN_HEMT_Process_Reliability_and_Characterization.md`](../04_Week4_GaN_HEMT_Process_Reliability_and_Characterization.md)

---

## 1. 오늘의 질문

> **"AlGaN/GaN 2DEG를 유지하면서 isolation·ohmic·gate·passivation을 어떻게 통합하는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | Isolation, ohmic, gate, dielectric |
| 10:45–12:15 | Lecture 2 | Passivation, field plate, thermal, flow |
| 13:15–15:15 | Lab | HEMT TCAD/process flow |
| 15:30–16:30 | Case | 650 V GaN HEMT 설계 |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제 |

---

## 3. 학습목표

1. Mesa vs implant isolation을 비교한다.
2. Ti/Al/Ni/Au ohmic의 어닐링과 접촉 저항을 설명한다.
3. Schottky gate vs MIS gate를 비교한다.
4. SiN/Al₂O₃ passivation의 역할을 설명한다.
5. Field plate가 전계·dynamic Ron에 미치는 영향을 설명한다.
6. 650 V HEMT 공정 flow를 설계한다.

---

## 4. Lecture 1 — 핵심 Module (1.5h)

### 4.1 Isolation

| 방식 | 원리 | 장점 | 단점 |
|---|---|---|---|
| Mesa etch | 2DEG 제거 | 단순, 확실 | topography, etch damage |
| Implant (N, He, Ar) | 2DEG compensation | planar | damage/anneal |
| Oxygen implant | isolation | planar | leakage |

- Mesa depth: 2DEG 아래까지 (50–100 nm+).
- Implant: dose/energy로 2DEG 차단.

### 4.2 Ohmic contact

- Stack: Ti/Al/Ni/Au (예: 20/100/40/50 nm).
- Anneal: 800–900°C, N₂, 30–60 s.
- Ti가 GaN과 반응 → TiN, N vacancy → n+ 영역.
- $ρ_c \sim 10^{-6}$ Ω·cm².
- 중요: 2DEG 유지, edge morphology, Au diffusion.

### 4.3 Gate

| 유형 | 구조 | $V_{th}$ | 장점 | 단점 |
|---|---|---|---|---|
| Schottky | Ni/Au | depletion (on) | 단순, low leakage? | normally-on |
| MIS | SiN/Al₂O₃ | tunable | low leakage, swing | hysteresis |
| p-GaN | p-GaN/Mg | enhancement | normally-off | Mg activation, gate reliability |
| Recess | etch + gate | enhancement | $V_{th}$ control | damage |

### 4.4 Dielectric/Passivation

| 재료 | 역할 | 특징 |
|---|---|---|
| SiN (PECVD) | passivation, encapsulation | H content, charge |
| Al₂O₃ (ALD) | gate dielectric, passivation | low leakage, hygroscopic |
| SiO₂ | ILD | poor GaN interface |
| Polyimide | thick passivation | organic |

> Passivation은 surface trap을 억제해 **current collapse/dynamic Ron**을 줄입니다.

---

## 5. Lecture 2 — Field Plate·Thermal·Flow (1.5h)

### 5.1 Field plate

- Gate-connected or source-connected.
- 역할: gate edge 전계 완화, BV↑, trapping↓.
- trade-off: $C_{gd}$↑, switching loss.

### 5.2 Thermal

- GaN-on-Si: $R_{th}$ 큼, buffer thermal resistance.
- GaN-on-SiC: $R_{th}$ 작음.
- 열 설계: die attach, substrate, package.

### 5.3 Process flow (예)

| # | Module | 온도 |
|---:|---|---|
| 1 | Epi (buffer/channel/barrier) | 1000–1100°C |
| 2 | Isolation (mesa/implant) | RT |
| 3 | Ohmic depo/anneal | 800–900°C |
| 4 | Passivation (SiN) | 300–400°C |
| 5 | Gate window etch | RT |
| 6 | Gate metal | <400°C |
| 7 | Interlayer dielectric | 300–400°C |
| 8 | Field plate | <400°C |
| 9 | Interconnect/pad | <400°C |

### 5.4 Failure modes

| 문제 | 원인 | 대응 |
|---|---|---|
| Dynamic Ron | surface/buffer traps | passivation, field plate |
| Gate leakage | Schottky/MIS defects | dielectric quality |
| Ohmic degradation | Au diffusion, voids | barrier metal |
| Isolation leakage | implant damage | anneal |
| Thermal runaway | $R_{th}$ | substrate/package |

---

## 6. Lab — HEMT TCAD/Flow (2h)

### 6.1 목표

- 650 V HEMT의 $V_{th}$, $R_{on}$, BV 계산.
- Field plate 유무 비교.
- Process flow/mask 정리.

### 6.2 구조

| 파라미터 | 값 |
|---|---|
| AlGaN | 25%, 20 nm |
| GaN channel | 1 µm |
| Buffer | C-doped 2 µm |
| Gate | Ni, 1 µm |
| Field plate | 3 µm |
| Passivation | SiN 200 nm |

### 6.3 절차

1. 2DEG/$R_{sh}$ 설정.
2. $V_{th}$ 계산.
3. Field plate 유무 BV.
4. $C_{gd}$ 비교.
5. Process flow 작성.

### 6.4 제출물

- $I$–$V$/$BV$ 그래프.
- Field plate 효과 표.
- Mask flow.
- "650 V 설계" 요약.

### 6.5 체크포인트

- [ ] 2DEG 농도.
- [ ] Gate length.
- [ ] Field plate length.
- [ ] Passivation charge.

---

## 7. Case — 650 V GaN HEMT (1h)

### 7.1 요구

- $V_{th}>1$ V (normally-off).
- $R_{on}<100$ mΩ.
- BV>650 V.
- Dynamic Ron<1.3.

### 7.2 설계

| 항목 | 선택 |
|---|---|
| Gate | p-GaN or recess |
| Passivation | SiN + Al₂O₃ |
| Field plate | source-connected |
| Isolation | implant |
| Substrate | GaN-on-Si |

### 7.3 리스크

- p-GaN gate reliability.
- Dynamic Ron.
- Thermal.

### 토론

1. Mesa isolation과 implant isolation의 trade-off?
2. Ohmic anneal이 2DEG에 미치는 영향?
3. Field plate가 $C_{gd}$를 증가시키는 이유?

---

## 8. Quiz (5문항)

1. Ti/Al/Ni/Au ohmic의 역할?
2. Mesa isolation의 원리?
3. Schottky gate의 단점?
4. SiN passivation의 역할?
5. Field plate의 장단점?

**정답**: 1) TiN 형성/N vacancy. 2) 2DEG 제거. 3) normally-on. 4) surface trap 억제. 5) BV↑/$C_{gd}$↑.

---

## 9. Homework

- 650 V HEMT mask flow를 8단계로 작성.
- Field plate length 1/3/5 µm에서 BV/$C_{gd}$ 비교.
- Day 18을 위해 **normally-off/dynamic Ron** 예습.

---

## 10. 강사 노트

- "2DEG를 죽이지 않는 공정"이 핵심.
- Ohmic anneal은 **2DEG/표면**에 영향.
- Passivation과 dynamic Ron을 연결.
- 다음 날 normally-off와 신뢰성.

---

*Day 17 / 20 — 140h program. 공정 조건은 교육용 대표값입니다.*
