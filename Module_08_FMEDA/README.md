# Module 08: FMEDA (Failure Modes, Effects and Diagnostic Analysis)

[← 이전 모듈](../Module_07_FTA/) | [메인으로 돌아가기](../README.md) | [다음 모듈 →](../Module_09_Traceability_Reports/)

## 📋 모듈 개요

| 항목 | 내용 |
|------|------|
| **학습 시간** | 4시간 |
| **난이도** | ★★★★☆ (중상급) |
| **선수 과목** | Module 01 ~ Module 07 |
| **학습 목표** | FMEDA 분석 및 ISO 26262 하드웨어 메트릭 계산 |

## 🎯 학습 목표

이 모듈을 완료하면 다음을 수행할 수 있습니다:
- FMEDA의 개념과 FMEA와의 차이 이해
- Failure Mode 분류 (Safe, Detected, Latent) 수행
- Diagnostic Coverage 계산
- ISO 26262 하드웨어 메트릭 (SPFM, LFM, PMHF) 이해 및 계산
- medini analyze에서 FMEDA 분석 수행

---

## 1. FMEDA 개요

### 1.1 FMEDA란?

**FMEDA (Failure Modes, Effects and Diagnostic Analysis)**는 FMEA를 확장하여 진단 메커니즘의 효과를 분석하고, ISO 26262 하드웨어 메트릭을 계산하기 위한 분석 방법입니다.

```
┌─────────────────────────────────────────────────────────────┐
│                     FMEDA Overview                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  FMEDA = FMEA + Diagnostic Analysis                        │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                      FMEA                            │   │
│  │  • Failure Mode 식별                                │   │
│  │  • Effect 분석                                      │   │
│  │  • Cause 분석                                       │   │
│  │  • S, O, D 평가                                     │   │
│  ├─────────────────────────────────────────────────────┤   │
│  │                   + Diagnostic                       │   │
│  │  • Failure Mode 분류 (Safe/Detected/Latent)         │   │
│  │  • Diagnostic Coverage 계산                         │   │
│  │  • 하드웨어 메트릭 계산                             │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  목적:                                                      │
│  • ISO 26262 Part 5 하드웨어 요구사항 충족 입증           │
│  • Safety Mechanism의 효과 정량화                         │
│  • 잔여 위험(Residual Risk) 계산                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 1.2 FMEA vs FMEDA 비교

| 항목 | FMEA | FMEDA |
|------|------|-------|
| **목적** | 위험 우선순위 결정 | 하드웨어 메트릭 계산 |
| **Failure Mode 분류** | 없음 | Safe/Detected/Latent |
| **정량화** | RPN/AP | Failure Rate, DC |
| **출력** | 개선 조치 | SPFM, LFM, PMHF |
| **표준** | SAE J1739, AIAG-VDA | ISO 26262 Part 5 |

---

## 2. Failure Mode 분류

### 2.1 분류 체계

```
┌─────────────────────────────────────────────────────────────┐
│              Failure Mode Classification                    │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│                    Total Failure Rate (λ)                   │
│                           │                                 │
│          ┌────────────────┴────────────────┐               │
│          │                                 │                │
│          ▼                                 ▼                │
│  ┌───────────────┐                ┌───────────────┐        │
│  │  Safe Fault   │                │ Safety-Related │        │
│  │     (λS)      │                │  Fault (λSR)   │        │
│  │               │                │                │        │
│  │ 안전에 영향   │                │ 안전에 영향    │        │
│  │ 없는 고장     │                │ 있는 고장      │        │
│  └───────────────┘                └───────┬───────┘        │
│                                           │                 │
│                          ┌────────────────┴────────────┐   │
│                          │                             │    │
│                          ▼                             ▼    │
│                 ┌────────────────┐           ┌────────────┐│
│                 │ Single-Point   │           │  Residual  ││
│                 │ Fault (λSPF)   │           │Fault (λRF) ││
│                 │                │           │            ││
│                 │ 단일 고장으로  │           │ 다중 고장  ││
│                 │ Safety Goal    │           │ 시에만     ││
│                 │ 위반           │           │ 영향       ││
│                 └───────┬────────┘           └─────┬──────┘│
│                         │                          │       │
│                         ▼                          ▼       │
│                 ┌────────────────┐          ┌────────────┐ │
│                 │   Detected     │          │   Latent   │ │
│                 │  Fault (λDet)  │          │Fault (λLat)│ │
│                 └────────────────┘          └────────────┘ │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 2.2 분류 정의

