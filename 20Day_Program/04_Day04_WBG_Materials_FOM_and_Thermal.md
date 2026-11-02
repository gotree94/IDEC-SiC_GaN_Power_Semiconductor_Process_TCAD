# Day 04 — WBG 물성·FOM·열설계

> **Week 1 / Day 4** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: Day 2–3
> **참조**: [`../01_Week1_Overview_and_WBG_Process_Fundamentals.md`](../01_Week1_Overview_and_WBG_Process_Fundamentals.md)

---

## 1. 오늘의 질문

> **"SiC/GaN이 Si보다 유리한 이유를 FOM과 열저항으로 어떻게 숫자화하며, 언제 그 우위가 사라지는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | Si/SiC/GaN/Ga₂O₃/Diamond 물성 |
| 10:45–12:15 | Lecture 2 | Baliga/Johnson FOM, Ron,sp, 열설계 |
| 13:15–15:15 | Lab | FOM·소재 선택 계산기 |
| 15:30–16:30 | Case | EV/PV/데이터센터/철도 소재 선택 |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제 |

---

## 3. 학습목표

1. 5개 소재의 핵심 물성을 비교한다.
2. 4H-SiC의 **이방성**과 결정면 영향을 설명한다.
3. Baliga FOM과 ideal $R_{on,sp}$를 계산한다.
4. 열저항·냉각·$T_j$를 설계에 반영한다.
5. 응용별 소재 선택을 정당화한다.
6. FOM의 한계(채널 이동도, 열, 가격)를 설명한다.

---

## 4. Lecture 1 — 소재 물성 (1.5h)

### 4.1 비교표

| 물성 | Si | 4H-SiC | GaN | Ga₂O₃ | Diamond |
|---|---:|---:|---:|---:|---:|
| $E_g$ (eV) | 1.12 | 3.26 | 3.4 | 4.8–4.9 | 5.5 |
| $E_c$ (MV/cm) | 0.3 | 2.5–3.0 | 3.0–3.5 | 8 | 10 |
| $\mu_n$ (cm²/Vs) | 1400 | 900–1000 | 1500–2000 (2DEG) | 100–200 | 2000 |
| $v_{sat}$ ($10^7$ cm/s) | 1.0 | 2.0 | 2.5 | 1.0 | 2.7 |
| $\varepsilon_r$ | 11.7 | 9.7 | 9.0 | 10 | 5.7 |
| $k_{th}$ (W/cm·K) | 1.5 | 4.9 | 1.3–2.0 | 0.1–0.3 | 20 |
| 열팽창 ($10^{-6}$/K) | 2.6 | 4.0–4.5 | 5.6 | 5.0 | 1.1 |

### 4.2 4H-SiC 이방성

- $c$-axis vs $a$-axis: 이동도, 항복전계, 열전도도가 다름.
- $c$-axis 전자 이동도가 높고, hole 이동도는 낮음.
- 결정면: Si-face, C-face, a-face; MOS 계면 특성 차이.
- Off-cut 4°: step-flow epitaxy, trench 면 제어.

> **실무**: 같은 4H-SiC라도 **결정면·off-cut·epi 조건**이 소자 성능을 바꿉니다.

### 4.3 GaN

- Wurtzite 구조, 강한 spontaneous/piezoelectric polarization.
- AlGaN/GaN 2DEG: 도핑 없이 고이동도 전자.
- GaN-on-Si, GaN-on-SiC, GaN-on-sapphire.
- 열전도도 낮음 → GaN-on-SiC가 고전력에 유리.

### 4.4 Ga₂O₃ / Diamond

- Ga₂O₃: 초광대역, melt growth 가능, but 열전도도 매우 낮음.
- Diamond: 최고 FOM, but 대면적/도핑/가격 한계.
- 교육 포인트: **FOM만으로 소재를 고르지 않는다.**

---

## 5. Lecture 2 — FOM과 열설계 (1.5h)

### 5.1 Ideal $R_{on,sp}$

$$
R_{on,sp} = \frac{4BV^2}{\varepsilon \mu E_c^3}
$$

유도: $W = 2BV/E_c$, $R_{on,sp} = W/(q\mu N_D)$, $BV = \varepsilon E_c^2/(2qN_D)$.

### 5.2 Baliga FOM

$$
BFOM = \varepsilon \mu E_c^3
$$

| 물질 | $BFOM$ (Si=1, 대표) |
|---|---:|
| Si | 1 |
| 4H-SiC | ~300–400 |
| GaN | ~800–1000 |
| Ga₂O₃ | ~3000 (이론) |
| Diamond | ~20000 (이론) |

