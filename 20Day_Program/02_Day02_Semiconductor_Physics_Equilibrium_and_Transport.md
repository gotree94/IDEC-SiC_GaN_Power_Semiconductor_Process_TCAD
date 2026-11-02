# Day 02 — 반도체 물리 I: 평형·캐리어·수송

> **Week 1 / Day 2** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: Day 1
> **참조**: [`../01_Week1_Overview_and_WBG_Process_Fundamentals.md`](../01_Week1_Overview_and_WBG_Process_Fundamentals.md)

---

## 1. 오늘의 질문

> **"도핑과 온도가 페르미 준위·캐리어 농도·전류를 어떻게 바꾸며, SiC/GaN에서 무엇이 달라지는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | 밴드 구조, 도핑, 페르미 준위, 캐리어 농도 |
| 10:45–12:15 | Lecture 2 | 드리프트·확산, 이동도, 고전계, 열 |
| 13:15–15:15 | Lab | 1D Poisson/C-V 계산 |
| 15:30–16:30 | Case | SiC 채널 이동도, GaN 2DEG |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제 |

---

## 3. 학습목표

1. 에너지 밴드·도핑·페르미 준위의 관계를 설명한다.
2. $n$, $p$, $n_i$를 계산하고 mass-action law를 적용한다.
3. SiC/GaN의 **불완전 이온화**를 설명한다.
4. 드리프트·확산 전류와 Einstein relation을 유도한다.
5. 이동도·포화 속도·열전도도가 소자 성능에 미치는 영향을 정량화한다.
6. 1D Poisson으로 C-V와 공핍을 계산한다.

---

## 4. Lecture 1 — 평형 상태의 반도체 (1.5h)

### 4.1 밴드 구조와 물성

| 물질 | $E_g$ (eV, 300K) | 전자 이동도 (cm²/Vs) | 포화 속도 (cm/s) | 열전도도 (W/cm·K) |
|---|---:|---:|---:|---:|
| Si | 1.12 | 1400 | $1.0\times10^7$ | 1.5 |
| 4H-SiC | 3.26 | 900–1000 (bulk) | $2.0\times10^7$ | 4.9 |
| GaN | 3.4 | 1500–2000 (2DEG) | $2.5\times10^7$ | 1.3–2.0 |
| Ga₂O₃ | 4.8–4.9 | 100–200 | $1.0\times10^7$ | 0.1–0.3 |
| Diamond | 5.5 | 2000 | $2.7\times10^7$ | 20 |

> **주의**: SiC MOSFET의 **채널 이동도**는 bulk와 다릅니다. 계면 산란으로 20–40 cm²/Vs 수준까지 낮아질 수 있습니다.

### 4.2 캐리어 농도

열평형에서:

$$
n = N_C \exp\left(-\frac{E_C-E_F}{kT}\right), \quad
p = N_V \exp\left(-\frac{E_F-E_V}{kT}\right)
$$

질량작용:

$$
np = n_i^2
$$

진성 농도:

$$
n_i = \sqrt{N_C N_V}\exp\left(-\frac{E_g}{2kT}\right)
$$

| 물질 | $n_i$ @300K (대표값) |
|---|---:|
| Si | $1.5\times10^{10}$ cm⁻³ |
| 4H-SiC | $\sim10^{-7}$ cm⁻³ |
| GaN | $\sim10^{-10}$ cm⁻³ |

> SiC/GaN의 $n_i$가 극히 작기 때문에 **열적 누설이 작고 고온 동작에 유리**합니다. 반대로 **완전 공핍**이 쉽고, 도핑 제어가 중요합니다.

### 4.3 도핑과 페르미 준위

- n형: $N_D \gg n_i$ → $n \approx N_D$, $E_F$는 $E_C$ 아래.
- p형: $N_A \gg n_i$ → $p \approx N_A$, $E_F$는 $E_V$ 위.
- 보상 도핑: $n \approx N_D - N_A$ (n형), $p \approx N_A - N_D$ (p형).

페르미 준위(비축퇴, n형):

$$
E_C - E_F = kT \ln\left(\frac{N_C}{N_D}\right)
$$

### 4.4 불완전 이온화 (SiC/GaN의 핵심)

SiC에서:

| Dopant | 역할 | 이온화 에너지(대표) | RT 이온화율 |
|---|---|---:|---:|
| N | donor | 0.05–0.1 eV | 높음(대부분) |
| Al | acceptor | 0.2 eV | 낮음(수십 %) |
| B | acceptor | 0.3 eV | 매우 낮음 |

이온화된 도펀트:

$$
N_D^+ = \frac{N_D}{1+g_D\exp\left(\frac{E_F-E_D}{kT}\right)}
$$

$$
N_A^- = \frac{N_A}{1+g_A\exp\left(\frac{E_A-E_F}{kT}\right)}
$$

> **실무 영향**: Al 도핑 p-well의 실제 hole 농도는 화학적 도핑보다 낮습니다. TCAD에서 incomplete ionization을 켜지 않으면 $V_{th}$와 $R_{on}$이 틀어집니다.

---

## 5. Lecture 2 — 수송과 열 (1.5h)

### 5.1 드리프트·확산

$$
J_n = q\mu_n n\mathcal{E} + qD_n\frac{dn}{dx}
$$

$$
J_p = q\mu_p p\mathcal{E} - qD_p\frac{dp}{dx}
$$

Einstein relation:

$$
D = \mu \frac{kT}{q}
$$

