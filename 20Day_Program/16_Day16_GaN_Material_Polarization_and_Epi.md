# Day 16 — GaN 물성·분극·Epitaxy

> **Week 4 / Day 16** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: Day 15
> **참조**: [`../04_Week4_GaN_HEMT_Process_Reliability_and_Characterization.md`](../04_Week4_GaN_HEMT_Process_Reliability_and_Characterization.md)

---

## 1. 오늘의 질문

> **"AlGaN/GaN 계면에 도핑 없이 2DEG가 생기는 이유는 무엇이며, 그 농도를 어떻게 설계하는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | Wurtzite, spontaneous/piezoelectric polarization |
| 10:45–12:15 | Lecture 2 | 2DEG, AlGaN barrier, MOCVD epi, buffer |
| 13:15–15:15 | Lab | 2DEG 농도 vs Al 조성/두께 |
| 15:30–16:30 | Case | GaN-on-Si vs GaN-on-SiC |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제 |

---

## 3. 학습목표

1. Wurtzite GaN의 결정 구조와 극성을 설명한다.
2. Spontaneous/piezoelectric polarization을 구분한다.
3. AlGaN/GaN 2DEG의 형성 원리를 설명한다.
4. Al 조성·barrier 두께로 $n_s$를 설계한다.
5. MOCVD epi stack(buffer/channel/barrier)을 설명한다.
6. GaN-on-Si/SiC/sapphire의 trade-off를 비교한다.

---

## 4. Lecture 1 — 분극과 2DEG (1.5h)

### 4.1 Wurtzite 구조

- GaN: hexagonal wurtzite, $c$-axis 극성.
- Ga-face (0001) vs N-face (000-1).
- Ga-face가 일반적; polarization field 방향이 다름.

### 4.2 Polarization

| 성분 | 원인 | 크기 |
|---|---|---|
| Spontaneous ($P_{sp}$) | 결정 비대칭 | GaN ~ -0.034 C/m² |
| Piezoelectric ($P_{pz}$) | strain | AlGaN barrier |
| Total ($P$) | $P_{sp}+P_{pz}$ | interface discontinuity |

AlGaN/GaN 계면 sheet charge:

$$
\sigma = P_{total}(AlGaN) - P_{total}(GaN)
$$

> $\sigma > 0$이면 계면에 전자 2DEG가 형성.

### 4.3 2DEG 농도

근사:

$$
n_s \approx \frac{\sigma}{q} - \frac{\varepsilon}{q d}\left(\phi_b + E_F - \Delta E_C\right)
$$

- $d$: barrier thickness.
- $\phi_b$: gate barrier.
- $\Delta E_C$: conduction band offset.

대표: Al$_{0.25}$Ga$_{0.75}$N 20–25 nm → $n_s \sim 1\times10^{13}$ cm⁻², $\mu \sim 1500$–2000 cm²/Vs.

### 4.4 Polarization field

- Barrier에 강한 field → 2DEG confinement.
- Field는 barrier thickness와 Al 조성에 의존.
- 너무 두꺼우면 strain relaxation/defects.

---

## 5. Lecture 2 — MOCVD Epitaxy (1.5h)

### 5.1 Epi stack

| Layer | 역할 | 두께(대표) |
|---|---|---|
| Substrate | mechanical | Si/SiC/sapphire |
| Nucleation | wetting/defect control | AlN 20–100 nm |
| Buffer | strain/defect filter | AlGaN/AlN superlattice 1–3 µm |
| Channel | 2DEG | GaN 1–2 µm |
| Barrier | 2DEG | AlGaN 15–30 nm |
| Cap | surface protection | GaN/SiN 1–5 nm |

### 5.2 MOCVD

- Precursors: TMGa, TMAl, NH₃.
- Temperature: 1000–1100°C.
- Pressure: 50–200 mbar.
- In-situ: reflectance, pyrometer.

