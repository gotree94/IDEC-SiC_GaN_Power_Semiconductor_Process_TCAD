# TCAD Verification Setup & Calibration

> **용도**: 4주 과정 전체의 TCAD 실습 공통 참조
> **대상**: Sentaurus / Silvaco 입문~중급, 라이선스가 없어도 무료 도구로 대체 가능
> **핵심 원칙**: "시뮬레이션은 측정의 대체가 아니라 **가설 검증 도구**다."

---

## 1. TCAD의 역할과 한계

### 1.1 TCAD가 잘하는 것

| 용도 | 설명 |
|---|---|
| **물리 이해** | 전계/캐리어 분포, 항복 위치, 누설 경로 |
| **설계 탐색** | doping/geometry/dose sweep |
| **DOE** | 다변수 최적화, sensitivity |
| **불량 분석** | 측정과 비교해 원인 추정 |
| **DTCO** | 공정–소자–회로 co-optimization |

### 1.2 TCAD가 못하는 것

- **예측**은 캘리브레이션 없이는 신뢰 불가.
- **결함/오염/수율**은 모델링 어려움.
- **패키지/시스템**은 별도 도구.
- **신뢰성 수명**은 가속 모델과 결합해야.

> **실무 원칙**: "TCAD 결과는 **경향**으로 읽고, **절대값**은 측정으로 보정한다."

---

## 2. 도구 선택

| 도구 | 용도 | 장점 | 단점 |
|---|---|---|---|
| **Sentaurus (Synopsys)** | 공정+소자 | 산업 표준, 강력 | 고가, 학습곡선 |
| **Silvaco (Atlas/Athena)** | 공정+소자 | 산업 표준 | 고가 |
| **DEVSIM** | 소자 | 무료/오픈소스 | 공정 없음 |
| **FEniCS/COMSOL** | 맞춤 | 유연 | 직접 구현 |
| **Python (numpy/scipy)** | 1D | 교육/검증 | 2D 확장 어려움 |
| **SRIM/TRIM** | implant | 무료, 정확 | 공정 전체 아님 |
| **ngspice** | 회로 | 무료 | 소자 물리 없음 |
| **KLayout** | layout | 무료 | 공정/소자 없음 |

> **교육 권장**: 라이선스가 있으면 Sentaurus로 통일, 없으면 **DEVSIM + SRIM + Python** 조합.

---

## 3. 프로젝트 구조(권장)

```
project/
├── 00_docs/
│   ├── spec.md
│   ├── calibration_log.md
│   └── measurement/
├── 01_process/
│   ├── sprocess/
│   │   ├── oxidation.cmd
│   │   ├── implant.cmd
│   │   └── activation.cmd
│   └── srim/
├── 02_device/
│   ├── sde/
│   │   ├── diode.cmd
│   │   ├── dmos.cmd
│   │   └── hemt.cmd
│   └── sdevice/
│       ├── iv.cmd
│       ├── bv.cmd
│       └── cv.cmd
├── 03_scripts/
│   ├── run_all.ps1
│   ├── extract.py
│   └── plot.py
├── 04_results/
│   ├── run_YYYYMMDD/
│   └── summary.xlsx
└── 05_reports/
```

**원칙**
- **모든 run에 날짜/commit/파라미터** 기록.
- 입력 deck은 **버전 관리**(git).
- 결과는 **원본(tdr/plt) + 요약(csv) + 그림** 분리.
- `calibration_log.md`에 **가정/변경/근거** 기록.

---

## 4. 재료 파라미터 (SiC / GaN)

### 4.1 4H-SiC (300 K, TCAD 입력 예)

