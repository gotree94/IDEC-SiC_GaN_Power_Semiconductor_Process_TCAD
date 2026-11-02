# Week 1 — Power Semiconductor Overview & WBG Process Fundamentals

> **소요**: 강의 4h + 실습 2h
> **원 강좌 대응**: D1-AM (전력반도체소자 공정 개요 / Wide-bandgap 공정 특징 / SiC·GaN 구조 및 공정 차이)
> **핵심 질문**: "왜 SiC/GaN인가? 어떤 소자를 어떤 공정 순서로 만드는가? 그 선택을 숫자로 어떻게 검증하는가?"

---

## 1. 학습목표

이 주차를 마치면 다음을 **말과 계산으로** 보여줄 수 있어야 합니다.

1. Si / 4H-SiC / GaN / Ga₂O₃ / Diamond의 물성 차이를 **5개 축**(E_g, E_c, μ, λ, ε)으로 비교한다.
2. 이상적 단극 소자의 `R_on,sp`를 **유도**하고, 650 V / 1.2 kV / 3.3 kV에서 소재별 값을 계산한다.
3. **Baliga FOM**과 **실제 소자 성능의 격차**를 설명한다.
4. 주요 전력소자(SBD, JBS, MPS, PiN, DMOS, Trench, SJ, HEMT, Cascode, IGBT)의 **구조–공정–특성**을 표로 정리한다.
5. WBG 공정의 **5가지 근본 차이**와 그 귀결을 설명한다.
6. SiC MOSFET / GaN HEMT의 **공정 flow와 thermal budget 충돌**을 설명한다.
7. TCAD 1D sanity check(공핍폭, Ron, 산화막 전계, 열저항)를 손으로 수행한다.

---

## 2. 전력반도체의 응용 지형

| 응용 | 대표 전압/전류 | 요구 특성 | 주력 소자 |
|---|---|---|---|
| 모바일·PC 전원 | 20–100 V | 고효율, 소형 | Si MOSFET, GaN HEMT |
| 데이터센터 PSU | 400 V / 48 V | 고효율, 고전력밀도 | Si SJ-MOSFET, GaN HEMT, SiC |
| EV 인버터 | 400–800 V | 고신뢰, 고온 | **SiC MOSFET** |
| EV OBC/DC-DC | 400–800 V | 양방향, 고효율 | SiC MOSFET, GaN HEMT |
| 태양광 인버터 | 600–1500 V | 효율, 수명 | SiC MOSFET/SBD |
| 철도 견인 | 1.7–3.3 kV | 고신뢰 | Si IGBT, SiC MOSFET |
| 전력망(HVDC/FACTS) | 3.3–10 kV+ | 초고압 | Si IGBT, SiC, (Ga₂O₃ 연구) |
| 우주·항공 | 100 V–1.2 kV | 방사선·극한온도 | SiC MOSFET (교수님 연구분야) |

**시장 관점**: SiC는 EV·PV·철도에서, GaN는 데이터센터·모바일·OBC에서 주로 성장. Si IGBT는 고전압·대전류에서 여전히 주류.

---

## 3. 물성 심화

### 3.1 기본 물성표 (300 K, 대표값)

| 물성 | Si | 4H-SiC | GaN | Ga₂O₃ (β) | Diamond |
|---|---|---|---|---|---|
| 밴드갭 E_g (eV) | 1.12 | 3.26 | 3.4 | 4.5–4.9 | 5.5 |
| 임계전계 E_c (MV/cm) | 0.3 | 2.2–3.0 | 3.3 | 8 | 10 |
| 전자 이동도 μ_n (cm²/V·s) | 1400 | 900–1000 | 1000–2000 (2DEG) | 150–200 | 2000 |
| 정공 이동도 μ_p (cm²/V·s) | 450 | 100–120 | ~30 | 10–30 | 1800 |
| 비유전율 ε_r | 11.7 | 9.66 | 9.5 | 10 | 5.7 |
| 열전도도 λ (W/cm·K) | 1.5 | 3.7–4.9 | 1.3–1.7 | 0.1–0.3 | 20 |
| 포화속도 v_sat (10⁷ cm/s) | 1.0 | 2.0 | 2.5 | 1.0 | 2.7 |
| 결정 구조 | Diamond | 4H hexagonal | Wurtzite | Monoclinic | Diamond |

