# Module 05: System Design (SysML)

[← 이전 모듈](../Module_04_Safety_Goals_Requirements/) | [메인으로 돌아가기](../README.md) | [다음 모듈 →](../Module_06_FMEA/)

## 📋 모듈 개요

| 항목 | 내용 |
|------|------|
| **학습 시간** | 4시간 |
| **난이도** | ★★★☆☆ (중급) |
| **선수 과목** | Module 01 ~ Module 04 |
| **학습 목표** | SysML 기반 시스템 모델링 및 Safety 분석 연결 |

## 🎯 학습 목표

이 모듈을 완료하면 다음을 수행할 수 있습니다:
- SysML 기본 개념과 다이어그램 유형 이해
- medini analyze에서 시스템 아키텍처 모델링
- Block Definition Diagram (BDD) 및 Internal Block Diagram (IBD) 작성
- Safety Requirements의 시스템 요소 할당
- Failure Mode 정의 및 FMEA/FTA와의 연계

---

## 1. SysML 개요

### 1.1 SysML이란?

**SysML (Systems Modeling Language)**은 시스템 엔지니어링을 위한 범용 모델링 언어입니다.

```
┌─────────────────────────────────────────────────────────────┐
│                      SysML Overview                         │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  SysML = Systems Modeling Language                          │
│        = UML의 시스템 엔지니어링 확장                       │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                   UML (소프트웨어)                    │   │
│  │                        ↓                             │   │
│  │              재사용  + 확장  + 수정                   │   │
│  │                        ↓                             │   │
│  │                 SysML (시스템)                        │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  특징:                                                      │
│  • 하드웨어, 소프트웨어, 데이터, 프로세스 통합 모델링      │
│  • 요구사항 모델링 지원                                    │
│  • 파라메트릭 모델링 (물리적 제약)                         │
│  • 할당 관계 표현                                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 1.2 SysML 다이어그램 유형

| 분류 | 다이어그램 | 용도 |
|------|-----------|------|
| **구조** | BDD (Block Definition) | 블록과 관계 정의 |
| **구조** | IBD (Internal Block) | 내부 구조 및 연결 |
| **행위** | Activity Diagram | 기능 흐름 |
| **행위** | State Machine | 상태 전이 |
| **행위** | Sequence Diagram | 상호작용 순서 |
| **요구사항** | Requirement Diagram | 요구사항 관계 |
| **파라메트릭** | Parametric Diagram | 제약 조건 |

### 1.3 medini에서 지원하는 SysML

medini analyze는 Safety 분석에 필요한 핵심 SysML 요소를 지원합니다:

```
medini SysML Support
├── Block Definition Diagram (BDD)
│   ├── Block (시스템 요소)
│   ├── Composition/Aggregation
│   └── Properties & Operations
├── Internal Block Diagram (IBD)
│   ├── Part (인스턴스)
│   ├── Port (인터페이스)
│   └── Connector (연결)
└── Safety Extensions
    ├── Failure Mode 정의
    ├── Failure Rate 속성
    └── Safety Requirements 할당
```

---

## 2. Block Definition Diagram (BDD)

### 2.1 BDD 개념

**BDD**는 시스템의 구조적 요소(Block)와 그들 간의 관계를 정의합니다.

```
┌─────────────────────────────────────────────────────────────┐
│                    Block 표기법                              │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  <<block>>                                                  │
│  ┌────────────────────────────────┐                        │
│  │         Block Name             │                        │
│  ├────────────────────────────────┤                        │
│  │ properties                     │                        │
│  │  - attribute1: Type            │                        │
│  │  - attribute2: Type = default  │                        │
│  ├────────────────────────────────┤                        │
│  │ operations                     │                        │
│  │  + operation1()                │                        │
│  │  + operation2(param): Type     │                        │
│  └────────────────────────────────┘                        │
│                                                             │
│  Block Stereotypes:                                         │
│  • <<block>>     : 기본 블록                               │
│  • <<hwBlock>>   : 하드웨어 블록                           │
│  • <<swBlock>>   : 소프트웨어 블록                         │
│  • <<system>>    : 시스템 블록                             │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 2.2 BDD 관계 유형

| 관계 | 기호 | 설명 | 예시 |
|------|------|------|------|
| **Composition** | ◆─── | 강한 포함 (생명주기 동일) | ECU ◆── CPU |
| **Aggregation** | ◇─── | 약한 포함 | System ◇── Sensor |
| **Association** | ─── | 일반 연결 | ECU ── Motor |
| **Generalization** | ─▷ | 상속 | SpecificSensor ─▷ Sensor |
| **Dependency** | ---> | 의존 | SW ---> Config |

