# QualityAI — Test Plan

## 1. Introduction
This test plan defines the testing strategy, scope, approach, and schedule for the QualityAI project. It serves as the master reference document for all testing activities carried out on the DVWA web application using automated and AI-assisted testing techniques.

## 2. Test Objectives
- Verify all critical user journeys function correctly
- Identify defects before they reach production
- Ensure application security controls are functioning
- Validate API endpoints return correct responses
- Confirm database operations execute accurately
- Demonstrate AI augmented testing capabilities

## 3. Scope

### In Scope
- Login and authentication functionality
- Navigation and page loading
- Form submission and validation
- SQL injection vulnerability detection
- XSS vulnerability detection
- Brute force protection validation
- API endpoint testing
- Database read and write operations

### Out of Scope
- Mobile browser testing
- Internet Explorer compatibility
- Third party integrations
- Payment processing
- Email notification testing

## 4. Test Levels

### 4.1 Unit Testing
What        → individual Python functions
Who         → developer during coding
Tools       → pytest
When        → before every commit

### 4.2 Integration Testing
What        → components working together
Selenium + browser + DVWA
Python + database
Who         → SDET engineer
Tools       → pytest, Selenium, Requests
When        → after each feature is built

### 4.3 End to End Testing
What        → complete user journeys
from login to task completion
Who         → SDET engineer
Tools       → Selenium, pytest
When        → before every release

### 4.4 Security Testing
What        → vulnerability detection
SQL injection, XSS, brute force
Who         → SDET engineer
Tools       → Selenium, Requests, pytest
When        → dedicated security test runs

### 4.5 Performance Testing
What        → response times under load
Who         → SDET engineer
Tools       → k6, pytest
When        → milestone releases

## 5. Test Approach

### Manual Testing First
Every feature is tested manually and documented in manual_test_cases.md before automation is written. This ensures the tester fully understands expected behaviour before writing code.

### Automation Second
Manual test cases are converted into automated pytest and Selenium scripts. Automation covers regression testing so manual effort focuses on exploratory testing.

### AI Augmentation Third
Local LLM generates additional test cases from feature descriptions. ML model identifies highest risk areas based on historical failure patterns.

## 6. Test Environment

### Application Under Test
| Component | Details |
|---|---|
| Application | DVWA (Damn Vulnerable Web Application) |
| URL | http://192.168.127.129 |
| Hosting | Docker container inside Ubuntu VM |
| Database | MariaDB inside DVWA container |
| Security Level | Low (for testing purposes) |

### Testing Machine
| Component | Details |
|---|---|
| OS | Ubuntu 24.04 |
| Python | 3.12 |
| Browser | Chrome via Selenium |
| Framework | pytest 9.0.3 |
| AI Engine | Ollama phi3 (local) |

### CI/CD Environment
| Component | Details |
|---|---|
| Pipeline | Jenkins LTS |
| URL | http://192.168.95.129:8080 |
| Trigger | Manual and scheduled |
| Reports | Allure |

## 7. Test Types and Tools

| Test Type | Tool | Location |
|---|---|---|
| UI Automation | Selenium + pytest | tests/ui_tests/ |
| API Testing | Requests + pytest | tests/api_tests/ |
| Performance | k6 | tests/performance_tests/ |
| AI Generated | Ollama + pytest | tests/ai/ |
| Reporting | Allure | reports/ |

## 8. Entry and Exit Criteria

### Entry Criteria
These conditions must be met before testing begins:
- Application is deployed and accessible
- Test environment is configured
- Test data is prepared
- Test cases are reviewed and approved

### Exit Criteria
Testing is complete when:
- All critical test cases executed
- No critical or high severity bugs open
- Test coverage meets agreed threshold
- Allure report generated and reviewed
- All findings documented

## 9. Bug Classification

| Severity | Definition | Example |
|---|---|---|
| Critical | System unusable, data loss | Login completely broken |
| High | Major feature broken | Form submission fails |
| Medium | Feature works with workaround | Error message unclear |
| Low | Minor cosmetic issue | Button misaligned |

## 10. Bug Reporting Process
Tester finds bug
└── Documents in bug_report_template.md
├── Unique ID assigned
├── Steps to reproduce written
├── Expected vs actual result
├── Severity assigned
├── Screenshot attached
└── Logged in tracking system

## 11. Risks and Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| DVWA container stops | Tests cannot run | restart unless-stopped policy set |
| IP address changes | Tests fail | store IP in config file |
| Browser version mismatch | Selenium fails | pin ChromeDriver version |
| AI model unavailable | Generation fails | fallback to manual test cases |

## 12. Deliverables

| Deliverable | Description | Location |
|---|---|---|
| Test Plan | This document | docs/test_plan.md |
| Manual Test Cases | Hand written scenarios | docs/manual_test_cases.md |
| Automated Tests | pytest and Selenium code | tests/ |
| Test Reports | Allure HTML reports | reports/ |
| Bug Reports | Documented defects | docs/bug_reports/ |

## 13. Approval
| Role | Name | Status |
|---|---|---|
| SDET Engineer | Moses | ✅ Approved |
| Reviewer | Pending | ⏳ |

## 14. Document History
| Version | Date | Changes |
|---|---|---|
| 1.0 | 2026-05-29 | Initial version created |