| 분류 | 설명 | 예시 |
|------|------|------|
| **Safe Fault** | 안전 기능에 영향 없는 고장 | LED 표시 오류 |
| **Single-Point Fault** | 단독으로 Safety Goal 위반 | 브레이크 센서 완전 고장 |
| **Residual Fault** | Safety Mechanism에 의해 완전히 방지되지 않는 고장 | 검출되지만 반응 시간 내 처리 불가 |
| **Detected Fault** | Safety Mechanism에 의해 검출되는 고장 | Watchdog으로 검출된 ECU 고장 |
| **Latent Fault** | 검출되지 않는 다중고장 구성 요소 | 이중화 시스템의 한쪽 고장 |

### 2.3 Failure Rate 관계식

```
┌─────────────────────────────────────────────────────────────┐
│              Failure Rate Relationships                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  λ_total = λ_S + λ_SPF + λ_RF                              │
│                                                             │
│  여기서:                                                    │
│  • λ_S: Safe Fault Rate                                    │
│  • λ_SPF: Single-Point Fault Rate                          │
│  • λ_RF: Residual Fault Rate                               │
│                                                             │
│  또한:                                                      │
│  λ_RF = λ_SPF_detected + λ_MPF                             │
│                                                             │
│  λ_MPF = λ_latent + λ_detected_MPF                         │
│                                                             │
│  여기서:                                                    │
│  • λ_MPF: Multiple-Point Fault Rate                        │
│  • λ_latent: Latent Fault Rate                             │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Diagnostic Coverage (DC)

### 3.1 DC 개념

**Diagnostic Coverage**는 Safety Mechanism이 고장을 검출하는 능력을 정량화한 지표입니다.

```
┌─────────────────────────────────────────────────────────────┐
│              Diagnostic Coverage Formula                    │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│                    λ_detected                               │
│         DC  =  ─────────────────                           │
│                 λ_total_dangerous                           │
│                                                             │
│  또는 Failure Mode 관점:                                    │
│                                                             │
│         Σ (λ_FM × DC_FM)                                   │
│  DC  =  ──────────────────                                 │
│              Σ λ_FM                                         │
│                                                             │
│  여기서:                                                    │
│  • λ_detected: 검출된 고장 비율                            │
│  • λ_total_dangerous: 전체 위험 고장 비율                  │
│  • DC_FM: 각 Failure Mode의 DC                             │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 3.2 DC 등급 (ISO 26262)

| DC 등급 | 범위 | 설명 |
|---------|------|------|
| **None** | DC < 60% | 낮은 진단 커버리지 |
| **Low** | 60% ≤ DC < 90% | 기본 진단 |
| **Medium** | 90% ≤ DC < 99% | 향상된 진단 |
| **High** | DC ≥ 99% | 높은 진단 커버리지 |

### 3.3 Safety Mechanism별 DC 예시

| Safety Mechanism | 일반적 DC | 비고 |
|------------------|----------|------|
| Watchdog Timer | 60-90% | SW 무한루프 검출 |
| CRC Check | 99% | 데이터 무결성 |
| Dual Channel Comparison | 99% | 이중화 비교 |
| Range Check | 60-90% | 값 범위 확인 |
| Plausibility Check | 60-90% | 논리적 타당성 |
| BIST (Built-In Self Test) | 90-99% | 시작 시 자가 진단 |
| Parity Check | 90% | 메모리 오류 검출 |
| ECC (Error Correction Code) | 99% | 메모리 오류 검출/정정 |

---

## 4. ISO 26262 하드웨어 메트릭

### 4.1 메트릭 개요

```
┌─────────────────────────────────────────────────────────────┐
│           ISO 26262 Hardware Metrics                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  SPFM (Single-Point Fault Metric)                   │   │
│  │  • 목적: 단일 고장에 대한 안전성 평가               │   │
│  │  • Random HW Failure로 인한 SPF 방지 능력           │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  LFM (Latent Fault Metric)                          │   │
│  │  • 목적: 잠재 고장에 대한 안전성 평가               │   │
│  │  • 다중 고장 시나리오에서의 취약성 평가             │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  PMHF (Probabilistic Metric for HW Failures)        │   │
│  │  • 목적: 잔여 위험의 정량적 평가                    │   │
│  │  • Safety Goal 위반 확률                            │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 4.2 SPFM (Single-Point Fault Metric)

```
┌─────────────────────────────────────────────────────────────┐
│                    SPFM Formula                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│              Σ(λ_S) + Σ(λ_SPF × DC_SPF) + Σ(λ_MPF)         │
│  SPFM  =  ──────────────────────────────────────────       │
│                         Σ λ_total                           │
│                                                             │
│  또는 간략화:                                               │
│                                                             │
│              λ_S + λ_SPF_detected + λ_MPF                  │
│  SPFM  =  ────────────────────────────────                 │
│                     λ_total                                 │
│                                                             │
│         = 1 - (λ_SPF_undetected / λ_total)                 │
│                                                             │
│  의미: Safe Fault + 검출된 SPF + MPF의 비율                │
│       = "단일 고장으로부터 얼마나 보호되는가"              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 4.3 LFM (Latent Fault Metric)

