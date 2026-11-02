# 22 — Assessment Bank (Quizzes·Exams·Rubrics)

> **대상**: Day 1–20 | **평가 비중**: 퀴즈 20%, 실습 25%, 주차 시험 20%, 캡스톤 25%, 참여 10%
> **참조**: 각 Day 파일의 Quiz, [`24_Capstone_Project_Brief.md`](./24_Capstone_Project_Brief.md)

---

## 1. 평가 운영 원칙

1. **매일 5문항** 퀴즈(17:00–17:15), 60% 이상 통과.
2. 오답은 **다음 날 09:00 복습 퀴즈**에 재출제.
3. 주차 시험은 **계산 5 + 개념 5**.
4. 실습은 **재현성·물리 타당성·비교** 중심.
5. 캡스톤은 **spec–근거–risk–재현** rubric.

---

## 2. 일일 퀴즈 매핑

| Day | 주제 | Day 파일 Quiz |
|---:|---|---|
| 01 | 손실 계산 | [`01_Day01...`](./01_Day01_Orientation_Power_Electronics_and_Course_Roadmap.md) |
| 02 | 물리/수송 | [`02_Day02...`](./02_Day02_Semiconductor_Physics_Equilibrium_and_Transport.md) |
| 03 | PN/항복 | [`03_Day03...`](./03_Day03_PN_Junction_Breakdown_and_1D_Design.md) |
| 04 | FOM/열 | [`04_Day04...`](./04_Day04_WBG_Materials_FOM_and_Thermal.md) |
| 05 | 소자/공정 | [`05_Day05...`](./05_Day05_Device_Taxonomy_and_Process_Overview.md) |
| 06 | 세정 | [`06_Day06...`](./06_Day06_SiC_Cleaning_and_Surface_Preparation.md) |
| 07 | 산화 | [`07_Day07...`](./07_Day07_SiC_Thermal_Oxidation_Kinetics.md) |
| 08 | 계면 | [`08_Day08...`](./08_Day08_Gate_Oxide_Interface_and_Nitridation.md) |
| 09 | Implant | [`09_Day09...`](./09_Day09_SiC_Ion_Implantation_and_Masking.md) |
| 10 | Activation/Etch | [`10_Day10...`](./10_Day10_High_Temperature_Activation_and_Etching.md) |
| 11 | TLM | [`11_Day11...`](./11_Day11_Contact_Physics_and_TLM.md) |
| 12 | JBS | [`12_Day12...`](./12_Day12_Ohmic_Schottky_and_JBS_Process.md) |
| 13 | MOSFET | [`13_Day13...`](./13_Day13_SiC_MOSFET_Integration.md) |
| 14 | Termination | [`14_Day14...`](./14_Day14_Edge_Termination_and_BV_Design.md) |
| 15 | Reliability | [`15_Day15...`](./15_Day15_SiC_Reliability_and_Qualification.md) |
| 16 | GaN/Epi | [`16_Day16...`](./16_Day16_GaN_Material_Polarization_and_Epi.md) |
| 17 | HEMT | [`17_Day17...`](./17_Day17_GaN_HEMT_Process_Integration.md) |
| 18 | Normally-off | [`18_Day18...`](./18_Day18_GaN_Normally_Off_Dynamic_Ron_and_Reliability.md) |
| 19 | TCAD/DOE | [`19_Day19...`](./19_Day19_TCAD_Calibration_DOE_and_Industry_Practice.md) |
| 20 | Capstone | [`20_Day20...`](./20_Day20_Capstone_Review_and_Final_Assessment.md) |

---

## 3. 추가 문제 Bank (40문항)

> 각 Day의 Quiz와 **다른 변형**. 계산/개념 혼합.

### Day 01–05

1. 800 V/100 A, $D=0.4$, $R_{on}=40$ mΩ → 도통 손실? **16 W**
2. $V=600$ V, $I=50$ A, $t_{sw}=50$ ns, $f=50$ kHz → 스위칭 손실? **37.5 W**
3. SiC MOSFET의 $R_{on}$이 온도와 함께 증가하는 이유? **phonon/interface scattering**
4. $V_{bi}$가 SiC에서 큰 이유? **작은 $n_i$**
5. Avalanche BV의 온도 계수가 positive인 이유? **phonon scattering↑ → mean free path↓**
6. 4H-SiC에서 Al acceptor 이온화율이 낮은 이유? **깊은 에너지 준위**
7. Baliga FOM이 큰 소재의 장점? **같은 BV에서 낮은 $R_{on,sp}$**
8. GaN-on-SiC를 쓰는 이유? **열전도**
9. Trench MOSFET의 corner field 문제? **oxide breakdown**
10. SiC 공정에서 activation을 먼저 하는 이유? **고온이 gate를 손상**

### Day 06–10

11. Piranha의 역할? **organic/carbon 제거**
12. SC2의 역할? **metal 제거**
13. HF 후 즉시 공정해야 하는 이유? **native oxide 재성장**
14. Sacrificial oxidation의 목적? **damage 제거**
15. Deal-Grove 포물선 영역의 rate-limiting step? **diffusion**
16. SiC C-face 산화가 빠른 이유? **결합/표면 에너지**
17. Dit가 높으면 $V_{th}$에 미치는 영향? **stretch-out/shift**
18. NO anneal의 trade-off? **Dit↓ vs mobility/reliability**
19. Implant channeling 방지법? **tilt/twist, screen oxide**
20. Box profile을 만드는 방법? **multi-energy implant**

