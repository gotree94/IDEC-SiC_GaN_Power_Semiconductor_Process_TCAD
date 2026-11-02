# Day 07 — SiC 열산화 Kinetics

> **Week 2 / Day 7** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: Day 6
> **참조**: [`../02_Week2_SiC_Frontend_Processes.md`](../02_Week2_SiC_Frontend_Processes.md)

---

## 1. 오늘의 질문

> **"SiC 산화는 왜 Si보다 느리고, 50 nm gate oxide를 균일하게 만드는 조건은 무엇인가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | Deal-Grove, SiC 산화 메커니즘 |
| 10:45–12:15 | Lecture 2 | 면 의존성, dry/wet, sacrificial oxidation |
| 13:15–15:15 | Lab | 산화 두께 fitting, rate constant 추출 |
| 15:30–16:30 | Case | Gate oxide uniformity/Dit |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제 |

---

## 3. 학습목표

1. Deal-Grove 선형-포물선 모델을 설명한다.
2. SiC 산화가 Si보다 느린 이유를 화학·확산 관점에서 설명한다.
3. Si-face/C-face/a-face의 산화 속도 차이를 정량화한다.
4. Dry/wet/N₂O/NO 산화의 trade-off를 비교한다.
5. 두께-시간 데이터로 $B/A$, $B$를 추출한다.
6. Gate oxide uniformity와 계면 품질을 평가한다.

---

## 4. Lecture 1 — Deal-Grove (1.5h)

### 4.1 모델

산화막 두께 $x$:

$$
x^2 + Ax = B(t+\tau)
$$

- $B$: parabolic rate constant (diffusion-limited).
- $B/A$: linear rate constant (reaction-limited).
- $\tau$: 초기 산화막 보정.

두 영역:

| 영역 | 조건 | 근사 |
|---|---|---|
| Linear | 얇은 oxide | $x \approx (B/A)t$ |
| Parabolic | 두꺼운 oxide | $x \approx \sqrt{Bt}$ |

### 4.2 물리

- 산화종이 oxide를 통해 확산 → 계면 반응.
- 반응 제한: 초기 선형.
- 확산 제한: 후기 포물선.
- 온도↑ → $B$, $B/A$ 증가 (Arrhenius).

$$
B, B/A \propto \exp(-E_a/kT)
$$

### 4.3 Si vs SiC

| 항목 | Si | 4H-SiC |
|---|---|---|
| 산화 속도 | 빠름 | 1자리 이상 느림 |
| 산화종 | O₂/H₂O | O₂/H₂O |
| 부산물 | — | CO/CO₂ |
| 계면 | 비교적 안정 | carbon cluster, Dit |
| 온도 | 800–1100°C | 1100–1300°C |
| 용도 | gate | gate, sacrificial |

> SiC는 **C를 기체(CO/CO₂)로 배출**해야 하므로 반응이 느리고, 계면에 carbon residue가 남기 쉽습니다.

---

## 5. Lecture 2 — 면 의존성과 공정 (1.5h)

### 5.1 결정면

| 면 | 산화 속도 | 계면 품질 | 용도 |
|---|---|---|---|
| Si-face (0001) | 느림 | 상대적 우수 | MOS gate |
| C-face (000-1) | 빠름 | trap 많음 | backside, ohmic |
| a-face | 중간 | 중간 | 연구 |

> **실무**: Si-face가 느리므로 **고온/장시간**이 필요하고, C-face는 빠르지만 계면 품질이 나쁩니다.

### 5.2 Dry vs Wet vs N₂O/NO

| 공정 | 속도 | 품질 | 용도 |
|---|---|---|---|
| Dry O₂ | 느림 | 좋음 | gate |
| Wet O₂ | 빠름 | 상대적 나쁨 | thick field oxide |
| N₂O | 중간 | N incorporation | gate |
| NO | 중간 | Dit 감소 | gate post-oxidation |
| TEOS CVD | 빠름 | as-deposited | ILD, cap |

### 5.3 Sacrificial oxidation

