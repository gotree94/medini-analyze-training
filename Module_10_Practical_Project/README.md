# Module 10: 실습 프로젝트

[← 이전 모듈](../Module_09_Traceability_Reports/) | [메인으로 돌아가기](../README.md)

## 📋 모듈 개요

| 항목 | 내용 |
|------|------|
| **학습 시간** | 8시간 |
| **난이도** | ★★★★★ (고급) |
| **선수 과목** | Module 01 ~ Module 09 |
| **학습 목표** | 전체 기능안전 분석 프로세스 실습 |

## 🎯 학습 목표

이 모듈을 완료하면 다음을 수행할 수 있습니다:
- 실제 시스템에 대한 End-to-End 기능안전 분석 수행
- ISO 26262 Concept Phase부터 Safety Analysis까지 전 과정 실습
- medini analyze의 모든 기능을 통합적으로 활용
- Safety Case 문서 작성 및 제출

---

## 1. 프로젝트 개요

### 1.1 실습 대상 시스템

**EPB (Electric Parking Brake) 시스템**

```
┌─────────────────────────────────────────────────────────────┐
│              EPB System Overview                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  EPB = Electric Parking Brake                               │
│      = 전자식 주차 브레이크                                 │
│                                                             │
│  주요 기능:                                                 │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ • 차량 정차 시 브레이크 자동 체결                   │   │
│  │ • 운전자 요청에 의한 브레이크 체결/해제             │   │
│  │ • 비상 시 동적 제동 (Dynamic Braking)               │   │
│  │ • 경사로 밀림 방지 (Hill Hold)                      │   │
│  │ • 자동 해제 (Auto Release)                          │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  시스템 구성:                                               │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                                                     │   │
│  │     [EPB Switch] ──────┐                           │   │
│  │                        │                            │   │
│  │     [Vehicle Speed] ───┼───▶ [EPB ECU] ───▶ [Motor]│   │
│  │                        │         │           │      │   │
│  │     [Brake Pedal] ─────┘         │           │      │   │
│  │                                  │           ▼      │   │
│  │     [Gear Position] ─────────────┘      [Caliper]  │   │
│  │                                                     │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 1.2 프로젝트 범위

| 항목 | 포함 | 제외 |
|------|------|------|
| **ECU** | EPB ECU HW/SW | 기타 ECU |
| **Actuator** | 브레이크 모터, 캘리퍼 | 유압 시스템 |
| **Sensor** | 스위치, 위치 센서 | 휠 속도 센서 (외부 입력) |
| **Interface** | CAN 통신 | FlexRay |

---

## 2. 프로젝트 일정

### 2.1 Phase별 일정

```
┌─────────────────────────────────────────────────────────────┐
│                  Project Schedule                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Phase 1: Item Definition & HARA (2시간)                   │
│  ═══════════════════════════════════════                    │
│  • Item Definition 작성                                    │
│  • Operational Situation 분석                              │
│  • Hazard 식별                                             │
│  • ASIL 결정                                               │
│  • Safety Goals 도출                                       │
│                                                             │
│  Phase 2: System Design (1.5시간)                          │
│  ═══════════════════════════════════════                    │
│  • System Architecture (BDD/IBD)                           │
│  • Failure Mode 정의                                       │
│  • Safety Requirements 할당                                │
│                                                             │
│  Phase 3: Safety Analysis (3시간)                          │
│  ═══════════════════════════════════════                    │
│  • FMEA 수행                                               │
│  • FTA 수행                                                │
│  • FMEDA 및 메트릭 계산                                    │
│                                                             │
│  Phase 4: Documentation (1.5시간)                          │
│  ═══════════════════════════════════════                    │
│  • Traceability 검증                                       │
│  • 보고서 생성                                             │
│  • Safety Case 완성                                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Phase 1: Item Definition & HARA

### 3.1 Task 1: Item Definition 작성

**작성할 내용:**

