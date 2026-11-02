# Week 4 — GaN HEMT Process, Reliability & Characterization

> **소요**: 강의 4h + 실습 2h
> **원 강좌 대응**: D2-PM (GaN epi / isolation·recess·normally-off / ohmic·dielectric / passivation·field plate / 결함·신뢰성 / integration·평가)
> **핵심 질문**: "2DEG를 어떻게 만들고, normally-off를 어떻게 구현하며, dynamic Ron을 어떻게 억제하고 보증하는가?"

---

## 1. 학습목표

1. AlGaN/GaN **polarization 2DEG**를 정량적으로 계산한다.
2. MOCVD epi stack과 **buffer doping(C, Fe)** 의 역할을 설명한다.
3. **normally-off** 구현 3방식(p-GaN, recess, cascode)을 비교한다.
4. Ti/Al/Ni/Au ohmic, ALD Al₂O₃/SiNx gate dielectric, SiNx passivation의 **공정 조건**을 설명한다.
5. **current collapse / dynamic Ron**의 원인과 field plate 설계를 설명한다.
6. HTRB/HTGB/HTOL/H3TRB, AEC-Q101, JEDEC 기준을 매핑한다.
7. TCAD에서 **polarization + trap + self-heating**을 설정한다.

---

## 2. GaN 물성과 분극(Polarization)

### 2.1 Wurtzite 구조

GaN는 **wurtzite** 결정구조로, c축 방향으로 **Ga 원자면과 N 원자면**이 교대합니다.
- **Spontaneous polarization** `P_sp`가 c축 방향으로 존재.
- **Piezoelectric polarization** `P_pz`가 strain에 의해 추가.
- AlGaN/GaN 이종접합에서 **polarization discontinuity** → **2DEG**.

### 2.2 Polarization 값

| 재료 | `P_sp` (C/m²) | 비고 |
|---|---|---|
| GaN | −0.034 | 기준 |
| AlN | −0.090 | 큰 분극 |
| AlGaN | 선형 보간 | 조성 의존 |

**AlGaN/GaN 계면의 polarization sheet charge**
```
σ(x) = P_sp(AlGaN) + P_pz(AlGaN) − P_sp(GaN)
```
- `x` = Al 조성.
- `x = 0.25–0.30`에서 `σ ≈ 0.012–0.016 C/m²`.
- `σ/q ≈ 0.8–1.3×10¹³ cm⁻²`.

### 2.3 2DEG 농도

```
n_s(x) = σ(x)/q − (ε_0 ε_r/(q·d))·(qφ_b + E_F − ΔE_c)
```
- `d`: AlGaN 두께
- `φ_b`: gate barrier
- `ΔE_c`: conduction band offset
- `E_F`: Fermi level relative to E_c

**간이 계산(예시, x=0.25, d=25 nm)**
- `σ/q ≈ 1.0×10¹³ cm⁻²`
- depletion term ≈ `2–3×10¹² cm⁻²`
- `n_s ≈ 7–8×10¹² cm⁻²`

**측정 대표값**
| 항목 | 값 |
|---|---|
| n_s (Hall) | 0.8–1.2×10¹³ cm⁻² |
| μ (2DEG) | 1500–2000 cm²/V·s |
| R_sh | 300–500 Ω/sq |
| AlGaN Eg (x=0.25) | ~3.9 eV |
| ΔE_c | ~0.3–0.4 eV |

### 2.4 Band diagram(개념)

```
E_c
 │      AlGaN            GaN
 │   ┌──────┐
 │   │      │╲
 │   │      │ ╲___ 2DEG (E_F below E_c)
 │───┘      │
 └────────────────────────→
```

> **중요**: 2DEG는 **도핑 없이** 분극으로 형성됩니다. 따라서 **이온화 산란**이 적어 고이동도. 대신 **분극의 안정성**이 공정·스트레스·온도에 민감.

### 2.5 분극의 온도/스트레스 의존

- 온도↑ → lattice constant 변화 → `P_sp`, `P_pz` 변화 → `n_s` 변화.
- 기판/버퍼와의 **열팽창 계수 차이** → wafer bow, crack.
- Si 기판: CTE mismatch 큼 → **buffer engineering** 필수.

