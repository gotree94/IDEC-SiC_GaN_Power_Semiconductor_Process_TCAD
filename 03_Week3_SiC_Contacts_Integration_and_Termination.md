# Week 3 — SiC Contacts, Integration & Edge Termination

> **소요**: 강의 4h + 실습 2h
> **원 강좌 대응**: D2-AM (Ohmic/Schottky 접촉 / SiC diode·MOSFET integration / edge termination·layout)
> **핵심 질문**: "접촉저항과 항복전압을 어떻게 설계하고, 실제 칩에서 어떻게 검증하는가?"

---

## 1. 학습목표

1. Ohmic/Schottky 접촉의 **전류 수송 메커니즘**을 설명하고, `ρc`를 TLM으로 계산한다.
2. Ni 실리사이드(n-type), Ti/Al(p-type) 접촉의 **계면 반응과 공정 조건**을 설명한다.
3. SiC DMOS/Trench/SJ의 **integration 순서와 thermal budget**을 비교한다.
4. JTE/guard ring/field plate/mesa의 **전계 분산 원리**와 설계 변수를 설명한다.
5. **JTE dose window**와 **surface charge 민감도**를 TCAD로 분석한다.
6. BPD/stacking fault와 **bipolar degradation**을 설명한다.
7. TCAD로 **BV, 전계 분포, oxide field**를 추출하고 측정과 비교한다.

---

## 2. 접촉 물리

### 2.1 Schottky 장벽

금속과 반도체 접합에서 이상적 장벽 높이:
```
qφ_Bn = q(φ_M − χ_s)        (n-type, ideal)
qφ_Bp = E_g − qφ_Bn          (p-type)
```
- `φ_M`: metal work function, `χ_s`: electron affinity.
- 실제로는 **Fermi level pinning**(interface states)으로 `φ_B`가 work function에 약하게 의존.

**4H-SiC의 대표 Schottky barrier (n-type, 참고값)**

| 금속 | φ_B (eV) | 비고 |
|---|---|---|
| Ti | 0.9–1.1 | 낮은 barrier, 높은 leakage |
| Ni | 1.3–1.5 | 상용 SBD |
| Pt | 1.3–1.5 | 고온 안정 |
| Au | 1.4–1.5 | 연구 |
| W | 1.0–1.2 | 고온 |

> 값은 **표면 처리·annealing·결정면**에 따라 달라집니다. 데이터시트/문헌값을 그대로 쓰지 말고 **I-V/C-V로 추출**하십시오.

### 2.2 전류 수송 메커니즘

| 메커니즘 | 지배 조건 | 전류식(개념) |
|---|---|---|
| **Thermionic emission (TE)** | `kT` > barrier fluctuation | `J ∝ T² exp(−qφ_B/kT)(exp(qV/nkT)−1)` |
| **Field emission (FE)** | 고도핑, 얇은 barrier | tunneling |
| **Thermionic-field emission (TFE)** | 중간 도핑 | TE+ tunneling |
| **Recombination** | 저도핑, 고주입 | `∝ exp(qV/2kT)` |
| **Leakage** | 결함, edge | — |

**Richardson constant**: `A* = 4πq m*k²/h³` ≈ 146 A/cm²K² (SiC 전자, m*=0.29m₀ 기준). 실제로는 `A*`를 fitting parameter로 사용.

### 2.3 Specific contact resistance

```
ρ_c = (∂J/∂V)⁻¹ |_{V=0}   [Ω·cm²]
```

| 접촉 | ρc (대표) | 요구 |
|---|---|---|
| n⁺ SiC (Ni silicide) | 10⁻⁶–10⁻⁵ Ω·cm² | < 10⁻⁵ |
| p⁺ SiC (Ti/Al) | 10⁻⁵–10⁻⁴ Ω·cm² | < 10⁻⁴ |
| n⁺ GaN (Ti/Al/Ni/Au) | 10⁻⁶–10⁻⁵ Ω·cm² | < 10⁻⁵ |
| p-GaN | 10⁻⁴–10⁻³ Ω·cm² | 과제 |

> `ρc`는 **도핑 농도**에 지수적으로 의존: `ρc ∝ exp(2√(ε_s m*) φ_B/(ħ√N))`. 도핑을 높이는 것이 가장 효과적.

---

## 3. TLM (Transmission Line Method)

### 3.1 구조

