# Author
**Moses Opaleye** — RESEARCH ENGINEER
GitHub: [@SimpleTimz](https://github.com/SimpleTimz)  
Project: [QualityAI](https://github.com/SimpleTimz/QualityAI)

# QualityAI — Project Brief

## 1. Project Title
QualityAI — Intelligent Test Automation Suite

## 2. Project Overview
QualityAI is a production-grade test automation suite built by an SDET engineer that integrates artificial intelligence to enhance software quality assurance. The project targets web applications and combines traditional test automation with locally hosted AI models to create a smarter, faster, and more comprehensive testing solution.

## 3. Problem Statement
Modern software development moves faster than manual testing can keep up with. QA teams face three core problems:

- **Speed** — Manual testing cannot match CI/CD deployment frequency
- **Coverage** — Human testers miss edge cases and regression bugs
- **Intelligence** — Traditional automation cannot learn from past failures or predict future ones

QualityAI solves all three by combining automation with AI running entirely offline.

## 4. Objectives
- Build a complete automated test suite covering UI, API, and performance testing
- Integrate a local LLM to generate test cases automatically from requirements
- Build an intelligent bug analyzer that identifies patterns in test failures
- Create a RAG pipeline that answers questions about project documentation
- Demonstrate full CI/CD integration via Jenkins
- Maintain complete offline operation for privacy sensitive environments

## 5. Scope

### In Scope
- UI test automation using Selenium against DVWA web application
- API test automation using Requests library
- AI powered test case generation using Ollama phi3
- Bug pattern analysis using scikit-learn
- Local RAG pipeline for documentation querying
- Jenkins CI/CD pipeline configuration
- Allure test reporting
- MySQL and PostgreSQL database test scenarios

### Out of Scope
- Mobile application testing
- Cloud based AI APIs (intentionally excluded for privacy)
- Load testing at enterprise scale
- Security penetration testing automation

## 6. Tech Stack

### Testing Layer
| Tool | Version | Purpose |
|---|---|---|
| Python | 3.12 | Primary language |
| pytest | 9.0.3 | Test framework |
| Selenium | 4.44 | Browser automation |
| Requests | 2.34 | API testing |
| Allure | 2.16 | Test reporting |

### AI/ML Layer
| Tool | Version | Purpose |
|---|---|---|
| Ollama | Latest | Local LLM runtime |
| phi3 | Latest | AI model for generation |
| scikit-learn | 1.8.0 | ML pattern recognition |
| MLflow | 3.12 | Experiment tracking |
| pandas | 2.3.3 | Data analysis |

### Infrastructure Layer
| Tool | Version | Purpose |
|---|---|---|
| Docker | Latest | Containerization |
| Jenkins | LTS | CI/CD pipeline |
| MySQL | 8.0 | Relational database |
| PostgreSQL | 15 | Advanced database |
| Git | 2.43 | Version control |

## 7. Project Structure
QualityAI/
├── ai_engine/
│   ├── test_generator/     → AI generates test cases from requirements
│   ├── bug_analyzer/       → ML pattern recognition on failures
│   └── RAG_pipeline/       → offline document Q&A system
├── data/                   → SQL scripts and test datasets
├── docs/                   → all project documentation
├── jenkins/                → Jenkinsfile and pipeline configs
├── reports/                → Allure generated test reports
├── tests/
│   ├── ui_tests/           → Selenium browser automation
│   ├── api_tests/          → REST API test cases
│   ├── performance_tests/  → load and stress tests
│   └── ai/                 → AI generated test cases
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt

## 8. Testing Strategy

### Level 1 — Manual Testing
All test scenarios documented manually first in docs/manual_test_cases.md before any automation is written. This ensures complete understanding of expected behaviour before code is created.

### Level 2 — Automated UI Testing
Selenium tests cover all critical user journeys including login, navigation, form submission, and error handling.

### Level 3 — Automated API Testing
Requests library tests cover all API endpoints for correct responses, error handling, authentication, and data validation.

### Level 4 — AI Augmented Testing
Local LLM generates additional test cases from feature descriptions. ML model predicts which areas of the application are highest risk based on historical failure data.

### Level 5 — CI/CD Integration
Jenkins pipeline runs full test suite automatically on every code change and publishes Allure reports.

## 9. AI/ML Integration Plan

### Test Case Generation
Developer writes feature description
└── AI reads description
generates test cases automatically
covering happy path
edge cases
negative scenarios
saved as pytest test files

### Bug Pattern Analysis
Test failures collected over time
└── ML model trained on failure data
predicts which features
are highest risk
QA focuses effort intelligently

### RAG Pipeline
Project documentation uploaded
└── AI answers questions about
the project without internet
completely private
useful for onboarding
and test planning

## 10. Timeline

| Phase | Description | Status |
|---|---|---|
| Phase 1 | Project setup and documentation | ✅ Complete |
| Phase 2 | Manual test cases | 🔄 In Progress |
| Phase 3 | UI test automation | ⏳ Pending |
| Phase 4 | API test automation | ⏳ Pending |
| Phase 5 | AI engine development | ⏳ Pending |
| Phase 6 | Jenkins CI/CD integration | ⏳ Pending |
| Phase 7 | Reporting and documentation | ⏳ Pending |

## 11. Success Criteria
- All critical user journeys covered by automated tests
- Test suite runs end to end without manual intervention
- AI successfully generates valid test cases from descriptions
- Jenkins pipeline triggers and runs tests automatically
- Allure reports generated and readable after each run
- Complete documentation available for every component
- Project reproducible on any machine using requirements.txt
