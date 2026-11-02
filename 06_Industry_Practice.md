# Industry Practice — Fab, PCM, Reliability & DTCO

> **용도**: 4주 과정의 산업 실무 연결 자료
> **핵심 원칙**: 공개 표준(JEDEC, AEC-Q101, IEC)과 공개 문헌·백서에 기반한 **일반 실무 프레임**을 제공합니다. 특정 회사의 내부 공정·수율·기밀 데이터는 포함하지 않습니다.

---

## 1. 이 파일을 읽는 방법

| 독자 | 읽을 부분 |
|---|---|
| 공정/소자 엔지니어 | §3–§6, §10 |
| TCAD 엔지니어 | §7, §9 |
| 신뢰성/품질 | §8 |
| 관리자/PM | §2, §11–§13 |
| 취업 준비생 | §12–§14 |

> ⚠️ **면책**: 삼성전자·SK하이닉스의 내부 공정 조건, 수율, 불량 데이터는 **공개 자료가 아닙니다.** 아래 내용은 "반도체 대기업이 일반적으로 사용하는 공정운영·품질관리 프레임"과 "전력반도체 IDM의 공개 표준"을 교육 목적으로 정리한 것입니다.

---

## 2. 산업 지형

### 2.1 비즈니스 모델

| 모델 | 특징 | 예(공개) |
|---|---|---|
| **IDM** | 설계+공정+제조+판매 | Infineon, ST, onsemi, Rohm, Toshiba, Wolfspeed |
| **Foundry** | 공정만 | TSMC, X-Fab, DB HiTek, SK keyfoundry |
| **Fabless** | 설계만 | Navitas, Power Integrations, EPC |
| **OSAT** | 패키지/테스트 | ASE, Amkor, JCET |

### 2.2 삼성전자·SK하이닉스의 위치(공개 정보 기준)

| 회사 | 주력 | 전력반도체와의 접점(공개) |
|---|---|---|
| **삼성전자** | DRAM, NAND, Foundry, System LSI | 메모리 periphery의 **고전압/전원 관리 소자**, PMIC, SiC/GaN 전력반도체 사업 추진(공개 보도), SiC foundry |
| **SK하이닉스** | DRAM, NAND, HBM | 메모리 공정운영 표준, 전력반도체 직접 제조보다는 **그룹 계열(SK Siltron CSS)의 SiC 웨이퍼** |
| **SK Siltron CSS** | SiC wafer | 4H-SiC substrate/epi |

> **교육 포인트**: 메모리 회사의 **PCM·SPC·FDC·수율관리**는 전력반도체 fab에도 그대로 적용되는 **운영 표준**입니다. 반면 SiC/GaN의 **결함·신뢰성·고온 공정**은 전력반도체 IDM의 노하우가 더 중요합니다.

### 2.3 전력반도체 주요 플레이어(공개)

- **SiC**: Wolfspeed, Infineon, STMicroelectronics, onsemi, Rohm, Coherent, SK Siltron CSS(웨이퍼)
- **GaN**: Infineon, Navitas, Power Integrations, EPC, GaN Systems(Infineon 인수), Transphorm(Renesas 인수)
- **Si IGBT/MOSFET**: Infineon, Mitsubishi, Fuji, Toshiba, Hitachi

---

## 3. 제품 정의 → 양산까지의 흐름

```
[1] Market/Product spec
      ↓
[2] Technology feasibility (TCAD, literature, IP)
      ↓
[3] Device/process architecture (trade-off, cost)
      ↓
[4] TCAD + compact model + corner
      ↓
[5] Test vehicle (TD) / split lot
      ↓
[6] Fab out → PCM → yield
      ↓
[7] Reliability qualification (AEC-Q101/JEDEC)
      ↓
[8] Product qualification → PPAP
      ↓
[9] Ramp → mass production
      ↓
[10] Monitoring (SPC, PCM, field return)
```

### 3.1 각 단계의 산출물

| 단계 | 산출물 | 책임 |
|---|---|---|
| Spec | datasheet target, application profile | Product/Marketing |
| Feasibility | TCAD report, risk list | Device/TCAD |
| Architecture | process flow, mask set, cost estimate | Integration |
| TCAD | calibrated model, design window | TCAD |
| Test vehicle | PCM structures, split plan | Integration/Test |
| Fab out | wafer map, inline data | Fab |
| PCM | parametrics, yield | PCM/Test |
| Reliability | qualification report | Reliability |
| PPAP | control plan, FMEA, MSA | Quality |
| Ramp | yield trend, Cpk | Yield/Integration |

