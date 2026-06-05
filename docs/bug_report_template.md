# QualityAI — Bug Report Template

## Document Information
| Field | Details |
|---|---|
| Project | QualityAI |
| Application | DVWA |
| Template Version | 1.0 |
| Author | Moses |
| Date | 2026-05-29 |

---

## How to Use This Template
Copy the bug report section below for every new bug found.
Replace all placeholder text with actual findings.
Assign a unique Bug ID following the format BUG-XXX.
Attach screenshots where possible.

---

## Bug Report

### Bug ID: BUG-XXX
### Title: One line summary of the bug

| Field | Details |
|---|---|
| Bug ID | BUG-XXX |
| Title | Short descriptive title |
| Reported By | Moses |
| Date Reported | YYYY-MM-DD |
| Application | DVWA |
| Module | Which feature area |
| Severity | Critical / High / Medium / Low |
| Priority | High / Medium / Low |
| Status | Open / In Progress / Fixed / Closed |
| Environment | Ubuntu 24.04, Chrome, DVWA Low Security |

---

### Description
Clear description of what the bug is and what impact it has on the application or user.

---

### Steps to Reproduce
1. Step one — exact action taken
2. Step two — exact action taken
3. Step three — exact action taken
4. Continue until bug appears

---

### Expected Result
What should have happened if the application was working correctly.

---

### Actual Result
What actually happened. Be specific and precise.

---

### Evidence
- Screenshot 1: description of what it shows
- Screenshot 2: description of what it shows
- Log output if available

---

### Root Cause (if known)
Technical explanation of why the bug exists.
Leave blank if unknown — developer investigates.

---

### Recommended Fix
Suggested solution from QA perspective.
Leave blank if unknown.

---

### Severity Guide

| Severity | Definition | Examples |
|---|---|---|
| Critical | System unusable, data loss, security breach | Login broken, data exposed, crash on launch |
| High | Major feature broken, no workaround | Form submission fails, page does not load |
| Medium | Feature works but with issues, workaround exists | Wrong error message, slow response |
| Low | Minor cosmetic or UI issue | Typo, misaligned button, wrong colour |

---

## Completed Bug Reports

---

### Bug ID: BUG-001
### Title: SQL injection vulnerability on User ID field exposes database errors

| Field | Details |
|---|---|
| Bug ID | BUG-001 |
| Title | SQL injection vulnerability on User ID field |
| Reported By | Moses |
| Date Reported | 2026-05-29 |
| Application | DVWA |
| Module | SQL Injection |
| Severity | Critical |
| Priority | High |
| Status | Open |
| Environment | Ubuntu 24.04, Firefox, DVWA Low Security |

**Description**
The User ID input field on the SQL Injection page accepts unsanitized input. When a single quote is entered the application returns a raw database error message exposing the database type, version, and query structure to the attacker.

**Steps to Reproduce**
1. Navigate to http://192.168.127.129/vulnerabilities/sqli/
2. Enter 1' in the User ID field
3. Click Submit

**Expected Result**
Application sanitizes input. Generic error shown or input rejected. No database information exposed.

**Actual Result**
Raw MariaDB error returned to browser confirming SQL injection vulnerability and exposing database internals.