> **주의**: 위 값은 **대표값**입니다. SiC는 결정면(0001 vs 000-1), 도핑 농도, 온도에 따라 이동도·항복전계가 크게 달라집니다. GaN는 **bulk 이동도**와 **2DEG 이동도**를 구분해야 합니다.

### 3.2 온도 의존성

- **E_g(T)**: Varshni 식
  ```
  E_g(T) = E_g(0) − αT²/(T+β)
  ```
- **μ(T)**: 격자 산란 지배 시 `μ ∝ T^(-3/2)` (고온), 불순물 산란 지배 시 증가.
- **E_c(T)**: 온도 상승 시 감소 → 고온 항복전압 감소.
- **ni(T)**: `n_i ∝ T^(3/2) exp(-E_g/2kT)` → WBG는 고온에서도 ni가 매우 작아 **누설·열폭주에 유리**.

**실무 포인트**: SiC 소자는 175°C 이상에서도 동작하지만, **게이트 산화막 신뢰성(BTI, TDDB)** 과 **패키지**가 병목입니다.

### 3.3 4H-SiC의 anisotropy

4H-SiC는 육방정계이므로 **c축 방향과 수직 방향**의 물성이 다릅니다.

| 항목 | c축 (∥) | c축 수직 (⊥) | 영향 |
|---|---|---|---|
| 전자 이동도 | 상대적으로 낮음 | 상대적으로 높음 | 채널 방향 설계 |
| 항복전계 | 상대적으로 높음 | 상대적으로 낮음 | termination 설계 |
| 열전도도 | 상대적으로 높음 | 상대적으로 낮음 | 열 확산 |
| 식각/산화 속도 | 면 의존 | 면 의존 | trench sidewall |

> **TCAD 팁**: 2D 단면 시뮬레이션에서 **c축 방향을 명시**하고, 가능하면 anisotropic mobility/impact ionization을 켭니다. isotropic으로만 계산하면 BV를 과대평가할 수 있습니다.

### 3.4 불완전 이온화 (Incomplete Ionization)

SiC의 도펀트는 이온화에너지가 커서 실온에서 일부만 이온화됩니다.

```
N_D⁺ = N_D / (1 + g_D · exp((E_F − E_D)/kT))
N_A⁻ = N_A / (1 + g_A · exp((E_A − E_F)/kT))
```

| 도펀트 | 형 | E_a (meV, 대표) | 실온 이온화율(대략) |
|---|---|---|---|
| Al | p | 190–240 | 낮음 (수십 %) |
| B | p | ~300 | 매우 낮음 |
| N | n | 50–100 | 중간~높음 |
| P | n | 60–100 | 중간~높음 |

**왜 중요한가**
- 실온 캐리어 농도 < 도펀트 농도 → **Vth, Ron, C-V 해석**에 직접 영향.
- TCAD에서 이 모델을 끄면 **Vth/Ron을 과소평가**.
- C-V로 도핑을 추출할 때 **이온화율 보정**이 필요.

---

## 4. FOM(Figure of Merit)와 이상적 한계

### 4.1 이상적 단극 소자의 비온저항 유도

가정: 비관통형(non-punch-through) 드리프트층, 일정 도핑, 이상적 항복.

1. 항복 시 최대 전계가 `E_c`이고, 공핍폭 `W`는
   ```
   W = 2 V_B / E_c
   ```
2. Poisson 방정식으로부터
   ```
   V_B = q N_D W² / (2 ε)
   ```
3. 위 두 식에서 도핑 농도
   ```
   N_D = ε E_c² / (2 q V_B)
   ```
4. 드리프트층 저항
   ```
   R_on,sp = W / (q μ N_D)
   ```
5. 대입하면
   ```
   R_on,sp = 4 V_B² / (ε μ E_c³)
   ```

> 이것이 **"같은 항복전압에서 소재의 이론적 최소 Ron"** 입니다. `E_c³`에 반비례하는 것이 핵심입니다.

### 4.2 Baliga FOM

```
BFOM = ε · μ · E_c³       (Si = 1 기준 상대값)
```

| 소재 | BFOM (Si=1) | 이상적 Ron,sp 비 (Si=1) | 비고 |
|---|---|---|---|
| Si | 1 | 1 | 기준 |
| 4H-SiC | ~300–400 | ~1/300–1/400 | 상용 성숙 |
| GaN | ~800–1200 | ~1/800–1/1000 | HEMT/2DEG 기준 |
| Ga₂O₃ | ~1500–3000 (이론) | ~1/1500–1/3000 | 열전도도가 병목 |
| Diamond | >10,000 (이론) | — | 가공·도핑 난이도 |