### 3.2 "Product spec → 소자 파라미터" 변환 예(1.2 kV SiC MOSFET)

| Spec | 목표 | 소자 파라미터 | 공정 변수 |
|---|---|---|---|
| BV ≥ 1200 V | 1400 V margin | JTE dose/length, epi doping | implant, epi |
| Ron ≤ 20 mΩ (at 25°C) | 15 mΩ | L_ch, W_J, μ_ch | gate ox, nitridation |
| Vth 2–4 V | 3 V | p-body dose, t_ox | implant, oxidation |
| Qg ≤ 50 nC | 40 nC | t_ox, gate area | oxidation, layout |
| SCWT ≥ 3 µs | 4 µs | JFET, channel | structure |
| HTRB 1000 h | leakage < 10 µA | termination, passivation | JTE, SiN |

> **실무**: spec은 **동시에** 만족해야 하므로, TCAD/DOE로 **trade-off curve**를 먼저 확보합니다.

---

## 4. Fab 운영: 8대 공정 + 모니터링

### 4.1 공정 흐름(개념)

```
Wafer in → Cleaning → Oxidation → Photolithography → Etch →
Implant → Activation → Deposition → CMP → Metallization →
Passivation → Test
```

### 4.2 Inline metrology

| 공정 | 측정 | 장비 |
|---|---|---|
| Oxidation | thickness, uniformity | ellipsometry |
| Litho | CD, overlay | CD-SEM, overlay |
| Etch | depth, profile, CD | SEM, AFM |
| Implant | dose, profile | SIMS, 4-point probe |
| Activation | Rs, junction | SRP, Hall |
| Deposition | thickness, stress | ellipsometry, XRD |
| CMP | thickness, dishing | profilometer |
| Metal | thickness, ρ | 4-point probe |

### 4.3 SPC (Statistical Process Control)

- **Control chart**: `X̄-R`, `I-MR`.
- **Nelson rules**: 1점 3σ, 9점 연속 같은 방향, 6점 연속 증가/감소 등.
- **Cpk**: `min(USL−μ, μ−LSL)/(3σ)`.
- 목표: `Cpk ≥ 1.33` (양산), `≥ 1.67` (안전 critical).

### 4.4 FDC (Fault Detection & Classification)

- 장비 sensor trace (pressure, power, temperature)를 실시간 모니터링.
- **model-based** vs **data-driven**.
- 이상 발생 시 **lot hold**.

### 4.5 APC (Advanced Process Control)

- **run-to-run** feedback/feedforward.
- 예: etch depth → 다음 lot의 etch time 보정.
- **virtual metrology**: 측정 없이 예측.

### 4.6 MES/데이터

| 시스템 | 역할 |
|---|---|
| **MES** | lot/wafer tracking, recipe |
| **SPC** | 통계 관리 |
| **FDC** | 장비 이상 |
| **YMS** | yield analysis |
| **RMS** | recipe management |
| **APC** | feedback control |

---

## 5. PCM (Process Control Monitor)

### 5.1 목적

- **공정 이상 조기 감지** (inline으로 못 잡는 전기적 특성).
- **소자 모델 검증**.
- **수율 예측**.

### 5.2 PCM 구조(예)

| 구조 | 측정 | 목적 |
|---|---|---|
| **TLM** | ρc, R_sh | 접촉/도핑 |
| **Kelvin** | contact resistance | metal/semiconductor |
| **Van der Pauw** | sheet n, μ | implant/activation |
| **MOS capacitor** | t_ox, Dit, Vfb | gate oxide |
| **Diode** | BV, Vf, leakage | junction/termination |
| **MOSFET** | Vth, Ron, gm, BV | 소자 |
| **Resistor** | Rs, TCR | metal/poly |
| **Ring oscillator** | delay | logic(메모리 periphery) |
| **SRAM** | yield | memory |

### 5.3 PCM spec 예(1.2 kV SiC MOSFET)

| 항목 | Target | LSL | USL | 단위 |
|---|---|---|---|---|
| BV | 1400 | 1200 | — | V |
| Ron | 15 | — | 20 | mΩ |
| Vth | 3.0 | 2.0 | 4.0 | V |
| Leakage (Vds=1200 V) | <1 | — | 10 | µA |
| ρc (n) | 5e-6 | — | 1e-5 | Ω·cm² |
| t_ox | 50 | 45 | 55 | nm |

