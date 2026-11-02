# 21 — TCAD Lab Manual (20-Day Program)

> **대상**: Day 1–20 실습 | **총 40h** (2h/일)
> **참조**: [`../05_TCAD_Verification_Setup_and_Calibration.md`](../05_TCAD_Verification_Setup_and_Calibration.md)

---

## 1. 목적

이 매뉴얼은 20일 프로그램의 **모든 실습을 하나의 재현 가능한 워크플로**로 묶습니다.

- 상용 TCAD(Sentaurus/Silvaco) 또는 무료 대체(DEVSIM/Python) 사용.
- 매일 **입력–가정–결과–검증–산출물**을 기록.
- 라이선스가 없어도 **물리·계산·해석**을 평가.

---

## 2. 실습 환경

| 용도 | 상용 | 무료 |
|---|---|---|
| 공정/소자 | Sentaurus, Silvaco | DEVSIM, Python |
| Implant | SProcess | SRIM |
| 데이터 | Excel, JMP | pandas, matplotlib |
| Layout | Virtuoso | KLayout |
| 회로 | Spectre | ngspice |

### 2.1 설치 체크

- [ ] Python 3.10+, numpy, scipy, matplotlib, pandas.
- [ ] SRIM(또는 SRIM 대체 계산).
- [ ] TCAD 라이선스/GUI 접속.
- [ ] 작업 폴더: `20Day_Program/labs/dayXX/`.
- [ ] 결과 저장 규칙: `dayXX_<topic>_<date>_<initials>/`.

---

## 3. 20일 실습 목록

| Day | 실습 | 도구 | 핵심 산출물 | 합격 기준 |
|---:|---|---|---|---|
| 01 | 손실 계산 | Python/Excel | $P_{cond}/P_{sw}$ 표 | 단위/항목 완전 |
| 02 | 1D Poisson/C-V | Python | band/C-V | 수렴/물리 타당 |
| 03 | 1D diode BV | TCAD/Python | BV vs $N_D$ | $E_{max}\approx E_c$ |
| 04 | FOM/소재 선택 | Excel/Python | FOM 표 | $R_{on,sp}$ 일관 |
| 05 | 1D sanity check | TCAD | $V_{th}/R_{on}/BV$ | 손계산 ±20% |
| 06 | 세정 sequence | 문서/AFM/XPS | sequence 표 | 오염원 매핑 |
| 07 | 산화 fitting | Excel/Python | $B/A$, $B$ | $R^2>0.98$ |
| 08 | Dit 추출 | Excel/Python | Dit 표 | 단위/방향 |
| 09 | SRIM/box profile | SRIM | profile | uniformity ±10% |
| 10 | SRP/Hall/etch | Excel/SEM | activation rate | 계산 일관 |
| 11 | TLM | Excel/Python | $\rho_c$ | $R^2>0.99$ |
| 12 | JBS TCAD | TCAD | $V_F$/leakage/BV | trade-off 설명 |
| 13 | Mask flow | 문서 | cross-section/mask | thermal budget |
| 14 | JTE sweep | TCAD | BV window | peak/창 도출 |
| 15 | Weibull/HTRB | Excel/Python | $\beta,\eta$ | 해석 타당 |
| 16 | 2DEG | Python | $n_s/R_{sh}$ | 단위/경향 |
| 17 | HEMT TCAD | TCAD | $V_{th}/BV/C_{gd}$ | field plate 비교 |
| 18 | Trap/dynamic Ron | TCAD/Python | recovery curve | time constant |
| 19 | Calibration/DOE | TCAD/Excel | main effect | 오차 기준 |
| 20 | Capstone | 전체 | 보고서/발표 | rubric 70% |

---

## 4. 표준 워크플로

### 4.1 구조 정의

1. 좌표/단위(cm, µm) 통일.
2. Layer: substrate, buffer, drift, well, source, oxide, metal.
3. Doping: analytic/Gaussian/SIMS profile.
4. Electrode: anode/cathode/gate/source/drain.

### 4.2 Mesh

| 영역 | Mesh |
|---|---|
| Junction | 0.5–2 nm |
| Oxide interface | 0.2–1 nm |
| Drift | 10–100 nm |
| Bulk | 100 nm–1 µm |