```
┌─────────────────────────────────────────────────────────────┐
│                     LFM Formula                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│       Σ(λ_S) + Σ(λ_MPF_detected) + Σ(λ_MPF × DC_latent)   │
│  LFM = ────────────────────────────────────────────────    │
│           Σ(λ_S) + Σ(λ_SPF) + Σ(λ_MPF)                    │
│                                                             │
│  또는 간략화:                                               │
│                                                             │
│         = 1 - (λ_latent / (λ_total - λ_SPF_undetected))   │
│                                                             │
│  의미: 잠재 고장이 아닌 고장의 비율                        │
│       = "다중 고장 시나리오에서 얼마나 보호되는가"         │
│                                                             │
│  참고: SPF는 이미 위험하므로 LFM 계산에서 제외 가능        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 4.4 PMHF (Probabilistic Metric for HW Failures)

```
┌─────────────────────────────────────────────────────────────┐
│                    PMHF Formula                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  PMHF = Σ(λ_SPF_residual) + Σ(λ_RF) + Σ(λ_DPF)            │
│                                                             │
│  여기서:                                                    │
│  • λ_SPF_residual: 잔여 SPF 비율                          │
│  • λ_RF: Residual Fault 비율                               │
│  • λ_DPF: Dual-Point Fault 기여분                          │
│                                                             │
│  단위: FIT (Failures In Time, 10⁻⁹/hour)                   │
│                                                             │
│  간략화된 계산:                                             │
│  PMHF ≈ λ_SPF × (1-DC) + λ_latent × λ_independent × T    │
│                                                             │
│  의미: Safety Goal 위반으로 이어지는 고장률                │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 4.5 ASIL별 메트릭 목표값

| 메트릭 | ASIL B | ASIL C | ASIL D |
|--------|--------|--------|--------|
| **SPFM** | ≥ 90% | ≥ 97% | ≥ 99% |
| **LFM** | ≥ 60% | ≥ 80% | ≥ 90% |
| **PMHF** | < 100 FIT | < 100 FIT | < 10 FIT |

---

## 5. FMEDA 수행 절차

### 5.1 분석 워크플로우