```
[pad]──[d1]──[pad]──[d2]──[pad]──[d3]──[pad]
        ↑ contact width W, gap L
```

### 3.2 이론

```
R_total(L) = 2R_c + (R_sh/W)·L
L_T = sqrt(ρ_c/R_sh)
R_c = (R_sh·L_T/W)·coth(L_contact/L_T)
```
- `L >> L_T`이면 `R_c ≈ R_sh·L_T/W`.
- `R_total` vs `L` 그래프:
  - 기울기 = `R_sh/W`
  - y절편 = `2R_c`
- 그 다음:
  ```
  ρ_c = R_c²·W²/R_sh
  ```

### 3.3 계산 예제

**주어진 값**: `W = 100 µm`, `R_total` vs `L` 기울기 = `50 Ω/µm`, 절편 = `20 Ω`

1. `R_sh = slope × W = 50 × 100 = 5000 Ω/sq`
2. `R_c = intercept/2 = 10 Ω`
3. `ρ_c = R_c² W² / R_sh = 100 × (100×10⁻⁴ cm)² / 5000`
   ```
   = 100 × 1×10⁻⁴ / 5000 = 2×10⁻⁶ Ω·cm²
   ```
4. `L_T = sqrt(ρc/R_sh) = sqrt(2×10⁻⁶/5000) = 2×10⁻⁵ cm = 0.2 µm`

**해석**: `L_T`가 0.2 µm이므로 contact length는 **≥ 1 µm**면 충분. Gap은 2–20 µm 범위로 설계.

### 3.4 TLM 실무 주의

| 이슈 | 대책 |
|---|---|
| Contact width 정의 | 실제 current path(W)로 보정 |
| Metal sheet resistance | 저항 낮은 metal, 4-wire |
| Current crowding | `L_T`보다 긴 contact |
| 열/응력 | 측정 전 안정화 |
| Parasitic probe resistance | calibration |
| 비균일 접촉 | 여러 패턴, 통계 |

---

## 4. SiC Ohmic Contact

### 4.1 n-type: Ni / Ni₂Si

```
Ni 증착 (50–200 nm) → RTA 950–1050°C, 30–120 s (Ar/N₂)
```
- 계면 반응: `Ni + SiC → Ni₂Si + C`
- **C가 interface에 축적** → 추가 annealing/etch로 제거 필요.
- Ni₂Si는 낮은 barrier + 높은 도핑 → tunneling.
- ρc ~ 10⁻⁶–10⁻⁵ Ω·cm².

**대안**: Ti, TiSi₂, W, Co, Mo. Ti는 C와 반응해 TiC 형성 → 계면 개선.

### 4.2 p-type: Ti/Al

```
Ti (20–50 nm) / Al (100–300 nm) → RTA 950–1050°C
```
- Ti가 SiC와 반응해 **TiC/TiSi** 형성, Al이 p-type 도핑/계면 개선.
- ρc ~ 10⁻⁵–10⁻⁴ Ω·cm².
- Al melting(660°C) → **짧은 RTA**, cap 필요.

**대안**: Al/Ni, Ni/Al, Pt/Ti, Mo/Al.

### 4.3 접촉 계면 공학

| 목표 | 방법 |
|---|---|
| 낮은 ρc | 고도핑(>10¹⁹), 계면 산화 제거, silicide |
| 열 안정성 | refractory metal, barrier layer |
| 표면 morphology | annealing 조건, cap |
| C 제거 | in-situ etch, sacrificial, Ti/Ni 반응 |
| 신뢰성 | barrier(TiN, WN), encapsulation |

### 4.4 접촉 후 검증

- TLM: ρc, R_sh, L_T
- SEM/TEM: 계면 반응, 두께
- XRD: silicide phase
- AES/XPS: C 축적
- 열처리 후 재측정: 안정성

---

## 5. SiC Schottky / JBS

### 5.1 SBD

- Ni/Pt Schottky + JTE/guard ring.
- Vf 1.0–1.5 V, BV 600–1.2 kV.
- Leakage: barrier lowering, defects, edge.
- 장점: no reverse recovery, 빠른 스위칭.
- 단점: surge current 제한, leakage.

### 5.2 JBS (Junction Barrier Schottky)

```
p⁺ grid + Schottky metal
```
- Reverse bias: p⁺/n⁻ junction이 공핍 → Schottky surface field 감소 → **leakage 감소, BV 증가**.
- Forward: Schottky가 전류 담당, p⁺는 낮은 Vf에서는 비활성.
- 설계: p⁺ width/spacing, dose, depth.
- Trade-off: p⁺ 면적↑ → forward active area↓.