```
┌─────────────────────────────────────────────────────────────┐
│                  EPB Item Definition                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. 시스템 명칭: Electric Parking Brake System             │
│                                                             │
│  2. 기능 목록:                                              │
│     F-001: 주차 브레이크 체결                              │
│     F-002: 주차 브레이크 해제                              │
│     F-003: 동적 제동 (비상 시)                             │
│     F-004: 경사로 밀림 방지                                │
│     F-005: 자동 해제                                       │
│                                                             │
│  3. 시스템 경계:                                            │
│     포함: EPB ECU, 모터, 캘리퍼, 스위치, 위치센서         │
│     제외: ESC ECU, 휠 속도 센서 (외부 입력)               │
│                                                             │
│  4. 운영 조건:                                              │
│     온도: -40°C ~ +85°C                                    │
│     전압: 9V ~ 16V                                         │
│     차량 속도: 0 ~ 200 km/h                                │
│                                                             │
│  5. 외부 인터페이스:                                        │
│     - CAN: 차량 속도, 기어 위치, 브레이크 페달            │
│     - 전원: 12V 배터리                                     │
│     - 스위치: 운전자 입력                                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 3.2 Task 2: HARA 수행

**Operational Situations:**

| ID | Road Type | Speed | Vehicle State | Exposure |
|----|-----------|-------|---------------|----------|
| OS-001 | Parking | 0 km/h | Stationary | E4 |
| OS-002 | Slope | < 5 km/h | Starting | E3 |
| OS-003 | Highway | > 100 km/h | Moving | E3 |
| OS-004 | Urban | 30-50 km/h | Moving | E4 |
| OS-005 | Any | Any | Reversing | E3 |

**Malfunctions:**

| ID | Function | Malfunction |
|----|----------|-------------|
| MF-001 | F-001 | 브레이크 체결 불가 |
| MF-002 | F-002 | 브레이크 해제 불가 |
| MF-003 | F-003 | 의도치 않은 체결 (주행 중) |
| MF-004 | F-002 | 의도치 않은 해제 (경사로) |

**HARA 결과 작성:**

| ID | Malfunction | Op. Situation | S | E | C | ASIL |
|----|-------------|---------------|---|---|---|------|
| HE-001 | MF-001 | OS-001 | ? | ? | ? | ? |
| HE-002 | MF-003 | OS-003 | ? | ? | ? | ? |
| HE-003 | MF-004 | OS-002 | ? | ? | ? | ? |
| ... | ... | ... | | | | |

### 3.3 Task 3: Safety Goals 도출

**Safety Goal 템플릿:**

```
┌─────────────────────────────────────────────────────────────┐
│                    Safety Goal Example                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ID:          SG-EPB-001                                    │
│  Title:       의도치 않은 주행 중 브레이크 체결 방지       │
│  ASIL:        ? (HARA 결과에서 도출)                       │
│  Safe State:  브레이크 해제 상태 유지                      │
│  FTTI:        ? ms                                         │
│  Related HE:  HE-002, HE-xxx                               │
│                                                             │
└─────────────────────────────────────────────────────────────┘

도출할 Safety Goals (최소 3개):
• SG-EPB-001: 
• SG-EPB-002: 
• SG-EPB-003: 
```

---

## 4. Phase 2: System Design

### 4.1 Task 4: System Architecture 작성

**BDD 작성:**

```
┌─────────────────────────────────────────────────────────────┐
│              EPB System - Block Definition                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│                    <<system>>                               │
│              ┌────────────────┐                            │
│              │   EPB_System   │                            │
│              └───────┬────────┘                            │
│                      │                                      │
│      ┌───────────────┼───────────────┐                     │
│      │               │               │                      │
│      ◆               ◆               ◆                      │
│      │               │               │                      │
│ ┌────┴────┐    ┌────┴────┐    ┌────┴────┐                │
│ │  EPB    │    │  EPB    │    │ Brake   │                │
│ │  ECU    │    │  Motor  │    │ Caliper │                │
│ └─────────┘    └─────────┘    └─────────┘                │
│      │                                                      │
│      ◆                                                      │
│ ┌────┴────┐                                                │
│ │  EPB    │                                                │
│ │  Switch │                                                │
│ └─────────┘                                                │
│                                                             │
│ 각 Block에 정의할 속성:                                    │
│ • Failure Rate (FIT)                                       │
│ • Failure Modes                                            │
│ • Safety-related 여부                                      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 4.2 Task 5: Failure Mode 정의

**각 Component의 Failure Mode:**

| Component | Failure Mode | λ (FIT) | Distribution |
|-----------|--------------|---------|--------------|
| EPB ECU | Complete failure | 50 | 10% |
| EPB ECU | Incorrect command | 200 | 40% |
| EPB ECU | No command | 150 | 30% |
| EPB ECU | Delayed response | 100 | 20% |
| EPB Motor | Winding open | 100 | 30% |
| EPB Motor | Winding short | 80 | 25% |
| EPB Motor | Stuck | 70 | 20% |
| EPB Motor | Reversed rotation | 50 | 15% |
| ... | ... | ... | ... |

### 4.3 Task 6: Requirements Allocation

**FSR 도출 및 할당:**

| FSR ID | Description | ASIL | Allocated To |
|--------|-------------|------|--------------|
| FSR-001 | 스위치 입력 유효성 검증 | ? | ECU |
| FSR-002 | 모터 위치 모니터링 | ? | ECU, Motor |
| FSR-003 | 속도 기반 체결 방지 | ? | ECU |
| FSR-004 | 이중 차단 경로 | ? | ECU, Motor |
| ... | ... | | |

