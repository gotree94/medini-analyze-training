# Module 01: 기능안전 개요 및 ISO 26262

[← 메인으로 돌아가기](../README.md) | [다음 모듈 →](../Module_02_Medini_Introduction/)

## 📋 모듈 개요

| 항목 | 내용 |
|------|------|
| **학습 시간** | 4시간 |
| **난이도** | ★☆☆☆☆ (입문) |
| **선수 과목** | 없음 |
| **학습 목표** | 기능안전의 기본 개념과 ISO 26262 표준 체계 이해 |

## 🎯 학습 목표

이 모듈을 완료하면 다음을 수행할 수 있습니다:
- 기능안전(Functional Safety)의 정의와 필요성 설명
- ISO 26262 표준의 구조와 각 Part별 내용 이해
- ASIL(Automotive Safety Integrity Level) 등급 결정 방법 적용
- 기능안전 개발 프로세스(V-Model) 설명

---

## 1. 기능안전(Functional Safety) 개요

### 1.1 기능안전이란?

**기능안전(Functional Safety)**은 E/E(Electrical and/or Electronic) 시스템의 오동작으로 인한 위험을 방지하기 위한 체계적인 접근 방법입니다.

> **ISO 26262 정의**: "전기/전자 시스템의 오동작으로 인한 위험이 없는 상태의 부재로 인한 불합리한 위험의 부재"
> (Absence of unreasonable risk due to hazards caused by malfunctioning behavior of E/E systems)

### 1.2 기능안전의 필요성

```
┌─────────────────────────────────────────────────────────────┐
│                 자동차 E/E 시스템의 증가                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1970년대: 기계식 시스템 중심                                │
│     ↓                                                       │
│  1990년대: ECU 도입 (ABS, 에어백)                           │
│     ↓                                                       │
│  2010년대: 복잡한 E/E 네트워크 (100+ ECUs)                  │
│     ↓                                                       │
│  현재: ADAS, 자율주행, 소프트웨어 정의 차량                 │
│                                                             │
│  → 시스템 복잡도 증가 = 잠재적 고장 모드 증가               │
│  → 체계적인 안전 관리 필요성 대두                           │
└─────────────────────────────────────────────────────────────┘
```

### 1.3 Safety vs Security vs Quality

| 구분 | Safety (안전) | Security (보안) | Quality (품질) |
|------|---------------|-----------------|----------------|
| **목적** | 인명/재산 피해 방지 | 악의적 공격 방지 | 제품 기능 만족 |
| **위험 원인** | 시스템 오동작 | 외부 공격자 | 설계/제조 결함 |
| **관련 표준** | ISO 26262 | ISO 21434 | IATF 16949 |
| **분석 방법** | HARA, FMEA, FTA | TARA | APQP, PPAP |

---

## 2. ISO 26262 표준 체계

### 2.1 표준 개요

ISO 26262는 자동차 E/E 시스템의 기능안전을 위한 국제 표준입니다.

| 항목 | 내용 |
|------|------|
| **정식 명칭** | Road vehicles — Functional safety |
| **초판 발행** | 2011년 11월 |
| **2판 발행** | 2018년 12월 |
| **기반 표준** | IEC 61508 (일반 기능안전 표준) |
| **적용 범위** | 승용차, 상용차 (모페드 제외) |
| **최대 중량** | 3,500kg 이하 차량 |

### 2.2 ISO 26262 구조 (12 Parts)

