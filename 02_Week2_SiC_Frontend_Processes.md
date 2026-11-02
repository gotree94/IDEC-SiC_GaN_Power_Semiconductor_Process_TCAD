# Week 2 — SiC Front-End Processes

> **소요**: 강의 4h + 실습 2h
> **원 강좌 대응**: D1-PM (SiC cleaning / 산화·계면 / gate oxide / implant / activation / etching)
> **핵심 질문**: "SiC에서 산화·주입·활성화·식각은 왜 Si과 다르고, 그 차이가 소자 파라미터로 어떻게 나타나는가?"

---

## 1. 학습목표

1. SiC 세정 시퀀스와 **sacrificial oxidation**의 목적을 설명한다.
2. Deal-Grove 모델을 쓰고, SiC 산화의 **면 의존성**을 정량적으로 설명한다.
3. Dit/NIT의 기원과 **NO/N₂O nitridation**의 효과를 수치로 설명한다.
4. Al/N/P implant의 **box profile**을 설계하고, 고온 활성화의 필요성을 설명한다.
5. 캡층(AlN/C/SiC)의 역할과 **surface morphology** 이슈를 설명한다.
6. ICP 식각의 가스 화학·마스크 선택비·profile 제어를 설명한다.
7. 각 단위공정을 **TCAD 모델**로 표현하고, SIMS/SRP/C-V로 캘리브레이션한다.

---

## 2. SiC 세정 (Cleaning)

### 2.1 SiC 표면의 특수성

| 문제 | 원인 | 결과 |
|---|---|---|
| **Carbon-rich surface** | Si–C 결합, 산화 후 잔류 C | 계면 준위, 접촉저항 증가 |
| **Polishing damage** | CMP/기계 연마 | subsurface damage, 누설 |
| **Native oxide** | 자연 산화(SiO₂, ~1–2 nm) | 계면 불균일 |
| **Metal/particle** | 공정 환경 | 누설, 신뢰성 |
| **Hydrophobicity** | C-terminated | wetting 불량, 파티클 |

### 2.2 표준 세정 시퀀스(예시)

| 순서 | 용액/조건 | 목적 | 시간/온도 |
|---|---|---|---|
| 1 | Piranha (H₂SO₄:H₂O₂ = 4:1) | 유기물 제거 | 10–15 min, 120°C |
| 2 | DI rinse + HF (1–5%) | 자연산화 제거 | 10–30 s |
| 3 | SC-1 (NH₄OH:H₂O₂:H₂O) | 파티클/유기물 | 10 min, 60–70°C |
| 4 | SC-2 (HCl:H₂O₂:H₂O) | 금속 이온 | 10 min, 60–70°C |
| 5 | HF dip | 산화막 제거 | 10–30 s |
| 6 | **Sacrificial oxidation** (1100–1300°C, dry) | 손상층/잔류 C 산화 | 30–60 min |
| 7 | HF dip | sacrificial oxide 제거 | 30–60 s |
| 8 | DI rinse + N₂ dry | 건조 | — |

> **왜 sacrificial oxidation인가**: SiC는 CMP 손상층과 잔류 C가 계면 품질을 크게 좌우합니다. 얇은 열산화막을 키웠다가 제거하면 **손상층을 산화·제거**하고 **fresh surface**를 얻을 수 있습니다. Si 공정의 "sacrificial oxide"와 같은 개념이지만 SiC에서는 **필수**에 가깝습니다.

### 2.3 세정 후 평가

| 항목 | 방법 | 합격 기준(예시) |
|---|---|---|
| 표면 조도 | AFM (5×5 µm²) | RMS < 0.5 nm |
| 잔류 금속 | TXRF/ICP-MS | < 10¹⁰ atoms/cm² |
| 탄소 | XPS | C–O/C–C peak 최소화 |
| 산화막 두께 | Ellipsometry | < 1 nm (HF 후) |
| 접촉각 | Contact angle | 친수성 확보 |
| 파티클 | Laser scattering | < 10 ea/wafer (≥0.1 µm) |

---

## 3. 열산화 (Thermal Oxidation)

### 3.1 Deal-Grove 모델

```
x_ox² + A·x_ox = B(t + τ)
```

- `x_ox`: 산화막 두께
- `A = 2D_eff(1/k_s + 1/h)`
- `B = 2D_eff·C*/N₁`
- `τ`: 초기 산화막을 반영하는 시간 offset

