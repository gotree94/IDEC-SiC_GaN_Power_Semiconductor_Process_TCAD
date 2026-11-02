# Day 06 — SiC 세정·표면 준비

> **Week 2 / Day 6** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: Day 5
> **참조**: [`../02_Week2_SiC_Frontend_Processes.md`](../02_Week2_SiC_Frontend_Processes.md)

---

## 1. 오늘의 질문

> **"SiC 표면의 탄소·산화막·손상층을 어떻게 제거하며, 그 결과를 어떤 지표로 검증하는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | SiC 표면 특성, 오염원, 세정 화학 |
| 10:45–12:15 | Lecture 2 | RCA/piranha/HF, sacrificial oxidation, metrology |
| 13:15–15:15 | Lab | 세정 sequence 설계 + AFM/XPS 분석 |
| 15:30–16:30 | Case | 세정 불량 → oxide leakage/Dit |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제 |

---

## 3. 학습목표

1. SiC 표면의 주요 오염·손상 메커니즘을 설명한다.
2. RCA(SC1/SC2), piranha, HF, sacrificial oxidation의 역할을 구분한다.
3. Si-face/C-face/a-face에 따른 세정·산화 차이를 설명한다.
4. AFM/XPS/SIMS/ellipsometry/contact angle로 표면을 평가한다.
5. 세정 sequence를 공정 흐름에 맞게 설계한다.
6. 세정 불량이 소자에 미치는 영향을 추적한다.

---

## 4. Lecture 1 — SiC 표면과 오염 (1.5h)

### 4.1 SiC 표면의 특징

| 특징 | 영향 |
|---|---|
| Native oxide (SiO₂) | 얇고 불균일, 계면 trap |
| Carbon residue | oxide leakage, Dit |
| Polishing scratches | breakdown, leakage |
| Micropipes/defects | yield, BV |
| Metal contamination | lifetime, leakage |
| Surface roughness | mobility, oxide integrity |
| Face polarity | oxidation rate, trap density |

### 4.2 오염원

| 오염 | 발생 | 제거 |
|---|---|---|
| Organic | resist, handling | piranha, SC1 |
| Particle | environment, polishing | SC1, brush, megasonic |
| Metal (Fe, Cu, Na) | tools, chemicals | SC2, HF |
| Carbon | resist ash, polishing | piranha, O₂ plasma |
| Native oxide | air, RCA | HF, in-situ H₂ |
| Damage layer | CMP, etch | sacrificial oxidation |

### 4.3 Si-face vs C-face

| 항목 | Si-face | C-face |
|---|---|---|
| 산화 속도 | 느림 | 빠름 |
| 계면 품질 | 상대적 우수 | trap 많음 |
| 용도 | MOS gate | backside, ohmic |
| 세정 | 표준 | carbon 제거 강화 |

---

## 5. Lecture 2 — 세정 Sequence와 Metrology (1.5h)

### 5.1 표준 세정 sequence (예)

1. **Piranha** (H₂SO₄:H₂O₂) — organic/carbon 제거.
2. **DI rinse** — chemical 제거.
3. **SC1** (NH₄OH:H₂O₂:H₂O) — particle/organic.
4. **DI rinse**.
5. **SC2** (HCl:H₂O₂:H₂O) — metal.
6. **DI rinse**.
7. **HF** (HF:H₂O) — native oxide 제거.
8. **DI rinse + N₂ dry**.
9. **Sacrificial oxidation** (선택) — damage layer 제거.
10. **HF strip** — sacrificial oxide 제거.
11. **In-situ H₂ anneal** (선택) — smooth surface.

> **SiC 주의**: HF 후 표면은 hydrophobic. 대기 산화를 최소화하고 **즉시 다음 공정**으로 이동.

### 5.2 Sacrificial oxidation

- 목적: polishing damage, implant damage, carbon residue 제거.
- 조건: 1100–1300°C dry O₂, 20–100 nm.
- strip: HF.
- 효과: oxide breakdown 개선, Dit 감소.
- trade-off: 표면 roughness 증가 가능.

