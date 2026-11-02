# Day 01 — 오리엔테이션: 전력전자 지형과 140시간 로드맵

> **Week 1 / Day 1** | **7시간** | 강의 4h + 실습 2h + 사례·퀴즈 1h
> **선수**: 없음
> **참조**: [`../01_Week1_Overview_and_WBG_Process_Fundamentals.md`](../01_Week1_Overview_and_WBG_Process_Fundamentals.md)

---

## 1. 오늘의 질문

> **"왜 지금 SiC/GaN인가? 어떤 응용에서 어떤 소자를 쓰며, 그 선택을 숫자로 어떻게 정당화하는가?"**

---

## 2. 시간표

| 시간 | 블록 | 내용 |
|---|---|---|
| 09:00–10:30 | Lecture 1 | 과정 운영, 140h 로드맵, 전력전자 기초 |
| 10:45–12:15 | Lecture 2 | 응용 지형, 손실 계산, WBG 가치 |
| 13:15–15:15 | Lab | Si vs SiC vs GaN 손실 계산 |
| 15:30–16:30 | Case | EV 인버터/데이터센터 PSU 사례 |
| 16:30–17:30 | Review | 퀴즈 5문항, 과제, Q&A |

---

## 3. 학습목표

1. 140시간 과정의 20일 구조와 평가 기준을 설명한다.
2. 전력변환 응용을 **전압·전류·주파수·열** 축으로 분류한다.
3. 도통·스위칭·게이트·Coss 손실을 계산한다.
4. Si IGBT / SiC MOSFET / GaN HEMT의 손실을 같은 조건에서 비교한다.
5. WBG가 유리한 조건과 불리한 조건을 구분한다.
6. 실습 환경(Python/Excel, TCAD, SRIM)을 준비한다.

---

## 4. Lecture 1 — 과정 운영과 전력전자 기초 (1.5h)

### 4.1 140시간의 구조

| Week | Days | 질문 | 산출물 |
|---|---|---|---|
| 1 | 1–5 | "무엇을 왜 만드는가?" | 손실/BV/FOM 리포트 |
| 2 | 6–10 | "SiC를 어떻게 만드는가?" | 세정/산화/implant recipe |
| 3 | 11–15 | "완성하고 신뢰하는가?" | 접촉/termination/신뢰성 |
| 4 | 16–20 | "GaN·TCAD·제품화" | 캡스톤 |

- 매일 **7h**: 강의 4h + 실습 2h + 사례/퀴즈 1h.
- 평가: 퀴즈 20%, 실습 25%, 주차 시험 20%, 캡스톤 25%, 참여 10%.
- 원칙: **"가정–계산–검증–산출물"**.

### 4.2 전력변환의 기본 블록

| Converter | 기능 | 대표 소자 | 핵심 stress |
|---|---|---|---|
| Buck | 고전압→저전압 | MOSFET/HEMT | 스위칭 손실, dead time |
| Boost | 저전압→고전압 | MOSFET/IGBT | 인덕터 전류, reverse recovery |
| Inverter | DC→AC | SiC MOSFET/IGBT | SCWT, UIS, thermal cycling |
| PFC | AC→DC | SiC SBD/MOSFET | diode reverse recovery |
| LLC | 공진 DC-DC | MOSFET/HEMT | Coss, ZVS |
| OBC | AC↔DC 양방향 | SiC/GaN | 양방향, isolation |

### 4.3 전력소자의 핵심 spec

| Spec | 의미 | 설계 영향 |
|---|---|---|
| $V_{DS,max}$ | 최대 내압 | BV, termination |
| $I_D$ | 정격 전류 | chip area, thermal |
| $R_{DS(on)}$ | 도통 저항 | conduction loss |
| $Q_g$ | gate charge | gate drive loss |
| $C_{oss}$ | output capacitance | ZVS, switching loss |
| $t_{rr}$ | reverse recovery | EMI, diode loss |
| $E_{on}/E_{off}$ | switching energy | fsw 한계 |
| $R_{th,jc}$ | thermal resistance | cooling |
| $T_{j,max}$ | 최대 junction temp | derating |