```
┌─────────────────────────────────────────────────────────────────┐
│                        ISO 26262:2018                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Part 1: Vocabulary (용어 정의)                                 │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │ Part 2: Management of Functional Safety                 │   │
│  │ (기능안전 관리)                                          │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │ Part 3: Concept Phase (개념 단계)                        │   │
│  │  - Item Definition                                       │   │
│  │  - HARA (Hazard Analysis and Risk Assessment)           │   │
│  │  - Functional Safety Concept                            │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │ Part 4: System Level (시스템 수준)                       │   │
│  │ Part 5: Hardware Level (하드웨어 수준)                   │   │
│  │ Part 6: Software Level (소프트웨어 수준)                 │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  Part 7: Production and Operation (생산 및 운영)               │
│  Part 8: Supporting Processes (지원 프로세스)                  │
│  Part 9: ASIL-oriented and Safety-oriented Analyses            │
│  Part 10: Guidelines (가이드라인)                              │
│  Part 11: Semiconductors (반도체)                              │
│  Part 12: Motorcycles (모터사이클)                             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 2.3 Safety Lifecycle (안전 수명주기)

ISO 26262는 제품의 전체 수명주기에 걸친 안전 활동을 정의합니다:

```
    Concept        Product Development        After SOP
    Phase              Phase                   Phase
       │                  │                      │
       ▼                  ▼                      ▼
┌──────────┐    ┌──────────────────┐    ┌──────────────┐
│ - Item   │    │ System Level     │    │ - Production │
│   Def.   │    │ - Technical      │    │ - Operation  │
│ - HARA   │───►│   Safety Concept │───►│ - Service    │
│ - FSC    │    │ - System Design  │    │ - Decommis-  │
│          │    ├──────────────────┤    │   sioning    │
│          │    │ HW Development   │    │              │
│          │    │ SW Development   │    │              │
└──────────┘    └──────────────────┘    └──────────────┘
```

---

## 3. ASIL (Automotive Safety Integrity Level)

### 3.1 ASIL 개요

ASIL은 자동차 시스템의 위험 수준을 분류하는 체계입니다.

| ASIL 등급 | 위험도 | 요구사항 엄격도 | 예시 |
|-----------|--------|-----------------|------|
| **ASIL D** | 최고 | 가장 엄격 | 조향 시스템, 제동 시스템 |
| **ASIL C** | 높음 | 엄격 | 크루즈 컨트롤, ESC |
| **ASIL B** | 중간 | 보통 | 헤드라이트, 브레이크 등 |
| **ASIL A** | 낮음 | 기본 | 실내등, 인포테인먼트 |
| **QM** | 없음 | 품질관리만 | 일반 비안전 기능 |

### 3.2 ASIL 결정 방법 (HARA)

ASIL은 세 가지 파라미터의 조합으로 결정됩니다:

```
                    ASIL = f(S, E, C)

    ┌─────────────────────────────────────────────────────────┐
    │                                                         │
    │  S (Severity) - 심각도                                  │
    │  ├── S0: 부상 없음                                      │
    │  ├── S1: 경미한 부상                                    │
    │  ├── S2: 중상 (생존 가능)                               │
    │  └── S3: 치명상 (생명 위협)                             │
    │                                                         │
    │  E (Exposure) - 노출 빈도                               │
    │  ├── E0: 매우 희박                                      │
    │  ├── E1: 희박 (< 1%)                                    │
    │  ├── E2: 가끔 (1-10%)                                   │
    │  ├── E3: 자주 (> 10%)                                   │
    │  └── E4: 항상 또는 거의 항상                            │
    │                                                         │
    │  C (Controllability) - 제어 가능성                      │
    │  ├── C0: 일반적으로 제어 가능                           │
    │  ├── C1: 쉽게 제어 가능 (> 99%)                         │
    │  ├── C2: 대부분 제어 가능 (90-99%)                      │
    │  └── C3: 제어 어려움 또는 불가 (< 90%)                  │
    │                                                         │
    └─────────────────────────────────────────────────────────┘
