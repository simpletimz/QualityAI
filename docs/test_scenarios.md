# Using the Three Mental Model Techniques

# MODEL 1-- WHAT WHO HOW WHEN
 1. WHAT could go wrong?
   │
   ├── AI generates test cases
   │   that are completely wrong
   │   looks correct but tests
   │   the wrong thing
   │
   ├── AI generates test cases
   │   with broken Python syntax
   │   cannot even run
   │
   ├── AI generates nothing
   │   empty response
   │   timeout
   │
   ├── AI repeats the same
   │   test case 50 times
   │   no variety
   │
   └── AI generates test cases
       for a different feature
       than what was described
       hallucination

2. WHO is affected?
   │
   ├── SDET engineer
   │   wastes hours running
   │   wrong test cases
   │   trusts AI blindly
   │
   ├── Development team
   │   receives false confidence
   │   feature tested incorrectly
   │   bug reaches production
   │
   └── End customer
       uses software with bugs
       that automated testing
       should have caught

3. HOW would we know it went wrong?
   │
   ├── Generated test runs and fails
   │   when feature actually works
   │   false negative detectable ✅
   │
   ├── Generated test runs and passes
   │   when feature is actually broken
   │   false positive very dangerous
   │   harder to detect ⚠️
   │
   ├── Syntax error on execution
   │   immediately visible ✅
   │
   └── Human review of generated cases
       comparing against requirements
       manual verification needed

4. WHEN would it go wrong?
   │
   ├── When description is too vague
   │   garbage in garbage out
   │
   ├── When description is too long
   │   AI loses context
   │   misses important parts
   │
   ├── When system is under load
   │   multiple people generating
   │   at the same time
   │
   └── When phi3 model is
       at edge of its knowledge
       unfamiliar technology stack
       described in the prompt


# BOUNDARY THINKING
The exact limit         → what happens at exactly 100 characters
One below the limit     → 99 characters
One above the limit     → 101 characters
Zero                    → empty input
Negative                → below zero where applicable
Maximum possible        → what is the largest input
Special characters      → quotes, slashes, emoji, spaces
Different languages     → Arabic, Chinese, French input

AI prompt boundaries
├── Empty prompt          → what happens?
├── One word prompt       → too vague to work?
├── 10000 word prompt     → too long, what happens?
├── Prompt in French      → handles other languages?
├── Prompt with SQL       → injection attempt
├── Prompt with HTML      → XSS attempt
└── Prompt asking AI      → to ignore its instructions
    to do something bad     prompt injection attack


# USER JOURNEY MAP
User opens QualityAI
│
├── Sees login or dashboard?
│   └── decision point — test both paths
│
├── Enters feature description
│   └── what format is expected?
│       free text? structured form?
│       test valid and invalid formats
│
├── Clicks generate
│   └── what happens immediately?
│       loading indicator? nothing?
│       test the feedback mechanism
│
├── Waits for AI response
│   └── how long is too long?
│       what if it never responds?
│       test timeout behaviour
│
├── Reviews generated test cases
│   └── can they edit them?
│       can they reject one?
│       test the review workflow
│
├── Saves or exports test cases
│   └── what formats are supported?
│       what if save fails?
│       test the output mechanism
│
└── Runs generated tests
    └── where do results appear?
        how are failures shown?
        test the results display


# QualityAI — Test Scenarios

## Document Information
| Field | Details |
|---|---|
| Project | QualityAI |
| Application | DVWA + QualityAI System |
| Version | 1.0 |
| Author | Moses |
| Date | 2026-05-29 |
| Status | Active |

---

## Testing Mental Model Applied
This document follows the Universal Seven Layer Testing Model:
1. Access — can users get in?
2. Movement — can users navigate?
3. Core Features — does the main function work?
4. Data — is data handled correctly?
5. Connections — do components communicate?
6. Boundaries — what happens at the edges?
7. Quality — how well does it perform?

Each scenario answers four questions:
- WHAT could go wrong?
- WHO is affected?
- HOW would we detect failure?
- WHEN would this surface?

---

## Layer 1 — Access

### TS-001 — Successful login with valid credentials
| Field | Details |
|---|---|
| Scenario ID | TS-001 |
| Layer | 1 — Access |
| Priority | Critical |
| User | All users |