### 5.2 이동도 모델

| 산란 | 영향 | 모델 |
|---|---|---|
| Phonon | 온도↑ → 이동도↓ | $\mu \propto T^{-3/2}$ |
| Ionized impurity | 도핑↑ → 이동도↓ | Caughey–Thomas |
| Surface roughness | 계면 → 이동도↓ | Lombardi |
| High-field | $\mathcal{E}$↑ → 속도 포화 | Canali |

속도 포화:

$$
v_d = \frac{\mu_0\mathcal{E}}{1+\mu_0\mathcal{E}/v_{sat}}
$$

### 5.3 열

열저항:

$$
R_{th} = \frac{L}{k_{th}A}
$$

Junction temperature:

$$
T_j = T_a + P \cdot R_{th}
$$

| 물질 | 열전도도 (W/cm·K) | 의미 |
|---|---:|---|
| SiC | 4.9 | 방열 유리 |
| GaN | 1.3–2.0 | GaN-on-SiC 사용 이유 |
| Si | 1.5 | 기준 |

> GaN은 열전도도가 SiC보다 낮지만, **GaN-on-SiC** 구조로 기판을 SiC로 쓰면 유리합니다.

### 5.4 항복 전계

| 물질 | $E_c$ (MV/cm) |
|---|---:|
| Si | 0.3 |
| 4H-SiC | 2.5–3.0 |
| GaN | 3.0–3.5 |

높은 $E_c$ 덕분에 같은 BV에서 **drift layer를 얇고 진하게** 만들 수 있습니다.

---

## 6. Lab — 1D Poisson/C-V (2h)

### 6.1 목표

- 도핑 profile에서 band diagram·공핍폭·C-V를 계산.
- Si/SiC/GaN의 차이를 비교.
- 불완전 이온화 on/off 영향 확인.

### 6.2 지배방정식

$$
\frac{d^2\phi}{dx^2} = -\frac{q}{\varepsilon}(p-n+N_D^+-N_A^-)
$$

유한차분:

$$
\frac{\phi_{i+1}-2\phi_i+\phi_{i-1}}{\Delta x^2} = -\frac{\rho_i}{\varepsilon}
$$

### 6.3 절차

1. 1D grid(예: 1 nm–10 nm) 설정.
2. 도핑 profile 정의(abrupt 또는 Gaussian).
3. 평형 $n,p$를 Fermi-Dirac로 계산.
4. Poisson을 Newton 반복으로 수렴.
5. C-V: $C = dQ/dV$.
6. SiC는 incomplete ionization 포함.

### 6.4 제출물

- band diagram 1개.
- C-V curve 1개.
- 표: Si/SiC/GaN의 공핍폭·$V_{bi}$·$C_{ox}$.
- 해석: "SiC에서 공핍폭이 작은 이유".

### 6.5 체크포인트

- [ ] $V_{bi}$ 부호와 크기.
- [ ] 도핑 농도와 $W$의 반비례.
- [ ] SiC 불완전 이온화 효과.
- [ ] 단위(cm⁻³, F/cm²).

---

## 7. Case — 채널 이동도와 2DEG (1h)

### 7.1 SiC MOSFET 채널 이동도

- bulk 900–1000 cm²/Vs vs channel 20–40 cm²/Vs.
- 원인: interface traps, Coulomb scattering, roughness, imperfect oxide.
- 결과: $R_{on,sp}$가 이론보다 크다.
- 대응: NO nitridation, buried channel, trench, counter-doping.

### 7.2 GaN 2DEG

- AlGaN/GaN polarization에 의한 2DEG (~$10^{13}$ cm⁻²).
- 이동도 1500–2000 cm²/Vs.
- 장점: 낮은 $R_{on}$.
- 단점: normally-on, dynamic Ron, buffer traps.

### 7.3 토론

1. SiC 채널 이동도를 2배 올리면 $R_{on}$은 얼마나 줄어드는가?
2. GaN 2DEG 농도를 높이면 어떤 trade-off가 있는가?
3. 불완전 이온화를 무시하면 TCAD $V_{th}$는 어느 방향으로 틀어지는가?

---

## 8. Quiz (5문항)

1. $np=n_i^2$의 의미는?
2. SiC에서 Al acceptor의 이온화율이 낮은 이유는?
3. Einstein relation을 쓰시오.
4. 속도 포화가 발생하면 $I_D$는 전계에 어떻게 의존하는가?
5. GaN-on-SiC를 쓰는 이유는?

**정답**: 1) 열평형 질량작용. 2) 이온화 에너지가 크고 $E_F$가 멀다. 3) $D=\mu kT/q$. 4) 선형→포화. 5) GaN 열전도도 한계를 SiC 기판으로 보완.

---

## 9. Homework

- SiC p-type $N_A=10^{17}$ cm⁻³에서 Al 이온화율을 300K/500K로 계산.
- 1D Poisson 코드를 SiC로 확장.
- Day 3을 위해 **PN 접합·항복** 복습.

---

## 10. 강사 노트

- 불완전 이온화는 **표 하나로 충분**하지만, TCAD 결과와 연결해 설명.
- 수강생이 $\mu$와 $D$ 단위에서 자주 틀림 → **cm²/Vs, cm²/s** 확인.
- SiC 채널 이동도 저하는 **"소자와 재료의 차이"** 로 강조.
- 다음 날 항복은 **edge effect**와 연결.

---

*Day 02 / 20 — 140h program. 물성값은 교육용 대표값입니다.*