### 5.3 MPS (Merged PiN Schottky)

- JBS + PiN 혼합: surge 시 PiN이 전도도 변조.
- 장점: surge robustness, 낮은 leakage.
- 단점: reverse recovery (소수 캐리어).

### 5.4 TCAD 포인트

| 항목 | 설정 |
|---|---|
| Schottky barrier | `Barrier = 1.3 eV` (Ni) |
| Thermionic emission | 필수 |
| Barrier lowering | image force, tunneling |
| JBS p⁺ | junction depth, doping |
| BV | impact ionization + edge termination |

---

## 6. SiC MOSFET Integration

### 6.1 Planar DMOS

```
[1] p-body implant (Al)
[2] n⁺ source implant (N)
[3] p⁺ contact implant (Al)
[4] activation (1600–1700°C)
[5] gate ox + nitridation
[6] poly gate (self-aligned)
[7] ILD
[8] ohmic
[9] passivation
```

**핵심 변수**
- channel length `L_ch` (0.5–1.5 µm)
- JFET width `W_J` (1–3 µm)
- p-body depth/doping
- gate oxide thickness

**문제**
- JFET 저항
- 채널 이동도 (20–40)
- gate oxide 전계 (corner)

### 6.2 Trench MOSFET

```
trench etch → corner rounding → gate ox → poly fill → planarization
```
- 장점: 수직 채널 → cell density↑, Ron↓.
- 단점: **trench bottom corner 전계 집중** → oxide breakdown.
- 대책:
  - **corner rounding** (sacrificial oxidation)
  - **bottom protection p-well** (교수님 2019 EPJ)
  - **tilted implant** (교수님 2023 JJAP)
  - **thick bottom oxide**

> **교수님 연구 연결**: 2023 JJAP 논문은 **tilted ion implantation**으로 trench bottom의 전계 집중을 억제한 1.2 kV SiC trench MOSFET 설계를 다룹니다. 이는 "공정(implant angle)으로 소자 전계를 바꾼다"는 대표 사례입니다.

### 6.3 Double Trench / P-Shield

- source trench + gate trench.
- **P-shield**로 gate oxide를 전계로부터 차폐.
- **Split gate**로 `Q_gd` 감소 → 스위칭 손실 개선.

> **교수님 연구 연결**: 2025 JEET 논문 "Improved Switching Characteristics of 1.2 kV P-Shielded Split Gate SiC MOSFETs"는 P-shield + split gate로 **스위칭 특성**을 개선한 사례입니다.

### 6.4 Super-Junction (SJ)

- charge balance: `N_D·W_n = N_A·W_p`.
- 이상적: `BV` 유지하면서 `Ron ∝ W` 감소.
- SiC 과제: **deep p column** 형성 (multi-epi implant, trench fill).
- 3.3 kV charge-balanced PN multi-epi termination (교수님 2025 Micromachines).

### 6.5 Self-aligned & Channeling Step Shielding

- Self-aligned: poly gate를 mask로 n⁺ source implant → `L_ch` 정의.
- **channeling step shielding**: channel 영역의 step 구조로 implant channeling/전계를 제어.

> **교수님 연구 연결**: 2025 SST 논문 "Channeling step shielding MOSFET design for improved short-circuit withstand time and UIS energy in 1200 V SiC devices"는 **structure-level 설계**로 SCWT와 UIS 에너지를 동시에 개선한 사례입니다. 세부 구조·메커니즘은 원문을 참조하십시오.

### 6.6 Integration 비교

| 구조 | 마스크 수(대략) | 장점 | 단점 | 주 용도 |
|---|---|---|---|---|
| Planar DMOS | 6–8 | 성숙, 안정 | JFET 저항 | 600–3.3 kV |
| Trench | 7–9 | Ron↓ | corner 전계 | 600–1.2 kV |
| Double trench | 8–10 | oxide shielding | 복잡 | 1.2 kV |
| SJ | 8–12 | Ron↓↓ | deep column | 600–900 V(Si), SiC 연구 |
| HEMT | 5–7 | 고주파 | normally-on | 100–650 V |

---

## 7. Edge Termination

### 7.1 왜 필요한가