- 목적: polishing/implant damage 제거.
- 두께: 20–100 nm.
- Strip: HF.
- 효과: oxide breakdown 개선.
- 주의: 반복 시 roughness 증가.

### 5.4 Gate oxide 요구

| 항목 | 목표(예) |
|---|---|
| thickness | 40–60 nm |
| uniformity | ±3–5% |
| $E_{bd}$ | >8 MV/cm |
| $Q_{bd}$ | >1 C/cm² |
| Dit | $<10^{12}$ cm⁻²eV⁻¹ |
| leakage @5 MV/cm | <1 µA/cm² |

---

## 6. Lab — 산화 두께 Fitting (2h)

### 6.1 목표

- 두께-시간 데이터로 Deal-Grove parameter 추출.
- Si vs SiC, Si-face vs C-face 비교.
- Gate oxide 조건 결정.

### 6.2 데이터(예)

| 시간 (min) | Si 1000°C (nm) | SiC 1200°C Si-face (nm) | SiC 1200°C C-face (nm) |
|---:|---:|---:|---:|
| 15 | 25 | 10 | 25 |
| 30 | 45 | 18 | 45 |
| 60 | 80 | 32 | 80 |
| 120 | 140 | 55 | 140 |

> 교육용 가상 데이터. 실제는 장비/조건에 따라 다름.

### 6.3 절차

1. $x$ vs $t$ plot.
2. $x^2$ vs $t$ plot → 기울기 $B$.
3. 얇은 영역 $x$ vs $t$ → 기울기 $B/A$.
4. Arrhenius plot로 $E_a$ 추정(데이터 있으면).
5. 50 nm 조건 역산.

### 6.4 제출물

- $x$–$t$, $x^2$–$t$ 그래프.
- $B$, $B/A$ 표.
- 50 nm gate oxide 조건(온도/시간).
- SiC가 느린 이유 설명.

### 6.5 체크포인트

- [ ] 단위(nm, min, µm).
- [ ] 선형/포물선 영역 구분.
- [ ] $\tau$ 보정.
- [ ] SiC C-face vs Si-face.

---

## 7. Case — Gate Oxide Uniformity (1h)

### 7.1 문제

- 150 mm wafer에서 oxide thickness ±10%.
- $V_{th}$ 분포 증가, breakdown yield 감소.

### 7.2 원인

1. 온도 uniformity.
2. O₂ flow 분포.
3. Surface orientation/off-cut.
4. Prior clean damage.
5. Furnace loading effect.

### 7.3 대응

- Temperature ramp/soak 최적화.
- Vertical furnace, gas distribution.
- Sacrificial oxidation.
- In-situ thickness mapping.

### 토론

1. SiC 산화가 diffusion-limited가 되는 두께는?
2. C-face를 gate에 쓰지 않는 이유는?
3. NO anneal이 Dit를 줄이는 메커니즘은?

---

## 8. Quiz (5문항)

1. Deal-Grove 식을 쓰시오.
2. 선형 영역과 포물선 영역의 물리적 의미는?
3. SiC 산화가 Si보다 느린 이유는?
4. Si-face와 C-face의 산화 속도 차이는?
5. Sacrificial oxidation의 목적은?

**정답**: 1) $x^2+Ax=B(t+\tau)$. 2) 반응 제한/확산 제한. 3) C 제거 반응, 느린 kinetics. 4) C-face가 빠름. 5) damage/carbon 제거.

---

## 9. Homework

- 50 nm SiC gate oxide를 1200°C dry O₂로 만들 때 시간을 추정.
- SiC 산화의 $E_a$ 문헌값 조사.
- Day 8을 위해 **Dit/C-V** 예습.

---

## 10. 강사 노트

- Deal-Grove는 **모델**이며 실제 SiC는 초기 비선형이 큼.
- "두께 균일도 = $V_{th}$ 균일도" 연결.
- C-face/Si-face를 **결정 구조**로 설명.
- 다음 날 C-V/Dit로 계면 품질을 정량화.

---

*Day 07 / 20 — 140h program. 산화 데이터는 교육용 예시입니다.*