- Avalanche/BV: junction fine mesh 필수.
- Mesh sensitivity: 결과 ±5% 확인.

### 4.3 모델

**SiC**

- Bandgap (Varshni), Fermi, incomplete ionization.
- Mobility (Lombardi/Canali), SRH, Auger.
- Impact ionization (Okuto/van Overstraeten).
- Anisotropy (가능하면).
- Interface traps (Dit), oxide tunneling.

**GaN**

- Polarization (spontaneous + piezoelectric).
- 2DEG/heterointerface.
- Mobility, traps (surface/buffer/dielectric).
- Self-heating (thermodynamic).
- Impact ionization, gate leakage.

### 4.4 Bias/추출

| 목표 | Bias | 추출 |
|---|---|---|
| $V_{th}$ | $V_{DS}$ small, $V_{GS}$ sweep | constant current |
| $R_{on}$ | linear region | $dV/dI$ |
| BV | $V_{GS}=0$, $V_{DS}$↑ | $I$ threshold |
| C-V | AC small signal | $C(V)$ |
| TLM | linear | $R(d)$ |
| transient | pulse | time constant |

### 4.5 검증

- 손계산/해석식과 비교.
- 측정 데이터와 비교.
- Mesh/model sensitivity.
- 독립 데이터로 재검증.

---

## 5. SiC/GaN 대표 파라미터

### 5.1 4H-SiC

| Parameter | Value |
|---|---|
| $E_g$ | 3.26 eV |
| $\varepsilon_r$ | 9.7 |
| $\mu_n$ (bulk) | 900–1000 cm²/Vs |
| $\mu_p$ (bulk) | 100–120 cm²/Vs |
| $v_{sat}$ | $2\times10^7$ cm/s |
| $E_c$ | 2.5–3.0 MV/cm |
| $k_{th}$ | 4.9 W/cm·K |
| $n_i$ | ~$10^{-7}$ cm⁻³ |
| Al ionization | ~0.2 eV |
| N ionization | ~0.05–0.1 eV |

### 5.2 GaN

| Parameter | Value |
|---|---|
| $E_g$ | 3.4 eV |
| $\varepsilon_r$ | 9.0 |
| $\mu_{2DEG}$ | 1500–2000 cm²/Vs |
| $v_{sat}$ | $2.5\times10^7$ cm/s |
| $E_c$ | 3.0–3.5 MV/cm |
| $k_{th}$ | 1.3–2.0 W/cm·K |
| $P_{sp}$ | -0.034 C/m² |
| 2DEG | ~$1\times10^{13}$ cm⁻² |

> 교육용 대표값. 실제는 온도/도핑/결정면/성장 조건에 따라 다름.

---

## 6. 수렴 문제 해결

| 증상 | 원인 | 해결 |
|---|---|---|
| Newton 실패 | 초기 bias/도핑 급변 | bias step↓, initial solution |
| Negative concentration | mesh/model | mesh refine, damping |
| Avalanche 미수렴 | junction mesh | local refinement |
| Current NaN | contact/float | contact 조건 확인 |
| Trap 불안정 | time step | transient step↓ |
| Oxide leakage | mesh | tunneling model |

---

## 7. 데이터 기록 템플릿

```
Date:
Day:
Operator:
Tool/version:
Structure:
Doping:
Models:
Bias:
Key results:
Observed issues:
Comparison (hand/measurement):
Conclusion:
Next action:
Files:
```

---

## 8. 실습 평가 기준

| 항목 | 배점 |
|---|---:|
| 목적/가정 명확 | 15 |
| 구조/모델 타당 | 20 |
| 결과 정확/재현 | 25 |
| 손계산/측정 비교 | 20 |
| 해석/결론 | 15 |
| 기록/파일 정리 | 5 |

> **금지**: 결과만 제출, 단위 누락, calibration 없는 절대값 주장.

---

## 9. 라이선스/데이터 정책

- 상용 TCAD 결과를 외부 공개 시 라이선스 확인.
- 회사/연구소 데이터는 **사용 허가** 확인.
- 교육용 예시 데이터와 실제 측정을 명확히 구분.
- 개인정보/기밀 정보 저장 금지.

---

*21 / 24 — TCAD Lab Manual. 20일 프로그램 부록.*