### 2.3 BDD 예시 (EPS System)

```
┌─────────────────────────────────────────────────────────────┐
│         bdd [Package] EPS System Architecture               │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│                    <<system>>                               │
│              ┌────────────────┐                            │
│              │   EPS_System   │                            │
│              │ ────────────── │                            │
│              │ -voltage: V    │                            │
│              │ -status: State │                            │
│              └───────┬────────┘                            │
│                      │                                      │
│      ┌───────────────┼───────────────┐                     │
│      │               │               │                      │
│      ◆               ◆               ◆                      │
│      │               │               │                      │
│ ┌────┴────┐    ┌────┴────┐    ┌────┴────┐                │
│ │<<hwBlock>>│  │<<hwBlock>>│  │<<hwBlock>>│                │
│ │  Torque  │   │   ECU    │   │  Motor   │                │
│ │  Sensor  │   │          │   │          │                │
│ ├──────────┤   ├──────────┤   ├──────────┤                │
│ │-torqueVal│   │-swVersion│   │-power: W │                │
│ │-voltage  │   │-hwVersion│   │-position │                │
│ └──────────┘   └────┬─────┘   └──────────┘                │
│                     │                                       │
│              ┌──────┴──────┐                               │
│              ◆             ◆                                │
│        ┌─────┴─────┐ ┌─────┴─────┐                        │
│        │<<swBlock>>│ │<<swBlock>>│                        │
│        │Input_Proc │ │Motor_Ctrl │                        │
│        └───────────┘ └───────────┘                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Internal Block Diagram (IBD)

### 3.1 IBD 개념

**IBD**는 Block 내부의 Part(인스턴스)들과 그들 간의 연결을 상세히 표현합니다.

```
┌─────────────────────────────────────────────────────────────┐
│                    IBD 구성 요소                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Part (Property)                                            │
│  ┌─────────────────────┐                                   │
│  │ partName : BlockType│  ← "이름 : 타입" 형식             │
│  │      ┌────┐         │                                   │
│  │      │port│─────────┼──► Port (인터페이스)              │
│  │      └────┘         │                                   │
│  └─────────────────────┘                                   │
│                                                             │
│  Port Types:                                                │
│  ┌──┐                                                      │
│  │▸ │  Flow Port (방향성) - 신호, 전력, 물질               │
│  └──┘                                                      │
│  ┌──┐                                                      │
│  │□ │  Standard Port - 서비스 인터페이스                   │
│  └──┘                                                      │
│                                                             │
│  Connector: ─────────  (Part 간 연결선)                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 3.2 IBD 예시 (EPS System)

```
┌─────────────────────────────────────────────────────────────┐
│        ibd [Block] EPS_System - Internal Structure          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌───────────────────────────────────────────────────────┐ │
│  │                 eps : EPS_System                       │ │
│  │                                                        │ │
│  │  ┌─────────────┐                   ┌─────────────┐    │ │
│  │  │sensor:Torque│    torque_sig     │  ecu : ECU  │    │ │
│  │  │   Sensor    │───────────────────▸│             │    │ │
│  │  │        [out]▸                   ◂[in]         │    │ │
│  │  └─────────────┘                   │             │    │ │
│  │                                    │             │    │ │
│  │  ┌─────────────┐    motor_cmd      │        [out]▸    │ │
│  │  │motor: Motor │◂──────────────────│             │    │ │
│  │  │             │                   └─────────────┘    │ │
│  │  │◂[cmd]       │                          │           │ │
│  │  │             │                          │           │ │
│  │  │        [fb]▸├──────────────────────────┘           │ │
│  │  └─────────────┘     position_fb                      │ │
│  │                                                        │ │
│  │       ▲                            ▲                   │ │
│  │  [pwr]│                       [can]│                   │ │
│  └───────┼────────────────────────────┼───────────────────┘ │
│          │                            │                     │
│       Power                        CAN Bus                  │
│       Supply                                                │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 4. medini에서 시스템 모델 생성

### 4.1 새 시스템 모델 생성

```
Step 1: 프로젝트에서 System Model 생성
────────────────────────────────────────
Project Explorer → Right-click → New → System Model

Step 2: Package 구조 정의
────────────────────────────────────────
MyProject
└── models
    └── system
        ├── EPS_Architecture.sysml
        └── packages
            ├── Hardware
            └── Software