### 5.4 PCM 데이터 해석

- **wafer map**: spatial pattern (edge, center, sector).
- **trend**: lot-to-lot drift.
- **correlation**: PCM vs yield.
- **excursion**: spec out → hold/analysis.

---

## 6. DOE & 통계

### 6.1 DOE 종류

| 방법 | 변수 수 | run 수 | 용도 |
|---|---|---|---|
| Full factorial | 2–3 | 2^k | screening |
| Fractional factorial | 4–6 | 2^(k-p) | screening |
| Response surface (CCD) | 2–3 | ~13 | optimization |
| Latin Hypercube | 다수 | 20–100 | simulation |
| Taguchi | 다수 | orthogonal array | robust design |

### 6.2 DOE 절차

```
1) 목표/응답 설정 (BV, Ron, yield)
2) 인자/수준 선정 (dose, temp, time)
3) randomization
4) 실행
5) ANOVA / regression
6) 최적 조건 도출
7) confirmation run
```

### 6.3 통계 도구

- **JMP, Minitab, R, Python(statsmodels)**.
- **ANOVA**: 인자 유의성.
- **Regression**: response surface.
- **Gage R&R**: 측정 시스템 변동.

### 6.4 Cpk 예

```
Cpk = min(USL−μ, μ−LSL)/(3σ)
```
- `μ=15`, `σ=1.5`, `USL=20`, `LSL=10` → `Cpk = min(5,5)/4.5 = 1.11` → 개선 필요.
- 목표: 1.33 이상.

---

## 7. TCAD in Industry

### 7.1 역할

| 단계 | TCAD 역할 |
|---|---|
| Feasibility | 구조/공정 후보 평가 |
| Design | design window, trade-off |
| DOE | simulation DOE로 실험 최소화 |
| Failure analysis | 측정과 비교해 원인 추정 |
| DTCO | 공정–소자–회로 co-optimization |
| Corner | PVT/process corner model |
| Reliability | aging, TDDB, BTI |

### 7.2 Calibration vs Prediction

| 구분 | 요구 |
|---|---|
| **Calibration** | 측정 데이터에 맞춤 (SIMS, C-V, I-V) |
| **Prediction** | calibration 후 **독립 데이터**로 검증 |
| **Extrapolation** | 위험. 반드시 validation |

> **실무**: "TCAD가 맞다"가 아니라 "**이 조건 범위에서** 측정과 일치한다"고 말해야 합니다.

### 7.3 Compact model & PDK

```
TCAD (device) → compact model (BSIM/PSP/Verilog-A) → circuit (SPICE) → PDK
```
- **PDK**: process design kit (model, DRC, LVS, PEX).
- **corner**: TT/FF/SS/FS/SF, temperature, voltage.
- **Monte Carlo**: mismatch, process variation.

### 7.4 DTCO 예

- **SiC MOSFET**: cell pitch↓ → Ron↓, 하지만 JFET/thermal trade-off.
- **GaN HEMT**: field plate length↑ → BV↑, 하지만 Cgd↑ → switching 손실.
- TCAD로 **Pareto front**를 만들고, circuit 시뮬로 최종 선택.

---

## 8. Reliability & Qualification

### 8.1 표준

| 표준 | 대상 |
|---|---|
| **AEC-Q101** | automotive discrete semiconductor |
| **AEC-Q100** | automotive IC |
| **JEDEC JESD22** | test methods |
| **JEDEC JESD47** | stress-test-driven qualification |
| **IEC 60747** | semiconductor devices |
| **IEC 60749** | mechanical/climatic |
| **ISO 26262** | functional safety (system) |
| **IATF 16949** | quality management |

### 8.2 Qualification test matrix (예)

| Test | Condition | Duration | Criteria |
|---|---|---|---|
| **HTOL** | 150°C, rated | 1000 h | param drift |
| **HTRB** | 150°C, 80% BV | 1000 h | leakage |
| **HTGB** | 150°C, ±Vgs | 1000 h | Vth |
| **H3TRB** | 85°C/85%RH, 80% BV | 1000 h | leakage |
| **TC** | −55~150°C | 1000 cyc | delamination |
| **PC** | ΔTj | 10⁵ cyc | Rth |
| **ESD** | HBM/CDM | — | class |
| **UIS** | inductive | — | E_as |
| **MSL** | 85°C/85%RH | 168 h | popcorning |

### 8.3 PPAP (Production Part Approval Process)