**두 극한**
- 얇은 산화막(반응 지배): `x_ox ≈ (B/A)(t+τ)` — **linear**
- 두꺼운 산화막(확산 지배): `x_ox ≈ √(B t)` — **parabolic**

### 3.2 SiC 산화의 특징

| 항목 | Si | 4H-SiC |
|---|---|---|
| 산화 온도 | 900–1100°C | **1100–1400°C** |
| 산화 속도 | 빠름 | **매우 느림** (Si의 1/10 이하) |
| 면 의존성 | 약함 | **매우 강함** (C-face ≫ Si-face) |
| 계면 Dit | 10¹⁰–10¹¹ | **10¹²–10¹³** |
| 산화 후 C 잔류 | 적음 | **많음** (C cluster, C–O) |
| 산화막 신뢰성 | 우수 | nitridation 없으면 열위 |

**면 의존성(대표 경향)**
- (0001) Si-face: 가장 느림, 계면 품질 상대적 양호 → **MOS 채널/게이트 산화에 사용**
- (000-1) C-face: 수 배~10배 빠름, 계면 품질 열위 → **sacrificial/thick oxide**에 유리
- a-face, m-face: 중간

> **실무**: 게이트 산화는 **Si-face**에서, 희생 산화는 **C-face**에서 빠르게 하는 전략이 가능. 다만 소자 채널은 Si-face가 일반적.

### 3.3 산화 조건 예시(대표값, 장비 의존)

| 조건 | 온도 | 시간 | 두께(대략) | 용도 |
|---|---|---|---|---|
| Dry O₂ | 1200°C | 2–4 h | 20–40 nm | thin gate |
| Dry O₂ | 1300°C | 3–6 h | 40–80 nm | gate ox |
| Wet O₂ | 1100–1200°C | 1–3 h | 100–300 nm | field/mask |
| Sacrificial | 1100–1200°C | 0.5–1 h | 20–40 nm | 손상 제거 |

> ⚠️ 위 두께는 **참고용**입니다. 실제로는 웨이퍼 off-angle, 도핑, 산화로 분위기, 로딩 효과에 따라 달라집니다. **반드시 두께 vs 시간 curve를 먼저 확보**하십시오.

### 3.4 산화막 품질

| 지표 | 측정 | 목표(예시) |
|---|---|---|
| 두께 균일도 | Ellipsometry map | ±3% (1σ) |
| 굴절률 | Ellipsometry | 1.45–1.47 |
| 항복전계 | I-V (MOS cap) | > 8 MV/cm |
| 누설 | I-V @3 MV/cm | < 10⁻⁸ A/cm² |
| Dit | C-V (conductance/HF-LF) | < 1×10¹² eV⁻¹cm⁻² |
| Flatband shift | C-V | < 0.5 V (BTS 후) |
| Interface roughness | TEM/AFM | < 1 nm |

### 3.5 Nitridation (NO / N₂O)

**목적**: SiC/SiO₂ 계면의 **C-related trap**을 N으로 passivation.

```
NO 분위기, 1175–1300°C, 30 min–3 h
또는 N₂O, 1250–1300°C
```

**효과(대표 문헌 경향)**

| 항목 | nitridation 전 | 후 |
|---|---|---|
| Dit (near E_c) | 10¹³ eV⁻¹cm⁻² | 10¹¹–10¹² |
| 채널 이동도 | 1–5 cm²/V·s | **20–40** |
| Vth 안정성 | 나쁨 | 개선 |
| TDDB | 열위 | 개선 |

**메커니즘**: N이 interface의 C dangling bond/Si–C 결합을 Si≡N 또는 C–N으로 치환하고, 계면 근처에 **N-rich layer**를 형성. 과도한 nitridation은 **fixed charge**와 **mobility 저하**를 유발할 수 있어 **window**가 존재.

> **교수님 연구 연결**: TEOS 기반 LPCVD 산화막 + NO post-deposition annealing으로 4H-SiC MOSFET gate oxide를 형성한 연구(Lee, Seok, Kim, Ha, *Current Applied Physics*, 2020)는 **열산화 대안**으로서 deposited oxide의 계면 개선을 다룹니다. 열산화는 고온·저속이므로, **LPCVD/ALD + PDA** 조합은 throughput과 계면 제어를 동시에 노리는 접근입니다.