| 파라미터 | 값 | 단위 | 비고 |
|---|---|---|---|
| `E_g` | 3.26 | eV | 온도 의존 |
| `χ` (electron affinity) | 3.7 | eV | 문헌 3.7–4.0 |
| `ε_r` | 9.66 | — | anisotropic |
| `N_c` | 1.6×10¹⁹ | cm⁻³ | — |
| `N_v` | 2.0×10¹⁹ | cm⁻³ | — |
| `n_i` | ~10⁻⁹ | cm⁻³ | 매우 작음 |
| `μ_n` | 900–1000 | cm²/V·s | doping 의존 |
| `μ_p` | 100–120 | cm²/V·s | — |
| `v_sat` | 2.0×10⁷ | cm/s | — |
| `λ` | 3.7–4.9 | W/cm·K | 온도 의존 |
| `τ_n` | 10⁻⁹–10⁻⁶ | s | 공정 의존 |
| Al `E_a` | 0.19–0.24 | eV | incomplete ionization |
| N `E_a` | 0.05–0.10 | eV | — |

### 4.2 GaN (300 K, TCAD 입력 예)

| 파라미터 | 값 | 단위 | 비고 |
|---|---|---|---|
| `E_g` | 3.42 | eV | — |
| `χ` | 4.1 | eV | — |
| `ε_r` | 9.5 | — | — |
| `n_i` | ~10⁻¹⁰ | cm⁻³ | — |
| `μ_n` | 1000–2000 | cm²/V·s | 2DEG는 별도 |
| `μ_p` | ~30 | cm²/V·s | — |
| `v_sat` | 2.5×10⁷ | cm/s | — |
| `λ` | 1.3–1.7 | W/cm·K | — |
| AlGaN `E_g` | `3.42 + 1.71x + x²` | eV | x=Al 조성 |
| `ΔE_c` | `0.7·ΔE_g` | eV | 근사 |

> ⚠️ **파라미터는 논문/툴 버전마다 다릅니다.** 반드시 출처를 기록하고, **측정 캘리브레이션**으로 보정하십시오.

### 4.3 불완전 이온화

SiC에서 필수:
```
N_D⁺ = N_D/(1 + g_D exp((E_F−E_D)/kT))
N_A⁻ = N_A/(1 + g_A exp((E_A−E_F)/kT))
```
- Sentaurus: `IncompleteIonization` (버전별 syntax 확인).
- Silvaco: `incomplete` model.
- **끄면 Vth/Ron을 과소평가**.

---

## 5. 메쉬 전략

### 5.1 원칙

- **물리적 길이 스케일**에 맞춘다: 계면(1 nm) ~ drift(10 µm).
- **전계 집중부**(junction, corner, termination)에 fine mesh.
- **bulk**는 coarse, **grading**으로 전환.
- **mesh convergence** 필수: 2배/0.5배로 결과 비교.

### 5.2 권장 mesh

| 영역 | 간격 |
|---|---|
| Oxide/SiC interface | 0.5–1 nm |
| Channel | 1–2 nm |
| Junction | 5–20 nm |
| Trench corner | 2–5 nm |
| Termination | 10–50 nm |
| Drift | 0.1–0.5 µm |
| Substrate | 1–5 µm |
| GaN heterointerface | 0.2–0.5 nm |
| GaN barrier | 0.5–1 nm |

### 5.3 Mesh convergence test

```
Run A: base mesh
Run B: 2x fine (global)
Run C: 0.5x coarse
```
- BV/Ron 변화 < 2% → 수렴.
- **BV는 mesh에 민감** → junction/termination mesh를 특히 주의.

---

## 6. 물리 모델

### 6.1 SiC diode/MOSFET

| 물리 | 모델 | 비고 |
|---|---|---|
| Bandgap | temperature dependent | Varshni |
| Statistics | Fermi | 고도핑 |
| Incomplete ionization | Al/N | SiC 필수 |
| Mobility | doping/temperature/field | Lombardi/Canali |
| SRH | doping dependent | lifetime |
| Auger | high injection | — |
| Impact ionization | Okuto/van Overstraeten | BV |
| Anisotropy | 4H-SiC | 가능하면 |
| Interface traps | Dit | MOS |
| Oxide tunneling | FN/PF | gate leakage |

### 6.2 GaN HEMT