```

### 3.3 ASIL 결정 매트릭스

| | C1 | C2 | C3 |
|---|---|---|---|
| **S1/E1** | QM | QM | QM |
| **S1/E2** | QM | QM | QM |
| **S1/E3** | QM | QM | A |
| **S1/E4** | QM | A | A |
| **S2/E1** | QM | QM | QM |
| **S2/E2** | QM | QM | A |
| **S2/E3** | QM | A | B |
| **S2/E4** | A | B | C |
| **S3/E1** | QM | QM | A |
| **S3/E2** | QM | A | B |
| **S3/E3** | A | B | C |
| **S3/E4** | B | C | D |

### 3.4 ASIL Decomposition

높은 ASIL 요구사항을 여러 요소로 분해하여 개별적으로 낮은 ASIL을 적용할 수 있습니다:

```
┌─────────────────────────────────────────────────────────────┐
│                    ASIL Decomposition                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│    원래 요구사항 (ASIL D)                                   │
│           │                                                 │
│           ▼                                                 │
│    ┌──────┴──────┐                                         │
│    │             │                                         │
│    ▼             ▼                                         │
│  요소 A       요소 B                                        │
│  ASIL B(D)   ASIL B(D)                                     │
│                                                             │
│  * (D)는 원래 ASIL D에서 분해되었음을 표시                  │
│  * 두 요소는 독립적(Independent)이어야 함                  │
│                                                             │
│  허용 가능한 분해:                                          │
│  D → D + QM, C + A, B + B                                  │
│  C → C + QM, B + A                                         │
│  B → B + QM, A + A                                         │
│  A → A + QM                                                │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 4. V-Model과 기능안전 활동

### 4.1 V-Model 개요

```
    개발 (Development)                    검증 (Verification)
    
    Item Definition ◄─────────────────────────► Safety Validation
           │                                           ▲
           ▼                                           │
    Hazard Analysis ◄──────────────────────────► Safety Assessment
           │                                           ▲
           ▼                                           │
    Functional Safety ◄────────────────────► Integration Testing
    Concept          │                              ▲
           │         │                              │
           ▼         │                              │
    Technical Safety ◄──────────────────────► System Testing
    Concept          │                              ▲
           │         │                              │
           ▼         │                              │
    System Design ◄─────────────────────────► HW/SW Testing
           │                                        ▲
           ├──────────────┬────────────────────────┤
           │              │                        │
           ▼              ▼                        │
    HW Development   SW Development ──────────────┘
```

### 4.2 각 단계별 주요 활동

| 단계 | 주요 활동 | 산출물 |
|------|----------|--------|
| Item Definition | 시스템 범위 정의, 기능 식별 | Item Definition 문서 |
| HARA | 위험 식별, ASIL 결정 | HARA 결과, Safety Goals |
| FSC | 기능안전 요구사항 도출 | Functional Safety Concept |
| TSC | 기술적 구현 방안 수립 | Technical Safety Concept |
| System Design | 시스템 아키텍처 설계 | System Architecture |
| HW/SW Dev | 상세 설계 및 구현 | HW/SW 설계 문서, 코드 |
| Testing | 단위/통합/시스템 테스트 | Test Reports |
| Validation | 안전 검증 | Validation Report |

---

## 5. 관련 표준 비교

### 5.1 산업별 기능안전 표준

| 산업 | 표준 | 위험 등급 |
|------|------|----------|
| 자동차 | ISO 26262 | ASIL A~D |
| 철도 | EN 50128, EN 50129 | SIL 1~4 |
| 항공 | DO-178C, DO-254 | DAL A~E |
| 의료기기 | IEC 62304 | Class A~C |
| 산업 | IEC 61508 | SIL 1~4 |
| 원자력 | IEC 61513 | Category A~C |

### 5.2 ISO 26262 vs IEC 61508 비교

| 항목 | ISO 26262 | IEC 61508 |
|------|-----------|-----------|
| 적용 분야 | 자동차 | 일반 산업 |
| 위험 등급 | ASIL A~D | SIL 1~4 |
| 위험 평가 | 정성적 (S, E, C) | 정량적 (확률) |
| SW 개발 모델 | V-Model | V-Model |
| ASIL D ≈ | SIL 3 수준 | SIL 3 |