### 4.3 계산 예제 (1.2 kV)

**4H-SiC**
```
E_c = 2.5×10⁶ V/cm
ε   = 9.66 × 8.854×10⁻¹⁴ = 8.55×10⁻¹³ F/cm
μ_n = 950 cm²/V·s
V_B = 1200 V

R_on,sp = 4(1200)² / (8.55×10⁻¹³ × 950 × (2.5×10⁶)³)
        = 5.76×10⁶ / (1.27×10¹⁰)
        = 4.5×10⁻⁴ Ω·cm²
        ≈ 0.45 mΩ·cm²
```

**Si**
```
E_c = 3×10⁵ V/cm, ε = 1.04×10⁻¹² F/cm, μ_n = 1400
R_on,sp = 5.76×10⁶ / (3.92×10⁷) = 0.147 Ω·cm²
        ≈ 147 mΩ·cm²
```

**GaN (수직 가정, μ=1000)**
```
E_c = 3.3×10⁶ V/cm, ε = 8.41×10⁻¹³ F/cm
R_on,sp = 5.76×10⁶ / (3.02×10¹⁰) ≈ 0.19 mΩ·cm²
```

| 소재 | 이상적 Ron,sp @1.2 kV | Si 대비 |
|---|---|---|
| Si | 147 mΩ·cm² | 1 |
| 4H-SiC | 0.45 mΩ·cm² | 1/327 |
| GaN (수직 가정) | 0.19 mΩ·cm² | 1/774 |

**650 V에서도 같은 식**으로 계산하면 SiC ≈ 0.13 mΩ·cm², Si ≈ 43 mΩ·cm².

### 4.4 실제 소자가 이론값에 못 미치는 이유

| 손실 요인 | 설명 | 대표 영향 |
|---|---|---|
| **채널 이동도** | SiC/SiO₂ 계면 산란, Dit | μ_ch 20–40 cm²/V·s (bulk의 1/25) |
| **JFET 저항** | DMOS의 p-body 사이 좁은 경로 | Ron +10–30% |
| **접촉저항** | ρc 10⁻⁵–10⁻⁶ Ω·cm² | Ron +5–15% |
| **기판 저항** | n⁺ substrate | 650 V에서 무시 못함 |
| **edge termination 효율** | BV < 이상값 | 80–95% |
| **self-heating** | 열저항 | 고전류에서 Ron 증가 |
| **패키지** | wire/bond | 시스템 Ron |

> 즉, **"소재 FOM은 상한"** 이고, 공정·설계의 목표는 **이론값과 실제값의 격차를 줄이는 것**입니다. 2–4주차 내용이 모두 이 격차와 관련됩니다.

### 4.5 열 FOM

```
R_th,sp = t / λ        [K·cm²/W]
```
- 10 µm SiC (λ=4): `R_th,sp = 10×10⁻⁴/4 = 2.5×10⁻⁴ K·cm²/W`
- 10 µm Si (λ=1.5): `6.7×10⁻⁴ K·cm²/W`
- SiC가 약 **2.7배** 유리.

> GaN는 λ가 낮아 **SiC 기판 위 GaN** 또는 **열 via/field plate** 설계가 중요합니다.

---

## 5. 전력소자 Taxonomy

### 5.1 단극 vs 양극

| 구분 | 단극(unipolar) | 양극(bipolar) |
|---|---|---|
| 전도 | 다수 캐리어 | 다수+소수 캐리어 |
| 대표 | SBD, MOSFET, JFET, HEMT | PiN, IGBT, thyristor |
| 장점 | 빠른 스위칭, 낮은 손실(저전압) | 전도도 변조, 높은 항복(고전압) |
| 단점 | 고전압에서 Ron↑ | 느린 스위칭, reverse recovery |
| SiC/GaN | 주력 | SiC PiN/IGBT(초고압) |

### 5.2 다이오드