---

## 3. Epitaxy (MOCVD)

### 3.1 기판 선택

| 기판 | 장점 | 단점 | 용도 |
|---|---|---|---|
| **Si** | 저가, 대구경(150–200 mm) | CTE/lattice mismatch, crack | 650 V 상용 |
| **SiC** | 열전도, lattice match | 고가 | 고전력/고주파 |
| **Sapphire** | 저가, 절연 | 열전도 낮음, 크기 | LED/연구 |
| **GaN (bulk)** | 최고 품질 | 매우 고가, 소구경 | 연구/고신뢰 |

> **실무**: 650 V GaN HEMT는 **Si 기판**이 주류. EV/고전력은 SiC 기판 또는 GaN-on-GaN 연구.

### 3.2 Epi stack (Si 기판 예)

```
[1] AlN nucleation layer (100–200 nm)
[2] AlGaN/AlN superlattice or graded AlGaN buffer
[3] C-doped GaN buffer (1–3 µm, C ~10¹⁸–10¹⁹ cm⁻³)
[4] GaN channel (100–300 nm, unintentionally doped)
[5] AlGaN barrier (20–25 nm, x=0.2–0.3)
[6] GaN cap (1–3 nm)
```

**각 층의 역할**

| 층 | 역할 | 핵심 변수 |
|---|---|---|
| Nucleation | Si와 GaN 사이 wetting, dislocation 제어 | AlN 두께, 성장 온도 |
| Superlattice/buffer | stress 완화, crack 억제 | 주기, 조성 |
| C-doped GaN | **vertical leakage 차단** | C 농도, profile |
| Channel | 2DEG transport | 두께, 배경 도핑 |
| Barrier | polarization, Vth | Al 조성, 두께 |
| Cap | surface 보호, ohmic | 두께 |

### 3.3 C-doped buffer

- C는 **deep acceptor**로 buffered layer를 **반절연**으로 만듦.
- 장점: vertical leakage 감소, BV 증가.
- 단점: **current collapse / dynamic Ron**의 원인 (buffer trapping).
- trade-off: C↑ → leakage↓, dynamic Ron↑.

**대안**: Fe doping, superlattice, AlGaN back barrier.

### 3.4 MOCVD 공정 변수

| 변수 | 영향 |
|---|---|
| Temperature | crystal quality, C incorporation |
| Pressure | uniformity, growth rate |
| V/III ratio | C incorporation, morphology |
| TMGa/TMAl/NH₃ | 조성, growth rate |
| Growth rate | dislocation, thickness |
| In-situ monitoring | reflectance, temperature |

### 3.5 Epi 평가

| 항목 | 방법 | 목표(예시) |
|---|---|---|
| Thickness | XRD, ellipsometry, SIMS | ±2% |
| Composition | XRD, SIMS | ±0.02 |
| Dislocation | XRD rocking curve, TEM | < 5×10⁸ cm⁻² |
| Surface roughness | AFM | RMS < 0.5 nm |
| Sheet resistance | Hall, eddy current | 300–500 Ω/sq |
| Bow/warp | Bow meter | < 50 µm (Si 200 mm) |
| Crack | optical microscope | none |

---

## 4. Device Structures & normally-off

### 4.1 Normally-on HEMT

```
source ──── gate ──── drain
          (Schottky)
   ┌──────────────────────┐
   │  AlGaN barrier       │
   │  GaN channel         │
   │  buffer              │
   └──────────────────────┘
```
- 2DEG가 **zero gate bias에서도 존재** → normally-on.
- 장점: 낮은 Ron, 빠른 스위칭.
- 단점: **fail-safe 아님** → 전원 인가 시 단락 위험.

### 4.2 normally-off 구현

