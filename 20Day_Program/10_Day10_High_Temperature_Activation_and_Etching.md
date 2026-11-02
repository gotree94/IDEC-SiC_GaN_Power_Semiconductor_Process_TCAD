# Day 10 — 고온 활성화·SiC 식각

> **Week 2 / Day 10** | **7시간** | 강의 4h + 실습 2h + 주차 시험 1h
> **선수**: Day 9
> **참조**: [`../02_Week2_SiC_Frontend_Processes.md`](../02_Week2_SiC_Frontend_Processes.md)

---

## 1. 오늘의 질문

> **"Implant한 Al/N을 전기적으로 활성화하면서 표면을 보존하려면, 그리고 trench를 수직·매끄럽게 etch하려면 어떻게 해야 하는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | 고온 활성화, cap, diffusion, morphology |
| 10:45–12:15 | Lecture 2 | ICP 식각, trench, corner rounding, damage |
| 13:15–15:15 | Lab | SRP/Hall 분석 + etch profile |
| 15:30–16:30 | Week 2 Exam | 10문항, 30분 + 리뷰 |
| 16:30–17:30 | Review | 시험 해설, Week3 예고 |

---

## 3. 학습목표

1. SiC activation 온도·시간·cap의 역할을 설명한다.
2. Al/N의 diffusion과 activation rate를 비교한다.
3. SRP/Hall/SIMS로 활성화를 평가한다.
4. ICP 식각 화학(SF₆/O₂, Cl₂)과 mask 선택을 설명한다.
5. Trench profile·corner rounding·damage를 제어한다.
6. Week2 내용을 공정 flow로 통합한다.

---

## 4. Lecture 1 — 고온 활성화 (1.5h)

### 4.1 왜 고온인가?

- SiC는 강한 Si-C 결합 → dopant diffusion/activation에 고에너지 필요.
- Al acceptor: activation 1600–1700°C.
- N donor: activation 1600–1700°C.
- SiC에서 dopant diffusion은 Si보다 매우 느림 → profile 유지에 유리.

### 4.2 Cap layer

| Cap | 장점 | 단점 |
|---|---|---|
| AlN | 고온 안정, SiC와 유사 | 습식 strip 어려움 |
| SiO₂ | 공정 친화 | 고온에서 결정화/박리 |
| Carbon | 고온 안정 | strip/오염 |
| SiN | barrier | stress |

> Cap 목적: **SiC 표면 decomposition(Si sublimation) 억제**, dopant out-diffusion 방지.

### 4.3 활성화 평가

| 기법 | 측정 | 해석 |
|---|---|---|
| SRP | spreading resistance | carrier profile |
| Hall | sheet resistance, mobility | activation rate |
| SIMS | chemical profile | diffusion |
| C-V | depletion | doping |
| Raman | crystallinity | damage |

활성화율:

$$
\eta = \frac{N_{active}}{N_{chemical}}
$$

> Al은 activation rate가 낮고(수십 %), N은 상대적으로 높음.

### 4.4 Morphology

- Step-bunching: off-cut/anneal에 의한 표면 계단.
- Pits: Si sublimation, cap defect.
- Roughness: oxide/etch damage.
- 대응: cap quality, ramp rate, temperature uniformity.

---

## 5. Lecture 2 — SiC 식각 (1.5h)

### 5.1 ICP 식각

| Gas | 역할 | 특징 |
|---|---|---|
| SF₆ | F radical, SiC etch | 빠름, isotropic component |
| O₂ | sidewall passivation | anisotropy |
| Cl₂ | SiC etch | 느림, corrosion |
| Ar | physical sputter | damage |

### 5.2 Trench 식각 요구

| 항목 | 목표(예) |
|---|---|
| depth | 1–2 µm |
| angle | 85–90° |
| mask selectivity | >5:1 |
| sidewall roughness | <5 nm |
| corner rounding | 20–50 nm |
| damage | 최소 |

### 5.3 Corner rounding

- Trench bottom corner 전계 집중 → oxide breakdown.
- 방법: sacrificial oxidation, isotropic etch, H₂ anneal.
- 목표: rounded corner + smooth sidewall.

### 5.4 Mask

- SiO₂: moderate selectivity.
- Ni: high selectivity, metal contamination.
- Photoresist: low selectivity, thick 필요.

### 5.5 식각 후 처리

- Sacrificial oxidation (damage 제거).
- HF strip.
- Clean.
- Gate oxide 전 surface 확인.

---

## 6. Lab — SRP/Hall + Etch Profile (2h)

### 6.1 목표

- SRP/Hall 데이터로 activation rate 계산.
- Etch profile에서 angle/depth/sidewall 평가.
- Week2 공정 flow에 반영.

### 6.2 데이터(예)

| Sample | Anneal | Sheet resistance (Ω/sq) | Hall mobility (cm²/Vs) |
|---|---|---:|---:|
| A | 1600°C/30s | 2,500 | 30 |
| B | 1650°C/30s | 1,800 | 40 |
| C | 1700°C/30s | 1,500 | 45 |
| D | 1700°C/2min | 1,300 | 50 |

### 6.3 절차

1. Sheet resistance → sheet carrier density.
2. Hall mobility로 active dose 추정.
3. Chemical dose와 비교 → activation rate.
4. Etch SEM image에서 depth/angle 측정.
5. Corner radius 추정.

### 6.4 제출물

- Activation rate 표.
- Etch profile 그림.
- "gate oxide 전 처리" 제안.

### 6.5 체크포인트

- [ ] $R_s$와 $\rho$ 구분.
- [ ] Hall mobility 단위.
- [ ] Etch angle 정의.
- [ ] Sacrificial oxidation 필요성.

---

## 7. Week 2 Exam (30분, 10문항)

1. Piranha/SC1/SC2/HF의 역할?
2. Deal-Grove 식?
3. SiC 산화가 느린 이유?
4. Si-face vs C-face 산화 속도?
5. Dit 추출법 2가지?
6. NO anneal의 목적?
7. Gaussian implant peak 농도?
8. Channeling 방지법?
9. SiC activation 온도와 cap 목적?
10. Trench corner rounding 방법?

**정답 요약**: 1) organic/particle/metal/oxide. 2) $x^2+Ax=B(t+\tau)$. 3) C 제거 반응. 4) C-face 빠름. 5) high-low, Terman/conductance. 6) Dit 감소. 7) $\Phi/(\sqrt{2\pi}\Delta R_p)$. 8) tilt/twist, screen oxide. 9) 1600–1700°C, Si sublimation 억제. 10) sacrificial oxidation/isotropic etch.

---

## 8. Homework

- 1.2 kV SiC MOSFET의 activation 조건을 정하고 근거를 작성.
- Trench etch recipe를 gas ratio 중심으로 설계.
- Week3를 위해 **TLM/접촉** 예습.

---

## 9. 강사 노트

- Activation은 **"도핑 ≠ 활성화"** 를 강조.
- Cap 품질이 morphology를 지배.
- Trench corner는 **oxide reliability**와 직결.
- Week3 시작 전 **접촉 물리** 복습.

---

*Day 10 / 20 — 140h program. 활성화/식각 조건은 교육용 대표값입니다.*