**WHAT** Valid credentials rejected, user cannot enter system.
**WHO** Every user. Login failure blocks all functionality.
**HOW** Login page reloads. No redirect to dashboard. Error shown.
**WHEN** Fresh installation. After system restart. After database restart.

**Scenarios**
- Verify valid username and password redirects to dashboard
- Verify dashboard loads completely after login
- Verify user session created and maintained
- Verify login works consistently after system restart

---

### TS-002 — Login rejected with invalid credentials
| Field | Details |
|---|---|
| Scenario ID | TS-002 |
| Layer | 1 — Access |
| Priority | Critical |
| User | All users |

**WHAT** Invalid credentials accepted, unauthorized access granted.
**WHO** Every user and the business. Security breach if this fails.
**HOW** Dashboard loads despite wrong password. No error shown.
**WHEN** Wrong password entered. Username does not exist. Account disabled.

**Scenarios**
- Verify wrong password shows error message
- Verify non existent username shows error message
- Verify error message does not reveal which field is wrong
- Verify user stays on login page after failed attempt

---

### TS-003 — Login rejected with empty fields
| Field | Details |
|---|---|
| Scenario ID | TS-003 |
| Layer | 1 — Access |
| Priority | High |
| User | All users |

**WHAT** Empty form submitted, system crashes or accepts blank input.
**WHO** All users. Poor validation frustrates legitimate users.
**HOW** System accepts empty login. Error message not shown.
**WHEN** User accidentally clicks login without filling fields.

**Scenarios**
- Verify empty username field shows validation message
- Verify empty password field shows validation message
- Verify both fields empty shows validation message
- Verify form does not submit with empty required fields

---

### TS-004 — Session persists correctly after page refresh
| Field | Details |
|---|---|
| Scenario ID | TS-004 |
| Layer | 1 — Access |
| Priority | High |
| User | All users |

**WHAT** Session lost on refresh, user forced to login repeatedly.
**WHO** All active users. Poor session management destroys usability.
**HOW** User redirected to login page after browser refresh.
**WHEN** User refreshes page. User opens new tab. User navigates back.

**Scenarios**
- Verify logged in user stays logged in after page refresh
- Verify session maintained when opening new tab
- Verify browser back button does not log user out
- Verify session data intact after navigation

---

### TS-005 — Session expires after inactivity timeout
| Field | Details |
|---|---|
| Scenario ID | TS-005 |
| Layer | 1 — Access |
| Priority | High |
| User | All users — security requirement |

**WHAT** Session never expires, leaving accounts vulnerable indefinitely.
**WHO** All users. Permanent sessions are a serious security risk.
**HOW** User inactive for extended period. Session still active.
**WHEN** User leaves workstation unattended. Overnight inactivity.

**Scenarios**
- Verify session expires after defined inactivity period
- Verify expired session redirects to login page
- Verify appropriate message shown on session expiry
- Verify user data not accessible after session expires

---

## Layer 2 — Movement

### TS-006 — Dashboard loads completely after login
| Field | Details |
|---|---|
| Scenario ID | TS-006 |
| Layer | 2 — Movement |
| Priority | Critical |
| User | All users |

**WHAT** Dashboard loads blank, broken, or partially.
**WHO** All users. Broken dashboard blocks all navigation.
**HOW** Missing elements. Console errors. Incomplete rendering.
**WHEN** First login. After cache clear. On slow connection.

**Scenarios**
- Verify dashboard displays all expected components
- Verify navigation menu visible and complete
- Verify no console errors on dashboard load
- Verify dashboard loads within acceptable time

---

### TS-007 — All navigation menu items accessible
| Field | Details |
|---|---|
| Scenario ID | TS-007 |
| Layer | 2 — Movement |
| Priority | High |
| User | All users |

**WHAT** Menu items missing, broken, or leading to wrong pages.
**WHO** All users. Broken navigation makes features unreachable.
**HOW** Clicking menu item loads wrong page or shows error.
**WHEN** After deployment. After UI changes. On different browsers.

**Scenarios**
- Verify every menu item navigates to correct page
- Verify active menu item highlighted correctly
- Verify menu visible on all pages not just dashboard
- Verify menu works on different screen sizes

---