| 방식 | 원리 | Vth | 장점 | 단점 |
|---|---|---|---|---|
| **p-GaN gate** | p-GaN이 2DEG를 depletion | 1–2 V | 상용 주류, 안정 | p-GaN etch damage, gate leakage |
| **Recess gate** | AlGaN/barrier 제거 → 2DEG 차단 | 0.5–1.5 V | 구조 단순 | etch damage, uniformity |
| **Cascode** | low-voltage Si MOSFET + HEMT | 2–3 V | normally-off 용이 | 시스템 복잡, parasitic |
| **Fluorine implant** | F가 2DEG compensate | 0.5–1 V | 간단 | 신뢰성/열안정 |
| **Hybrid** | p-GaN + recess | 1–2 V | 성능 | 공정 복잡 |

### 4.3 p-GaN gate

```
[1] p-GaN epi (Mg ~10¹⁹–10²⁰, 50–100 nm)
[2] gate pattern
[3] p-GaN etch (stop on AlGaN)
[4] gate metal (Pd/Au, Ti/Au, Ni/Au)
[5] activation (700–800°C)
```
- p-GaN이 **2DEG를 depletion** → Vth positive.
- **Mg activation**이 핵심: H passivation 제거.
- **gate leakage**: Schottky/p-GaN junction.
- **Vth 안정성**: p-GaN/dielectric interface trap.

### 4.4 Recess gate

- AlGaN barrier를 **부분/완전 제거** → 2DEG 농도 감소 → Vth positive.
- etch depth 제어가 **Vth uniformity**를 좌우.
- **ICP Cl₂/BCl₃**, low damage.
- **etch stop layer**(AlN) 또는 **in-situ monitoring**.

### 4.5 Cascode

- **normally-on HEMT** + **low-voltage Si MOSFET** 직렬.
- Si MOSFET off → HEMT source floating → 2DEG 차단.
- 장점: normally-off, HEMT 성능 유지.
- 단점: **parasitic inductance**, **reverse recovery**, **시스템 복잡**.

### 4.6 Field plate

- Gate/source에 연결된 metal이 **표면 전계 완화**.
- **source-connected FP**: gate charge 감소, switching 개선.
- **gate-connected FP**: 전계 완화, gain.
- 설계: FP length, oxide thickness, overlap.

---

## 5. Process Flow

### 5.1 Isolation

| 방식 | 방법 | 특징 |
|---|---|---|
| **Mesa** | ICP etch 100–300 nm | 간단, topography |
| **Implant** | N/F/He, 10¹⁴–10¹⁵ cm⁻² | planar, annealing |
| **Oxygen** | implantation | leakage |

- Mesa: Cl₂/BCl₃ ICP, sidewall angle, passivation.
- 평가: isolation leakage (< 1 µA/mm), breakdown.

### 5.2 Ohmic contact

```
Ti (20 nm) / Al (100 nm) / Ni (40 nm) / Au (50 nm)
→ RTA 800–900°C, 30 s, N₂
```
- Ti → TiN/TiSi? GaN와 반응해 **TiN** 형성, Al이 n-type 도핑.
- Ni: diffusion barrier; Au: oxidation 방지/접착.
- `ρc ~ 10⁻⁶–10⁻⁵ Ω·cm²`.
- **annealing 온도**가 높으면 Al melting/agglomeration → morphology.
- 대안: Ta/Al, Ti/Al/Ti/TiN, recessed ohmic.

### 5.3 Gate dielectric

| 재료 | 두께 | 증착 | 특징 |
|---|---|---|---|
| **Al₂O₃** | 10–30 nm | ALD 200–300°C | 높은 k(~9), 낮은 leakage |
| **SiNx** | 20–50 nm | PECVD | 계면 안정, MIS |
| **SiO₂** | 20–40 nm | PECVD/ALD | 낮은 k |
| **HfO₂** | 10–20 nm | ALD | high-k, hysteresis |
| **AlON** | 10–20 nm | ALD | 중간 |

**ALD Al₂O₃**가 가장 일반적. **pre-treatment**(NH₃ plasma, HCl)로 계면 산화 제거.

### 5.4 Gate metal

- **Schottky gate**: Ni/Au, Pt/Au, W.
- **MIS gate**: Ti/Au, Al, W on dielectric.
- **p-GaN gate**: Pd/Au, Ti/Au, Ni/Au.

### 5.5 Passivation