- **DFMEA/PFMEA**
- **Control plan**
- **MSA** (Gage R&R)
- **Process capability** (Cpk)
- **Qualification report**
- **PSW** (Part Submission Warrant)

### 8.4 8D / RCA

```
D1 team → D2 problem → D3 containment → D4 root cause →
D5 corrective action → D6 verify → D7 prevent → D8 congratulate
```

**도구**: 5 Why, Fishbone, FTA, Pareto.

### 8.5 Field return / DPPM

- **DPPM** = defective parts per million.
- **FIT** = failures in time (1e9 device-hours).
- 목표: automotive < 1 DPPM (성숙 제품).

---

## 9. 전력반도체 특유의 실무 이슈

### 9.1 SiC

| 이슈 | 실무 대응 |
|---|---|
| **BPD/stacking fault** | 저 BPD epi, body diode stress 관리, screening |
| **Vth drift** | gate ox nitridation, BTI test |
| **Ohmic resistance** | TLM monitor, annealing control |
| **JTE dose drift** | implant monitor, SRP |
| **Wafer bow** | epi stress, chuck |
| **Defect density** | KLA inspection, yield model |
| **Cost** | 150 mm → 200 mm 전환 |

### 9.2 GaN

| 이슈 | 실무 대응 |
|---|---|
| **Dynamic Ron** | passivation, field plate, buffer |
| **Vth drift** | gate dielectric, p-GaN |
| **Current collapse** | surface treatment |
| **Epi uniformity** | MOCVD control |
| **Wafer bow/crack** | buffer engineering |
| **Thermal** | package, thermal via |
| **Test** | pulsed I-V, dynamic Ron |

### 9.3 Test

| 항목 | 방법 |
|---|---|
| DC | Id-Vg, Id-Vd, BV, leakage |
| Dynamic | hard switching, double pulse |
| Avalanche | UIS |
| Short circuit | SCWT |
| Thermal | Rth, T_j |
| Reliability | HTOL/HTRB/HTGB |

---

## 10. 원가·수율·생산성

### 10.1 원가 구조(개념)

```
Wafer cost + Process cost + Test cost + Package cost + Yield loss
```

- SiC: **substrate/epi**가 원가의 큰 부분.
- GaN-on-Si: **epi + Si substrate**.
- 200 mm 전환: die per wafer↑ → cost↓.

### 10.2 Yield

```
Yield = (good dies)/(total dies)
```
- **die yield** = `1/(1+A·D)^n` (Murphy/negative binomial)
- `A`: die area, `D`: defect density, `n`: clustering.

**예**: `A=25 mm²`, `D=0.5/cm²`, `n=1` → `Y = 1/(1+12.5) ≈ 7.4%` (defect-limited). 실제로는 더 복합적.

### 10.3 Ramp

- **yield learning curve**.
- **excursion** → containment → root cause → fix.
- **copy exactly** (fab-to-fab).

---

## 11. 데이터·디지털 트윈

| 요소 | 내용 |
|---|---|
| **Digital twin** | 공정/장비/소자 시뮬레이션 |
| **Virtual metrology** | 측정 대체 예측 |
| **Predictive maintenance** | 장비 고장 예측 |
| **AI/ML** | yield prediction, defect classification |
| **Data lake** | MES/SPC/FDC 통합 |

> **주의**: AI 모델은 **데이터 품질·편향**에 취약. 물리 기반 TCAD와 결합하는 **hybrid** 접근이 실무적.

---

## 12. 역할과 필요 역량

| 역할 | 주요 업무 | 필요 역량 |
|---|---|---|
| **Process engineer** | 단위공정 최적화, recipe | 화학/물리, DOE, SPC |
| **Device engineer** | 소자 설계, TCAD | 반도체 물리, TCAD |
| **Integration engineer** | 공정 순서, thermal budget | 공정 전반, 문제해결 |
| **TCAD engineer** | 모델/캘리브레이션 | 수치해석, 물리, 코딩 |
| **Reliability engineer** | qualification, FA | 통계, 신뢰성 |
| **Product engineer** | spec, yield, test | 데이터 분석 |
| **Test engineer** | ATE, test program | 전기/계측 |
| **Quality engineer** | PPAP, 8D, audit | 표준, 통계 |

### 12.1 공통 역량

- **통계**: SPC, DOE, Cpk.
- **데이터**: Python/Excel/JMP.
- **문제해결**: 8D, 5 Why, Fishbone.
- **문서**: spec, report, control plan.
- **협업**: fab/design/test/quality.