| 소자 | 구조 | Vf | 누설 | 항복 | 스위칭 | 용도 |
|---|---|---|---|---|---|---|
| **SBD** | metal/n-drift | 낮음 | 높음 | 중간 | 매우 빠름 | PFC, OBC |
| **JBS** | p⁺ grid + Schottky | 중간 | 낮음 | 높음 | 빠름 | EV, PV |
| **MPS** | PiN + Schottky 혼합 | 중간 | 낮음 | 높음 | 빠름 | 서지 내성 |
| **PiN** | p⁺/n⁻/n⁺ | 높음 | 매우 낮음 | 매우 높음 | 느림 | HVDC, 철도 |

### 5.3 스위치

| 소자 | 채널 | 대표 전압 | 장점 | 단점 |
|---|---|---|---|---|
| **Planar DMOS** | 수평 | 600–3.3 kV | 공정 성숙, 안정 | JFET 저항, μ_ch 낮음 |
| **Trench MOSFET** | 수직 | 600–1.2 kV | Ron↓, 밀도↑ | corner 전계, 산화막 신뢰성 |
| **Double Trench** | 수직+source trench | 1.2 kV | 산화막 전계 완화 | 공정 복잡 |
| **SJ MOSFET** | 수직 | 600–900 V (Si) | Ron↓ (charge balance) | SiC deep column 어려움 |
| **HEMT** | 2DEG(수평) | 100–650 V | 고주파, 고이동도 | normally-on, dynamic Ron |
| **Cascode** | HEMT+Si MOSFET | 600–900 V | normally-off 용이 | 시스템 복잡 |
| **IGBT** | 양극 | 1.2–6.5 kV | 고전압 | 느림, tail current |

### 5.4 구조–공정 매핑

| 소자 | 핵심 공정 | 핵심 계면/결함 | TCAD 핵심 모델 |
|---|---|---|---|
| SiC SBD | Schottky metal, JTE | metal/SiC 배리어 | thermionic emission, barrier lowering |
| SiC JBS | p⁺ implant + Schottky | p⁺/n⁻ junction | impact ionization, potential |
| SiC DMOS | self-aligned p-body/n⁺, gate ox | SiO₂/SiC Dit | interface traps, mobility |
| SiC Trench | trench etch, corner rounding | trench corner field | impact ionization, oxide field |
| SiC SJ | deep p column (multi-epi/implant) | charge balance | doping profile, BV |
| GaN HEMT | epi, ohmic, passivation | AlGaN/GaN interface, surface | polarization, traps |
| GaN p-GaN gate | p-GaN epi/etch, gate metal | p-GaN/metal | heterojunction, traps |

---

## 6. WBG 공정의 5가지 근본 차이

| # | 차이 | 물리적 이유 | 공정적 귀결 | 관련 주차 |
|---|---|---|---|---|
| 1 | **도펀트 확산이 거의 없음** | 높은 결합에너지, 낮은 확산계수 | 열확산 junction 불가 → **implant + 1600–1800°C activation** | W2 |
| 2 | **화학적 불활성** | 강한 Si–C / Ga–N 결합 | 습식식각 제한 → **ICP 건식식각**, 세정 난이도↑ | W2 |
| 3 | **산화막 계면 품질** | SiC만 열산화 가능, C 잔류 | **Dit/NIT↑ → μ_ch↓, Vth 불안정** → nitridation | W2 |
| 4 | **불완전 이온화** | E_a ≫ kT | 실온 캐리어 < 도펀트 → TCAD/설계 필수 고려 | W1–W3 |
| 5 | **이종접합·분극(GaN)** | Wurtzite spontaneous/piezoelectric | **2DEG 형성**, 도핑 없는 고이동도 채널 | W4 |

### 6.1 Si vs SiC vs GaN 공정 비교(요약)

| 공정 | Si | 4H-SiC | GaN |
|---|---|---|---|
| 웨이퍼 | Czochralski | PVT/SF, 4° off-axis | Si/SiC/sapphire 위 heteroepi |
| 절연막 | 열산화(쉬움) | 열산화(어려움, Dit↑) | 열산화 불가 → ALD/PECVD |
| 도핑 | 확산+implant | **implant only** | epi doping (Si, C, Mg) |
| 활성화 | 1000–1100°C | **1600–1800°C** | epi 성장 중/700–800°C(p-GaN) |
| 식각 | RIE/ICP | **ICP (SF₆/O₂/Ar)** | ICP (Cl₂/BCl₃) |
| 접촉 | NiSi, TiSi | Ni silicide, Ti/Al | Ti/Al/Ni/Au |
| passivation | SiO₂/SiN | SiO₂/SiN | SiNx (PECVD) |
| 최대 공정 온도 | ~1100°C | **~1800°C** | ~1100°C (epi) |