| 물리 | 모델 | 비고 |
|---|---|---|
| Polarization | spontaneous + piezoelectric | 핵심 |
| Heterointerface | band offset | AlGaN/GaN |
| Mobility | 2DEG | interface roughness |
| Traps | surface/buffer/dielectric | dynamic Ron |
| Self-heating | thermodynamic | 고전류 |
| Impact ionization | GaN | BV |
| Gate leakage | Schottky/tunneling | — |

### 6.3 모델 on/off 민감도

| 모델 | 끄면 생기는 오차 |
|---|---|
| Incomplete ionization | Vth↓, Ron↓ |
| Dit | Vth↑/hysteresis 무시 |
| Impact ionization | BV 과대 |
| Self-heating | Ron 과소 |
| Traps | dynamic Ron 무시 |
| Polarization | 2DEG 없음 |

---

## 7. Sentaurus 예제 (개념)

> 실제 syntax는 버전(Sentaurus 2019/2022/2023 등)에 따라 다릅니다. 아래는 **구조 이해용**입니다.

### 7.1 SDE (구조)

```
(sde:clear)
(sdegeo:create-rectangle (position 0 0 0) (position 10 3 0)
  "SiliconCarbide" "R.SiC")
(sdegeo:create-rectangle (position 0 3 0) (position 10 3.05 0)
  "SiO2" "R.Ox")
(sdegeo:set-contact (find-edge-id (position 5 3.05 0)) "gate")
(sdegeo:set-contact (find-edge-id (position 5 0 0)) "cathode")
(sdedr:define-constant-profile "P.Body" "Boron" 1e17)
(sdedr:define-constant-profile-placement "P.Body" "R.SiC" 1.0)
(sde:build-mesh "snmesh" "" "n@node@")
```

### 7.2 SDevice (I-V / BV)

```
File {
  Grid = "@tdr@"
  Current = "@plot@"
  Plot = "@tdr@"
  Output = "@log@"
}

Electrode {
  { Name="gate" Voltage=0.0 }
  { Name="cathode" Voltage=0.0 }
}

Physics {
  Temperature = 300
  Fermi
  IncompleteIonization
  Mobility(
    DopingDependence
    HighFieldSaturation
    Enormal
  )
  Recombination(
    SRH(DopingDependence)
    Auger
  )
  ImpactIonization(Okuto)
}

Math {
  Extrapolate
  RelErrControl
  Digits = 5
  Iterations = 20
  Method = ILS
}

Solve {
  # initial
  Poisson
  Coupled { Poisson electron hole }

  # reverse sweep
  Quasistationary(
    InitialStep = 1e-3
    MinStep = 1e-6
    MaxStep = 1e-2
    Goal { Name="cathode" Voltage = 1500 }
  ) {
    Coupled { Poisson electron hole }
  }
}
```

### 7.3 BV 추출

- `cathode` 전압 sweep 중 `Current`가 `1 mA/cm²`에 도달하는 전압.
- Sentaurus `BreakCriteria { Current(Contact="cathode") AbsVal=1e-3 }` 활용.
- 추출 스크립트로 `BV`, `E_max`, `E_ox` 기록.

### 7.4 C-V

```
Solve {
  Quasistationary(
    InitialStep=1e-4 MaxStep=1e-2
    Goal { Name="gate" Voltage=10 }
  ) { Coupled { Poisson electron hole } }
}
```
- `ACExtract`로 `C = dQ/dV`.
- Dit는 `C-V` frequency/conductance로 별도.

---

## 8. Silvaco 예제 (개념)

### 8.1 Atlas

```
go atlas

mesh space.mult=1.0
x.mesh loc=0.0 spac=0.05
x.mesh loc=5.0 spac=0.5
y.mesh loc=0.0 spac=0.01
y.mesh loc=3.0 spac=0.2

region num=1 material=SiC
region num=2 material=SiO2

electrode name=gate top
electrode name=cathode bottom

doping region=1 n.type conc=1e16 uniform

models srh auger fldmob incomplete
impact selb

solve init
solve vcathode=0.0 vstep=1.0 vfinal=1200 name=cathode
```

