# Ansys medini analyze 기능안전 분석 교육 과정

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Version](https://img.shields.io/badge/version-1.0-green.svg)
![Standard](https://img.shields.io/badge/standard-ISO%2026262-orange.svg)

## 📋 개요

본 교육 과정은 **Ansys medini analyze**를 활용한 기능안전(Functional Safety) 분석 방법론을 체계적으로 학습하기 위해 설계되었습니다. ISO 26262, IEC 61508 등 국제 기능안전 표준을 기반으로 실무에서 바로 활용할 수 있는 지식과 기술을 습득할 수 있습니다.

## 🎯 교육 목표

- 기능안전의 기본 개념과 ISO 26262 표준 이해
- Ansys medini analyze 도구의 전반적인 기능 숙지
- HARA, FMEA, FTA, FMEDA 등 핵심 분석 기법 실습
- 모델 기반 안전 분석(Model-Based Safety Analysis) 역량 확보
- 실무 프로젝트를 통한 종합적인 기능안전 분석 능력 배양

## 👥 대상 수강자

| 대상 | 권장 선수 지식 |
|------|---------------|
| 자동차 E/E 시스템 개발자 | 기초 전자회로, 임베디드 시스템 |
| 기능안전 엔지니어 | 품질관리, 시스템 엔지니어링 |
| 품질/신뢰성 분석가 | FMEA/FTA 기초 |
| 시스템 아키텍트 | SysML/UML 모델링 |
| 안전 관리자 | 프로젝트 관리, 표준 인증 |

## 📚 커리큘럼 구성

### Phase 1: 기초 이론 (Day 1-2)

| 모듈 | 내용 | 소요 시간 |
|------|------|-----------|
| [Module 01](./Module_01_Functional_Safety_Basics/) | 기능안전 개요 및 ISO 26262 | 4시간 |
| [Module 02](./Module_02_Medini_Introduction/) | Ansys medini analyze 소개 및 환경 설정 | 4시간 |

### Phase 2: 핵심 분석 기법 (Day 3-6)

| 모듈 | 내용 | 소요 시간 |
|------|------|-----------|
| [Module 03](./Module_03_Item_Definition_HARA/) | Item Definition 및 HARA | 4시간 |
| [Module 04](./Module_04_Safety_Goals_Requirements/) | Safety Goals 및 Safety Requirements | 4시간 |
| [Module 05](./Module_05_System_Design_SysML/) | System Design (SysML) | 4시간 |
| [Module 06](./Module_06_FMEA/) | FMEA (Failure Mode and Effects Analysis) | 6시간 |
| [Module 07](./Module_07_FTA/) | FTA (Fault Tree Analysis) | 6시간 |
| [Module 08](./Module_08_FMEDA/) | FMEDA (Failure Modes, Effects and Diagnostic Analysis) | 4시간 |

### Phase 3: 통합 및 실습 (Day 7-8)

| 모듈 | 내용 | 소요 시간 |
|------|------|-----------|
| [Module 09](./Module_09_Traceability_Reports/) | Traceability 및 보고서 생성 | 4시간 |
| [Module 10](./Module_10_Practical_Project/) | 실습 프로젝트 | 8시간 |

**총 교육 시간: 48시간 (6일 과정)**

## 🛠️ 실습 환경 요구사항

### 하드웨어
- CPU: Intel Core i5 이상
- RAM: 16GB 이상 권장
- Storage: 50GB 이상 여유 공간
- Display: 1920x1080 이상 해상도

### 소프트웨어
- OS: Windows 10/11 (64-bit)
- Ansys medini analyze (최신 버전)
- Microsoft Office (보고서 생성용)
- Java Runtime Environment (JRE 11 이상)

## 📊 ISO 26262 V-Model과 medini analyze

```
                    Safety Lifecycle (ISO 26262)
                              │
    ┌─────────────────────────┼─────────────────────────┐
    │                         │                         │
    │  Concept Phase          │          Validation     │
    │  ┌─────────────┐        │        ┌─────────────┐  │
    │  │Item Definition│◄────────────►│Safety Validation│
    │  └──────┬──────┘        │        └──────▲──────┘  │
    │         │               │               │         │
    │  ┌──────▼──────┐        │        ┌──────┴──────┐  │
    │  │    HARA     │◄─────────────►│Safety Assessment│
    │  └──────┬──────┘        │        └──────▲──────┘  │
    │         │               │               │         │
    │  ┌──────▼──────┐        │        ┌──────┴──────┐  │
    │  │Safety Goals │◄─────────────►│ Integration  │  │
    │  └──────┬──────┘        │        │   Testing    │  │
    │         │               │        └──────▲──────┘  │
    │  ┌──────▼──────┐        │        ┌──────┴──────┐  │
    │  │Functional   │◄─────────────►│System Testing │  │
    │  │Safety Concept│       │        └──────▲──────┘  │
    │  └──────┬──────┘        │               │         │
    │         │               │               │         │
    │  ┌──────▼──────┐  ┌─────┴─────┐  ┌──────┴──────┐  │
    │  │Technical    │  │HW/SW     │  │Unit Testing │  │
    │  │Safety Concept│─►│Development│─►│             │  │
    │  └─────────────┘  └───────────┘  └─────────────┘  │
    │                                                   │
    │              medini analyze Coverage              │
    │  ═══════════════════════════════════════════════  │
    │  [HARA] [FTA] [FMEA] [FMEDA] [Traceability]      │
    └───────────────────────────────────────────────────┘
```

## 📁 디렉토리 구조

```
medini-analyze-training/
│
├── README.md                              # 메인 문서 (현재 파일)
│
├── Module_01_Functional_Safety_Basics/    # 기능안전 기초
│   └── README.md
│
├── Module_02_Medini_Introduction/         # medini 소개
│   └── README.md
│
├── Module_03_Item_Definition_HARA/        # Item Definition & HARA
│   └── README.md
│
├── Module_04_Safety_Goals_Requirements/   # Safety Goals & Requirements
│   └── README.md
│
├── Module_05_System_Design_SysML/         # System Design (SysML)
│   └── README.md
│
├── Module_06_FMEA/                        # FMEA
│   └── README.md
│
├── Module_07_FTA/                         # FTA
│   └── README.md
│
├── Module_08_FMEDA/                       # FMEDA
│   └── README.md
│
├── Module_09_Traceability_Reports/        # Traceability & Reports
│   └── README.md
│
└── Module_10_Practical_Project/           # 실습 프로젝트
    └── README.md
```

## 🔗 관련 표준 및 참고 자료

### 기능안전 표준
- **ISO 26262:2018** - Road vehicles — Functional safety
- **IEC 61508** - Functional safety of E/E/PE safety-related systems
- **ISO 21448 (SOTIF)** - Safety of the Intended Functionality
- **ARP 4761** - Guidelines for Aerospace Safety Assessment
- **MIL-STD-882E** - System Safety

### 품질 관련 표준
- **SAE J1739** - FMEA for Design and Manufacturing
- **VDA-AIAG FMEA Handbook** - Automotive Industry FMEA Standards
- **IEC 60812** - Failure Mode and Effects Analysis (FMEA/FMECA)

### Ansys 공식 자료
- [Ansys medini analyze 공식 페이지](https://www.ansys.com/products/safety-analysis/ansys-medini-analyze)
- [Ansys Learning Hub](https://www.ansys.com/academic/learning-resources)
- [Ansys Documentation](https://www.ansys.com/support/documentation)

## ✅ 학습 체크리스트

### 기초 이론
- [ ] 기능안전의 정의와 중요성 이해
- [ ] ISO 26262 표준 구조 파악
- [ ] ASIL 등급 결정 방법 숙지
- [ ] medini analyze 기본 인터페이스 익히기

### 핵심 분석 기법
- [ ] Item Definition 작성 능력
- [ ] HARA 수행 및 Safety Goals 도출
- [ ] Safety Requirements 관리
- [ ] SysML 기반 시스템 모델링
- [ ] FMEA 분석 수행
- [ ] FTA 분석 수행
- [ ] FMEDA 분석 수행

### 통합 역량
- [ ] End-to-End Traceability 구축
- [ ] 보고서 템플릿 커스터마이징
- [ ] 실무 프로젝트 완수

## 📝 평가 기준

| 평가 항목 | 비중 | 설명 |
|-----------|------|------|
| 이론 시험 | 30% | 기능안전 개념 및 표준 이해도 |
| 실습 과제 | 40% | 각 모듈별 실습 과제 수행 |
| 최종 프로젝트 | 30% | 종합 프로젝트 완성도 |

## 📞 문의

교육 과정에 대한 문의사항이 있으시면 아래로 연락해 주세요.

- **Email**: training@example.com
- **Issue**: GitHub Issues 활용

## 📜 라이선스

본 교육 자료는 MIT 라이선스 하에 배포됩니다.

---

**© 2025 Functional Safety Training. All rights reserved.**