Step 3: BDD 생성
────────────────────────────────────────
Package → Right-click → New → Block Definition Diagram
```

### 4.2 Block 생성 및 편집

```
┌─────────────────────────────────────────────────────────────┐
│              Block 생성 절차 in medini                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. Palette에서 Block 선택                                  │
│     └── Palette → Blocks → Block                           │
│                                                             │
│  2. Diagram에 배치                                          │
│     └── 원하는 위치에 클릭                                 │
│                                                             │
│  3. Properties 설정                                         │
│     ┌─────────────────────────────────────────────────┐    │
│     │ Properties View                                 │    │
│     ├─────────────────────────────────────────────────┤    │
│     │ Name:        [Torque_Sensor          ]         │    │
│     │ Stereotype:  [<<hwBlock>>            ▼]        │    │
│     │ Description: [토크 센서 모듈          ]         │    │
│     │                                                 │    │
│     │ Attributes:                                     │    │
│     │ ┌─────────────────────────────────────────┐    │    │
│     │ │ + voltage : Real = 5.0                  │    │    │
│     │ │ + torqueValue : Real                    │    │    │
│     │ │ [Add Attribute]                         │    │    │
│     │ └─────────────────────────────────────────┘    │    │
│     └─────────────────────────────────────────────────┘    │
│                                                             │
│  4. Port 추가                                               │
│     └── Block 선택 → Right-click → Add Port               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 4.3 관계 연결

```
Composition 생성:
1. Palette → Relationships → Composition 선택
2. Parent Block에서 시작
3. Child Block에서 종료
4. Multiplicity 설정 (예: 1, 0..1, 1..*)

Association 생성:
1. Palette → Relationships → Association 선택
2. 첫 번째 Block 클릭
3. 두 번째 Block 클릭
4. Association name 및 role 설정
```

---

## 5. Failure Mode 정의

### 5.1 Block에 Failure Mode 추가

medini analyze의 핵심 기능 중 하나는 시스템 Block에 직접 Failure Mode를 정의할 수 있다는 것입니다.

```
┌─────────────────────────────────────────────────────────────┐
│            Block with Failure Modes (medini 확장)           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  <<hwBlock>>                                                │
│  ┌────────────────────────────────────────┐                │
│  │           Torque_Sensor                │                │
│  ├────────────────────────────────────────┤                │
│  │ properties                             │                │
│  │  - voltage: Real = 5.0                 │                │
│  │  - torqueValue: Real                   │                │
│  │  - failureRate: Real = 100 [FIT]       │  ← 고장률     │
│  ├────────────────────────────────────────┤                │
│  │ failure modes              ← medini 확장│                │
│  │  ▪ FM-TS-001: No Output (30%)         │                │
│  │  ▪ FM-TS-002: Stuck High (25%)        │                │
│  │  ▪ FM-TS-003: Stuck Low (25%)         │                │
│  │  ▪ FM-TS-004: Erratic (20%)           │                │
│  ├────────────────────────────────────────┤                │
│  │ ports                                  │                │
│  │  ▸ out: torque_signal                  │                │
│  └────────────────────────────────────────┘                │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 5.2 Failure Mode 속성

| 속성 | 설명 | 예시 |
|------|------|------|
| **Name** | 고장 모드 이름 | No Output |
| **ID** | 고유 식별자 | FM-TS-001 |
| **Description** | 상세 설명 | 센서 출력 없음 |
| **Failure Rate** | 고장률 (FIT) | 30 FIT |
| **Distribution** | 분포 비율 (%) | 30% |
| **Detectable** | 검출 가능 여부 | Yes/No |
| **Detection Method** | 검출 방법 | Range check |

### 5.3 medini에서 Failure Mode 정의

```
┌─────────────────────────────────────────────────────────────┐
│           Failure Mode Editor in medini                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Block: Torque_Sensor                                       │
│  ══════════════════                                         │
│                                                             │
│  Base Failure Rate: [100    ] FIT                          │
│                                                             │
│  Failure Modes:                                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │ ID         │ Name        │ Dist% │ Rate  │ Detect │    │
│  ├────────────┼─────────────┼───────┼───────┼────────┤    │
│  │ FM-TS-001  │ No Output   │ 30%   │ 30 FIT│ Yes    │    │
│  │ FM-TS-002  │ Stuck High  │ 25%   │ 25 FIT│ Yes    │    │
│  │ FM-TS-003  │ Stuck Low   │ 25%   │ 25 FIT│ Yes    │    │
│  │ FM-TS-004  │ Erratic     │ 20%   │ 20 FIT│ No     │    │
│  └────────────┴─────────────┴───────┴───────┴────────┘    │
│                                          Total: 100%        │
│                                                             │
│  [Add]  [Edit]  [Delete]  [Import from Library]            │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 6. Requirements Allocation