- **SiNx (PECVD)**: 100–200 nm, 300–400°C.
- 목적: surface trap passivation, humidity barrier, mechanical.
- **pre-passivation treatment**: NH₃ plasma, N₂ plasma, HCl.
- **SiNx/GaN interface**가 current collapse를 좌우.

### 5.6 Field plate & pad

- FP: Ti/Au or Al, 0.5–2 µm.
- ILD: SiO₂/SiNx.
- Pad: Au, Cu.

### 5.7 Backside/package

- Si 기판: thinning, backside metal, die attach.
- **thermal via** for heat.
- Package: QFN, TO-247, embedded.

---

## 6. Traps & Current Collapse

### 6.1 Trap 종류

| Trap | 위치 | 에너지 | 영향 |
|---|---|---|---|
| **Surface trap** | SiNx/GaN, AlGaN surface | 0.5–1.0 eV | virtual gate, current collapse |
| **Buffer trap (C)** | C-doped GaN | 0.9–1.2 eV | dynamic Ron, BV |
| **Barrier trap** | AlGaN | — | Vth shift |
| **Gate dielectric trap** | Al₂O₃/SiNx | — | BTI, hysteresis |
| **Dislocation trap** | threading dislocation | — | leakage |

### 6.2 Current collapse

**메커니즘**
1. High drain bias → **hot electrons** inject into surface/buffer traps.
2. Trapped charge → **virtual gate** → 2DEG depletion.
3. Ron 증가, drain current 감소 (collapse).

**평가**
- **Pulsed I-V**: quiescent bias vs pulse.
- **Dynamic Ron**: switching 후 Ron/Ron,initial.
- **Transient**: recovery time constant.

### 6.3 억제 방법

| 방법 | 원리 |
|---|---|
| **SiNx passivation** | surface trap passivation |
| **Field plate** | surface field 완화, hot electron 억제 |
| **Buffer optimization** | C 농도/위치, superlattice |
| **Back barrier** | electron confinement |
| **Gate dielectric** | interface quality |
| **Surface treatment** | NH₃/N₂ plasma, HCl |

### 6.4 Dynamic Ron

```
Dynamic Ron ratio = Ron(t) / Ron(initial)
```
- 목표: < 1.3 (상용 수준), 연구 < 1.2.
- 측정: hard switching, inductive load, different V_ds, T.

### 6.5 TCAD trap 모델

```
Traps {
  (EnergeticDistribution = Single  Level = 0.6  Density = 1e13
   CaptureCrossSection = 1e-15  Type = Acceptor)
}
```
- surface trap: 0.5–0.9 eV
- buffer trap: 0.9–1.2 eV
- capture cross section: 10⁻¹⁵–10⁻¹³ cm²
- **transient** simulation으로 dynamic Ron 재현.

---

## 7. Reliability

### 7.1 주요 시험

| 시험 | 조건 | 목적 | 기준(예) |
|---|---|---|---|
| **HTRB** | 150°C, 80% BV, 1000 h | drain leakage, buffer | < 10 µA/mm |
| **HTGB** | 150°C, Vgs = +/−20 V, 1000 h | gate oxide | ΔVth < 0.5 V |
| **HTOL** | 150°C, operating, 1000 h | lifetime | — |
| **H3TRB** | 85°C/85% RH, 80% BV, 1000 h | humidity | < 10 µA/mm |
| **TC** | −55~150°C, 1000 cycles | package | no crack |
| **PC** | ΔTj 100°C, 10⁵ cycles | solder | — |
| **ESD** | HBM/CDM | robustness | class |
| **UIS** | inductive | avalanche | — |

> **AEC-Q101**은 자동차용 이산소자 신뢰성 표준. **JEDEC JESD22/JESD47**은 범용.

### 7.2 열화 메커니즘

| 현상 | 원인 | 결과 |
|---|---|---|
| **Vth drift** | gate dielectric trap, p-GaN | threshold shift |
| **Gate leakage** | dielectric defect, Schottky | Idss 증가 |
| **Dynamic Ron** | buffer/surface trap | 손실 증가 |
| **Current collapse** | surface trap | 전류 감소 |
| **TDDB** | gate dielectric | breakdown |
| **Electromigration** | metal | open/short |
| **Delamination** | package | thermal |

