# QualityAI — Manual Test Cases

## Document Information
| Field | Details |
|---|---|
| Project | QualityAI |
| Application | DVWA |
| Version | 1.0 |
| Author | Moses |
| Date | 2026-05-29 |
| Status | In Progress |

---

## Module 1 — Authentication

### TC-001 — Login with valid credentials
| Field | Details |
|---|---|
| Test Case ID | TC-001 |
| Module | Authentication |
| Priority | Critical |
| Status | Pass |

**Preconditions**
- DVWA is running and accessible
- Valid account exists (admin/password)

**Test Steps**
1. Navigate to http://192.168.127.129/login.php
2. Enter username: admin
3. Enter password: password
4. Click Login button

**Expected Result**
User is redirected to DVWA dashboard. Welcome message displays.

**Actual Result**
User redirected to dashboard successfully.

---

### TC-002 — Login with invalid credentials
| Field | Details |
|---|---|
| Test Case ID | TC-002 |
| Module | Authentication |
| Priority | Critical |
| Status | Pass |

**Preconditions**
- DVWA is running and accessible

**Test Steps**
1. Navigate to http://192.168.127.129/login.php
2. Enter username: admin
3. Enter password: wrongpassword
4. Click Login button

**Expected Result**
Login fails. Error message displays. User stays on login page.

**Actual Result**
Error message displayed. User remained on login page.

---

### TC-003 — Login with empty credentials
| Field | Details |
|---|---|
| Test Case ID | TC-003 |
| Module | Authentication |
| Priority | High |
| Status | Pending |

**Preconditions**
- DVWA is running and accessible

**Test Steps**
1. Navigate to http://192.168.127.129/login.php
2. Leave username empty
3. Leave password empty
4. Click Login button

**Expected Result**
Login fails. Validation message prompts user to fill in fields.

**Actual Result**
Pending execution.

---

### TC-004 — Login with SQL injection in username
| Field | Details |
|---|---|
| Test Case ID | TC-004 |
| Module | Authentication / Security |
| Priority | Critical |
| Status | Pass |

**Preconditions**
- DVWA is running
- Security level set to Low

**Test Steps**
1. Navigate to http://192.168.127.129/login.php
2. Enter username: admin' OR '1'='1
3. Enter any password
4. Click Login button

**Expected Result**
Login should fail. Application should not be bypassable via SQL injection.

**Actual Result**
Further investigation required. Document findings.

---

## Module 2 — SQL Injection

### TC-005 — SQL injection with single quote
| Field | Details |
|---|---|
| Test Case ID | TC-005 |
| Module | SQL Injection |
| Priority | Critical |
| Status | Pass |

**Preconditions**
- DVWA is running
- Security level set to Low
- User logged in as admin

**Test Steps**
1. Navigate to SQL Injection page
2. Enter 1' in the User ID field
3. Click Submit

**Expected Result**
Secure application should sanitize input and show no error.

**Actual Result**
Database error exposed. Confirms SQL injection vulnerability exists.

**Bug Raised**
BUG-001 — SQL injection vulnerability on User ID field.

---

### TC-006 — SQL injection data extraction
| Field | Details |
|---|---|
| Test Case ID | TC-006 |
| Module | SQL Injection |
| Priority | Critical |
| Status | Pass |

**Preconditions**
- TC-005 passed confirming vulnerability
- User logged in as admin

**Test Steps**
1. Navigate to SQL Injection page
2. Enter: 1' UNION SELECT user, password FROM users#
3. Click Submit

**Expected Result**
Secure application should block this query.

**Actual Result**
All usernames and MD5 password hashes extracted from database.

**Bug Raised**
BUG-002 — Full credential extraction via UNION based SQL injection.

---

## Module 3 — XSS Cross Site Scripting

### TC-007 — Reflected XSS with script tag
| Field | Details |
|---|---|
| Test Case ID | TC-007 |
| Module | XSS Reflected |
| Priority | Critical |
| Status | Pass |

**Preconditions**
- DVWA is running
- Security level set to Low
- User logged in

**Test Steps**
1. Navigate to XSS Reflected page
2. Enter: <script>alert('XSS')</script>
3. Click Submit

**Expected Result**
Input should be sanitized. No popup should appear.

**Actual Result**
JavaScript executed. Alert popup appeared confirming XSS vulnerability.

**Bug Raised**
BUG-003 — Reflected XSS vulnerability on name input field.

---

### TC-008 — Stored XSS via guestbook
| Field | Details |
|---|---|
| Test Case ID | TC-008 |
| Module | XSS Stored |
| Priority | Critical |
| Status | Pass |

**Preconditions**
- DVWA is running
- Security level set to Low
- User logged in

**Test Steps**
1. Navigate to XSS Stored page
2. Enter Name: Moses
3. Enter Message: <script>alert('Stored XSS')</script>
4. Click Sign Guestbook
5. Reload the page

**Expected Result**
Script should be stored as plain text. No execution on reload.

**Actual Result**
Script executed on every page load. Persistent XSS confirmed.

**Bug Raised**
BUG-004 — Stored XSS vulnerability persists across page reloads.

---

## Module 4 — Brute Force

### TC-009 — Brute force login with wordlist
| Field | Details |
|---|---|
| Test Case ID | TC-009 |
| Module | Brute Force |
| Priority | High |
| Status | Pass |

**Preconditions**
- DVWA is running
- Security level set to Low
- Hydra installed on Kali

**Test Steps**
1. Open Kali terminal
2. Run Hydra with rockyou.txt wordlist
3. Target DVWA brute force page
4. Observe results

**Expected Result**
Application should lock account after multiple failed attempts.

**Actual Result**
Password cracked successfully. No account lockout implemented.

**Bug Raised**
BUG-005 — No brute force protection on login form.

---

## Module 5 — Navigation

### TC-010 — Dashboard loads after login
| Field | Details |
|---|---|
| Test Case ID | TC-010 |
| Module | Navigation |
| Priority | High |
| Status | Pending |

**Preconditions**
- User logged in successfully

**Test Steps**
1. Complete successful login
2. Observe page that loads
3. Check all menu items are visible

**Expected Result**
Dashboard loads with full left navigation menu visible.

**Actual Result**
Pending execution.

---

## Bug Summary

| Bug ID | Title | Severity | Status |
|---|---|---|---|
| BUG-001 | SQL injection on User ID field | Critical | Open |
| BUG-002 | Full credential extraction via SQL injection | Critical | Open |
| BUG-003 | Reflected XSS on name field | Critical | Open |
| BUG-004 | Stored XSS persists on reload | Critical | Open |
| BUG-005 | No brute force protection on login | High | Open |

---

## Test Execution Summary

| Total | Passed | Failed | Pending |
|---|---|---|---|
| 10 | 6 | 0 | 4 |