### TS-008 — Invalid URL shows appropriate error page
| Field | Details |
|---|---|
| Scenario ID | TS-008 |
| Layer | 2 — Movement |
| Priority | Medium |
| User | All users |

**WHAT** Invalid URL crashes system or exposes sensitive error details.
**WHO** All users and security team. Stack traces expose system internals.
**HOW** Raw error shown instead of friendly page. System crashes.
**WHEN** User types wrong URL. Old bookmarked link no longer exists.

**Scenarios**
- Verify invalid URL shows friendly 404 page
- Verify 404 page does not expose system information
- Verify 404 page provides navigation back to dashboard
- Verify direct URL access without login redirects to login

---

## Layer 3 — Core Features

### TS-009 — AI generates test cases from feature description
| Field | Details |
|---|---|
| Scenario ID | TS-009 |
| Layer | 3 — Core Features |
| Priority | Critical |
| User | SDET Engineer |

**WHAT** AI generates nothing, wrong content, or broken syntax.
**WHO** SDET engineers. Core value of QualityAI destroyed if this fails.
**HOW** Empty response. Python syntax errors. Irrelevant test cases.
**WHEN** Normal usage. Under load. With vague descriptions.

**Scenarios**
- Verify AI generates test cases from clear description
- Verify generated test cases are valid Python syntax
- Verify generated cases cover happy path scenarios
- Verify generated cases cover negative scenarios
- Verify generated cases are relevant to the description given

---

### TS-010 — AI generated test cases are executable
| Field | Details |
|---|---|
| Scenario ID | TS-010 |
| Layer | 3 — Core Features |
| Priority | Critical |
| User | SDET Engineer |

**WHAT** Generated tests look valid but fail to execute.
**WHO** SDET engineers waste time debugging AI output.
**HOW** pytest reports syntax errors. Import failures. Runtime errors.
**WHEN** Complex feature descriptions. Unfamiliar technology in prompt.

**Scenarios**
- Verify generated test file runs without syntax errors
- Verify generated tests import correctly
- Verify generated tests follow pytest conventions
- Verify generated tests produce pass or fail result

---

### TS-011 — RAG pipeline answers questions from uploaded documents
| Field | Details |
|---|---|
| Scenario ID | TS-011 |
| Layer | 3 — Core Features |
| Priority | High |
| User | SDET Engineer, QA Manager |

**WHAT** RAG returns wrong answers, ignores document, or hallucinates.
**WHO** Anyone relying on document Q&A for accurate information.
**HOW** Answer contradicts document content. Answer is completely fabricated.
**WHEN** Large documents uploaded. Questions about specific details.

**Scenarios**
- Verify RAG answers question correctly from uploaded document
- Verify RAG answer references document content not general knowledge
- Verify RAG responds when answer is not in document
- Verify RAG handles multiple documents correctly

---

### TS-012 — Bug analyzer identifies patterns in test failures
| Field | Details |
|---|---|
| Scenario ID | TS-012 |
| Layer | 3 — Core Features |
| Priority | High |
| User | SDET Engineer, QA Manager |

**WHAT** Analyzer misidentifies patterns or produces no insights.
**WHO** QA teams making decisions based on incorrect analysis.
**HOW** Known pattern not detected. False patterns reported.
**WHEN** Small failure datasets. Similar but different failure types.

**Scenarios**
- Verify analyzer correctly identifies recurring failure pattern
- Verify analyzer highlights highest risk areas accurately
- Verify analyzer handles small datasets gracefully
- Verify analyzer output is readable and actionable

---

## Layer 4 — Data

### TS-013 — Generated test cases save and persist correctly
| Field | Details |
|---|---|
| Scenario ID | TS-013 |
| Layer | 4 — Data |
| Priority | High |
| User | SDET Engineer |

**WHAT** Generated tests lost after save. Data corrupted on storage.
**WHO** Engineers lose work. Must regenerate wasting time.
**HOW** Saved test not found on reload. Content different from saved.
**WHEN** After system restart. After session timeout. Storage full.

**Scenarios**
- Verify generated test cases save successfully
- Verify saved tests retrievable after session ends
- Verify saved tests retrievable after system restart
- Verify saved content identical to original generated content

---