```
┌─────────────────────────────────────────────────────────────┐
│                 FMEDA Workflow                              │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Step 1: 시스템 구조 정의                                   │
│  ─────────────────────────                                  │
│  • Safety-related 컴포넌트 식별                            │
│  • 블록 다이어그램 작성                                    │
│                                                             │
│  Step 2: Failure Mode 식별                                  │
│  ─────────────────────────                                  │
│  • 각 컴포넌트의 Failure Mode 나열                         │
│  • Failure Rate 할당 (FIT)                                 │
│                                                             │
│  Step 3: Failure Mode 분류                                  │
│  ─────────────────────────                                  │
│  • Safe / Safety-Related 분류                              │
│  • SPF / MPF 분류                                          │
│                                                             │
│  Step 4: Safety Mechanism 분석                              │
│  ─────────────────────────                                  │
│  • 각 Failure Mode에 대한 SM 식별                          │
│  • Diagnostic Coverage 결정                                │
│                                                             │
│  Step 5: Detected / Latent 분류                            │
│  ─────────────────────────                                  │
│  • DC에 따라 Detected/Latent 비율 계산                    │
│                                                             │
│  Step 6: 메트릭 계산                                        │
│  ─────────────────────────                                  │
│  • SPFM, LFM, PMHF 계산                                    │
│  • 목표값 대비 평가                                        │
│                                                             │
│  Step 7: 개선 조치                                          │
│  ─────────────────────────                                  │
│  • 목표 미달 시 SM 추가/개선                               │
│  • 재계산 및 검증                                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 5.2 FMEDA 테이블 구조

```
┌────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    FMEDA Table                                                 │
├──────┬─────────┬─────────┬───────┬───────┬──────┬────────┬────────┬─────────┬────────┬───────┤
│ Item │ Failure │ λ(FIT)  │ Safe/ │ SPF/  │ SM   │ DC (%) │ λ_Det  │ λ_Lat   │ λ_SPF  │ λ_RF  │
│      │ Mode    │         │ SR    │ MPF   │      │        │ (FIT)  │ (FIT)   │ (FIT)  │ (FIT) │
├──────┼─────────┼─────────┼───────┼───────┼──────┼────────┼────────┼─────────┼────────┼───────┤
│Sensor│No Output│   50    │  SR   │  SPF  │Range │   90   │   45   │    -    │   5    │   -   │
│      │         │         │       │       │Check │        │        │         │        │       │
├──────┼─────────┼─────────┼───────┼───────┼──────┼────────┼────────┼─────────┼────────┼───────┤
│Sensor│Drift    │   30    │  SR   │  SPF  │Plaus.│   60   │   18   │    -    │  12    │   -   │
│      │         │         │       │       │Check │        │        │         │        │       │
├──────┼─────────┼─────────┼───────┼───────┼──────┼────────┼────────┼─────────┼────────┼───────┤
│Sensor│Stuck    │   20    │  SR   │  MPF  │Dual  │   99   │   -    │  0.2    │   -    │   -   │
│ (B)  │         │         │       │       │Comp. │        │        │         │        │       │
├──────┼─────────┼─────────┼───────┼───────┼──────┼────────┼────────┼─────────┼────────┼───────┤
│ECU   │Reset    │   10    │ Safe  │   -   │  -   │   -    │   -    │    -    │   -    │   -   │
├──────┴─────────┴─────────┴───────┴───────┴──────┴────────┴────────┴─────────┴────────┴───────┤
│ Total                      110                            63       0.2       17             │
└──────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 6. medini analyze에서 FMEDA 수행

### 6.1 FMEDA 모델 생성

```
┌─────────────────────────────────────────────────────────────┐
│              FMEDA Creation in medini                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Step 1: FMEDA 생성                                         │
│  ─────────────────                                          │
│  Project → New → FMEDA                                     │
│                                                             │
│  Step 2: 시스템 모델 연결                                   │
│  ─────────────────                                          │
│  • System Model의 Block 가져오기                           │
│  • Failure Mode 자동 Import                                │
│  • Failure Rate 동기화                                     │
│                                                             │
│  Step 3: Safety Mechanism 카탈로그                          │
│  ─────────────────                                          │
│  • 표준 SM 라이브러리 활용                                 │
│  • ISO 26262 Annex D 기반 DC 값                            │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 6.2 FMEDA Editor

```
┌─────────────────────────────────────────────────────────────┐
│                  FMEDA Editor View                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─ Item Tree ─────┐  ┌─ FMEDA Table ─────────────────────┐│
│  │                 │  │                                    ││
│  │ ▼ EPS_System    │  │ [Filter: All ▼] [Show: Details ▼] ││
│  │   ▼ Sensor      │  │                                    ││
│  │     ○ No Output │  │ ┌────────────────────────────────┐││
│  │     ○ Drift     │  │ │Item: Sensor                    │││
│  │     ○ Stuck     │  │ │FM: No Output                   │││
│  │   ▼ ECU         │  │ │λ = 50 FIT                      │││
│  │     ○ Lockup    │  │ │Classification: SR, SPF         │││
│  │     ○ Reset     │  │ │SM: Range Check (DC=90%)        │││
│  │   ▼ Motor       │  │ │λ_detected = 45 FIT             │││
│  │     ○ Open      │  │ │λ_SPF_residual = 5 FIT          │││
│  │     ○ Short     │  │ └────────────────────────────────┘││
│  │                 │  │                                    ││
│  └─────────────────┘  └────────────────────────────────────┘│
│                                                             │
│  ┌─ Metrics Summary ───────────────────────────────────────┐│
│  │ SPFM: 94.5%  [Target: ≥90%]  ✓                         ││
│  │ LFM:  87.3%  [Target: ≥80%]  ✓                         ││
│  │ PMHF: 45 FIT [Target: <100]  ✓                         ││
│  └─────────────────────────────────────────────────────────┘│
│                                                             │
│  [Add SM] [Calculate Metrics] [Generate Report] [Validate] │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 6.3 Safety Mechanism 할당