### 3.6 대체 게이트 절연막

| 재료 | 유전상수 | 장점 | 과제 |
|---|---|---|---|
| SiO₂ (thermal) | 3.9 | 성숙, 안정 | Dit, 낮은 μ_ch |
| SiO₂ + NO | 3.9 | Dit 개선 | 공정 window |
| Al₂O₃ (ALD) | ~9 | 높은 k, GaN에 적합 | SiC 계면 안정성 |
| HfO₂ | ~20 | high-k | 열적 불안정, hysteresis |
| SiNx/SiO₂ stack | 4–7 | barrier | 계면 |

> SiC MOSFET 상용 게이트는 여전히 **열산화 SiO₂ + nitridation**이 주류입니다. high-k는 연구 단계.

---

## 4. 계면 물리: Dit, NIT, Border Traps

### 4.1 용어 정리

| 용어 | 정의 | 측정 |
|---|---|---|
| **Dit** | interface trap density (eV⁻¹cm⁻²) | C-V conductance, HF-LF |
| **NIT** | near-interface traps (oxide 내 수 nm) | C-V frequency dispersion |
| **Border traps** | oxide 내부의 slow trap | hysteresis, BTI |
| **Qf** | fixed oxide charge | flatband shift |
| **Qm** | mobile ion charge | BTS |

### 4.2 Dit가 소자에 미치는 영향

```
ΔV_th = −(q/C_ox) ∫ Dit(E) dE
μ_eff ∝ 1/(1 + α·Dit)
```
- Dit↑ → **Vth shift, subthreshold slope 악화, μ_ch 저하, hysteresis**.
- SiC MOSFET의 낮은 μ_ch(20–40)의 주원인.

### 4.3 C-V로 Dit 추출(개념)

- High-low frequency C-V: `Dit = (C_ox/q)[(C_LF/C_ox −1)⁻¹ − (C_HF/C_ox −1)⁻¹]` (근사)
- Conductance method: `G_p/ω` peak → `Dit`
- 실제로는 **면적·누설·series resistance** 보정 필요.

### 4.4 신뢰성과의 연결

| 현상 | 원인 | 평가 |
|---|---|---|
| **BTI** | oxide trap charging | Vth shift vs stress time/field/temp |
| **TDDB** | oxide breakdown | Weibull, t_BD |
| **Hysteresis** | border traps | sweep 방향별 Vth |
| **Gm degradation** | interface trap generation | stress 전후 |

> 3주차에서 BTI/TDDB를 더 자세히 다룹니다.

---

## 5. 이온주입 (Ion Implantation)

### 5.1 기본 물리

- 이온이 고체에 들어가면 **전자/핵 저지**로 에너지를 잃고 정지.
- 정지 분포: **Gaussian (LSS theory)**
  ```
  N(x) = (Φ/(√(2π)ΔR_p)) exp(−(x−R_p)²/(2ΔR_p²))
  ```
- `R_p`: projected range, `ΔR_p`: straggle.
- 실제로는 **channeling tail**이 발생 → tilt/rotation, screen oxide, random angle.

### 5.2 SiC implant의 특징

| 항목 | Si | 4H-SiC |
|---|---|---|
| 도펀트 | B, P, As | **Al(p), N/P(n), B** |
| 확산 | 열확산 가능 | **거의 불가** → implant 필수 |
| 활성화 | 1000–1100°C | **1600–1800°C** |
| 이온화 | 거의 100% | **불완전** |
| Channeling | <111> 정렬 | 4° off-axis + 7° tilt |
| 고온 implant | 선택 | **300–600°C 권장** (damage 회복) |
| 최대 농도 | 10²¹ | ~10²⁰–10²¹ (solid solubility) |

### 5.3 주요 도펀트

| 도펀트 | 형 | E_a (meV) | 에너지 범위 | 도즈 범위 | 용도 |
|---|---|---|---|---|---|
| **Al** | p | 190–240 | 30–500 keV | 10¹²–10¹⁵ cm⁻² | body, JTE, p⁺ |
| **B** | p | ~300 | 20–200 keV | 10¹²–10¹⁴ | shallow p (연구) |
| **N** | n | 50–100 | 20–300 keV | 10¹³–10¹⁶ | source, channel stop |
| **P** | n | 60–100 | 30–400 keV | 10¹³–10¹⁶ | source (대안) |