---

## 7. 공정 Flow와 Thermal Budget

### 7.1 SiC Planar DMOS (개념)

```
[1] n⁺ substrate + n⁻ epi (10–12 µm, 5×10¹⁵ cm⁻³)
[2] JTE implant (Al)
[3] P-body implant (Al)
[4] N⁺ source implant (N/P)
[5] P⁺ contact implant (Al)
[6] Activation anneal (1600–1700°C, 30 s–5 min, cap)
[7] Gate oxidation (1200–1400°C) + NO/N₂O nitridation
[8] Poly-Si gate deposition/patterning
[9] ILD (SiO₂/SiN)
[10] Ohmic contact (Ni for n, Ti/Al for p; 950–1050°C RTA)
[11] Passivation (SiN/SiO₂)
[12] Pad / backside metal
```

### 7.2 GaN HEMT (개념)

```
[1] substrate (Si/SiC/sapphire)
[2] AlN nucleation
[3] buffer (C-doped GaN / superlattice)
[4] GaN channel
[5] AlGaN barrier
[6] GaN cap
[7] mesa isolation (ICP)
[8] ohmic (Ti/Al/Ni/Au, 800–900°C)
[9] gate dielectric (ALD Al₂O₃) / p-GaN gate
[10] gate metal
[11] SiNx passivation
[12] field plate
[13] pad
```

### 7.3 Thermal budget 충돌

| 공정 | 온도 | 뒤 공정에 미치는 영향 |
|---|---|---|
| SiC activation | 1600–1700°C | 금속/게이트 형성 **전**에 해야 함 |
| SiC gate oxidation | 1200–1400°C | 계면/도핑 재분포 |
| SiC ohmic anneal | 950–1050°C | gate oxide/passivation 손상 가능 → **짧은 RTA** |
| GaN epi | 1000–1100°C | 기판/버퍼 stress |
| GaN ohmic | 800–900°C | 표면 morphology, passivation |
| p-GaN activation | 700–800°C | Mg 활성화, 수소 제거 |
| ALD gate dielectric | 200–300°C | 열예산 낮음 |

> **설계 원칙**: "가장 높은 온도 공정을 먼저, 가장 민감한 계면을 나중에." SiC는 **activation → gate ox → ohmic** 순서가 강제됩니다.

---

## 8. 차세대 소재 비교 (참고)

| 소재 | 장점 | 결정적 한계 | 상용화 수준 |
|---|---|---|---|
| 4H-SiC | 성숙, 열전도, 고전압 | 웨이퍼 가격, Dit, BPD | 양산 |
| GaN | 고이동도, 고주파 | normally-on, dynamic Ron, 열 | 양산(650 V) |
| Ga₂O₃ | 초광대역, 저가 대량 성장 가능 | **열전도도 0.1–0.3 W/cm·K** | 연구/초기 |
| Diamond | 최고 FOM, 열 | 도핑/웨이퍼 크기/가공 | 연구 |
| AlN | 초광대역, 열 | 도핑/기판 | 연구 |

> **실무 판단**: 1.2 kV 이하 EV/PSU는 SiC/GaN, 3.3 kV 이상은 SiC, 초고압·극한환경은 Ga₂O₃/Diamond 연구가 진행 중입니다.

---

## 9. 1D Sanity Check 워크숍

시뮬레이션 전에 **손으로 답을 알아야** 결과를 의심할 수 있습니다.

### 9.1 문제 1: 1.2 kV SiC epi 설계

**주어진 조건**: `V_B = 1200 V`, `E_c = 2.5 MV/cm`, `ε = 8.55×10⁻¹³ F/cm`, `q = 1.602×10⁻¹⁹ C`

1. 도핑 농도
   ```
   N_D = ε E_c²/(2qV_B)
       = (8.55×10⁻¹³)(6.25×10¹²)/(2×1.602×10⁻¹⁹×1200)
       = 1.39×10¹⁶ cm⁻³
   ```
2. 공핍폭
   ```
   W = 2V_B/E_c = 2400/2.5×10⁶ = 9.6×10⁻⁴ cm = 9.6 µm
   ```
3. 이상적 Ron,sp ≈ **0.45 mΩ·cm²**

