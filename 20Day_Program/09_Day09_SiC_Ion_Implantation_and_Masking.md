# Day 09 — SiC 이온주입·마스킹

> **Week 2 / Day 9** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: Day 8
> **참조**: [`../02_Week2_SiC_Frontend_Processes.md`](../02_Week2_SiC_Frontend_Processes.md)

---

## 1. 오늘의 질문

> **"SiC에 Al/N을 원하는 깊이·농도로 넣으려면 에너지·도즈·마스크를 어떻게 설계하는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | LSS, range/straggle, channeling |
| 10:45–12:15 | Lecture 2 | Box profile, mask, damage, high-temp implant |
| 13:15–15:15 | Lab | SRIM range + SProcess box profile |
| 15:30–16:30 | Case | p-well box profile 설계 |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제 |

---

## 3. 학습목표

1. $R_p$, $\Delta R_p$와 implant profile을 계산한다.
2. Channeling과 tilt/twist의 영향을 설명한다.
3. Multi-implant로 box profile을 설계한다.
4. Mask 두께를 stopping range로 결정한다.
5. Implant damage와 high-temp implant의 효과를 설명한다.
6. SRIM/SProcess로 profile을 검증한다.

---

## 4. Lecture 1 — Implant 기초 (1.5h)

### 4.1 LSS 이론

Gaussian profile:

$$
N(x) = \frac{\Phi}{\sqrt{2\pi}\Delta R_p}\exp\left(-\frac{(x-R_p)^2}{2\Delta R_p^2}\right)
$$

- $\Phi$: dose (cm⁻²).
- $R_p$: projected range.
- $\Delta R_p$: straggle.

Peak concentration:

$$
N_{peak} = \frac{\Phi}{\sqrt{2\pi}\Delta R_p}
$$

### 4.2 에너지-범위 관계

| Ion | Energy (keV) | $R_p$ in SiC (nm, 대표) |
|---|---:|---:|
| Al | 50 | ~50 |
| Al | 200 | ~180 |
| N | 50 | ~80 |
| N | 200 | ~300 |
| B | 50 | ~150 |

> 실제 값은 SRIM/stopping data로 계산. 교육용 대표값.

### 4.3 Channeling

- 결정축을 따라 이온이 깊게 침투.
- 방지: tilt 4–7°, twist 20–30°, screen oxide.
- SiC는 hexagonal 구조로 channeling anisotropy가 큼.

### 4.4 Damage

| Damage | 원인 | 영향 |
|---|---|---|
| Point defects | low dose | recombination |
| Amorphization | high dose | activation 어려움 |
| Extended defects | implant/anneal | leakage, yield |
| Surface damage | low energy | contact/oxide |

---

## 5. Lecture 2 — Box Profile·Mask·고온주입 (1.5h)

### 5.1 Box profile

목표: $N(x) \approx$ constant over depth.

방법: 여러 에너지/도즈를 겹침.

| Energy (keV) | Dose (cm⁻²) | 역할 |
|---:|---:|---|
| 30 | $1\times10^{13}$ | surface |
| 70 | $2\times10^{13}$ | shallow |
| 150 | $3\times10^{13}$ | mid |
| 300 | $4\times10^{13}$ | deep |

> 각 implant의 weighted sum이 box에 가깝도록 최적화.

### 5.2 Mask

- Photoresist: thick, low stopping power.
- SiO₂: good mask, etch selectivity.
- Poly/SiN: high stopping.
- Metal: contamination risk.

Mask 두께:

$$
t_{mask} > R_p + 3\Delta R_p
$$

### 5.3 High-temperature implant

- 온도: 300–500°C.
- 효과: dynamic annealing, amorphization 억제, defect 감소.
- trade-off: resist 사용 불가(무기 mask), 장비 복잡.

### 5.4 Dopant 선택

| Dopant | Type | 특징 |
|---|---|---|
| Al | p | 느린 diffusion, activation 1600°C+ |
| B | p | 빠른 diffusion, shallow |
| N | n | donor, activation 1600°C+ |
| P | n | donor, deep |

> SiC에서는 **implant 후 1600–1700°C activation**이 필수. 이는 Day 10.

---

## 6. Lab — SRIM + Box Profile (2h)

### 6.1 목표

- SRIM으로 $R_p$, $\Delta R_p$ 추출.
- 4개 implant로 box profile 설계.
- Mask 두께 결정.

### 6.2 절차

1. SRIM에 SiC target, Al/N ion 설정.
2. 에너지별 $R_p$, $\Delta R_p$ 기록.
3. Gaussian sum으로 depth profile 계산.
4. Box uniformity ±10% 목표로 dose 조정.
5. Mask(SiO₂) 두께 계산.

### 6.3 제출물

- SRIM 결과 표.
- Depth profile 그래프.
- Mask 두께 계산.
- "channeling 방지 조건" 1페이지.

### 6.4 체크포인트

- [ ] $R_p$ 단위(nm).
- [ ] Dose 단위(cm⁻²).
- [ ] Mask stopping.
- [ ] Box uniformity.

---

## 7. Case — p-well Box Profile (1h)

### 7.1 요구

- p-well: 0–0.5 µm, $N_A \approx 1\times10^{17}$ cm⁻³.
- Surface concentration 제어.
- Source/body contact와 overlap.

### 7.2 설계

| Energy (keV) | Dose (cm⁻²) |
|---:|---:|
| 40 | $2\times10^{13}$ |
| 100 | $3\times10^{13}$ |
| 200 | $4\times10^{13}$ |
| 350 | $5\times10^{13}$ |

### 7.3 검증

- SRIM/SProcess profile.
- Activation 후 SIMS.
- $V_{th}$와 body diode.

### 토론

1. Channeling이 발생하면 $R_p$는 어떻게 변하는가?
2. High-temp implant가 resist 공정에 주는 제약은?
3. Al diffusion이 느린 것이 장점/단점은?

---

## 8. Quiz (5문항)

1. Gaussian implant peak concentration 공식은?
2. Channeling 방지법 2가지는?
3. Box profile을 만드는 방법은?
4. Mask 두께 기준은?
5. High-temp implant의 장점은?

**정답**: 1) $\Phi/(\sqrt{2\pi}\Delta R_p)$. 2) tilt/twist, screen oxide. 3) multi-energy implant. 4) $R_p+3\Delta R_p$. 5) damage/amorphization 감소.

---

## 9. Homework

- 1.2 kV SiC p-well box profile을 4개 implant로 설계.
- SRIM으로 Al 200 keV의 $R_p$ 계산.
- Day 10을 위해 **activation/etch** 예습.

---

## 10. 강사 노트

- SRIM은 **amorphous target** 가정 → SiC channeling은 별도 보정.
- Box profile은 **완벽한 box가 아님**을 강조.
- Mask 두께 계산에서 **safety margin**.
- 다음 날 activation과 연결해 "implant만으로는 안 된다"는 점.

---

*Day 09 / 20 — 140h program. Implant 범위는 교육용 대표값입니다.*
