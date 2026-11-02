# Day 19 — TCAD Calibration·DOE·산업 실무

> **Week 4 / Day 19** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: Day 18
> **참조**: [`../05_TCAD_Verification_Setup_and_Calibration.md`](../05_TCAD_Verification_Setup_and_Calibration.md), [`../06_Industry_Practice.md`](../06_Industry_Practice.md)

---

## 1. 오늘의 질문

> **"TCAD를 측정에 맞추는 절차는 무엇이며, DOE·PCM·SPC·PPAP를 어떻게 하나의 제품화 흐름으로 연결하는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | Calibration workflow, 데이터/오차 |
| 10:45–12:15 | Lecture 2 | DOE, PCM, SPC, PPAP, DTCO |
| 13:15–15:15 | Lab | Calibration/DOE 실습 |
| 15:30–16:30 | Case | 1.2 kV SiC calibration 사례 |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제 |

---

## 3. 학습목표

1. TCAD calibration의 단계와 데이터 우선순위를 설명한다.
2. SIMS/SRP/C-V/I-V/TLM을 parameter에 매핑한다.
3. 오차 지표와 acceptance criteria를 정의한다.
4. DOE(Full factorial/Taguchi/RSM)를 설계한다.
5. PCM/SPC/Cpk/PPAP/8D를 제품화 흐름으로 연결한다.
6. DTCO 관점에서 공정-소자-회로 trade-off를 설명한다.

---

## 4. Lecture 1 — TCAD Calibration (1.5h)

### 4.1 Workflow

1. **목표 정의**: BV, $R_{on}$, $V_{th}$, leakage, switching.
2. **구조 설정**: epi/implant/oxide/mesh.
3. **물리 모델 선택**: incomplete ionization, mobility, SRH, impact ionization, traps.
4. **파라미터 추출**: SIMS → doping, C-V → $N_A$/Dit, I-V → lifetime/barrier.
5. **비교/오차**: 실험 vs TCAD.
6. **민감도 분석**: 어떤 파라미터가 결과를 지배?
7. **검증**: 독립 데이터로 확인.

### 4.2 데이터 매핑

| 측정 | 추출 | TCAD parameter |
|---|---|---|
| SIMS | chemical profile | implant/diffusion |
| SRP | carrier profile | doping/activation |
| C-V | $N_A$, $t_{ox}$, Dit | doping/oxide/interface |
| $I_D$–$V_{GS}$ | $V_{th}$, $\mu_{ch}$ | channel mobility |
| $I_D$–$V_{DS}$ | $R_{on}$ | mobility/contact |
| TLM | $\rho_c$ | contact |
| BV | $E_c$, termination | impact ionization/charge |
| transient | lifetime/traps | SRH/trap |

### 4.3 오차 지표

$$
\text{Error}(\%) = \frac{|X_{sim}-X_{exp}|}{X_{exp}}\times100
$$

| 항목 | 목표(예) |
|---|---:|
| $V_{th}$ | ±0.2 V |
| $R_{on}$ | ±10% |
| BV | ±5% |
| leakage | ±2× |
| $C_{iss}/C_{oss}$ | ±10% |

> **원칙**: 모든 항목을 동시에 맞추지 말고, **물리적으로 독립적인 데이터**로 순차 calibration.

### 4.4 Mesh/수렴

- Junction/oxide/interface에 fine mesh.
- Avalanche simulation은 mesh 민감.
- Convergence 실패: doping gradient, model 조합, initial bias.

---

## 5. Lecture 2 — DOE·산업 실무 (1.5h)

### 5.1 DOE

| 설계 | 장점 | 단점 |
|---|---|---|
| Full factorial | interaction 파악 | 실험 수 많음 |
| Taguchi | 효율적 | interaction 제한 |
| RSM | 최적점 탐색 | 모델 필요 |
| Latin hypercube | 공간 충전 | 해석 복잡 |