---

## 13. 문서·회의·의사결정

### 13.1 핵심 문서

| 문서 | 목적 |
|---|---|
| **Spec** | 제품/공정 요구 |
| **POR** (process of record) | 표준 recipe |
| **DOE report** | 실험 결과 |
| **FMEA** | 리스크 |
| **Control plan** | 관리 항목 |
| **Qualification report** | 신뢰성 |
| **8D** | 불량 대응 |
| **Change control** | 변경 관리 |

### 13.2 의사결정 원칙

1. **데이터 기반**: 감이 아니라 측정.
2. **통계적 유의성**: n, 신뢰구간.
3. **risk-based**: FMEA 우선순위.
4. **documented**: 결정/근거 기록.
5. **change control**: 검증 후 적용.

---

## 14. 케이스 스터디(일반化)

### Case 1: JTE dose drift → BV 하락

- **증상**: PCM BV가 lot 12부터 5% 하락.
- **데이터**: SRP active dose 3% 감소, wafer map edge.
- **원인**: implant 장비 beam current drift → dose 보정 실패.
- **대응**: 장비 calibration, APC 재설정, affected lot hold.
- **재발방지**: dose monitor 주기 단축, SPC rule 추가.

### Case 2: Ohmic resistance excursion

- **증상**: TLM ρc가 spec 상한 초과.
- **데이터**: annealing 온도 20°C 낮음, C 잔류 증가.
- **원인**: RTA thermocouple drift.
- **대응**: thermocouple 교체, recipe 재검증.
- **재발방지**: RTA 온도 monitor wafer.

### Case 3: GaN dynamic Ron 열화

- **증상**: field return에서 dynamic Ron 1.5x.
- **데이터**: HTRB 후 Ron 증가, passivation 두께 불균일.
- **원인**: SiNx 증착 uniformity → surface trap.
- **대응**: PECVD recipe 개선, passivation 두께 monitor.
- **재발방지**: dynamic Ron을 PCM/qualification에 추가.

### Case 4: Vth shift

- **증상**: HTGB 후 Vth +0.4 V.
- **데이터**: gate ox Dit, nitridation 부족.
- **원인**: NO anneal 온도 낮음.
- **대응**: anneal recipe 조정, C-V monitor.
- **재발방지**: Dit/Vth를 inline PCM에 추가.

---

## 15. KPI

| KPI | 정의 | 목표(예) |
|---|---|---|
| Yield | good/total | > 90% (성숙) |
| Cpk | process capability | > 1.33 |
| DPPM | defective per million | < 10 (auto) |
| OEE | equipment effectiveness | > 85% |
| Cycle time | lot throughput | < target |
| Rework | 재작업 | < 1% |
| Scrap | 폐기 | < 0.5% |
| Rth | thermal resistance | spec |
| Dynamic Ron | GaN | < 1.3 |

---

## 16. 커리어 조언

### 16.1 전력반도체 분야 진입

1. **기초**: 반도체 물리 + 소자 + 공정.
2. **도구**: TCAD(Sentaurus/Silvaco), Python, 통계.
3. **실무**: PCM/DOE/SPC/8D.
4. **전문성**: SiC 또는 GaN 중 하나를 깊게.
5. **포트폴리오**: TCAD 캘리브레이션 리포트, DOE 분석, 논문.

### 16.2 면접에서 자주 나오는 질문

- "SiC가 Si보다 유리한 이유를 FOM으로 설명하시오."
- "JTE dose window를 어떻게 정하나?"
- "Ohmic 저항을 낮추는 방법은?"
- "GaN dynamic Ron의 원인과 대책은?"
- "TCAD 결과가 측정과 다를 때 어떻게 하나?"
- "Cpk 1.0이면 어떤 조치를 하나?"

### 16.3 성장 경로

```
Process/Device Engineer → Senior → Principal → Manager/Director
                ↘ TCAD/Reliability specialist
                ↘ Product/Quality
```

---

## 17. 참고 표준·문헌

- AEC-Q101 (automotive discrete)
- JEDEC JESD22, JESD47
- IEC 60747, IEC 60749
- ISO 26262 (functional safety)
- IATF 16949 (quality)
- *SEMI* standards (equipment)
- 교수님 논문(07 파일)

---

*© 2026 SiC/GaN Process Course Notes. 본 문서는 교육용 일반 프레임이며, 특정 회사의 내부 정보를 포함하지 않습니다.*