**Evidence**
- Screenshot: MariaDB syntax error displayed in browser
- Error text: You have an error in your SQL syntax near ''1'''

**Root Cause**
User input passed directly to SQL query without parameterization or sanitization.

**Recommended Fix**
Implement parameterized queries. Never concatenate user input directly into SQL statements.

---

### Bug ID: BUG-002
### Title: Full credential extraction possible via UNION based SQL injection

| Field | Details |
|---|---|
| Bug ID | BUG-002 |
| Title | Full credential extraction via UNION SQL injection |
| Reported By | Moses |
| Date Reported | 2026-05-29 |
| Application | DVWA |
| Module | SQL Injection |
| Severity | Critical |
| Priority | High |
| Status | Open |
| Environment | Ubuntu 24.04, Firefox, DVWA Low Security |

**Description**
Building on BUG-001, the SQL injection vulnerability allows an attacker to extract all usernames and password hashes from the database using a UNION SELECT statement. All five user accounts were successfully extracted.

**Steps to Reproduce**
1. Navigate to http://192.168.127.129/vulnerabilities/sqli/
2. Enter: 1' UNION SELECT user, password FROM users#
3. Click Submit

**Expected Result**
Query blocked. No unauthorized data returned.

**Actual Result**
All five usernames and MD5 password hashes returned. Hashes subsequently cracked using SQLMap built in wordlist.

**Evidence**
- Screenshot: All user credentials displayed in browser
- Tool used: Manual injection confirmed by SQLMap automated scan

**Root Cause**
No input sanitization. Database user has excessive SELECT privileges across all tables.

**Recommended Fix**
Parameterized queries. Database user should have minimal privileges. Passwords should use bcrypt not MD5.

---

### Bug ID: BUG-003
### Title: Reflected XSS vulnerability on name input field executes JavaScript

| Field | Details |
|---|---|
| Bug ID | BUG-003 |
| Title | Reflected XSS on name input field |
| Reported By | Moses |
| Date Reported | 2026-05-29 |
| Application | DVWA |
| Module | XSS Reflected |
| Severity | Critical |
| Priority | High |
| Status | Open |
| Environment | Ubuntu 24.04, Firefox, DVWA Low Security |

**Description**
The name input field on the XSS Reflected page does not sanitize HTML or JavaScript input. An attacker can inject a script tag that executes in the victim's browser.

**Steps to Reproduce**
1. Navigate to XSS Reflected page
2. Enter: <script>alert('XSS')</script>
3. Click Submit

**Expected Result**
Input sanitized. Script tag displayed as plain text or rejected entirely.

**Actual Result**
JavaScript executed immediately. Alert popup appeared confirming script injection.

**Evidence**
- Screenshot: Alert popup displaying XSS in browser

**Root Cause**
User input rendered directly in HTML response without encoding or sanitization.

**Recommended Fix**
HTML encode all user input before rendering. Convert < to &lt; and > to &gt; so browser treats it as text not code. Set Content Security Policy headers.

---

### Bug ID: BUG-004
### Title: Stored XSS in guestbook persists and executes on every page load

| Field | Details |
|---|---|
| Bug ID | BUG-004 |
| Title | Stored XSS persists in guestbook across page reloads |
| Reported By | Moses |
| Date Reported | 2026-05-29 |
| Application | DVWA |
| Module | XSS Stored |
| Severity | Critical |
| Priority | High |
| Status | Open |
| Environment | Ubuntu 24.04, Firefox, DVWA Low Security |

**Description**
The guestbook message field stores unsanitized JavaScript in the database. The malicious script executes for every user who visits the page including administrators making this significantly more dangerous than reflected XSS.

**Steps to Reproduce**
1. Navigate to XSS Stored page
2. Enter Name: Moses
3. Enter Message: <script>alert('Stored XSS')</script>
4. Click Sign Guestbook
5. Reload the page

**Expected Result**
Message stored as plain text. No script execution on reload.

**Actual Result**
Script executes on every page load affecting all visitors automatically.

**Evidence**
- Screenshot: Alert popup on page reload confirming persistence

**Root Cause**
Input stored directly in database without sanitization. Output rendered without HTML encoding.

**Recommended Fix**
Sanitize input before storing. Encode output before rendering. Set HttpOnly flag on session cookies to prevent cookie theft even if XSS exists.

---

### Bug ID: BUG-005
### Title: No brute force protection on login form allows automated password cracking

| Field | Details |
|---|---|
| Bug ID | BUG-005 |
| Title | No brute force protection on login form |
| Reported By | Moses |
| Date Reported | 2026-05-29 |
| Application | DVWA |
| Module | Brute Force |
| Severity | High |
| Priority | High |
| Status | Open |
| Environment | Ubuntu 24.04, Kali Linux, Hydra, DVWA Low Security |

**Description**
The login form has no account lockout, rate limiting, or CAPTCHA protection. An automated tool can attempt thousands of password combinations per minute without restriction until the correct password is found.

**Steps to Reproduce**
1. Open Kali terminal
2. Run Hydra with rockyou.txt wordlist against login form
3. Observe password cracked within minutes

**Expected Result**
Account locked after 5 failed attempts. CAPTCHA presented. Rate limiting applied.

**Actual Result**
No lockout triggered. Password cracked successfully using automated wordlist attack.

**Evidence**
- Hydra output showing successful password discovery
- No lockout or delay observed during attack

**Root Cause**
No account lockout policy. No rate limiting on authentication endpoint. No CAPTCHA implementation.

**Recommended Fix**
Implement account lockout after 5 failed attempts. Add rate limiting per IP address. Consider MFA for sensitive accounts. Add CAPTCHA for repeated failures.