### 5.4 Box profile 설계

단일 에너지로는 Gaussian이므로, **여러 에너지/도즈를 중첩**해 box를 만듭니다.

예: 0.3 µm box, 농도 1×10¹⁸ cm⁻³ (Al)
| 에너지 | 도즈 | 목적 |
|---|---|---|
| 30 keV | 2×10¹³ | 표면 |
| 80 keV | 3×10¹³ | 중간 |
| 150 keV | 4×10¹³ | 깊이 |
| 250 keV | 5×10¹³ | tail |

> 실제 도즈는 **SRIM/TCAD로 계산**하고, SIMS로 검증합니다. 도즈는 `Φ = ∫N(x)dx`.

### 5.5 마스크

| 마스크 | 두께 | 선택비 | 비고 |
|---|---|---|---|
| Photoresist | 1–3 µm | 낮음 | 고도즈/고에너지에 부적합 |
| SiO₂ | 0.5–2 µm | 중간 | 공정 호환 |
| Ni | 0.3–1 µm | 높음 | 고도즈 |
| Al | 0.5–1 µm | 높음 | 제거 용이 |
| Poly-Si | 0.3–0.5 µm | 중간 | 고온 |

> **고온 implant(300–600°C)** 는 photoresist 사용이 어려우므로 **hard mask**(SiO₂/Ni)를 사용합니다.

### 5.6 Damage와 회복

- Implant는 **amorphization**을 유발 → 1600°C 이상에서 **재결정**.
- 고온 implant는 **in-situ damage recovery**를 촉진.
- 잔류 결함은 **leakage, activation 저하, BPD/stacking fault**의 원인.
- **channeling tail**은 junction depth를 늘리고 농도 profile을 흐트립니다 → 4° off + 7° tilt.

### 5.7 TCAD에서 implant

- **Analytic (Gaussian)**: 빠름, 캘리브레이션 필요.
- **Monte Carlo (SRIM-like)**: 정확, 느림, channeling 고려.
- **Tabulated (SIMS-based)**: 실측 profile 사용 — **가장 정확**.

> 실무 순서: SRIM으로 1차 설계 → TCAD analytic/MC → SIMS로 검증 → table/profile 보정.

---

## 6. 고온 활성화 (Activation Anneal)

### 6.1 왜 1600–1800°C인가

- SiC의 도펀트는 **치환 자리**에 들어가야 활성화.
- Al의 경우 ~1600°C 이상에서 충분한 이동도와 재결정.
- SiC는 **1650°C 이상에서도 안정** (melting ~2830°C).
- 그러나 **표면 decomposition**(Si sublimation)이 문제 → **cap layer** 필요.

### 6.2 캡층

| 캡 | 증착 | 장점 | 단점 |
|---|---|---|---|
| **AlN** | Sputter/MOCVD | 고온 안정, 산소 barrier | 후속 제거(인산) |
| **Carbon** | Photoresist carbonization | 저가, conformal | 제거(O₂ plasma), 잔류 |
| **SiC** | PECVD | 열팽창 일치 | 증착/식각 필요 |
| **Face-to-face** | SiC wafer | 오염 없음 | 균일도 한계 |
| **SiO₂** | PECVD | 쉬움 | 고온에서 불안정 |

### 6.3 활성화 조건(대표)

| 조건 | 온도 | 시간 | 분위기 | 비고 |
|---|---|---|---|---|
| RTA | 1600–1700°C | 30 s–5 min | Ar | 짧은 thermal budget |
| Furnace | 1600–1650°C | 30–60 min | Ar/Si | 균일도 |
| Microwave | 1600–1800°C | 분 | — | 연구 단계 |

### 6.4 활성화율과 sheet resistance

| 도펀트 | 도즈 | 활성화율(대표) | Rs(대표) |
|---|---|---|---|
| Al (p⁺) | 1×10¹⁵ | 50–90% | 1–10 kΩ/sq |
| N (n⁺) | 1×10¹⁵ | 80–95% | 100–500 Ω/sq |
| Al (JTE) | 1×10¹³ | 30–70% | — |

> 활성화율은 **도즈·온도·시간·캡·결정면**에 의존. **Hall + SRP**로 확인.

### 6.5 표면 morphology