---

## 5. Phase 3: Safety Analysis

### 5.1 Task 7: FMEA 수행

**FMEA 테이블 작성:**

```
┌────────────────────────────────────────────────────────────────────────────┐
│                              EPB FMEA                                      │
├──────┬──────────┬──────────┬──────────┬──────────┬───┬───┬───┬─────┬──────┤
│ ID   │ Item/    │ Failure  │ Effect   │ Cause    │ S │ O │ D │ AP  │Action│
│      │ Function │ Mode     │          │          │   │   │   │     │      │
├──────┼──────────┼──────────┼──────────┼──────────┼───┼───┼───┼─────┼──────┤
│FM-001│ECU/      │Incorrect │Unintended│SW bug    │ ? │ ? │ ? │  ?  │      │
│      │Control   │command   │brake     │          │   │   │   │     │      │
├──────┼──────────┼──────────┼──────────┼──────────┼───┼───┼───┼─────┼──────┤
│FM-002│ECU/      │No command│Unable to │HW failure│ ? │ ? │ ? │  ?  │      │
│      │Control   │          │release   │          │   │   │   │     │      │
├──────┼──────────┼──────────┼──────────┼──────────┼───┼───┼───┼─────┼──────┤
│FM-003│Motor/    │Stuck     │Brake     │Mechanical│ ? │ ? │ ? │  ?  │      │
│      │Actuate   │          │locked    │jam       │   │   │   │     │      │
├──────┴──────────┴──────────┴──────────┴──────────┴───┴───┴───┴─────┴──────┤
│ 최소 10개 이상의 Failure Mode 분석 필요                                   │
└────────────────────────────────────────────────────────────────────────────┘
```

### 5.2 Task 8: FTA 수행

**Top Events:**

```
FTA-001: "주행 중 의도치 않은 브레이크 체결"
FTA-002: "경사로에서 브레이크 의도치 않은 해제"
FTA-003: "브레이크 해제 불가"
```

**FTA 구조 작성:**

```
                    [주행 중 의도치 않은 체결]
                              │
                          ┌───┴───┐
                          │  OR   │
                          └───┬───┘
              ┌───────────────┼───────────────┐
              │               │               │
    ┌─────────┴─────┐  ┌─────┴─────┐  ┌─────┴─────┐
    │   ECU 오류    │  │  센서 오류 │  │  SW 오류  │
    └───────────────┘  └───────────┘  └───────────┘
              │
          ┌───┴───┐
          │  OR   │
          └───┬───┘
      ┌───────┼───────┐
      │       │       │
     ○       ○       ○
   HW고장  전원이상  EMI
   
각 FTA에 대해:
• Basic Event 정의
• 확률값 할당
• MCS 계산
• Top Event 확률 계산
```

### 5.3 Task 9: FMEDA 수행

**FMEDA 분석:**

| Component | FM | λ (FIT) | Safe/SR | SPF/MPF | SM | DC | λ_Det | λ_SPF |
|-----------|-----|---------|---------|---------|-----|-----|-------|-------|
| ECU | Incorrect | 200 | SR | SPF | Plaus. | 90% | 180 | 20 |
| ECU | No cmd | 150 | SR | SPF | WDG | 95% | 142.5 | 7.5 |
| Motor | Stuck | 70 | SR | MPF | Pos.Mon | 99% | - | - |
| ... | | | | | | | | |

**메트릭 계산:**

```
SPFM = (λ_S + λ_SPF_detected + λ_MPF) / λ_total = ?%
Target (ASIL ?): ≥ ?%

LFM = (λ_S + λ_MPF_detected) / (λ_S + λ_MPF) = ?%
Target (ASIL ?): ≥ ?%

PMHF = λ_SPF_residual + λ_RF = ? FIT
Target (ASIL ?): < ? FIT
```

---

## 6. Phase 4: Documentation

### 6.1 Task 10: Traceability 검증

**검증 체크리스트:**

```
☐ 모든 Hazard → Safety Goal 연결
☐ 모든 Safety Goal → FSR 연결
☐ 모든 FSR → TSR 연결
☐ 모든 TSR → System Element 할당
☐ 모든 Safety Goal → FTA Top Event 연결
☐ 모든 Failure Mode → FMEA 항목 연결
☐ Orphan 요소 없음 확인
☐ Coverage 100% 확인
```

### 6.2 Task 11: 보고서 생성

**생성할 보고서:**