### 7.3 수명 모델

- **Arrhenius**: `t_f = A·exp(E_a/kT)`
- **Power law**: `t_f ∝ V^(-n)`
- **Weibull**: failure distribution.
- 가속계수: `AF = exp(E_a/k(1/T_use − 1/T_stress))`.

### 7.4 신뢰성 설계 원칙

1. **Field plate**로 surface field < 3 MV/cm.
2. **SiNx passivation** 품질.
3. **buffer C 농도** 최적화 (leakage vs dynamic Ron).
4. **gate dielectric** 두께/품질.
5. **thermal design** (junction temp).
6. **package** 신뢰성.

---

## 8. Characterization

| 항목 | 방법 | 출력 |
|---|---|---|
| n_s, μ | Hall | sheet density, mobility |
| R_sh | Eddy current, TLM | Ω/sq |
| ρc | TLM | Ω·cm² |
| Epi thickness | XRD, ellipsometry | nm |
| Al composition | XRD, SIMS | x |
| Dislocation | XRD, TEM | cm⁻² |
| Interface trap | C-V, conductance | Dit |
| Id-Vg, Id-Vd | DC | Vth, gm, Ron |
| BV | I-V | V |
| Pulsed I-V | pulse | current collapse |
| Dynamic Ron | switching | ratio |
| Transient | drain current | trap time constant |
| EL | imaging | defects, hot spots |
| DLTS | capacitance transient | trap level |
| Thermal | Raman, IR | T_j |

### 8.1 C-V 측정

- **MIS C-V**: dielectric thickness, Vth, hysteresis.
- **Heterostructure C-V**: 2DEG density profile.
- 주의: **series resistance**, **leakage**, **frequency dispersion**.

### 8.2 Pulsed I-V

- Quiescent bias `(V_gsq, V_dsq)`에서 trap filling.
- Pulse 후 `Id` 측정 → collapse 정도.
- `V_dsq` 증가 → collapse 증가.

### 8.3 Dynamic Ron

- Hard-switching test circuit.
- `Ron = V_ds/Id` at on-state.
- `t_on`, `V_ds,off`, `T` variation.

---

## 9. TCAD for GaN HEMT

### 9.1 모델

| 물리 | 모델 |
|---|---|
| Polarization | spontaneous + piezoelectric |
| Heterointerface | AlGaN/GaN band offset |
| Mobility | 2DEG mobility, field dependent |
| Traps | surface/buffer/dielectric |
| Self-heating | thermodynamic |
| Impact ionization | GaN parameters |
| Gate leakage | tunneling/Schottky |

### 9.2 Sentaurus 설정(개념)

```
Physics {
  Piezoelectric
  Thermodynamic
  Mobility(HighFieldSaturation)
  Recombination(SRH)
}
Physics(Material="AlGaN") {
  Piezoelectric
}
```

**Heterointerface**
```
HeteroInterface {
  Material1 = "AlGaN"  Material2 = "GaN"
  Model = "Polarization"
}
```

### 9.3 Trap 설정

```
Traps {
  (EnergeticDistribution = Single  Level = 0.6
   Density = 1e13  CaptureCrossSection = 1e-15
   Type = Acceptor)
}
```

### 9.4 Self-heating

- `Thermodynamic` + thermal conductivity.
- `ThermalContact` at bottom.
- `T` dependent mobility/velocity.

### 9.5 캘리브레이션

| 단계 | 데이터 |
|---|---|
| 1 | Hall `n_s`, `μ` |
| 2 | C-V (epi) |
| 3 | Id-Vg (Vth, gm) |
| 4 | Id-Vd (Ron, saturation) |
| 5 | BV/leakage |
| 6 | pulsed I-V / dynamic Ron (traps) |
| 7 | thermal (Rth) |

### 9.6 주의

- **polarization charge**를 직접 넣는 방법 vs **piezoelectric model**.
- **mesh** at heterointerface (≤1 nm).
- **trap** 파라미터는 측정 의존 → 과도한 fitting 금지.
- **self-heating** 없으면 고전류에서 과대평가.

---

## 10. 실습 (2h)