### Day 11–15

21. TLM 기울기의 의미? **$R_s/W$**
22. $L_T$의 의미? **current crowding length**
23. Ni/n-SiC ohmic의 mechanism? **silicide/carbon vacancy**
24. p-SiC ohmic이 어려운 이유? **Fermi pinning/barrier**
25. JBS p+ grid의 역할? **field shielding**
26. Schottky barrier lowering의 원인? **image force**
27. DMOS의 JFET 저항을 줄이는 방법? **cell pitch/JFET width**
28. Trench corner rounding 방법? **sacrificial oxidation**
29. JTE dose too high의 결과? **not depleted/field crowding**
30. BPD degradation의 원인? **stacking fault expansion**

### Day 16–20

31. Spontaneous polarization의 원인? **wurtzite 비대칭**
32. AlGaN barrier 두께↑ → $n_s$? **증가 후 포화**
33. GaN HEMT ohmic stack? **Ti/Al/Ni/Au**
34. p-GaN gate의 원리? **depletion**
35. Cascode의 단점? **parasitic inductance**
36. Dynamic Ron의 원인? **trap trapping**
37. Field plate의 trade-off? **BV↑/$C_{gd}$↑**
38. TCAD calibration의 첫 단계? **목표/데이터 정의**
39. $C_{pk}$의 의미? **공정 능력**
40. DTCO의 목적? **설계-기술 공동최적화**

---

## 4. 주차 시험

### Week 1 (Day 5) — 10문항

1. 400 V/30 A, $D=0.5$, $R_{on}=30$ mΩ → **13.5 W**
2. 400 V/30 A, $t_{sw}=40$ ns, $f=20$ kHz → **4.8 W**
3. $V_{bi}$ 공식 → **$(kT/q)\ln(N_AN_D/n_i^2)$**
4. 일방 abrupt $W$ → **$\sqrt{2\varepsilon(V_{bi}-V)/(qN_D)}$**
5. Avalanche TC → **positive**
6. Baliga FOM → **$\varepsilon\mu E_c^3$**
7. 4H-SiC 이방성 → **결정축별 물성 차이**
8. DMOS vs Trench → **JFET/vertical channel**
9. Activation 선행 이유 → **고온 gate 손상**
10. GaN normally-on 이유 → **polarization 2DEG**

### Week 2 (Day 10) — 10문항

1. Piranha/SC1/SC2/HF → **organic/particle/metal/oxide**
2. Deal-Grove → **$x^2+Ax=B(t+\tau)$**
3. SiC 산화 느린 이유 → **C 제거 반응**
4. Si-face vs C-face → **C-face 빠름**
5. Dit 추출 2법 → **high-low/Terman**
6. NO anneal 목적 → **Dit↓**
7. Implant peak → **$\Phi/(\sqrt{2\pi}\Delta R_p)$**
8. Channeling 방지 → **tilt/twist**
9. Activation/cap → **1600–1700°C/Si sublimation 억제**
10. Corner rounding → **sacrificial oxidation**

### Week 3 (Day 15) — 10문항

1. TLM $\rho_c$ → **$R_sL_T^2$**
2. Barrier lowering → **image force**
3. JBS p+ grid → **field shielding**
4. DMOS vs trench 저항 → **JFET vs vertical**
5. Trench corner → **oxide breakdown**
6. JTE optimal dose → **$\varepsilon_sE_c/q$**
7. JTE too high → **not depleted**
8. BPD → **SF expansion**
9. Weibull $\beta$ → **failure mode**
10. AEC-Q101 → **discrete qualification**

### Week 4 (Day 20) — Final 20문항

Day 20 파일의 Final Exam 20문항 사용. 계산 8 + 개념 12.

---

## 5. 실습 Rubric (25%)

| 항목 | 배점 | 0–5 기준 |
|---|---:|---|
| 목적/가정 | 15 | 명확/누락 |
| 구조/모델 | 20 | 타당/오류 |
| 결과/재현 | 25 | 재현/불가 |
| 비교/검증 | 20 | 측정/문헌 비교 |
| 해석 | 15 | 물리적 결론 |
| 기록 | 5 | 파일/단위 |

---

## 6. 캡스톤 Rubric (25%)

| 항목 | 배점 |
|---|---:|
| Spec/문제 정의 | 10 |
| 구조/소재 | 15 |
| 공정 flow | 20 |
| TCAD/검증 | 25 |
| 신뢰성/risk | 15 |
| 발표/Q&A | 15 |

**합격**: 70/100 이상, 각 항목 50% 이상.

---

## 7. 시험 운영

| 항목 | 규칙 |
|---|---|
| 오픈북 | 계산/개념은 허용, 통신 금지 |
| 시간 | 주차 30분, 최종 120분 |
| 재시험 | 1회, 70% 미만 |
| 부정행위 | 0점 |
| 피드백 | 48h 내 |

---

## 8. 문제 출제 원칙

1. **계산은 단위까지** 채점.
2. 개념은 **"왜"** 를 묻는다.
3. TCAD 결과는 **가정/한계** 포함.
4. 논문/표준 인용은 **DOI/표준번호** 확인.
5. 확인되지 않은 수치는 출제 금지.

---

*22 / 24 — Assessment Bank. 20일 프로그램 부록.*