| 보고서 | 형식 | 내용 |
|--------|------|------|
| Item Definition | Word | 시스템 정의 문서 |
| HARA Report | Word | 위험 분석 결과 |
| Safety Goals | Excel | SG 목록 및 속성 |
| System Architecture | Word | BDD/IBD 포함 |
| FMEA Report | Excel | 전체 FMEA 테이블 |
| FTA Report | Word | FTA 다이어그램 및 결과 |
| FMEDA Report | Excel | 메트릭 계산 결과 |
| Traceability Matrix | Excel | 전체 추적성 |

### 6.3 Task 12: Safety Case 완성

**Safety Case 구성:**

```
EPB_Safety_Case/
├── 01_Safety_Plan.docx
├── 02_Item_Definition.docx
├── 03_HARA_Report.docx
├── 04_Safety_Goals.xlsx
├── 05_Functional_Safety_Concept.docx
├── 06_Technical_Safety_Concept.docx
├── 07_System_Architecture.docx
├── 08_Safety_Analysis/
│   ├── FMEA_Report.xlsx
│   ├── FTA_Report.docx
│   └── FMEDA_Report.xlsx
├── 09_Traceability_Matrix.xlsx
└── 10_Safety_Assessment_Summary.docx
```

---

## 7. 평가 기준

### 7.1 평가 항목

| 항목 | 배점 | 평가 기준 |
|------|------|----------|
| Item Definition | 10점 | 완전성, 명확성 |
| HARA | 15점 | 체계적 분석, ASIL 정확성 |
| Safety Goals | 10점 | 적절성, 완전성 |
| System Design | 15점 | 아키텍처 품질, FM 정의 |
| FMEA | 15점 | 분석 깊이, S/O/D 적절성 |
| FTA | 15점 | 구조 정확성, MCS 계산 |
| FMEDA | 10점 | 메트릭 계산 정확성 |
| Traceability | 5점 | Coverage 100% |
| Documentation | 5점 | 보고서 품질 |

### 7.2 합격 기준

- 총점 70점 이상
- 각 항목 50% 이상
- Traceability Coverage 100%
- 메트릭 목표값 충족

---

## 8. 제출물

### 8.1 제출 파일 목록

```
[이름]_EPB_Project/
├── medini_project/          # medini 프로젝트 전체
│   └── EPB_Safety_Analysis.medini
├── reports/                  # 생성된 보고서
│   ├── HARA_Report.docx
│   ├── FMEA_Report.xlsx
│   ├── FTA_Report.docx
│   ├── FMEDA_Report.xlsx
│   └── Traceability_Matrix.xlsx
└── summary/                  # 요약 문서
    └── Project_Summary.docx
```

### 8.2 Project Summary 포함 내용

```
1. 프로젝트 개요 (1페이지)
2. HARA 결과 요약 (1페이지)
3. Safety Goals 목록 (1페이지)
4. 주요 분석 결과 (2페이지)
   - FMEA 주요 발견사항
   - FTA MCS 결과
   - FMEDA 메트릭 결과
5. 개선 권고사항 (1페이지)
6. 결론 (0.5페이지)
```

---

## 9. 참고 자료

### 9.1 EPB 시스템 참고 정보

- EPB 일반 작동 원리
- 자동차 브레이크 시스템 구조
- ISO 26262 Part 3, 4, 5, 9 관련 조항

### 9.2 분석 가이드

- Failure Rate Database (예: SN 29500, MIL-HDBK-217)
- Diagnostic Coverage 참고값 (ISO 26262 Part 5 Annex D)
- FMEA/FTA 작성 가이드라인

---

## ✅ 최종 체크리스트

### Phase 1
- [ ] Item Definition 완성
- [ ] Operational Situation 정의 (5개 이상)
- [ ] Malfunction 식별 (4개 이상)
- [ ] HARA 테이블 완성 (10개 이상 HE)
- [ ] Safety Goals 도출 (3개 이상)

### Phase 2
- [ ] BDD 작성 (모든 컴포넌트)
- [ ] IBD 작성 (연결 관계)
- [ ] Failure Mode 정의 (각 컴포넌트당 3개 이상)
- [ ] FSR 도출 및 할당

### Phase 3
- [ ] FMEA 완성 (10개 이상 FM)
- [ ] FTA 작성 (3개 Top Event)
- [ ] MCS 계산
- [ ] FMEDA 완성
- [ ] 메트릭 계산 및 검증

### Phase 4
- [ ] Traceability 100% 달성
- [ ] 모든 보고서 생성
- [ ] Safety Case 폴더 구성
- [ ] 제출물 준비

---

**축하합니다! 🎉**

이 프로젝트를 완료하면 Ansys medini analyze를 활용한 ISO 26262 기능안전 분석 역량을 갖추게 됩니다.

---

[← 이전 모듈: Traceability 및 Reports](../Module_09_Traceability_Reports/) | [메인으로 돌아가기](../README.md)