### 6.1 Allocation 개념

**Allocation**은 Safety Requirements를 시스템 구현 요소(Block)에 할당하는 것입니다.

```
┌─────────────────────────────────────────────────────────────┐
│              Requirements Allocation Flow                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│       Requirements                    System Blocks         │
│                                                             │
│  ┌─────────────────┐              ┌─────────────────┐      │
│  │    FSR-001      │   allocate   │      ECU        │      │
│  │   [ASIL C]      │─────────────▸│                 │      │
│  └─────────────────┘              └─────────────────┘      │
│                                                             │
│  ┌─────────────────┐              ┌─────────────────┐      │
│  │   TSR-HW-001    │   allocate   │  Torque_Sensor  │      │
│  │   [ASIL B]      │─────────────▸│                 │      │
│  └─────────────────┘              └─────────────────┘      │
│                                                             │
│  ┌─────────────────┐              ┌─────────────────┐      │
│  │   TSR-SW-001    │   allocate   │   Motor_Ctrl    │      │
│  │   [ASIL C]      │─────────────▸│   (Software)    │      │
│  └─────────────────┘              └─────────────────┘      │
│                                                             │
│  결과: Block의 ASIL = 할당된 요구사항 중 최고 ASIL         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 6.2 medini Allocation Matrix

```
┌─────────────────────────────────────────────────────────────┐
│              Allocation Matrix View                         │
├───────────┬────────┬────────┬────────┬─────────┬───────────┤
│           │  ECU   │ Sensor │ Motor  │ SW_Ctrl │ Computed  │
│           │        │        │        │         │ ASIL      │
├───────────┼────────┼────────┼────────┼─────────┼───────────┤
│ FSR-001   │   ●    │        │        │    ●    │           │
│ [ASIL C]  │        │        │        │         │           │
├───────────┼────────┼────────┼────────┼─────────┼───────────┤
│ FSR-002   │        │   ●    │        │         │           │
│ [ASIL B]  │        │        │        │         │           │
├───────────┼────────┼────────┼────────┼─────────┼───────────┤
│ TSR-HW-01 │        │   ●    │   ●    │         │           │
│ [ASIL B]  │        │        │        │         │           │
├───────────┼────────┼────────┼────────┼─────────┼───────────┤
│ TSR-SW-01 │        │        │        │    ●    │           │
│ [ASIL C]  │        │        │        │         │           │
├───────────┼────────┼────────┼────────┼─────────┼───────────┤
│ Block     │   C    │   B    │   B    │    C    │           │
│ Max ASIL  │        │        │        │         │           │
└───────────┴────────┴────────┴────────┴─────────┴───────────┘

● = Allocated
```

### 6.3 Allocation 수행 절차

```
medini에서 Allocation 수행:

1. Allocation View 열기
   └── Window → Show View → Allocation Matrix

2. 행(Row)에 Requirements 배치
   └── Drag & Drop from Requirements Model

3. 열(Column)에 System Blocks 배치
   └── Drag & Drop from System Model

4. 교차점에서 할당 설정
   └── Cell 클릭하여 Allocation 생성

5. ASIL 자동 계산 확인
   └── Block의 ASIL이 자동으로 결정됨
```

---

## 7. Safety Analysis 연계

### 7.1 System Model → FMEA 연계

시스템 모델의 Block과 Failure Mode가 FMEA로 자동 연계됩니다:

```
┌─────────────────────────────────────────────────────────────┐
│            System Model to FMEA Linkage                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  System Model                         FMEA                  │
│  ┌─────────────────┐           ┌────────────────────┐      │
│  │ Torque_Sensor   │           │ FMEA Table         │      │
│  │ ───────────────│           │ ┌────────────────┐ │      │
│  │ FM: No Output   │──────────▸│ │Item: Sensor   │ │      │
│  │ FM: Stuck High  │    FM     │ │FM: No Output  │ │      │
│  │                 │           │ │Effect: ...    │ │      │
│  └─────────────────┘           │ │S: 8, O: 4     │ │      │
│                                │ └────────────────┘ │      │
│                                └────────────────────┘      │
│                                                             │
│  자동 매핑:                                                 │
│  • System Block    →  FMEA Item                            │
│  • Failure Mode    →  FMEA Failure Mode                    │
│  • Block Function  →  FMEA Function                        │
│  • Failure Rate    →  FMEA Occurrence                      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 7.2 System Model → FTA 연계