- 고온 활성화 후 **step bunching**, **pits**, **Si droplet** 발생 가능.
- 원인: off-angle, cap 불균일, Si overpressure 부족.
- 대책: **Si overpressure**(SiC 코팅 susceptor), **AlN cap**, **최적 ramp rate**.
- 평가: AFM RMS, Nomarski, defect map.

> 표면 거칠기는 후속 **gate oxide 품질**과 **photolithography**에 직접 악영향. 2주차에서 가장 실무적으로 까다로운 부분 중 하나.

---

## 7. 식각 (Etching)

### 7.1 SiC 건식식각

SiC는 **화학적으로 불활성** → 습식식각 거의 불가 → **ICP-RIE** 사용.

**대표 가스 화학**
```
SF₆ (F 라디칼) + O₂ (SiOx 제거) + Ar (이온 bombard)
또는 Cl₂/BCl₃ (research)
```

| 파라미터 | 대표 범위 | 영향 |
|---|---|---|
| Source power | 500–1500 W | 플라즈마 밀도 |
| Bias power | 100–300 W | 이온 에너지, damage |
| Pressure | 5–20 mTorr | mean free path |
| SF₆ | 10–30 sccm | F 라디칼 |
| O₂ | 2–10 sccm | sidewall passivation |
| Ar | 5–20 sccm | physical etch |
| Stage temp | 20–200°C | polymer, selectivity |

**식각률**: 0.3–1 µm/min (장비 의존)
**선택비**: Ni 10–30:1, SiO₂ 2–5:1, PR 1–3:1

### 7.2 Profile 제어

| 목표 | 방법 |
|---|---|
| 수직 sidewall | O₂ 증가(sidewall passivation), bias↑ |
| Taper | O₂ 감소, pressure↑ |
| Smooth surface | Ar↑, bias↓, 후속 sacrificial oxidation |
| Corner rounding | sacrificial oxidation, H₂ anneal |
| Notch 억제 | end-point detection, low bias overetch |

### 7.3 Trench 식각

- 깊이 1–2 µm, 폭 0.5–2 µm.
- **corner rounding**이 중요: sharp corner → 전계 집중 → oxide breakdown.
- 방법: sacrificial oxidation(30–50 nm) 후 제거, 또는 H₂ annealing(1500–1600°C).
- **tilted implant**로 trench bottom doping(교수님 2023 JJAP 논문) → bottom oxide 전계 완화.

### 7.4 Mesa isolation

- SiC MOSFET 간 isolation, GaN HEMT mesa.
- SiC: ICP etch 1–3 µm (epi 두께 이상).
- GaN: Cl₂/BCl₃ ICP, 100–300 nm (2DEG 차단).
- 평가: isolation leakage, step height, sidewall angle.

### 7.5 Wet etch (defect delineation)

- **KOH/NaOH molten** (400–500°C) 또는 electrochemically assisted KOH.
- 용도: **defect etch pit** (BPD, TSD, TED), 연구/분석.
- 소자 공정에는 사용하지 않음.

---

## 8. Metrology & Inspection

| 목적 | 장비 | 출력 | 캘리브레이션 대상 |
|---|---|---|---|
| 도핑 profile | SIMS | concentration vs depth | implant/activation |
| 활성 농도 | SRP (spreading resistance) | active carrier | activation model |
| 이동도/면저항 | Hall | μ, Rs, sheet n | mobility model |
| 산화막 두께 | Ellipsometry | t_ox, n | oxidation model |
| 계면 Dit | C-V (Hg/MOS) | Dit(E) | interface trap model |
| 결정 결함 | TEM, XRT, etch pit | dislocation density | defect model |
| 표면 조도 | AFM | RMS | — |
| 조성 | XPS, AES | C/O/Si ratio | — |
| 두께/단면 | SEM, TEM | profile | — |
| 응력/휨 | Bow/warp | stress | — |
| 파티클 | Laser scatter | count/map | — |

### 8.1 SIMS 캘리브레이션 실무

1. **Reference sample**(implanted standard)로 sensitivity factor 결정.
2. Matrix effect 보정(SiC vs SiO₂).
3. Depth scale: **profilometer/TEM**으로 crater depth 측정.
4. **Al tail**은 SIMS detection limit와 함께 해석.
5. TCAD profile과 overlay → 필요 시 **table model**로 대체.

### 8.2 SRP/Hall