### 8.2 TonyPlot

- `tonyplot -st structure.str`
- `tonyplot iv.dat`

> Silvaco는 `Athena`(공정) + `Atlas`(소자) 조합. Sentaurus는 `SProcess` + `SDevice`.

---

## 9. 캘리브레이션 워크플로

### 9.1 순서

```
[1] 측정 데이터 수집 (SIMS, SRP, Hall, C-V, I-V, TLM)
[2] 공정 시뮬레이션 → SIMS/SRP 비교
[3] 소자 시뮬레이션 → C-V/I-V 비교
[4] 파라미터 민감도 분석
[5] 최적 파라미터 세트 확정
[6] 검증용 독립 데이터로 테스트
[7] corner/DOE
```

### 9.2 합격 기준(예시)

| 항목 | 지표 | 허용 오차 |
|---|---|---|
| 산화막 두께 | TEM/ellipsometry | ±5% |
| Implant profile | SIMS | ±10% (peak), ±20% (tail) |
| Active profile | SRP | ±15% |
| Sheet resistance | Hall | ±10% |
| Vth | Id-Vg | ±0.1 V |
| Ron | Id-Vd | ±10% |
| BV | I-V | ±5% |
| C-V | MIS | ±5% |
| Leakage | I-V | ±2× (order) |

> **BV ±5%**는 엄격해 보이지만, termination 설계에서는 필수. 안 맞으면 **impact ionization 파라미터**를 먼저 의심.

### 9.3 캘리브레이션 로그 예

| 날짜 | 파라미터 | 변경 전 | 변경 후 | 근거 | 결과 |
|---|---|---|---|---|---|
| 2026-11-02 | `μ_n` | 1000 | 950 | Hall | Ron +5% |
| 2026-11-02 | `τ_n` | 1e-6 | 1e-7 | reverse leakage | leakage ↑ |
| 2026-11-03 | `E_c` | 2.2 | 2.5 MV/cm | BV | BV +10% |

---

## 10. DOE & Sensitivity

### 10.1 변수 선정

| 소자 | 주요 변수 |
|---|---|
| Diode | epi doping/thickness, JTE dose/length, metal barrier |
| DMOS | L_ch, W_J, p-body dose, t_ox |
| Trench | trench depth/width, corner radius, bottom implant |
| HEMT | Al composition, barrier thickness, FP length, trap density |

### 10.2 DOE 방법

| 방법 | 용도 |
|---|---|
| **Full factorial** | 변수 2–3개, level 2–3 |
| **Latin Hypercube** | 변수 많을 때 |
| **Sobol sensitivity** | 기여도 |
| **Response surface** | 최적화 |

### 10.3 예: JTE 최적화

```
변수: dose (3e12–1e13), length (20–80 µm), surface charge (−1e12–+1e12)
목표: BV ≥ 1200 V, E_ox < 3 MV/cm, window 중앙
```
- 3×3×3 = 27 runs (full factorial).
- 결과를 `BV` contour로 시각화.
- **robust optimum**: worst-case에서도 BV 유지.

---

## 11. 신뢰성 시뮬레이션

### 11.1 TDDB

- oxide에 constant field stress.
- percolation model 또는 empirical `t_BD`.
- Weibull fitting.

### 11.2 BTI

- gate bias + temp.
- trap charging model (`ΔVth = A t^n`).
- Sentaurus `Age`/trap model.

### 11.3 Dynamic Ron (GaN)

- trap filling (high V_ds) → transient.
- `Traps` + `Transient` solve.
- extraction of recovery time.

### 11.4 Self-heating

- `Thermodynamic` + thermal boundary.
- `R_th` extraction.
- electro-thermal loop.

---

## 12. 자주 나오는 오류와 해결