### 5.3 Metrology

| 기법 | 측정 | 활용 |
|---|---|---|
| AFM | roughness (RMS) | polishing/etch damage |
| XPS | 화학 결합, C/O | carbon residue |
| SIMS | depth profile | contamination, implant |
| Ellipsometry | oxide thickness | gate oxide |
| Contact angle | hydrophilicity | clean surface |
| Lifetime | carrier lifetime | metal contamination |
| SPV | surface charge | process control |

### 5.4 세정 평가 지표

- RMS roughness < 0.5 nm (목표).
- XPS C1s/O1s ratio 감소.
- Contact angle < 10° (hydrophilic) 또는 HF 후 hydrophobic.
- Oxide breakdown field > 8 MV/cm (목표).
- Dit < $10^{12}$ cm⁻²eV⁻¹ (목표).

---

## 6. Lab — 세정 Sequence 설계 (2h)

### 6.1 목표

- 주어진 오염 시나리오에 맞는 세정 sequence 설계.
- AFM/XPS 데이터를 해석.
- 세정 후 oxide 품질과 연결.

### 6.2 시나리오

| 시료 | 상태 | 목표 |
|---|---|---|
| A | CMP 후 scratch | damage 제거 |
| B | implant 후 carbon | carbon/residue 제거 |
| C | metal 오염 | metal 제거 |
| D | gate oxide 전 | particle/organic 제거 |

### 6.3 절차

1. 각 시료의 오염원을 표로 정리.
2. 세정 단계 선택(필수/선택).
3. 온도·시간·농도 설정.
4. Metrology 2개 선택.
5. 합격 기준 정의.

### 6.4 제출물

- 시료별 sequence 표.
- AFM/XPS 그래프 해석.
- "세정 → oxide → Dit" 연결 설명.

### 6.5 체크포인트

- [ ] HF가 oxide를 제거하는 역할.
- [ ] SC1/SC2 순서.
- [ ] Sacrificial oxidation의 목적.
- [ ] 세정 후 대기 산화 방지.

---

## 7. Case — 세정 불량의 소자 영향 (1h)

### 7.1 증상

- Gate oxide leakage 증가.
- $V_{th}$ shift/hysteresis.
- Breakdown field 저하.
- Yield 감소.

### 7.2 원인 추적

1. XPS carbon residue 확인.
2. AFM scratch 확인.
3. SIMS metal contamination.
4. C-V Dit 증가.

### 7.3 대응

- Piranha 강화.
- Sacrificial oxidation 추가.
- HF 후 in-situ H2 anneal.
- Chemical/tool 교체.

### 토론

1. HF 후 hydrophobic 표면을 방치하면 왜 문제인가?
2. Sacrificial oxidation이 roughness를 증가시키는 이유는?
3. C-face 세정이 Si-face와 다른 이유는?

---

## 8. Quiz (5문항)

1. Piranha의 주요 역할은?
2. SC1과 SC2의 차이는?
3. HF가 제거하는 것은?
4. Sacrificial oxidation의 목적 2가지는?
5. XPS로 확인할 수 있는 것은?

**정답**: 1) organic/carbon 제거. 2) SC1 particle/organic, SC2 metal. 3) native oxide. 4) damage/carbon 제거, 계면 개선. 5) 화학 결합/C residue.

---

## 9. Homework

- SiC gate oxide 전 세정 sequence를 10단계로 작성.
- AFM RMS와 oxide breakdown의 상관관계 조사.
- Day 7을 위해 **Deal-Grove** 예습.

---

## 10. 강사 노트

- "세정은 눈에 보이지 않지만 yield를 지배" 강조.
- HF 후 **즉시 공정** 원칙.
- C-face/Si-face 차이를 산화 속도와 연결.
- 실습에서 **합격 기준**을 반드시 수치로.

---

*Day 06 / 20 — 140h program. 세정 조건은 교육용 대표값입니다.*