### TS-014 — Data deleted completely when user requests deletion
| Field | Details |
|---|---|
| Scenario ID | TS-014 |
| Layer | 4 — Data |
| Priority | High |
| User | All users |

**WHAT** Deleted data still accessible or partially remaining.
**WHO** All users. Privacy violation if deleted data persists.
**HOW** Deleted item still appears in list. Still retrievable via direct URL.
**WHEN** After delete action confirmed. After bulk delete.

**Scenarios**
- Verify deleted test case no longer appears in list
- Verify deleted data not retrievable via direct access
- Verify confirmation prompt shown before deletion
- Verify deletion cannot be triggered accidentally

---

## Layer 5 — Connections

### TS-015 — Ollama responds within acceptable timeout
| Field | Details |
|---|---|
| Scenario ID | TS-015 |
| Layer | 5 — Connections |
| Priority | Critical |
| User | All users |

**WHAT** Ollama unresponsive. System hangs indefinitely waiting.
**WHO** All users. Unresponsive system feels broken.
**HOW** Request sent. No response. No timeout message. UI frozen.
**WHEN** Ollama service crashed. High system load. Large prompts.

**Scenarios**
- Verify system shows loading indicator while waiting for Ollama
- Verify request times out after defined period
- Verify meaningful error shown when Ollama unavailable
- Verify system recovers when Ollama comes back online

---

### TS-016 — Jenkins pipeline triggers and runs tests automatically
| Field | Details |
|---|---|
| Scenario ID | TS-016 |
| Layer | 5 — Connections |
| Priority | High |
| User | SDET Engineer, Development Team |

**WHAT** Jenkins does not trigger. Tests do not run automatically.
**WHO** Development team. Manual testing required defeating CI/CD purpose.
**HOW** Code pushed. Jenkins shows no new build triggered.
**WHEN** After configuration change. After Jenkins restart. Network issues.

**Scenarios**
- Verify Jenkins triggers build on code push
- Verify Jenkins runs full test suite automatically
- Verify Jenkins publishes Allure report after run
- Verify Jenkins notifies team of pass or fail result

---

### TS-017 — System recovers gracefully from database connection loss
| Field | Details |
|---|---|
| Scenario ID | TS-017 |
| Layer | 5 — Connections |
| Priority | High |
| User | All users |

**WHAT** Database drops. System crashes with no recovery.
**WHO** All active users lose work. System requires manual restart.
**HOW** Database stopped mid session. System crashes or hangs.
**WHEN** Database container restarted. Network interruption. Storage full.

**Scenarios**
- Verify system shows error when database unavailable
- Verify system does not crash on database disconnection
- Verify system reconnects automatically when database returns
- Verify no data corruption during connection loss

---

## Layer 6 — Boundaries

### TS-018 — Empty prompt handled gracefully
| Field | Details |
|---|---|
| Scenario ID | TS-018 |
| Layer | 6 — Boundaries |
| Priority | High |
| User | SDET Engineer |

**WHAT** Empty prompt crashes AI engine or returns confusing output.
**WHO** Engineers accidentally submitting empty forms.
**HOW** System crashes. Confusing error. Infinite loading.
**WHEN** User clicks generate without entering description.

**Scenarios**
- Verify empty prompt shows validation message
- Verify generate button disabled or blocked with empty input
- Verify system does not call Ollama with empty prompt
- Verify clear guidance shown to help user fill input

---

### TS-019 — Extremely long prompt handled without system failure
| Field | Details |
|---|---|
| Scenario ID | TS-019 |
| Layer | 6 — Boundaries |
| Priority | High |
| User | SDET Engineer |

**WHAT** Very long prompt crashes system or exhausts memory.
**WHO** Engineers pasting large requirement documents as prompts.
**HOW** System freezes. Memory error. Browser crashes.
**WHEN** User pastes entire requirements document into prompt field.

**Scenarios**
- Verify system handles prompt of 5000 words without crashing
- Verify system shows character limit if one exists
- Verify system truncates or rejects gracefully if limit exceeded
- Verify memory usage stays within acceptable range

---

### TS-020 — Special characters in prompt handled safely
| Field | Details |
|---|---|
| Scenario ID | TS-020 |
| Layer | 6 — Boundaries |
| Priority | Critical |
| User | SDET Engineer — Security |