> **주의**: BFOM은 **이론적 unipolar 한계**입니다. 실제는 채널 이동도, 계면, 열, 패키지가 지배합니다.

### 5.3 Johnson FOM

$$
JFOM = E_c \cdot v_{sat}
$$

고주파·고전력 한계를 나타냅니다.

### 5.4 열설계

$$
T_j = T_a + P_{loss} R_{th,j-a}
$$

$$
R_{th,j-a} = R_{th,jc} + R_{th,cs} + R_{th,sa}
$$

| 요소 | 의미 | 개선 |
|---|---|---|
| $R_{th,jc}$ | chip→case | 소재, solder, area |
| $R_{th,cs}$ | case→heatsink | TIM |
| $R_{th,sa}$ | heatsink→ambient | 면적, airflow |

> SiC는 $k_{th}$가 크지만, **die attach/solder/package**가 전체 열저항을 좌우합니다.

### 5.5 FOM의 한계

1. 채널 이동도(SiC MOS) → 실제 $R_{on}$ 악화.
2. 열전도도(GaN) → self-heating.
3. 가격/대면적/결함밀도.
4. 신뢰성(oxide, trapping).
5. Short-circuit robustness.

---

## 6. Lab — FOM·소재 선택 (2h)

### 6.1 목표

- 650 V, 1.2 kV, 3.3 kV, 10 kV에 소재를 선택.
- FOM과 실제 $R_{on,sp}$를 비교.
- 열저항을 포함한 손실 계산.

### 6.2 절차

1. $BV$ = 650, 1200, 3300, 10000 V.
2. 각 소재의 $\varepsilon, \mu, E_c$ 입력.
3. ideal $R_{on,sp}$ 계산.
4. 실제 $R_{on,sp}$는 **채널/계면 factor**를 곱해 보정.
5. $P_{cond}$, $T_j$ 계산.
6. 표와 radar chart 작성.

### 6.3 제출물

- 소재×전압 표.
- "선택 + 근거 + 리스크" 1페이지.
- FOM vs 실제 성능 gap 설명.

### 6.4 체크포인트

- [ ] $R_{on,sp}$ 단위(Ω·cm²).
- [ ] $\mu$와 $E_c$의 단위.
- [ ] 채널 이동도 보정.
- [ ] 열저항 포함.

---

## 7. Case — 응용별 선택 (1h)

| 응용 | 1순위 | 이유 | 리스크 |
|---|---|---|---|
| EV 인버터 800 V | SiC MOSFET | 고온·고효율·신뢰 | SCWT, oxide |
| 데이터센터 PSU | GaN HEMT | 고주파·소형 | dynamic Ron, EMI |
| PV 1500 V | SiC | 고전압·효율 | cost |
| 철도 3.3 kV | SiC | 고전압·경량 | reliability, cost |
| RF/5G | GaN | high frequency | thermal |
| 초고압 grid | SiC/IGBT | robustness | maturity |

### 토론

1. Ga₂O₃가 BFOM 1위인데 EV에 안 쓰이는 이유는?
2. SiC MOSFET의 채널 이동도를 2배로 올리면 FOM gap은 얼마나 줄어드는가?
3. GaN-on-Si와 GaN-on-SiC의 선택 기준은?

---

## 8. Quiz (5문항)

1. Baliga FOM 공식은?
2. $R_{on,sp}$와 $BV$의 관계는?
3. 4H-SiC 이방성이란?
4. GaN 열전도도가 낮은데 고전력에 쓰는 이유는?
5. FOM이 실제 성능과 다른 이유 2가지는?

**정답**: 1) $\varepsilon\mu E_c^3$. 2) $R_{on,sp}\propto BV^2$. 3) 결정축에 따라 물성이 다름. 4) GaN-on-SiC 기판. 5) 채널 이동도/계면, 열/패키지.

---

## 9. Homework

- 1.2 kV SiC MOSFET의 ideal vs actual $R_{on,sp}$를 계산.
- 열저항 3단 직렬 모델로 $T_j$ 계산.
- Day 5를 위해 **소자 taxonomy** 예습.

---

## 10. 강사 노트

- FOM은 **"이론적 상한"** 임을 강조.
- SiC 채널 이동도 저하를 실제 $R_{on}$과 연결.
- GaN 열문제는 **package** 예시로 설명.
- 다음 날 소자 구조로 넘어가며 "FOM→구조→공정" 연결.

---

*Day 04 / 20 — 140h program. 물성/FOM은 교육용 대표값입니다.*