### 5.2 PCM

- Process Control Monitor: 공정 후 전기/물리 측정.
- 예: $V_{th}$, $R_{on}$, $R_s$, $t_{ox}$, $\rho_c$.
- 목적: lot/wafer 이상 조기 감지.

### 5.3 SPC/Cpk

$$
C_{pk} = \min\left(\frac{USL-\mu}{3\sigma},\frac{\mu-LSL}{3\sigma}\right)
$$

- $C_{pk}>1.33$: 양호.
- $C_{pk}<1.0$: 개선 필요.
- Control chart: trend/shift/outlier.

### 5.4 PPAP/8D

| 도구 | 목적 |
|---|---|
| PPAP | 양산 전 공정 승인 |
| FMEA | risk 우선순위 |
| Control Plan | 관리 항목 |
| MSA | 측정 시스템 |
| 8D | 불량 근본원인/재발방지 |

### 5.5 DTCO

- Design-Technology Co-Optimization.
- 공정/소자/회로를 함께 최적화.
- 예: cell pitch vs $R_{on}$ vs $C_{gd}$ vs switching loss.

---

## 6. Lab — Calibration/DOE (2h)

### 6.1 목표

- 1.2 kV SiC MOSFET의 $V_{th}$/$R_{on}$/BV calibration.
- 2-factor DOE로 민감도 평가.
- Acceptance criteria 작성.

### 6.2 절차

1. Baseline TCAD 실행.
2. $t_{ox}$ 40/50/60 nm, $N_A$ $8/10/12\times10^{16}$ cm⁻³.
3. $V_{th}$, $R_{on}$, BV 추출.
4. Main effect plot.
5. 실험값과 비교.
6. Calibration parameter 선택.

### 6.3 제출물

- DOE 표.
- Main effect 그래프.
- Calibration 전/후 비교.
- "acceptance criteria" 1페이지.

### 6.4 체크포인트

- [ ] Factor/level 정의.
- [ ] Run 수.
- [ ] 오차 계산.
- [ ] 물리적 타당성.

---

## 7. Case — 1.2 kV SiC Calibration (1h)

### 7.1 문제

- TCAD $R_{on}$이 측정보다 30% 낮음.
- $V_{th}$는 0.3 V 높음.

### 7.2 원인

1. Channel mobility 과대.
2. Dit/incomplete ionization 미반영.
3. Contact resistance 누락.
4. $t_{ox}$ 과소.

### 7.3 대응

- $\mu_{ch}$ 조정.
- Dit 추가.
- $\rho_c$ 반영.
- $t_{ox}$ TEM 확인.

### 토론

1. $R_{on}$과 $V_{th}$를 동시에 맞추기 어려운 이유?
2. BV calibration에서 termination을 어떻게 처리?
3. DOE 없이 one-factor-at-a-time의 문제는?

---

## 8. Quiz (5문항)

1. Calibration 순서 4단계?
2. SIMS/SRP/C-V의 역할?
3. $C_{pk}$ 공식?
4. DOE의 목적?
5. DTCO란?

**정답**: 1) 목표/구조/모델/추출/비교/검증. 2) chemical/carrier/interface. 3) $\min((USL-\mu)/3\sigma,(\mu-LSL)/3\sigma)$. 4. 효율적 실험/최적화. 5. 설계-기술 공동최적화.

---

## 9. Homework

- 1.2 kV SiC calibration plan 작성.
- $C_{pk}$ 1.33을 위한 spec 폭 계산.
- Day 20 캡스톤 준비.

---

## 10. 강사 노트

- "Calibration은 curve fitting이 아니라 **물리 검증**".
- 모든 데이터를 한 번에 맞추지 말 것.
- DOE는 **factor interaction**을 봐야.
- 다음 날 캡스톤.

---

*Day 19 / 20 — 140h program. Calibration 기준은 교육용 예시입니다.*