### 실습 4-A: 2DEG 계산
- Al 조성 0.2/0.25/0.3, 두께 20/25/30 nm에서 `σ/q`, `n_s` 계산.
- Hall 데이터와 비교.
- `R_sh = 1/(q n_s μ)` 계산.

### 실습 4-B: HEMT TCAD
- AlGaN/GaN HEMT 2D 구조.
- `Id-Vg`, `Id-Vd`, `Vth`, `gm`, `Ron`.
- polarization on/off 비교.

### 실습 4-C: Trap & dynamic Ron
- surface trap 0.6 eV, 10¹³ cm⁻² 추가.
- quiescent bias 후 transient → Ron recovery.
- trap density vs dynamic Ron.

### 실습 4-D: 신뢰성 매트릭스
- 제품 spec(650 V/30 A)에 대해 HTRB/HTGB/H3TRB/TC/PC 조건 표 작성.
- AEC-Q101 항목 매핑.
- 실패 시 8D/RCA 관점 분석.

---

## 11. 퀴즈 8문항

**Q1.** GaN HEMT에서 2DEG가 형성되는 이유는?
> A. AlGaN/GaN 이종접합의 spontaneous/piezoelectric polarization 차이로 계면에 sheet charge가 생기고, 전자가 이를 compensate하기 위해 축적되기 때문.

**Q2.** AlGaN 조성 25%에서 대표적인 n_s와 μ는?
> A. n_s ~0.8–1.2×10¹³ cm⁻², μ ~1500–2000 cm²/V·s.

**Q3.** C-doped buffer의 trade-off는?
> A. Vertical leakage 감소 vs buffer trapping에 의한 dynamic Ron 증가.

**Q4.** p-GaN gate의 Vth 결정 요인은?
> A. p-GaN 두께/도핑, Mg activation, gate metal, AlGaN barrier 두께.

**Q5.** Current collapse의 메커니즘은?
> A. High field에서 hot electron이 surface/buffer trap에 포획되어 virtual gate를 형성, 2DEG를 depletion.

**Q6.** Field plate가 current collapse를 줄이는 이유는?
> A. 표면 전계를 완화해 hot electron injection과 trap charging을 감소시키기 때문.

**Q7.** HTRB와 HTGB의 차이는?
> A. HTRB는 high temp + reverse drain bias(누설/termination), HTGB는 high temp + gate bias(gate oxide).

**Q8.** TCAD에서 self-heating을 켜지 않으면 어떤 오차가 생기는가?
> A. 고전류에서 실제 온도 상승에 의한 mobility 저하/Ron 증가를 반영하지 못해 Ron을 과소평가.

---

## 12. 강사 노트

| 주제 | 강조 |
|---|---|
| Polarization | "도핑 없이 2DEG" — 분극이 핵심. 조성/두께 민감. |
| Epi | C-doped buffer의 trade-off가 dynamic Ron의 핵심. |
| normally-off | p-GaN이 상용 주류. recess는 uniformity, cascode는 system. |
| Ohmic | annealing morphology와 ρc trade-off. |
| Passivation | SiNx 품질이 신뢰성의 절반. |
| 신뢰성 | "dynamic Ron"은 GaN 고유의 평가 항목. |
| TCAD | trap 파라미터 fitting은 측정 기반으로. |

---

## 13. 4주 과정 마무리

- **Week 1**: 소재·FOM·소자·공정 개요
- **Week 2**: SiC 전공정
- **Week 3**: SiC 접촉·integration·termination·신뢰성
- **Week 4**: GaN HEMT·신뢰성·평가

다음 파일:
- TCAD 실습: [`05_TCAD_Verification_Setup_and_Calibration.md`](./05_TCAD_Verification_Setup_and_Calibration.md)
- 산업 실무: [`06_Industry_Practice.md`](./06_Industry_Practice.md)
- 참고문헌: [`07_References_and_Professor_Publications.md`](./07_References_and_Professor_Publications.md)

---

*© 2026 SiC/GaN Process Course Notes. 공정 조건은 대표값이며, 실제 레시피는 장비·기판·목표에 따라 최적화해야 합니다.*