이상적 평면 접합의 BV:
```
BV_pp = ε E_c²/(2qN_D)
```
실제 소자는 **junction curvature**에서 전계 집중 → BV 감소.

**Curvature 효과(개념)**
- Cylindrical/spherical junction: `E_max > E_plane`
- 깊은 junction일수록 완화
- termination은 **표면/edge 전계를 낮춰** 이상값에 가깝게 만듦.

**Termination efficiency**
```
η = BV_actual / BV_ideal × 100 (%)
```
- 목표: 80–95%.

### 7.2 종류

| 방식 | 원리 | 장점 | 단점 |
|---|---|---|---|
| **Mesa** | etch step으로 공핍 확장 | 간단 | 표면 전계, passivation |
| **Guard ring** | floating p⁺ ring이 공핍 spread | 안정 | 면적, ring 수 |
| **JTE** | implanted p⁻가 표면 전계 완화 | 효율 높음 | dose 민감 |
| **Field plate** | metal/poly over oxide | 표면 전계↓ | oxide 신뢰성 |
| **RESURF** | lateral charge balance | 면적 효율 | 설계 복잡 |
| **JTE + FP** | 조합 | 고효율 | 공정 복잡 |

### 7.3 JTE 설계

**핵심 변수**
- dose `Φ_JTE` (10¹²–10¹³ cm⁻²)
- depth `x_j`
- length `L_JTE`
- profile (box vs graded)

**동작**
- Reverse bias: JTE가 완전 공핍 → 표면 전계 분산.
- Dose too low: JTE가 공핍되지 않음 → junction edge 전계 집중.
- Dose too high: JTE edge 전계 집중, type conversion.

**Dose window(개념)**

```
BV
 ↑
 │        ┌──────┐
 │      ┌─┘      └─┐
 │    ┌─┘          └─┐
 │  ┌─┘              └─┐
 └──────────────────────→ Φ_JTE
        window
```

**설계 목표**: window 중앙, **surface charge ±1×10¹² cm⁻²**에서도 BV 유지.

### 7.4 Multi-Floating-Zone JTE

- 여러 floating p zone을 **지수적으로 변하는 dimension**으로 배치.
- 표면 전계를 단계적으로 분산 → 높은 BV.
- 4.5 kV SiC (교수님 2021 JJAP).

> **교수님 연구 연결**: "Multi-floating-zone JTE for 4.5 kV SiC power devices with exponentially modulated dimensions" — JTE 구조를 **다중 floating zone**으로 확장하고 dimension을 지수적으로 변조한 설계.

### 7.5 Charge-Balanced PN Multi-Epi Termination

- PN multi-epi layer로 **charge balance**.
- 3.3 kV SiC (교수님 2025 Micromachines, open access).
- 장점: 높은 BV, 면적 효율.
- 과제: epi 두께/도핑 정밀 제어.

### 7.6 Radiation Hardness와 Termination

- Proton irradiation 시 **termination 구조**가 방사선 내성에 영향.
- 1.2 kV SiC power devices with advanced edge termination (교수님 2026 J. Semicond.).
- Gamma irradiation + gate bias (교수님 2024 Micromachines).

> **실무 시사점**: termination은 단순 BV 설계가 아니라 **신뢰성(방사선·표면 전하·습도)** 까지 포함한 설계 변수입니다.

### 7.7 TCAD JTE 실습 설계

| 단계 | 내용 |
|---|---|
| 1 | 1.2 kV planar diode, JTE 없음 → BV, E-field peak 확인 |
| 2 | JTE dose 1×10¹²–1×10¹³ sweep |
| 3 | BV vs dose curve → window |
| 4 | surface charge ±1×10¹² 추가 → window shift |
| 5 | JTE length 20–100 µm sweep |
| 6 | oxide field, metal FP 유무 비교 |
| 7 | 최적 설계 선정, tolerance analysis |

---

## 8. 신뢰성 (Reliability)

### 8.1 BPD와 Bipolar Degradation

**메커니즘**
1. SiC epi/substrate에 **BPD**(basal plane dislocation) 존재.
2. Forward bias에서 **전자-정공 재결합** 에너지가 dislocation을 이동시켜 **stacking fault** 확장.
3. Stacking fault가 **carrier lifetime/이동도** 저하 → **Vf 증가, Ron 증가**.

**평가**
- Forward current stress (body diode) → Vf drift.
- EL/PL imaging으로 stacking fault 확인.
- Ron increase vs stress time.