### 5.3 Buffer engineering

| 구조 | 목적 |
|---|---|
| AlN nucleation | Si와 GaN 반응 억제 |
| AlGaN/GaN superlattice | strain management, dislocation bending |
| C-doped buffer | vertical leakage suppression |
| Fe-doped buffer | high resistivity |

### 5.4 Substrate 선택

| Substrate | 장점 | 단점 |
|---|---|---|
| Si | 저가, 대면적 | lattice/thermal mismatch, buffer 필요 |
| SiC | 열전도, lattice match | 고가 |
| Sapphire |绝缘, 저가 | 열전도 낮음 |
| GaN | homoepi | 고가, 소면적 |

### 5.5 Defects

- Threading dislocations ($10^8$–$10^{10}$ cm⁻²).
- V-pits, cracks.
- Point defects, impurities (C, O, Si).
- 영향: leakage, dynamic Ron, reliability.

---

## 6. Lab — 2DEG 설계 (2h)

### 6.1 목표

- Al 조성/barrier 두께에 따른 $n_s$ 계산.
- Sheet resistance $R_{sh} = 1/(q n_s \mu)$.
- GaN-on-Si vs SiC 비교.

### 6.2 파라미터

| 변수 | 범위 |
|---|---|
| Al 조성 $x$ | 0.15–0.35 |
| Barrier $d$ | 10–30 nm |
| $\mu$ | 1500–2000 cm²/Vs |

### 6.3 절차

1. $P_{sp}(x)$, $P_{pz}(x)$ 계산.
2. $\sigma(x)$ 계산.
3. $n_s(x,d)$ 계산.
4. $R_{sh}$ 계산.
5. Contour plot.

### 6.4 제출물

- $n_s$ vs $x$ 그래프.
- $n_s$ vs $d$ 그래프.
- $R_{sh}$ 표.
- "최적 barrier" 제안.

### 6.5 체크포인트

- [ ] 단위(C/m², cm⁻²).
- [ ] $x$에 따른 bandgap.
- [ ] Strain relaxation 한계.
- [ ] $\mu$ 가정.

---

## 7. Case — GaN-on-Si vs GaN-on-SiC (1h)

### 7.1 GaN-on-Si

- 장점: 저가, 200/300 mm.
- 단점: buffer, thermal, breakdown.
- 응용: consumer PSU, OBC.

### 7.2 GaN-on-SiC

- 장점: 열전도, RF/high power.
- 단점: 고가, 소면적.
- 응용: RF, aerospace, high-power.

### 토론

1. Al 조성을 높이면 $n_s$는 증가하지만 왜 무한정 올릴 수 없는가?
2. C-doped buffer가 dynamic Ron에 미치는 영향은?
3. GaN-on-Si에서 buffer 두께가 왜 중요한가?

---

## 8. Quiz (5문항)

1. Spontaneous vs piezoelectric polarization?
2. 2DEG sheet charge 공식?
3. AlGaN barrier 두께↑ → $n_s$?
4. GaN-on-SiC 장점?
5. Threading dislocation 영향?

**정답**: 1) 결정 비대칭 vs strain. 2) $\sigma=P_{AlGaN}-P_{GaN}$. 3) 증가(포화). 4) 열전도. 5) leakage/reliability.

---

## 9. Homework

- Al$_{0.25}$Ga$_{0.75}$N 20 nm의 $n_s$ 계산.
- GaN-on-Si buffer 설계를 3층으로 정리.
- Day 17을 위해 **HEMT 공정** 예습.

---

## 10. 강사 노트

- "도핑 없이 전자가 생긴다"를 polarization으로 설명.
- $n_s$ 계산은 **단위**에서 많이 틀림.
- Buffer는 **vertical leakage/dynamic Ron**과 연결.
- 다음 날 HEMT 공정으로.

---

*Day 16 / 20 — 140h program. 2DEG 값은 교육용 대표값입니다.*