**WHAT** Special characters cause injection attacks or system errors.
**WHO** All users. Security risk if injection succeeds.
**HOW** SQL injection via prompt. XSS via prompt. Prompt injection.
**WHEN** User includes code snippets in description. Malicious input.

**Scenarios**
- Verify SQL characters in prompt do not affect database
- Verify HTML tags in prompt rendered as text not executed
- Verify prompt injection attempt does not override AI behaviour
- Verify special characters stored and displayed correctly

---

### TS-021 — Concurrent users handled without data leakage
| Field | Details |
|---|---|
| Scenario ID | TS-021 |
| Layer | 6 — Boundaries |
| Priority | Critical |
| User | All users — Privacy |

**WHAT** User A sees User B session data. Cross session contamination.
**WHO** All users. Catastrophic privacy violation.
**HOW** User A prompt appears in User B results. Sessions mixed.
**WHEN** Multiple users active simultaneously. High load periods.

**Scenarios**
- Verify User A results never visible to User B
- Verify sessions completely isolated from each other
- Verify simultaneous requests do not mix responses
- Verify system queues concurrent requests fairly

---

## Layer 7 — Quality

### TS-022 — System operates completely offline
| Field | Details |
|---|---|
| Scenario ID | TS-022 |
| Layer | 7 — Quality |
| Priority | Critical |
| User | All users — Core requirement |

**WHAT** System phones home. Features fail without internet.
**WHO** All users especially in air gapped environments.
**HOW** Network monitor shows outbound requests. Features fail offline.
**WHEN** Internet disconnected. Firewall blocks outbound traffic.

**Scenarios**
- Verify all features work with internet disconnected
- Verify network monitor shows zero outbound requests during use
- Verify Ollama operates without internet after model downloaded
- Verify no telemetry or analytics sent to external servers

---

### TS-023 — AI response time meets performance benchmark
| Field | Details |
|---|---|
| Scenario ID | TS-023 |
| Layer | 7 — Quality |
| Priority | High |
| User | All users |

**WHAT** AI response too slow. Users abandon tool.
**WHO** All users. Slow tools get replaced.
**HOW** Response time measured and exceeds 10 seconds consistently.
**WHEN** Normal usage. After extended use. Under multiple requests.

**Scenarios**
- Verify AI responds within 10 seconds for standard prompt
- Verify response time consistent across multiple requests
- Verify system does not degrade after 1 hour continuous use
- Verify loading indicator shown during any wait over 2 seconds

---

### TS-024 — System runs stably for extended period
| Field | Details |
|---|---|
| Scenario ID | TS-024 |
| Layer | 7 — Quality |
| Priority | High |
| User | All users |

**WHAT** Memory leak causes system to slow down and crash over time.
**WHO** All users. Unreliable system cannot be trusted.
**HOW** System slows progressively. Eventually crashes or becomes unusable.
**WHEN** After 4 hours continuous use. Overnight runs. Extended sessions.

**Scenarios**
- Verify system stable after 4 hours continuous use
- Verify memory usage does not grow unbounded over time
- Verify system recovers automatically from unexpected errors
- Verify no data loss during extended operation

---

## Scenario Summary

| Layer | Scenarios | Count |
|---|---|---|
| Layer 1 Access | TS-001 to TS-005 | 5 |
| Layer 2 Movement | TS-006 to TS-008 | 3 |
| Layer 3 Core Features | TS-009 to TS-012 | 4 |
| Layer 4 Data | TS-013 to TS-014 | 2 |
| Layer 5 Connections | TS-015 to TS-017 | 3 |
| Layer 6 Boundaries | TS-018 to TS-021 | 4 |
| Layer 7 Quality | TS-022 to TS-024 | 3 |
| **Total** | | **24** |

---

## Traceability

Every scenario traces back to a business driver:

| Business Driver | Covered By |
|---|---|
| Privacy and confidentiality | TS-021, TS-022 |
| Offline operation | TS-022 |
| AI accuracy | TS-009, TS-010, TS-011, TS-012 |
| System reliability | TS-015, TS-017, TS-024 |
| Security | TS-002, TS-005, TS-020, TS-021 |
| Performance | TS-023, TS-024 |
| Usability | TS-003, TS-006, TS-007, TS-008 |