| 증상 | 원인 | 해결 |
|---|---|---|
| **Solution diverged** | mesh/initial guess/scale | mesh fine, `Extrapolate`, step↓ |
| **BV too high** | impact ionization off/params | model on, Okuto, mesh |
| **BV too low** | edge field, termination | termination mesh, doping |
| **Ron too low** | mobility/contact | mobility model, contact resistance |
| **Vth wrong** | work function/Dit | `MetalWorkfunction`, Dit |
| **Leakage too low** | SRH/lifetime | lifetime, trap |
| **C-V distorted** | series resistance/leakage | contact, frequency |
| **2DEG absent** | polarization off | `Piezoelectric`, heterointerface |
| **Dynamic Ron absent** | traps off | trap model |
| **Temperature unrealistic** | self-heating off | thermodynamic |

### 12.1 Convergence 팁

1. **Poisson only** → **Poisson+electron** → **Poisson+electron+hole**.
2. Step size를 **작게** 시작.
3. `Method=ILS` (또는 `Blocked`).
4. `Extrapolate` 사용.
5. **Mesh**를 coarse→fine.
6. **Scaling** (potential, concentration).

---

## 13. 검증 체크리스트

### 13.1 입력 검증

- [ ] 단위 일관성 (cm vs µm, cm⁻³ vs m⁻³)
- [ ] doping vs SIMS
- [ ] geometry vs SEM/TEM
- [ ] boundary condition (ohmic/Schottky)
- [ ] material parameter 출처
- [ ] mesh convergence
- [ ] model on/off 민감도

### 13.2 결과 검증

- [ ] 손계산(1D)과 비교
- [ ] 측정 C-V/I-V와 비교
- [ ] 물리적 타당성 (E-field 위치, leakage path)
- [ ] 에너지 보존
- [ ] 온도 의존성
- [ ] corner에서 거동

### 13.3 보고서 검증

- [ ] 가정 명시
- [ ] 파라미터 출처
- [ ] 오차 원인
- [ ] 한계/불확실성
- [ ] 재현 명령

---

## 14. 무료/저가 대체 워크플로

| 단계 | 도구 |
|---|---|
| Implant | SRIM |
| 1D drift-diffusion | Python (numpy/scipy) |
| 2D 소자 | DEVSIM |
| 회로 | ngspice |
| Layout | KLayout |
| 시각화 | matplotlib/ParaView |
| 데이터 | pandas/Excel |

### 14.1 Python 1D 예제(개념)

```python
import numpy as np
from scipy.integrate import solve_bvp

# Poisson + continuity (개념)
# d2psi/dx2 = -q/eps (p - n + Nd+ - Na-)
# Jn = q mu n E + q D dn/dx
# ...
```

> 교육 목적: **직접 구현**하면 물리 이해가 깊어집니다. 산업에서는 상용 TCAD.

---

## 15. 실습 템플릿

### 15.1 1D diode (W1)

- 구조: n⁺ substrate + n⁻ drift 10 µm.
- doping: 1.39×10¹⁶ cm⁻³.
- 목표: BV 1200 V, Ron 0.45 mΩ·cm².

### 15.2 공정 (W2)

- Al implant 30/80/150/250 keV.
- activation 1650°C.
- 목표: SIMS/SRP profile.

### 15.3 Termination (W3)

- JTE dose sweep.
- surface charge ±1×10¹².
- 목표: BV window.

### 15.4 HEMT (W4)

- AlGaN 25%, 25 nm.
- polarization + traps.
- 목표: Vth, Ron, dynamic Ron.

---

## 16. 보고서 템플릿

```
1. 목적
2. 구조/공정 (그림, 파라미터 표)
3. 물리 모델 (on/off, 출처)
4. 메쉬 (convergence)
5. 결과 (I-V, BV, E-field)
6. 측정 비교 (표, 오차)
7. 민감도/DOE
8. 논의 (한계, 불확실성)
9. 결론
10. 재현 (명령, 버전)
```

---

*© 2026 SiC/GaN Process Course Notes. TCAD syntax는 버전에 따라 다르므로 반드시 해당 버전 매뉴얼을 확인하십시오.*