**해석**: 실제 1.2 kV SiC MOSFET의 Ron,sp는 1.5–2.5 mΩ·cm² 수준 → **이론의 3–5배**. 차이는 채널/JFET/접촉/기판 저항.

### 9.2 문제 2: Gate oxide 전계

**조건**: `t_ox = 50 nm`, `V_gs = 20 V`

```
E_ox = V_ox/t_ox = 20/50×10⁻⁷ cm = 4×10⁶ V/cm = 4 MV/cm
```

- SiC MOSFET에서 **4 MV/cm는 높은 편** (신뢰성 한계 근처).
- 실무: `t_ox` 40–50 nm, `V_gs` 15–18 V → 3–4 MV/cm. **TDDB/ BTB** 관점에서 margin 확보 필요.

### 9.3 문제 3: 열저항

**조건**: SiC 10 µm, `λ = 4 W/cm·K`, 면적 1 cm²

```
R_th = t/(λA) = 10×10⁻⁴/(4×1) = 2.5×10⁻⁴ K/W
```
단위 면적당으로는 `0.25 mK·cm²/W`.

### 9.4 문제 4: TLM 미리보기

**조건**: `R_sh = 10 kΩ/sq`, `ρc = 1×10⁻⁵ Ω·cm²`

```
L_T = sqrt(ρc/R_sh) = sqrt(1×10⁻⁵/1×10⁴) = sqrt(1×10⁻⁹) = 3.16×10⁻⁵ cm = 0.316 µm
```

- `L_T`가 작을수록 접촉이 좋음.
- TLM 패턴 간격은 `L_T`의 3–10배로 설계.

---

## 10. TCAD 1D 검증 계획

### 10.1 목표
- 손계산과 시뮬레이션을 **같은 가정**으로 비교하여 툴을 신뢰할 수 있는지 확인.

### 10.2 절차

| 단계 | 내용 | 도구 |
|---|---|---|
| 1 | 1D diode 구조 (n⁻ drift + n⁺ substrate) | SDE / Athena |
| 2 | doping: `N_D = 1.39×10¹⁶ cm⁻³`, thickness 10 µm | — |
| 3 | physics: SRH, incomplete ionization, doping-dependent mobility, impact ionization | SDevice |
| 4 | reverse sweep → BV 추출 (@1 mA/cm²) | — |
| 5 | forward sweep → Ron,sp, Vf | — |
| 6 | 손계산 값과 비교 | Excel/Python |

### 10.3 합격 기준(예시)

| 항목 | 손계산 | 시뮬 허용 오차 | 비고 |
|---|---|---|---|
| BV | 1200 V | ±10% | impact ionization 파라미터 의존 |
| W @ BV | 9.6 µm | ±10% | doping 정확도 |
| Ron,sp | 0.45 mΩ·cm² | ±20% | mobility 모델 의존 |
| Vf @100 A/cm² | ~1.0–1.2 V | ±15% | barrier/lifetime |

> 오차가 크면 **파라미터/모델**을 먼저 의심하고, 그 다음 메쉬를 의심합니다.

### 10.4 라이선스가 없을 때

- **DEVSIM**으로 1D PN diode/ Schottky diode 해석.
- **Python**(numpy/scipy)으로 Poisson–drift-diffusion 1D solver 직접 구현(교육 효과 큼).
- **SRIM**으로 implant range/straggle 확인(W2).

---

## 11. 실습 과제

### 과제 1-A: FOM 계산 시트
- Si, 4H-SiC, GaN에 대해 650 V / 1.2 kV / 3.3 kV의 `N_D`, `W`, `R_on,sp(ideal)`를 계산.
- 결과를 표로 정리하고, **실제 상용 소자 datasheet**의 Ron,sp와 비교.
- 비교 결과를 "이론 대비 %"로 표시하고 원인을 3가지 이상 서술.

### 과제 1-B: 소자 선택 논리
- 응용 시나리오 3개(EV 인버터 800 V, 데이터센터 PSU 400 V, 철도 1.7 kV)를 골라.
- 각각에 대해 **소자 종류/소재/구조**를 선택하고, 이유를 FOM·열·신뢰성·비용 관점에서 1페이지로 정리.