**대책**
- 저 BPD epi (≤10² cm⁻²)
- body diode 사용 최소화 (synchronous rectification 회피)
- **embedded Schottky** (JBS/MPS)로 소수 캐리어 주입 억제
- 소자 구조/protection

> **교수님 연구 연결**: SiC PiN diode, SJ diode, body diode 특성 연구가 이 주제와 직접 연결됩니다. 3주차 실습에서는 "body diode stress 후 Vf drift"를 시뮬레이션/문헌 데이터로 분석합니다.

### 8.2 TDDB

- Gate oxide에 일정 전계 stress → **percolation path** → breakdown.
- Weibull 분포:
  ```
  ln(−ln(1−F)) = β·ln(t) − β·ln(η)
  ```
- `β`: shape (1–3), `η`: scale (63.2% failure time).
- 평가: constant voltage/current stress, multiple temperatures.
- 목표: 10년 수명, `E_ox` 3–4 MV/cm.

### 8.3 BTI (Bias Temperature Instability)

- Gate bias + temperature → oxide trap charging → **Vth shift**.
- SiC MOSFET: `Vth` shift, recovery.
- 평가: stress/measure sequence, `ΔVth` vs time.
- 모델: `ΔVth = A·t^n` (n~0.1–0.3).

### 8.4 HTRB / HTGB / HTOL

| 시험 | 조건 | 목적 |
|---|---|---|
| **HTRB** | high temp, reverse bias (80% rated) | leakage, termination |
| **HTGB** | high temp, gate bias | gate oxide |
| **HTOL** | high temp, operating | lifetime |
| **H3TRB** | 85°C/85% RH, reverse bias | humidity |
| **TC** | −55~150°C cycling | package |
| **PC** | power cycling | solder/bond |
| **UIS** | unclamped inductive | avalanche |
| **SCWT** | short circuit | robustness |

### 8.5 UIS / Short Circuit

- **UIS**: inductor에 저장된 에너지를 avalanche로 소모 → `E_as` (avalanche energy).
- **SCWT**: short circuit 시 **전류 제한 + 열** → withstand time.
- 설계: JFET 저항, channel, thermal capacitance, gate resistance.

> **교수님 연구 연결**: 2025 SST "Channeling step shielding MOSFET"은 **SCWT와 UIS energy를 동시에 개선**한 구조 설계 사례입니다.

### 8.6 방사선

| 방사선 | 영향 | 평가 |
|---|---|---|
| Gamma | oxide trap, Vth shift | TID, gate bias dependence |
| Proton | displacement damage, leakage | fluence vs BV/leakage |
| Neutron | displacement | SEE |
| Heavy ion | single event burnout | SEE |

- 교수님 2024 Micromachines: gamma irradiation + gate bias 조건.
- 교수님 2026 J. Semicond.: proton irradiation + advanced termination.

---

## 9. TCAD BV / Termination 검증

### 9.1 모델

| 물리 | 모델 |
|---|---|
| Bandgap | SiC (temperature dependent) |
| Incomplete ionization | Al/N |
| Mobility | doping/temperature/field dependent |
| Recombination | SRH, Auger |
| Impact ionization | Okuto-Crowell / van Overstraeten / Bologna |
| Anisotropy | 4H-SiC (optional) |
| Interface traps | Dit (for MOS) |
| Oxide | Fowler-Nordheim, Poole-Frenkel |

### 9.2 메쉬

| 영역 | Mesh |
|---|---|
| Junction/termination | 10–50 nm |
| Oxide interface | 1–5 nm |
| Drift | 0.1–0.5 µm |
| Substrate | 1–5 µm |
| Contact | fine |

> **메쉬 민감도**: BV는 junction/termination mesh에 민감. 2배/0.5배로 변화 확인.

### 9.3 BV 추출

- 조건: `I_anode = 1 mA/cm²` (또는 10 µA) at reverse.
- `BV` = voltage at criteria.
- `E_max` 위치/값, `E_ox` 확인.
- `η = BV_sim/BV_ideal`.

### 9.4 JTE dose sweep 예시(개념)

| Dose (cm⁻²) | BV (V) | E_max 위치 | 비고 |
|---|---|---|---|
| 1×10¹² | 700 | junction edge | under-depleted |
| 3×10¹² | 1050 | junction edge | — |
| 5×10¹² | 1180 | JTE edge | near optimal |
| 7×10¹² | 1200 | JTE edge | optimal |
| 1×10¹³ | 1120 | JTE edge | over-doped |
| 2×10¹³ | 850 | JTE edge | type conversion |