시스템 구조를 기반으로 FTA의 기본 구조를 도출할 수 있습니다:

```
┌─────────────────────────────────────────────────────────────┐
│             System Model to FTA Linkage                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  System Structure                  FTA                      │
│                                                             │
│       EPS_System                [Top Event]                 │
│           │                    EPS_Failure                  │
│     ┌─────┼─────┐                  │                       │
│     │     │     │            ┌─────┴─────┐                 │
│  Sensor  ECU  Motor         OR                              │
│                        ┌─────┼─────┐                       │
│                     Sensor  ECU   Motor                     │
│                     Fail   Fail   Fail                      │
│                                                             │
│  구조적 경로 분석:                                          │
│  • Composition 관계 → OR Gate 구조                         │
│  • 신호 경로 → Propagation Path                            │
│  • Failure Mode → Basic Event                              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 8. 실습 과제

### 과제 1: BDD 작성

EPS 시스템의 Block Definition Diagram을 작성하세요:

```
요구사항:
1. Top-level Block: EPS_System
2. 하위 Block 구성:
   - Torque_Sensor (<<hwBlock>>)
   - Angle_Sensor (<<hwBlock>>)
   - EPS_ECU (<<hwBlock>>)
   - Assist_Motor (<<hwBlock>>)
3. ECU 내부 SW Block:
   - Input_Processing (<<swBlock>>)
   - Torque_Calculator (<<swBlock>>)
   - Motor_Controller (<<swBlock>>)
4. 적절한 관계(Composition) 설정
```

### 과제 2: IBD 작성

위에서 정의한 BDD를 기반으로 IBD를 작성하세요:

```
요구사항:
1. 각 Part에 Port 정의
2. 신호 연결 (Connector):
   - Torque_Sensor → ECU (torque_signal)
   - Angle_Sensor → ECU (angle_signal)
   - ECU → Motor (motor_command)
   - Motor → ECU (position_feedback)
3. 외부 인터페이스:
   - Power Supply
   - CAN Bus
```

### 과제 3: Failure Mode 정의

각 Block에 Failure Mode를 정의하세요:

```
Torque_Sensor:
- FM-001: No Output (30%)
- FM-002: Stuck High (25%)
- FM-003: Stuck Low (25%)
- FM-004: Erratic Output (20%)

EPS_ECU:
- FM-101: Complete Failure (10%)
- FM-102: Incorrect Calculation (40%)
- FM-103: Communication Loss (30%)
- FM-104: Delayed Response (20%)
```

### 과제 4: Requirements Allocation

다음 Requirements를 적절한 Block에 할당하세요:

| Requirement | ASIL | 할당 대상 Block |
|-------------|------|-----------------|
| FSR-001: Detect sensor fault | B | ? |
| FSR-002: Limit max torque | C | ? |
| TSR-HW-01: Sensor redundancy | B | ? |
| TSR-SW-01: Range validation | C | ? |

---

## 9. 핵심 용어 정리

| 용어 | 정의 |
|------|------|
| **SysML** | 시스템 엔지니어링을 위한 모델링 언어 |
| **Block** | 시스템의 구조적 요소를 나타내는 단위 |
| **BDD** | Block의 정의와 관계를 표현하는 다이어그램 |
| **IBD** | Block 내부 구조와 연결을 표현하는 다이어그램 |
| **Part** | Block의 인스턴스 (IBD에서 사용) |
| **Port** | Block의 상호작용 지점 |
| **Connector** | Part 간의 연결 |
| **Allocation** | 요구사항을 시스템 요소에 할당 |
| **Failure Mode** | 컴포넌트가 실패할 수 있는 방식 |

---

## ✅ 학습 체크리스트

- [ ] SysML의 기본 개념과 다이어그램 유형을 설명할 수 있다
- [ ] BDD에서 Block과 관계를 정의할 수 있다
- [ ] IBD에서 Part, Port, Connector를 사용할 수 있다
- [ ] medini에서 시스템 모델을 생성할 수 있다
- [ ] Block에 Failure Mode를 정의할 수 있다
- [ ] Requirements를 System Block에 할당할 수 있다
- [ ] System Model과 FMEA/FTA의 연계를 이해한다

---

[← 이전 모듈: Safety Goals 및 Requirements](../Module_04_Safety_Goals_Requirements/) | [메인으로 돌아가기](../README.md) | [다음 모듈: FMEA →](../Module_06_FMEA/)
