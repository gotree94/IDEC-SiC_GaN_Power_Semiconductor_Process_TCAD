# Day 11 — 접촉 물리·TLM

> **Week 3 / Day 11** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: Day 10
> **참조**: [`../03_Week3_SiC_Contacts_Integration_and_Termination.md`](../03_Week3_SiC_Contacts_Integration_and_Termination.md)

---

## 1. 오늘의 질문

> **"SiC에 낮은 비저항 오믹 접촉(ρc)을 형성하려면 어떻게 해야 하며, TLM으로 얼마나 정확히 추출할 수 있는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | 금속-반도체 접촉, 쇼트키 장벽, 오믹화 |
| 10:45–12:15 | Lecture 2 | TLM 이론, 전송선 모델, 추출법 |
| 13:15–15:15 | Lab | TLM 데이터 분석, ρc/Rs 추출 |
| 15:30–16:30 | Case | Ni/SiC 실리사이드, Ti/Al p-ohmic |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제 |

---

## 3. 학습목표

1. Schottky 장벽과 오믹 접촉의 차이를 설명한다.
2. Thermionic emission, tunneling, TE+TFE 메커니즘을 구분한다.
3. TLM 패턴(Circular/Linear)의 원리와 측정법을 설명한다.
4. $R_{total}$ vs $d$ 그래프로 $R_s$, $R_c$, $L_T$를 추출한다.
5. 4H-SiC에서 Ni/n-SiC, Ti/Al/p-SiC의 오믹화 메커니즘을 설명한다.
6. 접촉 저항이 소자 성능($R_{on}$, switching)에 미치는 영향을 정량화한다.

---

## 4. Lecture 1 — 접촉 물리 (1.5h)

### 4.1 금속-반도체 접촉

이론적 쇼트키 장벽:

$$
\phi_{Bn} = \phi_m - \chi_s
$$

실제 장벽은 Fermi level pinning으로 인해 $\phi_{Bn}$이 예상보다 작아지는 경향이 있습니다.

### 4.2 전류 수송 메커니즘

| 메커니즘 | 조건 | 특징 |
|---|---|---|
| Thermionic Emission (TE) | 낮은 도핑, 중간 장벽 | $J \propto \exp(-q\phi_B/kT)$ |
| Thermionic-Field Emission (TFE) | 중간-고도핑 | 열+터널링 혼합 |
| Field Emission (FE) | 고도핑 ($>10^{19}$ cm⁻³) | 주로 터널링, 온도 의존성 작음 |

### 4.3 오믹화 전략 (SiC)

| 방식 | 적용 | 원리 |
|---|---|---|
| 고도핑 ($N_D, N_A > 10^{19}$ cm⁻³) | n+/p+ 영역 | 터널 확률↑, depletion 폭 감소 |
| 실리사이드화 | Ni/SiC 등 | 인터페이스 화학 결합, 장벽 감소 |
| 그래파이트/카바이드 형성 | 고온 어닐링 | $n$형에서 유리, $p$형은 어려움 |
| 전이금속/금속적층 | Ti/Al | $p$형에 효과적 |

> $p$-SiC의 오믹 접촉은 $n$-SiC보다 훨씬 어려우며, 일반적으로 $ρ_c$가 1~2자리 더 큽니다.

### 4.4 목표 $ρ_c$ (대표)

| 접촉 | 목표 (Ω·cm²) | 비고 |
|---|---:|---|
| Ni/n-SiC ($10^{19}$ cm⁻³) | $10^{-6} - 10^{-5}$ | 양호 |
| Ti/Al/p-SiC ($10^{19}$ cm⁻³) | $10^{-4} - 10^{-3}$ | 개선 필요 |
| GaN ohmic (Ti/Al/Ni/Au) | $10^{-6}$ | 양호 |

---

## 5. Lecture 2 — TLM 이론 (1.5h)

### 5.1 Transmission Line Model

Linear TLM: 금속 패드 간격 $d_1,...,d_n$에 전압 인가, 전류 측정.

총 저항:

$$
R_{total} = \frac{R_s}{W}(d + 2L_T)
$$

- $R_s$: 시트저항 (Ω/□)
- $L_T$: 전송길이 ($L_T = \sqrt{\rho_c/R_s}$)
- $W$: 접촉 폭

### 5.2 그래프 해석

$R_{total}$ vs $d$를 직선 fitting:

- 기울기: $R_s/W$
- $x$절편: $-2L_T$
- $y$절편: $2R_c = 2\rho_c/(L_c W)$ (long contact 가정)
- Specific contact resistance: $\rho_c = R_c L_c W = R_s L_T^2$ (Ω·cm²)

> 접촉 길이 $L_c >> L_T$이면 위 근사 유효. $L_c <= L_T$이면 correction 필요.

### 5.3 Circular TLM (CTLM)

선형 TLM보다 mesa 분리 불필요, 더 일반적입니다.

$$
R_{total} = \frac{R_s}{2\pi}\ln\frac{r_2}{r_1} + \frac{R_s}{2\pi}\frac{L_T}{r_1}C
$$

근사로 $L_T, R_s, \rho_c$ 추출합니다.

### 5.4 주의사항

- Current crowding
- Mesa isolation 유무
- Probe alignment
- Junction leakage
- High temperature 측정

---

## 6. Lab — TLM 데이터 분석 (2h)

### 6.1 데이터(예)

Linear TLM, $W = 100$ µm

| 패드간격 $d$ (µm) | $R_{total}$ (Ω) |
|---:|---:|
| 5 | 12.5 |
| 10 | 17.0 |
| 20 | 26.0 |
| 40 | 44.0 |

### 6.2 절차

1. $R_{total}$ vs $d$ plot.
2. Linear regression으로 기울기, 절편 계산.
3. $R_s = (slope)*W$.
4. $L_T = -x_{intercept}/2$.
5. $\rho_c = R_s * L_T^2$.
6. 측정값 유효성 확인($L_c >> L_T$?).

### 6.3 제출물

- TLM plot.
- 계산표($R_s, R_c, L_T, \rho_c$).
- 접촉 저항이 $R_{on}$에 미치는 영향(가정).
- 개선 방안 제안.

### 6.4 체크포인트

- [ ] 단위 변환(µm → cm).
- [ ] Linear fit $R^2 > 0.99$.
- [ ] $\rho_c$ 단위(Ω·cm²).
- [ ] Physical한 값인지 검토.

---

## 7. Case — Ni/n-SiC 실리사이드 (1h)

### 7.1 공정

1. HF clean 후 Ni 증착(50–100 nm)
2. RTA 900–1000°C, N₂ 분위기
3. Unreacted Ni 제거(HCl)
4. Forming gas anneal(선택)

### 7.2 메커니즘

- Ni + SiC → Ni₂Si, NiSi, graphite
- Interface에서 C-rich layer 형성
- $n$-SiC에서 효과적, $p$-SiC는 부적합
- $ρ_c \sim 10^{-6}$ Ω·cm² 달성

### 7.3 Ti/Al/p-SiC

- Ti/Al (70/30), 950–1000°C
- Ti silicide 형성 + Al 도핑 효과
- $ρ_c \sim 10^{-4}$ Ω·cm²
- p+ implant 농도에 매우 민감

### 토론

1. 왜 $p$-SiC ohmic이 더 어려운가?
2. Unreacted Ni 제거가 필요한 이유는?
3. 접촉 면적을 늘리면 $\rho_c$는 변하는가?

---

## 8. Quiz (5문항)

1. Schottky 장벽 공식($\phi_{Bn}$)?
2. TLM에서 $L_T = \sqrt{\rho_c/R_s}$의 의미는?
3. $R_{total}$ vs $d$ 그래프의 기울기/절편은?
4. Ni/n-SiC 실리사이드의 장점은?
5. Field emission이 지배적이려면 필요한 조건은?

**정답**: 1) $\phi_m - \chi_s$. 2) 전류가 접촉 아래로 퍼지는 특성 길이. 3) 기울기 $R_s/W$, 절편 $2R_c$. 4) 낮은 $ρ_c$, 공정 호환. 5) 고도핑($>10^{19}$ cm⁻³), 얇은 장벽.

---

## 9. Homework

- 주어진 TLM 데이터로 모든 파라미터 추출.
- p+ 농도를 $10^{18}\to10^{19}$ cm⁻³로 올리면 $\rho_c$는 어떻게 변할 것인가?
- Day 12를 위해 **JBS/MPS** 예습.

---

## 10. 강사 노트

- TLM은 **"측정 실수"** 가 가장 많음(단위, fit).
- $L_T << L_c$ 여부를 반드시 확인.
- $p$-ohmic은 **개선의 여지**가 큰 영역.
- 다음 날 JBS/MPS로 전계 완화와 연결.

---

*Day 11 / 20 — 140h program. 접촉 저항값은 교육용 대표값입니다.*