### 과제 1-C: 1D TCAD sanity check
- 9.1의 조건으로 1D diode를 시뮬레이션.
- 손계산 vs 시뮬레이션 표를 만들고, 오차 원인을 분석.
- **메쉬 크기**를 2배/0.5배로 바꿔 BV 변화를 기록(메쉬 민감도).

---

## 12. 퀴즈 10문항 (답 포함)

**Q1.** `R_on,sp ∝ 1/E_c³`인 이유를 한 문장으로 설명하시오.
> A. 항복전압이 `E_c`와 공핍폭에 비례하고, 공핍폭이 `E_c`에 반비례하므로, 결과적으로 도핑과 두께의 곱이 `E_c³`에 반비례하기 때문.

**Q2.** SiC의 BFOM이 Si의 약 300–400배인데 실제 Ron 개선이 100배 수준인 이유는?
> A. 채널 이동도 저하(계면 산란), JFET/접촉/기판 저항, termination 효율, self-heating 등 비이상 요인 때문.

**Q3.** 4H-SiC에서 Al을 p형 도펀트로 쓸 때 실온에서 이온화율이 낮은 이유는?
> A. Al의 acceptor level이 ~0.2 eV로 kT(0.026 eV)보다 훨씬 커서, 실온에서 상당수 acceptor가 전자를 받지 않기 때문.

**Q4.** SiC에서 열확산으로 junction을 만들 수 없는 이유와 대안은?
> A. 도펀트 확산계수가 매우 작아 실용적 확산이 어렵다. 대안은 이온주입 후 1600–1800°C 고온 활성화.

**Q5.** SiC MOSFET의 gate oxide 두께 50 nm에 20 V를 인가하면 전계는?
> A. 4 MV/cm.

**Q6.** 1.2 kV SiC epi의 대표 도핑/두께를 쓰시오.
> A. ~1×10¹⁶ cm⁻³ (5×10¹⁵~1.5×10¹⁶), 10–12 µm.

**Q7.** SBD와 PiN의 trade-off를 한 줄로 설명하시오.
> A. SBD는 빠르고 Vf가 낮지만 누설이 크고, PiN은 누설이 작고 항복이 높지만 느리다.

**Q8.** GaN HEMT가 "도핑 없이" 고이동도 채널을 갖는 이유는?
> A. AlGaN/GaN 이종접합의 spontaneous/piezoelectric polarization 차이로 2DEG가 형성되기 때문.

**Q9.** SiC 공정에서 "가장 높은 온도 공정을 먼저" 해야 하는 이유는?
> A. 1600–1800°C 활성화가 금속/게이트/계면을 손상시키므로, thermal budget상 앞단에 배치해야 하기 때문.

**Q10.** TCAD 1D sanity check에서 BV가 손계산보다 30% 높게 나왔다. 가장 먼저 의심할 것은?
> A. impact ionization 파라미터/모델(그리고 doping·메쉬). 파라미터를 문헌값으로 재확인하고, 메쉬 민감도를 확인한다.

---

## 13. 강사 노트: 자주 나오는 오해

| 오해 | 사실 |
|---|---|
| "SiC는 무조건 수직형" | 600 V급은 수평형/planar도 많고, GaN은 수평형이 주류 |
| "GaN도 열산화로 게이트 산화막을 만든다" | GaN는 열산화 불가 → ALD/PECVD 절연막 |
| "SiC는 Si보다 이동도가 높다" | bulk 전자 이동도는 오히려 낮음(900 vs 1400). 이점은 **E_c** |
| "Ron이 낮으면 항상 좋다" | 스위칭 손실(Coss, Qg), 신뢰성, 열과 trade-off |
| "시뮬레이션이 맞으면 소자도 맞다" | 파라미터·모델·메쉬·경계조건이 모두 맞아야 함. 측정 캘리브레이션 필수 |
| "JTE는 도즈만 높으면 좋다" | 너무 높으면 표면 전계 집중, 너무 낮으면 공핍 부족 → **window** 존재 |

---

## 14. 다음 주차 예고

- **Week 2**: SiC 전공정 6대 단위공정 — 세정, 열산화/계면, gate oxide, 이온주입, 고온 활성화, mesa/trench 식각.
- 준비물: Deal-Grove 식 복습, SRIM 설치(무료), 05 파일의 "TCAD 입문자 트랙" 확인.

---

*© 2026 SiC/GaN Process Course Notes. 본 문서는 교육용이며, 공정 조건은 대표값입니다.*