> **중요**: datasheet의 $R_{DS(on)}$는 **$T_j=25°C$** 값이 많습니다. 실제는 100–175°C에서 1.5–3배 증가합니다.

---

## 5. Lecture 2 — 응용 지형과 손실 계산 (1.5h)

### 5.1 응용별 요구사항

| 응용 | 전압 | 전류 | 주파수 | 핵심 요구 | 주력 |
|---|---|---|---|---|---|
| 모바일 충전기 | 20–100 V | 1–10 A | 100–500 kHz | 소형·고효율 | GaN HEMT |
| 데이터센터 PSU | 400 V/48 V | 10–100 A | 100–500 kHz | 효율·전력밀도 | SiC/GaN |
| EV 인버터 | 400–800 V | 200–600 A | 10–20 kHz | 고신뢰·고온 | SiC MOSFET |
| EV OBC | 400–800 V | 10–50 A | 50–200 kHz | 양방향·isolated | SiC/GaN |
| PV 인버터 | 600–1500 V | 10–100 A | 16–50 kHz | 효율·수명 | SiC |
| 철도/Grid | 1.7–6.5 kV | 100–2000 A | 1–10 kHz | 고내압·신뢰 | SiC/IGBT |
| 항공우주 | 270–1000 V | 10–200 A | 20–100 kHz | 경량·방사선 | SiC/GaN |

### 5.2 손실 분해

**도통 손실** (MOSFET/HEMT):

$$
P_{cond} = I_{rms}^2 R_{DS(on),T_j} \cdot D
$$

**스위칭 손실** (근사):

$$
P_{sw} \approx \frac{1}{2} V_{DS} I_D (t_r + t_f) f_{sw}
$$

**Coss 손실**:

$$
P_{oss} = \frac{1}{2} C_{oss} V_{DS}^2 f_{sw}
$$

**게이트 손실**:

$$
P_{gate} = Q_g V_{GS} f_{sw}
$$

**Si IGBT**는 도통 시 $V_{CE,sat}$와 tail current가 추가됩니다.

### 5.3 예제: 400 V / 30 A, 20 kHz

가정:

| 항목 | Si IGBT | SiC MOSFET | GaN HEMT |
|---|---:|---:|---:|
| $R_{DS(on)}$ or $V_{CE,sat}$ | 1.8 V | 30 mΩ | 25 mΩ |
| $t_r+t_f$ | 400 ns | 40 ns | 20 ns |
| $Q_g$ | 150 nC | 60 nC | 20 nC |
| $C_{oss}$ | 300 pF | 150 pF | 100 pF |
| $D$ | 0.5 | 0.5 | 0.5 |

계산:

- Si IGBT conduction: $30 \times 1.8 \times 0.5 = 27$ W.
- SiC conduction: $30^2 \times 0.03 \times 0.5 = 13.5$ W.
- GaN conduction: $30^2 \times 0.025 \times 0.5 = 11.25$ W.
- Switching (Si IGBT): $0.5 \times 400 \times 30 \times 400n \times 20k = 48$ W.
- Switching (SiC): $0.5 \times 400 \times 30 \times 40n \times 20k = 4.8$ W.
- Switching (GaN): $0.5 \times 400 \times 30 \times 20n \times 20k = 2.4$ W.

> **결론**: 고주파·고전압에서는 **스위칭 손실**이 지배적이며, WBG의 $t_r+t_f$가 작은 것이 결정적입니다.

### 5.4 WBG가 불리한 조건

- 매우 낮은 주파수(수 kHz 이하): Si IGBT가 가격 경쟁력.
- 낮은 전압(<100 V): Si MOSFET이 충분.
- 높은 단락 전류 내량이 필요한 응용: SiC short-circuit withstand time이 짧음.
- Gate oxide 신뢰성·EMI 대책 비용이 큰 경우.