```
┌─────────────────────────────────────────────────────────────┐
│           Safety Mechanism Assignment Dialog                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Failure Mode: Sensor - No Output                          │
│  Current λ: 50 FIT                                         │
│  Classification: Safety-Related, Single-Point Fault        │
│                                                             │
│  Available Safety Mechanisms:                               │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ ☑ Input Range Check          DC: [90%  ▼]          │   │
│  │ ☐ Redundant Sensor           DC: [99%  ▼]          │   │
│  │ ☐ Plausibility Check         DC: [60%  ▼]          │   │
│  │ ☐ Watchdog                   DC: [90%  ▼]          │   │
│  │ ☑ Signal Timeout Detection   DC: [95%  ▼]          │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  Combined DC Calculation:                                   │
│  DC_combined = 1 - (1-0.90) × (1-0.95) = 99.5%            │
│                                                             │
│  Result:                                                    │
│  • λ_detected = 50 × 0.995 = 49.75 FIT                    │
│  • λ_residual = 50 × 0.005 = 0.25 FIT                     │
│                                                             │
│  [Cancel]                           [Apply] [OK]           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 7. 실습 과제

### 과제 1: Failure Mode 분류

다음 Failure Mode를 분류하세요:

| Component | Failure Mode | λ (FIT) | Safe/SR | SPF/MPF |
|-----------|--------------|---------|---------|---------|
| Torque Sensor | No Output | 50 | ? | ? |
| Torque Sensor | Stuck High | 30 | ? | ? |
| ECU | SW Lockup | 20 | ? | ? |
| Motor | Winding Open | 40 | ? | ? |
| 이중화 Sensor B | Drift | 25 | ? | ? |

### 과제 2: DC 계산

다음 Safety Mechanism 조합의 Combined DC를 계산하세요:

```
Failure Mode: ECU Lockup
Safety Mechanisms:
1. Watchdog Timer: DC = 90%
2. Program Flow Monitoring: DC = 80%
3. Alive Signal Check: DC = 70%

Combined DC = ?
```

### 과제 3: 메트릭 계산

다음 데이터로 SPFM, LFM을 계산하세요:

```
총 Failure Rate: 200 FIT
- Safe Fault: 30 FIT
- SPF (undetected): 10 FIT
- SPF (detected): 80 FIT
- MPF (latent): 20 FIT
- MPF (detected): 60 FIT

SPFM = ?
LFM = ?
```

### 과제 4: medini FMEDA 실습

medini analyze에서 FMEDA를 수행하세요:

```
수행할 내용:
1. EPS 시스템 FMEDA 생성
2. System Model 연결
3. Failure Mode 분류
4. Safety Mechanism 할당
5. DC 입력
6. 메트릭 자동 계산
7. 목표값 대비 검증
8. 보고서 생성
```

---

## 8. 핵심 용어 정리

| 용어 | 정의 |
|------|------|
| **FMEDA** | Failure Modes, Effects and Diagnostic Analysis |
| **Safe Fault** | 안전에 영향 없는 고장 |
| **SPF** | Single-Point Fault - 단일 고장으로 SG 위반 |
| **MPF** | Multiple-Point Fault - 다중 고장 조합 |
| **Latent Fault** | 검출되지 않는 잠재 고장 |
| **DC** | Diagnostic Coverage - 진단 커버리지 |
| **SPFM** | Single-Point Fault Metric |
| **LFM** | Latent Fault Metric |
| **PMHF** | Probabilistic Metric for HW Failures |
| **FIT** | Failures In Time (10⁻⁹/hour) |

---

## ✅ 학습 체크리스트

- [ ] FMEDA의 목적과 FMEA와의 차이를 설명할 수 있다
- [ ] Failure Mode 분류 (Safe, SPF, MPF, Latent)를 수행할 수 있다
- [ ] Diagnostic Coverage의 개념을 이해한다
- [ ] Safety Mechanism의 DC를 적용할 수 있다
- [ ] SPFM, LFM, PMHF 공식을 이해하고 계산할 수 있다
- [ ] ASIL별 메트릭 목표값을 알고 있다
- [ ] medini에서 FMEDA를 수행할 수 있다

---

[← 이전 모듈: FTA](../Module_07_FTA/) | [메인으로 돌아가기](../README.md) | [다음 모듈: Traceability 및 Reports →](../Module_09_Traceability_Reports/)