- SRP: bevel + probe → **active carrier profile**.
- Hall: van der Pauw → **sheet concentration, mobility**.
- **불완전 이온화** 때문에 SIMS(chemical)와 SRP(electrical)가 다를 수 있음 → **이 차이가 activation rate**.

---

## 9. 공정 Integration & Thermal Budget

### 9.1 SiC MOSFET 전공정 순서(상세)

| # | 공정 | 온도 | 핵심 이슈 | 측정 |
|---|---|---|---|---|
| 1 | Substrate/epi 준비 | — | epi 두께/도핑 | SIMS, C-V |
| 2 | Mark/alignment | — | off-angle 정렬 | — |
| 3 | JTE implant (Al) | RT~500°C | 도즈 window | SIMS |
| 4 | P-body implant (Al) | RT~500°C | box profile | SIMS |
| 5 | N⁺ source implant | RT~500°C | shallow junction | SIMS |
| 6 | P⁺ contact implant | RT~500°C | contact | SIMS |
| 7 | Activation | **1600–1700°C** | cap, morphology | SRP, AFM |
| 8 | Sacrificial oxidation | 1100–1200°C | damage 제거 | Ellipsometry |
| 9 | Gate oxidation | 1200–1400°C | Dit | C-V |
| 10 | Nitridation (NO) | 1175–1300°C | Dit, μ_ch | C-V |
| 11 | Poly-Si gate | 600–650°C | doping, etch | — |
| 12 | ILD | 400–700°C | step coverage | — |
| 13 | Contact etch | — | selectivity | SEM |
| 14 | Ohmic anneal | 950–1050°C | ρc | TLM |
| 15 | Passivation | 300–400°C | adhesion | — |
| 16 | Pad/backside | 300–400°C | solderability | — |

### 9.2 Thermal budget 규칙

```
T_max(activation) > T_gate_ox > T_ohmic > T_passivation > T_pad
```

- **역순으로 가면** 앞 공정이 손상됩니다.
- SiC에서 **ohmic anneal(1000°C)** 은 gate oxide(이미 형성)에 stress → **짧은 RTA**와 **온도 ramp 최적화** 필수.
- 실제로는 **ohmic을 gate ox 전에** 하는 integration도 연구됩니다(공정 순서 trade-off).

### 9.3 결함 공학

| 결함 | 영향 | 대책 |
|---|---|---|
| **BPD** | bipolar degradation (stacking fault) | 저 BPD epi, gettering, 소자 설계 |
| **TSD** | leakage, breakdown | epi 품질 |
| **TED** | leakage (minor) | epi |
| **Stacking fault** | Ron 증가, Vf drift | forward stress 최소화 |
| **Carrots/polytype** | killer defect | wafer inspection |
| **Micropipe** | killer (과거) | wafer spec |

> **BPD/stacking fault**는 SiC PiN/MOSFET body diode의 **forward bias**에서 확장되어 **Ron 증가**를 유발합니다. 3주차 신뢰성에서 자세히 다룹니다.

---

## 10. TCAD 모델링 (SProcess)

### 10.1 산화

**Sentaurus SProcess 개념**
```
Diffuse {
  Time = 3600  Temp = 1300  O2 = 1.0
}
```
- 모델: Deal-Grove + SiC-specific rate.
- 캘리브레이션: **두께 vs 시간** curve를 맞춘 뒤, **면 의존성** 파라미터 조정.
- `O2`/`H2O` partial pressure, `H2` addition 고려.

### 10.2 Implant

```
Implant {
  Phosphorus  Dose = 1e15  Energy = 100  Tilt = 7  Rotation = 22
}
```
- analytic vs Monte Carlo.
- **channeling**: `Tilt`/`Rotation`, screen oxide.
- **damage**: amorphization model, 후속 anneal.

### 10.3 Activation/Diffusion

```
Diffuse {
  Time = 60  Temp = 1650  Ambient = Ar
}
```
- SiC는 diffusion이 거의 없으므로 **diffusion model을 끄거나 최소화**.
- **activation model**: solid solubility, clustering, incomplete ionization.
- 캘리브레이션: **SRP/Hall**로 active profile 비교.

### 10.4 식각

- TCAD는 식각을 **geometry 연산**으로 처리.
- `Etch { Material = ... Time = ... }` 또는 structure editor.
- Trench angle/corner rounding은 **geometry로 직접 반영**.
- 식각 damage는 **trap/damage layer**로 근사.