---

## 6. Lab — 손실 계산기 (2h)

### 6.1 목표

- 세 소자의 손실을 **같은 조건**에서 계산.
- 주파수·$R_{DS(on)}$·$t_{sw}$ 민감도 분석.
- 결과를 표와 그래프로 제출.

### 6.2 절차

1. Excel 또는 Python에 입력 변수 정의.
2. $f_{sw}$ = 10k, 20k, 50k, 100k Hz로 sweep.
3. 각 소자의 $P_{cond}$, $P_{sw}$, $P_{oss}$, $P_{gate}$, $P_{total}$ 계산.
4. $T_j$ 상승을 $R_{th}$로 추정.
5. 그래프: $P_{total}$ vs $f_{sw}$.

### 6.3 제출물

- 계산 시트(또는 `.py`).
- 4개 주파수 표.
- 1페이지 해석: "SiC/GaN이 유리해지는 주파수와 이유".

### 6.4 체크포인트

- [ ] 단위(µs, ns, kHz) 일관성.
- [ ] $R_{DS(on)}$ 온도 보정.
- [ ] $P_{oss}$와 $P_{gate}$ 누락 여부.
- [ ] Si IGBT의 $V_{CE,sat}$ 사용.

---

## 7. Case — EV 인버터와 데이터센터 PSU (1h)

### 7.1 EV 인버터

- 400 V → 800 V 시스템 전환: SiC MOSFET 채택 이유.
- 요구: 높은 $T_j$, 낮은 $R_{DS(on)}$, short-circuit robustness, UIS.
- trade-off: SiC는 switching loss↓ but **SCWT↓**, gate oxide 신뢰성 이슈.

### 7.2 데이터센터 PSU

- 48 V → 400 V 또는 400 V → 12 V.
- GaN HEMT: 높은 fsw, 작은 magnetization.
- trade-off: dynamic Ron, EMI, thermal.

### 7.3 토론 질문

1. 800 V EV에서 Si IGBT를 쓰면 어떤 손실이 커지는가?
2. 100 kHz에서 GaN이 SiC보다 유리한 이유는?
3. SiC MOSFET의 SCWT가 짧은 것이 시스템 설계에 주는 영향은?

---

## 8. Quiz (5문항)

1. 400 V/30 A, $D=0.5$, $R_{DS(on)}=30$ mΩ일 때 도통 손실은?
2. $V=400$ V, $I=30$ A, $(t_r+t_f)=40$ ns, $f=20$ kHz일 때 스위칭 손실은?
3. Si IGBT가 SiC MOSFET보다 유리한 조건은?
4. $C_{oss}$ 손실 공식은?
5. WBG 소자의 단점 2가지는?

**정답**: 1) 13.5 W. 2) 4.8 W. 3) 낮은 주파수·가격 민감·높은 단락 내량. 4) $\frac{1}{2}C_{oss}V^2f$. 5) gate oxide 신뢰성, 짧은 SCWT, EMI, 비용.

---

## 9. Homework

- Sze & Ng 1장 읽기.
- Python/Excel, SRIM, TCAD(또는 DEVSIM) 설치.
- 800 V/300 A EV 인버터 조건으로 손실 표 1개 작성.
- 내일 Day 2를 위해 **밴드갭·페르미 준위·캐리어 농도** 복습.

---

## 10. 강사 노트

- 수강생 배경이 다양하면 **손실 계산을 30분 더** 할애.
- "SiC=무조건 좋다"는 오해를 **SCWT/가격/EMI**로 균형.
- 실습에서 단위 오류가 가장 많으므로 **ns→s 변환**을 칠판에 명시.
- 다음 날을 위해 **도핑·페르미 준위**를 5분 예고.

---

*Day 01 / 20 — 140h program. 공정·수치는 교육용 대표값입니다.*