---

## 6. 실습 과제

### 과제 1: ASIL 결정 연습

다음 시나리오에 대해 ASIL을 결정하세요:

**시나리오: 전동 파워 스티어링(EPS) 시스템의 갑작스러운 조향 보조력 상실**

| 파라미터 | 평가 | 근거 |
|----------|------|------|
| Severity (S) | ? | |
| Exposure (E) | ? | |
| Controllability (C) | ? | |
| **ASIL** | ? | |

### 과제 2: Safety Goal 도출

위 시나리오에 대한 Safety Goal을 작성하세요:

```
Safety Goal ID: SG-EPS-001
Title: 
Description:
ASIL:
Safe State:
```

### 과제 3: ISO 26262 Part 이해

다음 활동이 ISO 26262의 어느 Part에 해당하는지 연결하세요:

| 활동 | Part |
|------|------|
| FMEA 수행 | Part ? |
| 소프트웨어 단위 테스트 | Part ? |
| Item Definition 작성 | Part ? |
| 하드웨어 메트릭 계산 | Part ? |
| Configuration Management | Part ? |

---

## 7. 핵심 용어 정리

| 용어 | 정의 |
|------|------|
| **Functional Safety** | E/E 시스템의 오동작으로 인한 불합리한 위험의 부재 |
| **Item** | 기능안전 요구사항이 적용되는 시스템 또는 시스템의 조합 |
| **ASIL** | 자동차 안전 무결성 등급 (A~D) |
| **Safety Goal** | HARA로부터 도출된 최상위 안전 요구사항 |
| **Safe State** | 불합리한 위험이 없는 시스템의 운전 상태 |
| **FTTI** | Fault Tolerant Time Interval - 고장 허용 시간 간격 |
| **Hazard** | 피해를 유발할 수 있는 잠재적 위험 요소 |
| **Risk** | 특정 위험 상황의 발생 확률과 그 피해의 조합 |

---

## 8. 퀴즈

1. ISO 26262에서 가장 높은 ASIL 등급은?
   - a) ASIL A
   - b) ASIL C
   - c) ASIL D
   - d) SIL 4

2. ASIL 결정에 사용되지 않는 파라미터는?
   - a) Severity
   - b) Exposure
   - c) Probability
   - d) Controllability

3. ISO 26262에서 소프트웨어 개발을 다루는 Part는?
   - a) Part 4
   - b) Part 5
   - c) Part 6
   - d) Part 7

4. ASIL D를 분해할 때 허용되지 않는 조합은?
   - a) D + QM
   - b) C + A
   - c) B + B
   - d) A + A

<details>
<summary>정답 확인</summary>

1. c) ASIL D
2. c) Probability (ISO 26262는 정성적 방법 사용)
3. c) Part 6
4. d) A + A (합이 D가 되지 않음)

</details>

---

## 📚 참고 자료

- ISO 26262:2018 Road vehicles — Functional safety (All Parts)
- IEC 61508 Functional safety of E/E/PE safety-related systems
- Automotive SPICE (Process Assessment Model)
- SAE J2980 - Considerations for ISO 26262 ASIL Hazard Classification

---

## ✅ 학습 체크리스트

- [ ] 기능안전의 정의와 필요성을 설명할 수 있다
- [ ] ISO 26262의 12개 Part 구조를 이해한다
- [ ] ASIL 등급(A~D)의 의미와 결정 방법을 안다
- [ ] S, E, C 파라미터를 이용한 ASIL 결정을 할 수 있다
- [ ] ASIL Decomposition의 개념과 규칙을 이해한다
- [ ] V-Model에서의 기능안전 활동을 설명할 수 있다
- [ ] 관련 산업별 기능안전 표준을 비교할 수 있다

---

[← 메인으로 돌아가기](../README.md) | [다음 모듈: Ansys medini analyze 소개 →](../Module_02_Medini_Introduction/)