> 위 숫자는 **예시**입니다. 실제 값은 epi/구조에 따라 다릅니다. 실습에서는 직접 sweep합니다.

### 9.5 Oxide field

- Trench corner, gate edge에서 `E_ox` 추출.
- 목표: `< 3 MV/cm` (long-term reliability).
- `E_ox`가 높으면: corner rounding, thick bottom oxide, P-shield, field plate.

### 9.6 TLM 시뮬레이션

- 2D resistor structure, contact resistivity boundary.
- `R_total` vs `L` → `ρc`, `R_sh`.
- 실제 TLM 측정과 비교.

---

## 10. 실습 (2h)

### 실습 3-A: TLM 분석
- 제공된 `R_total` vs `L` 데이터로 `R_sh`, `R_c`, `ρc`, `L_T` 계산.
- 오차 원인 토의(contact width, probe, 비균일).

### 실습 3-B: 1.2 kV planar diode
- JTE 없는 구조 → BV, E-field peak.
- JTE dose sweep → window.
- surface charge ±1×10¹² → BV shift.
- 최적 dose 선정.

### 실습 3-C: Trench corner
- corner radius 0/50/100 nm → `E_ox` 비교.
- P-shield 유무 → `E_ox`, Ron 비교.

### 실습 3-D: 신뢰성 데이터 해석
- 주어진 TDDB/HTRB 데이터로 Weibull plot.
- `β`, `η` 추출, 10년 수명 추정.
- BPD stress 데이터로 Vf drift 분석.

---

## 11. 퀴즈 8문항

**Q1.** TLM에서 `R_total` vs `L`의 기울기와 절편이 의미하는 것은?
> A. 기울기 = `R_sh/W`, 절편 = `2R_c`.

**Q2.** `ρc`를 낮추는 가장 효과적인 방법은?
> A. 접촉 영역 도핑을 높여 tunneling을 촉진하는 것.

**Q3.** Ni/SiC ohmic에서 C가 문제되는 이유와 대책은?
> A. Ni-SiC 반응 시 C가 계면에 축적되어 저항/접착을 악화. Ti/Ni 조합, 추가 anneal/etch, cap으로 제어.

**Q4.** JTE dose가 너무 낮을 때와 높을 때 각각 어떤 문제가 생기는가?
> A. 낮으면 JTE가 충분히 공핍되지 않아 junction edge에 전계 집중. 높으면 JTE edge에 전계 집중/type conversion.

**Q5.** Termination efficiency 정의와 목표값은?
> A. `BV_actual/BV_ideal × 100`, 목표 80–95%.

**Q6.** Bipolar degradation의 메커니즘은?
> A. Forward bias에서 BPD가 stacking fault로 확장되어 lifetime/이동도 저하 → Vf/Ron 증가.

**Q7.** TDDB Weibull plot에서 `β`와 `η`의 의미는?
> A. `β`: failure 분포 shape, `η`: 63.2% failure time(scale).

**Q8.** Trench MOSFET에서 corner rounding이 중요한 이유는?
> A. Sharp corner에 전계가 집중되어 gate oxide breakdown/신뢰성 저하가 발생하기 때문.

---

## 12. 강사 노트

| 주제 | 강조 |
|---|---|
| 접촉 | "ρc는 도핑의 함수" — 도핑이 안 되면 metal을 바꿔도 한계. |
| TLM | `L_T`보다 긴 contact, width 정의, 4-wire. |
| JTE | window와 surface charge tolerance를 **항상 같이** 본다. |
| Integration | thermal budget 순서가 설계를 지배. |
| 신뢰성 | BPD는 "소자 설계+epi+운용"의 공동 문제. |
| TCAD | BV는 mesh/impact ionization 파라미터에 민감 → 캘리브레이션 필수. |

---

## 13. 다음 주차 예고

- **Week 4**: GaN HEMT — epi/MOCVD, polarization 2DEG, isolation/recess/p-GaN, ohmic/dielectric/passivation, current collapse, 신뢰성, TCAD.

---

*© 2026 SiC/GaN Process Course Notes. 공정/시뮬레이션 값은 예시이며, 실제 설계는 측정 캘리브레이션을 전제로 합니다.*