### 10.5 캘리브레이션 순서

```
1) 산화 두께 (TEM/ellipsometry)
2) Implant profile (SIMS)
3) Active profile (SRP/Hall)
4) Sheet resistance (Hall)
5) C-V (Dit, Vfb)
6) I-V (BV, Ron)
```

> **각 단계가 독립적으로 맞아야** 다음 단계로 갑니다. 한 번에 다 맞추려 하면 파라미터가 얽힙니다.

---

## 11. 실습 (2h)

### 실습 2-A: SRIM으로 implant profile
- Al 100 keV, 300 keV를 SiC에 implant.
- `R_p`, `ΔR_p`를 읽고 Gaussian으로 box profile 설계.
- 4° tilt의 영향(개념) 논의.

### 실습 2-B: 산화 두께 vs 시간
- 문헌/장비 데이터로 `x_ox` vs `t` 그래프 작성.
- Deal-Grove `A`, `B`를 fitting.
- 1300°C에서 50 nm를 얻는 시간을 추정.

### 실습 2-C: SProcess 1D
- 1D 구조에 Al implant → activation → SRP profile 추출.
- SIMS(가상)와 overlay.
- **incomplete ionization** on/off에 따른 active concentration 차이 기록.

### 실습 2-D: 공정 window 표
- JTE dose 5×10¹²–5×10¹³ cm⁻² 범위에서 BV를 시뮬레이션.
- BV > 1200 V를 만족하는 **dose window**를 표로 정리.
- (3주차 JTE 설계와 연결)

---

## 12. 퀴즈 8문항

**Q1.** SiC에서 sacrificial oxidation을 하는 이유 2가지는?
> A. CMP 손상층 제거, 잔류 C/오염 제거로 fresh interface 확보.

**Q2.** Deal-Grove 식에서 얇은 산화막과 두꺼운 산화막의 지배 메커니즘은?
> A. 얇을 때는 계면 반응(linear), 두꺼울 때는 산화제 확산(parabolic).

**Q3.** Si-face와 C-face의 산화 속도 차이와 그 이유는?
> A. C-face가 수 배~10배 빠름. 표면 원자 배열/결합 에너지 차이로 산화 반응 속도가 다르기 때문.

**Q4.** NO anneal이 Dit를 줄이는 메커니즘은?
> A. N이 계면의 C dangling bond/Si–C 결합을 passivation하여 interface trap을 감소.

**Q5.** SiC에서 열확산 대신 implant를 쓰는 이유는?
> A. 도펀트 확산계수가 매우 작아 실용적 junction 형성이 어렵기 때문.

**Q6.** 고온 활성화에서 cap layer가 필요한 이유는?
> A. 1600°C 이상에서 Si sublimation/표면 decomposition을 억제하기 위해.

**Q7.** ICP 식각에서 O₂를 넣는 이유는?
> A. SiOx sidewall passivation으로 수직 profile을 형성하고, Si 재증착을 억제.

**Q8.** SIMS와 SRP의 차이가 의미하는 것은?
> A. SIMS는 chemical concentration, SRP는 active carrier concentration → 차이가 activation rate/불완전 이온화.

---

## 13. 강사 노트

| 주제 | 강조 포인트 |
|---|---|
| 산화 | "SiC 산화는 Si 산화의 느린 버전"이 아니라 **계면 화학이 다름**. C 잔류가 핵심. |
| Nitridation | Dit 개선과 mobility trade-off. **과nitridation은 fixed charge** 증가. |
| Implant | "도즈 계산"보다 **profile tail과 activation**이 실제 소자에 중요. |
| Activation | 캡층과 morphology가 **후속 수율**을 좌우. |
| Etch | corner rounding이 **신뢰성**을 좌우. |
| TCAD | "예쁜 그림"보다 **SIMS/SRP/C-V 캘리브레이션**. |

---

## 14. 다음 주차 예고

- **Week 3**: Ohmic/Schottky 접촉, TLM, SiC MOSFET integration, edge termination(JTE/guard ring/FP), BPD 신뢰성, TCAD BV 검증.

---

*© 2026 SiC/GaN Process Course Notes. 공정 조건은 대표값이며, 실제 레시피는 장비·웨이퍼·목표에 따라 최적화해야 합니다.*
