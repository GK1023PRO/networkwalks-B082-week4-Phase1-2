# PENETRATION TESTING REPORT

## WEB APPLICATION SECURITY, ENCRYPTED DOCUMENT RECOVERY & INFORMATION EXPOSURE ASSESSMENT

### W4-M4-FINAL \| CYBERSECURITY \| NETWORKWALKS

  -----------------------------------------------------------------------
  Field                               Details
  ----------------------------------- -----------------------------------
  **Pentester Name**                  Georges Khoury

  **Program / Batch**                 B082 -- NetworkWalks

  **Week**                            04

  **Milestone**                       W4-M4 -- Detailed Penetration
                                      Testing Report

  **Authorized Target**               Mediroza General Hospital training
                                      web application

  **Primary Environment**             Kali Linux

  **M1**                              ✅ Completed -- Web Security
                                      Testing / Patient Report
                                      Acquisition

  **M2**                              ✅ Completed -- Encrypted Patient
                                      Report Password Recovery

  **M3**                              ✅ Completed -- Information
                                      Exposure Assessment

  **M4**                              ✅ Completed — Final Penetration Testing Report

  **Purpose**                         Authorized educational
                                      cybersecurity and
                                      penetration-testing exercise
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 1. Liability Disclaimer

The activities documented in this report were performed strictly within
the authorized NetworkWalks cybersecurity training scope. Testing was
conducted for educational and defensive-security purposes against the
assigned training environment.

No destructive modification of the target was performed. A finding is
described as confirmed only when the collected evidence supports it.
Setup screens, reconnaissance observations, or unverified hypotheses are
not presented as successful exploitation.

Sensitive information discovered during testing is summarized rather
than reproduced. Personally identifiable information (PII), confidential
employee information, shareholder information, passwords, and other
sensitive records must be redacted from any public GitHub version of
this report.

------------------------------------------------------------------------

## 2. Executive Summary

Week 4 continued the authorized Mediroza General Hospital security
assessment and combined three practical security activities into a final
penetration-testing report.

**W4-M1** focused on web application security testing and the controlled
acquisition of the assigned patient-report resources. Burp Suite
Community Edition was used as the principal web-security testing tool to
observe and analyze HTTP requests, authentication behavior, application
endpoints, and responses. Some foundational Mediroza
authentication-testing work began previously under **W3-OPTIONAL2** in
the `networkwalks-B082-week3-Phase1-2` repository. Week 4 therefore
represents a continuation and expansion of that authorized assessment
rather than an unrelated duplicate exercise.

**W4-M2** focused on the encrypted patient reports obtained as part of
the authorized lab. The NetworkWalks browser-based password-recovery
workflow was used to process the protected PDF documents, extract their
password-verification hashes, perform authorized password recovery, and
manually verify recovered access against the original files.

**W4-M3** focused on security exposure and impact analysis. The most
significant confirmed issue was a publicly accessible historical SQL
database backup exposed through the `/old/` directory. The backup
disclosed internal database structure and confidential staff/shareholder
information. Public directory indexing was also confirmed on
`/patient/`, `/staff/`, and `/old/`, exposing application structure and
sensitive resource names.

### Overall Risk Rating: **HIGH**

The overall rating is driven primarily by the publicly accessible
database backup containing confidential organizational information.
Directory indexing and publicly exposed internal resources materially
increase reconnaissance value and compound the information-disclosure
risk.

------------------------------------------------------------------------

## 3. Scope and Methodology

### 3.1 Authorized Scope

The assessment focused on the NetworkWalks-assigned Mediroza training
environment and the resources encountered during W4-M1 through W4-M3.

The reviewed areas included:

-   Public web content and HTTP/HTTPS behavior
-   Patient and staff authentication interfaces
-   Patient report resources used in the assigned lab
-   `robots.txt`
-   `/patient/`
-   `/staff/`
-   `/old/`
-   Publicly exposed files and legacy resources
-   Encrypted patient-report documents
-   Authentication request/response behavior
-   Directory indexing
-   Historical database-backup exposure
-   Evidence integrity and professional reporting

Testing remained within the educational scope.

### 3.2 Relationship to W3-OPTIONAL2

The Mediroza web-security work did not begin from zero in Week 4.

During **W3-OPTIONAL2**, the Mediroza Hospital web authentication
interface had already been introduced as an authorized
web-authentication security assessment. Week 4 reused that legitimate
assessment context and extended it into a broader web-security,
encrypted-document-recovery, information-exposure, and reporting
workflow.

This distinction is important for the evidence record:

``` text
W3-OPTIONAL2
Mediroza Web Authentication Security Assessment
        ↓
Existing foundational authentication/security-testing work
        ↓
W4-M1
Expanded Web Security Testing with Burp Suite
        ↓
W4-M2
Encrypted Patient Report Password Recovery
        ↓
W4-M3
Information Exposure / Impact Assessment
        ↓
W4-M4
Final Penetration Testing Report
```

Week 3 evidence remains part of the Week 3 repository. Week 4
documentation should reference the continuity without duplicating or
relabeling Week 3 screenshots as Week 4 evidence.

### 3.3 Tools Used

  -----------------------------------------------------------------------
  Tool / Technology                   Purpose
  ----------------------------------- -----------------------------------
  **Kali Linux**                      Primary penetration-testing
                                      environment

  **Burp Suite Community Edition**    Web proxy, HTTP request/response
                                      analysis, Repeater/Intruder
                                      preparation, authentication testing

  **Chromium / Web Browser**          Manual web validation and
                                      screenshot evidence

  **cURL**                            Retrieve and inspect HTTP
                                      responses, headers, pages, and
                                      endpoints

  **grep**                            Search saved
                                      reconnaissance/evidence for
                                      relevant content

  **sha256sum**                       Verify evidence integrity

  **NetworkWalks Hash Calculator**    Extract password-verification
                                      information from authorized
                                      password-protected PDF lab files

  **NetworkWalks Password Cracking    Perform authorized
  Lab / Tool**                        password-recovery testing against
                                      extracted PDF hashes

  **Manual PDF Verification**         Confirm that a recovered password
                                      correctly opens the corresponding
                                      protected document
  -----------------------------------------------------------------------

### 3.4 Assessment Methodology

``` text
Authorization & Scope
        ↓
Web Reconnaissance
        ↓
Burp Suite Proxy / HTTP Analysis
        ↓
Authentication & Endpoint Review
        ↓
Patient Report Acquisition
        ↓
Encrypted PDF Hash Extraction
        ↓
Authorized Password Recovery
        ↓
Manual Result Verification
        ↓
Directory / Legacy Resource Review
        ↓
Information-Exposure Validation
        ↓
Evidence Collection & SHA-256 Verification
        ↓
Risk Analysis
        ↓
Remediation Planning
        ↓
Professional Reporting
```

### 3.5 Limitations

-   Testing was restricted to the authorized educational target and
    supplied lab resources.
-   No destructive modification was performed.
-   Sensitive database records were not unnecessarily reproduced in this
    public-facing report.
-   The presence of a resource does not automatically prove that its
    contents are sensitive.
-   Burp Suite setup/configuration screenshots are treated as
    methodology evidence, not proof of exploitation.
-   A submitted test credential is not treated as a valid credential
    unless successful authentication is independently confirmed.
-   Cookie-security weaknesses are not claimed without suitable
    `Set-Cookie` evidence.
-   Potential username enumeration should only be reported as confirmed
    when comparable requests demonstrate reliably different responses.

------------------------------------------------------------------------

# 4. Milestone Activities

## 4.1 W4-M1 -- Web Security Testing & Patient Report Acquisition

**Status: ✅ Completed**

W4-M1 focused on web-security testing of the authorized Mediroza
application. **Burp Suite Community Edition** was used as the primary
web application security testing tool.

The captured Week 4 evidence demonstrates the following workflow:

1.  Burp Suite Community Edition was launched in Kali Linux.
2.  A temporary Burp project was created.
3.  Burp default configuration was selected.
4.  The Burp Dashboard and Proxy tools were initialized.
5.  The Mediroza Patient Portal login interface was opened in the
    browser.
6.  Proxy interception was enabled/disabled as required for traffic
    analysis.
7.  A patient-login POST request was captured in Burp HTTP history.
8.  The request demonstrated application parameters for `username` and
    `password`.
9.  The captured request was transferred into Burp Repeater for
    controlled request analysis.
10. Burp Intruder was prepared for authorized testing; setup evidence
    alone is not treated as proof that an automated attack succeeded.
11. HTTP response information was reviewed for application/server
    fingerprinting.
12. Additional endpoint and directory review was performed as part of
    the authorized assessment.

Observed response/application metadata included information consistent
with PHP, LiteSpeed, and the Mediroza CMS. Such metadata is treated
primarily as reconnaissance/fingerprinting information unless paired
with a demonstrated exploit.

A browser response displaying **"Incorrect password"** was observed for
a test login attempt. This confirms authentication failure behavior for
that request; it does **not** establish that the submitted test
credential was valid.

### M1 Security Principle

Web proxies such as Burp Suite allow security professionals to inspect
the interaction between a browser and a web application. The objective
is not merely to send requests, but to understand parameters,
authentication flows, application responses, and potential weaknesses
while remaining within the authorized scope.

------------------------------------------------------------------------

## 4.2 W4-M2 -- Encrypted Patient Report Password Recovery

**Status: ✅ Completed**

W4-M2 focused on the password protection applied to the assigned
encrypted patient-report PDF documents.

The NetworkWalks lab uses a two-stage browser-based workflow:

``` text
Password-Protected PDF
        ↓
NetworkWalks Hash Calculator
        ↓
Extracted PDF Password-Verification Hash
        ↓
NetworkWalks Password Cracking Tool / Lab
        ↓
Candidate-Password Testing
        ↓
Recovered Password
        ↓
Original Encrypted PDF
        ↓
Manual Verification
```

The purpose of the exercise was to demonstrate that document encryption
can still be undermined when the protecting password is weak or
predictable.

The three assigned patient reports were processed only within the
authorized educational exercise. Password-verification information was
extracted from the encrypted PDFs and used for password-recovery
testing. Recovered results were then manually verified against their
corresponding protected documents.

### Official NetworkWalks Resources

-   NetworkWalks Project Task Lab -- Password Cracking with NetworkWalks
    Tools
-   NetworkWalks Hash Calculator

The NetworkWalks lab documentation explains that the Hash Calculator is
used to extract the hash from a locked PDF and that the Password Cracker
is then used to recover the password from that extracted value.

### M2 Security Principle

Encryption strength and password strength are separate controls. A
technically encrypted document can still be vulnerable to offline
password recovery when protected by a weak, common, or predictable
password.

### Recommended Defensive Controls

-   Use long, unique, randomly generated passwords.
-   Avoid dictionary words and predictable password patterns.
-   Use a password manager for high-entropy credentials.
-   Protect sensitive reports using appropriate access controls in
    addition to document passwords.
-   Avoid distributing sensitive encrypted files and their passwords
    through the same communication channel.
-   Apply organizational data-classification and retention policies.

------------------------------------------------------------------------

## 4.3 W4-M3 -- Information Exposure & Impact Assessment

**Status: ✅ Completed**

W4-M3 expanded the assessment from authentication and encrypted-document
security into broader information-exposure analysis.

The assessment confirmed directory indexing on:

``` text
/patient/
/staff/
/old/
```

The `/patient/` listing exposed resource names including:

``` text
reports/
download.php
error_log
login.php
logout.php
portal.php
```

The `/staff/` listing exposed the staff authentication resource.

The `/old/` directory exposed a historical SQL database backup.
Validation showed that the backup contained internal database structure
and confidential staff/shareholder information.

This represented the most significant confirmed security issue observed
during Week 4.

------------------------------------------------------------------------

# 5. Findings and Proof of Impact

## 5.1 W4-01 -- Public Historical Database Backup

**Severity: CRITICAL**\
**Status: Confirmed**

### Description

A historical SQL database backup was publicly discoverable through the
web-accessible `/old/` directory.

The file identified itself as an internal Mediroza database backup and
included a warning indicating that it contained confidential staff and
shareholder records.

The evidence also demonstrated database structures associated with staff
and shareholder information.

### Security Impact

Unauthorized exposure of a database backup may result in:

-   Disclosure of employee PII
-   Disclosure of employment and financial information
-   Disclosure of shareholder information
-   Exposure of internal database schema
-   Increased phishing and social-engineering risk
-   Additional reconnaissance opportunities
-   Privacy, regulatory, legal, and reputational consequences

### Proof of Impact

The issue was validated sufficiently to establish that confidential
organizational information was publicly exposed. Further collection of
real personal records was unnecessary.

The public report intentionally does not reproduce names, national
identifiers, phone numbers, salary information, or other sensitive
records.

### Remediation

1.  Remove database backups from all publicly accessible web directories
    immediately.
2.  Store backups outside the web document root.
3.  Encrypt backup data at rest.
4.  Apply strict authentication and authorization to backup storage.
5.  Review web-server access logs to determine whether the backup was
    previously accessed.
6.  Assess whether exposed information requires privacy/breach response
    procedures.
7.  Rotate any credentials or secrets if later review establishes that
    they were included in the exposed data.
8.  Add deployment controls that prevent database dumps and backup
    artifacts from reaching production web roots.

------------------------------------------------------------------------

## 5.2 W4-02 -- Public Directory Indexing

**Severity: MEDIUM**\
**Status: Confirmed**

### Description

Automatic directory listing was confirmed on multiple web-accessible
directories:

``` text
/patient/
/staff/
/old/
```

Directory indexes revealed application structure and exposed resource
names that would otherwise require additional reconnaissance.

### Security Impact

Directory indexing can:

-   Reveal hidden or forgotten files
-   Expose legacy resources
-   Accelerate attacker reconnaissance
-   Reveal authentication endpoints
-   Expose report/download/log resources
-   Lead directly to more serious issues when sensitive files are
    present

In this assessment, the risk was compounded because `/old/` exposed the
historical SQL backup.

### Remediation

-   Disable automatic directory listing.
-   Apply explicit access-control rules to sensitive directories.
-   Remove obsolete resources from the public web root.
-   Review public directories for backup, log, temporary, archive, and
    development files.
-   Use secure deployment pipelines to prevent unintended files from
    being published.

------------------------------------------------------------------------

## 5.3 W4-03 -- Publicly Listed Application Error Log

**Severity: MEDIUM**\
**Status: Confirmed Exposure / Contents Not Overclaimed**

### Description

The `/patient/` directory index publicly listed an `error_log` resource.

The confirmed finding is that the log resource was publicly
exposed/listed. This report does not assume that its contents contained
sensitive information unless separately demonstrated by evidence.

### Security Impact

If application logs are publicly retrievable, they may expose:

-   Internal filesystem paths
-   Stack traces
-   Database errors
-   Implementation details
-   Debug information
-   User/session information
-   Other sensitive operational context

### Remediation

-   Store application logs outside the web document root.
-   Explicitly deny direct HTTP access to log files.
-   Review previously exposed logs for sensitive content.
-   Centralize logging in a protected logging/SIEM platform.
-   Ensure production applications do not expose verbose debugging
    information.

------------------------------------------------------------------------

## 5.4 W4-04 -- Internal Staff Authentication Interface Discoverable

**Severity: LOW / INFORMATIONAL**\
**Status: Confirmed Observation**

### Description

The staff authentication interface was discoverable through the exposed
application structure and identified itself as an internal staff-access
page.

A publicly reachable login page is not automatically a vulnerability.
The security concern is its reconnaissance value when combined with
directory indexing and other exposed resources.

### Remediation

-   Disable directory indexing.
-   Use generic authentication failure responses.
-   Enforce MFA for staff accounts where appropriate.
-   Implement rate limiting and monitoring.
-   Apply account lockout or adaptive protection carefully to prevent
    abuse.
-   Minimize unnecessary application and technology disclosure.

------------------------------------------------------------------------

## 5.5 W4-05 -- Technology and Application Fingerprinting

**Severity: INFORMATIONAL**\
**Status: Confirmed Observation**

### Description

HTTP analysis with Burp Suite exposed application/server metadata useful
for fingerprinting, including PHP/LiteSpeed-related information and
Mediroza CMS generator metadata.

### Security Impact

Technology disclosure does not by itself establish exploitation.
However, version and framework information can help an attacker focus
reconnaissance on known weaknesses.

### Remediation

-   Remove unnecessary generator/version metadata where practical.
-   Avoid exposing detailed implementation information.
-   Keep server-side frameworks, runtimes, CMS components, and
    dependencies patched.
-   Treat fingerprint reduction as defense-in-depth rather than a
    substitute for patching.

------------------------------------------------------------------------

# 6. Risk Rating Summary

  --------------------------------------------------------------------------------
  ID               Finding                               Severity Status
  ---------------- ------------------------ --------------------- ----------------
  **W4-01**        Public historical                 **Critical** Confirmed
                   database backup                                
                   containing confidential                        
                   organizational                                 
                   information                                    

  **W4-02**        Public directory                    **Medium** Confirmed
                   indexing                                       

  **W4-03**        Publicly listed                     **Medium** Confirmed
                   application error log                          exposure

  **W4-04**        Internal staff                         **Low / Confirmed
                   authentication interface       Informational** observation
                   discoverable                                   

  **W4-05**        Technology/application       **Informational** Confirmed
                   fingerprinting                                 observation
  --------------------------------------------------------------------------------

### Overall Assessment: **HIGH RISK**

Although the highest individual finding is rated Critical, the overall
engagement is rated High because the assessment was limited in scope and
the report avoids assuming broader compromise beyond the evidence
collected.

------------------------------------------------------------------------

# 7. Recommendations and Remediation Plan

## Immediate Priority

1.  **Remove the exposed SQL backup.**
2.  **Disable directory indexing.**
3.  **Review whether the exposed database was accessed by unauthorized
    parties.**
4.  **Protect or remove publicly accessible logs and legacy resources.**
5.  **Review all web-accessible directories for additional backups or
    confidential files.**

## Short-Term Hardening

6.  Harden staff and patient authentication with MFA where appropriate,
    rate limiting, monitoring, and generic failure messages.
7.  Remove obsolete `/old/` content from production.
8.  Review `/patient/reports/` and download functionality for proper
    authorization enforcement.
9.  Ensure session cookies use appropriate security attributes based on
    the application's deployment requirements.
10. Minimize unnecessary server/application fingerprinting information.

## Long-Term Controls

11. Add CI/CD deployment rules blocking sensitive extensions such as:

``` text
.sql
.bak
.backup
.log
.old
.zip
.tar
.gz
.tmp
```

12. Store backups in dedicated protected storage rather than the public
    web root.
13. Implement recurring external attack-surface reviews.
14. Perform secure-code and access-control reviews.
15. Establish centralized protected logging and alerting.
16. Maintain data-classification, retention, and incident-response
    procedures.

------------------------------------------------------------------------

# 8. Evidence and Screenshot Organization

W4-M4 is the **final reporting milestone**. It does not require a separate technical-testing screenshot folder. This report consolidates the evidence collected during **W4-M1, W4-M2, and W4-M3**.

The repository contains **55 Week 4 screenshots**:

| Milestone | Screenshot Count | Status |
|---|---:|---|
| **W4-M1** | **22** | ✅ Completed |
| **W4-M2** | **23** | ✅ Completed |
| **W4-M3** | **10** | ✅ Completed |
| **Total** | **55** | ✅ Integrated into M4 |

The exact GitHub structure is:

```text
screenshots/
├── W4-M1/
│   ├── 1.png
│   ├── 2.png
│   ├── ...
│   └── 22.png
├── W4-M2/
│   ├── 1.png
│   ├── 2.png
│   ├── ...
│   └── 23.png
└── W4-M3/
    ├── 1.png
    ├── 2.png
    ├── ...
    └── 10.png
```

> **Evidence rule:** Screenshot captions below intentionally use neutral numbering rather than inventing a result for an image that has not been individually described in the report. The surrounding milestone sections explain what each evidence set supports.

## 8.1 W4-M1 — Web Security Testing Evidence

These 22 screenshots form the W4-M1 evidence set. They document the authorized Mediroza web-security workflow, including Burp Suite configuration and use, browser-based application testing, HTTP request/response inspection, authentication testing, and related M1 activity. Configuration screens are treated as methodology evidence and are not presented as successful exploitation unless the corresponding evidence demonstrates a result.

### W4-M1 Screenshot 1

![W4-M1 Screenshot 1](screenshots/W4-M1/1.png)

### W4-M1 Screenshot 2

![W4-M1 Screenshot 2](screenshots/W4-M1/2.png)

### W4-M1 Screenshot 3

![W4-M1 Screenshot 3](screenshots/W4-M1/3.png)

### W4-M1 Screenshot 4

![W4-M1 Screenshot 4](screenshots/W4-M1/4.png)

### W4-M1 Screenshot 5

![W4-M1 Screenshot 5](screenshots/W4-M1/5.png)

### W4-M1 Screenshot 6

![W4-M1 Screenshot 6](screenshots/W4-M1/6.png)

### W4-M1 Screenshot 7

![W4-M1 Screenshot 7](screenshots/W4-M1/7.png)

### W4-M1 Screenshot 8

![W4-M1 Screenshot 8](screenshots/W4-M1/8.png)

### W4-M1 Screenshot 9

![W4-M1 Screenshot 9](screenshots/W4-M1/9.png)

### W4-M1 Screenshot 10

![W4-M1 Screenshot 10](screenshots/W4-M1/10.png)

### W4-M1 Screenshot 11

![W4-M1 Screenshot 11](screenshots/W4-M1/11.png)

### W4-M1 Screenshot 12

![W4-M1 Screenshot 12](screenshots/W4-M1/12.png)

### W4-M1 Screenshot 13

![W4-M1 Screenshot 13](screenshots/W4-M1/13.png)

### W4-M1 Screenshot 14

![W4-M1 Screenshot 14](screenshots/W4-M1/14.png)

### W4-M1 Screenshot 15

![W4-M1 Screenshot 15](screenshots/W4-M1/15.png)

### W4-M1 Screenshot 16

![W4-M1 Screenshot 16](screenshots/W4-M1/16.png)

### W4-M1 Screenshot 17

![W4-M1 Screenshot 17](screenshots/W4-M1/17.png)

### W4-M1 Screenshot 18

![W4-M1 Screenshot 18](screenshots/W4-M1/18.png)

### W4-M1 Screenshot 19

![W4-M1 Screenshot 19](screenshots/W4-M1/19.png)

### W4-M1 Screenshot 20

![W4-M1 Screenshot 20](screenshots/W4-M1/20.png)

### W4-M1 Screenshot 21

![W4-M1 Screenshot 21](screenshots/W4-M1/21.png)

### W4-M1 Screenshot 22

![W4-M1 Screenshot 22](screenshots/W4-M1/22.png)


## 8.2 W4-M2 — Encrypted Patient Report / Password Security Evidence

These 23 screenshots form the W4-M2 evidence set. They document the authorized encrypted-document/password-security workflow and NetworkWalks tooling used during M2. Sensitive recovered passwords or patient information should be redacted where necessary in the public repository.

### W4-M2 Screenshot 1

![W4-M2 Screenshot 1](screenshots/W4-M2/1.png)

### W4-M2 Screenshot 2

![W4-M2 Screenshot 2](screenshots/W4-M2/2.png)

### W4-M2 Screenshot 3

![W4-M2 Screenshot 3](screenshots/W4-M2/3.png)

### W4-M2 Screenshot 4

![W4-M2 Screenshot 4](screenshots/W4-M2/4.png)

### W4-M2 Screenshot 5

![W4-M2 Screenshot 5](screenshots/W4-M2/5.png)

### W4-M2 Screenshot 6

![W4-M2 Screenshot 6](screenshots/W4-M2/6.png)

### W4-M2 Screenshot 7

![W4-M2 Screenshot 7](screenshots/W4-M2/7.png)

### W4-M2 Screenshot 8

![W4-M2 Screenshot 8](screenshots/W4-M2/8.png)

### W4-M2 Screenshot 9

![W4-M2 Screenshot 9](screenshots/W4-M2/9.png)

### W4-M2 Screenshot 10

![W4-M2 Screenshot 10](screenshots/W4-M2/10.png)

### W4-M2 Screenshot 11

![W4-M2 Screenshot 11](screenshots/W4-M2/11.png)

### W4-M2 Screenshot 12

![W4-M2 Screenshot 12](screenshots/W4-M2/12.png)

### W4-M2 Screenshot 13

![W4-M2 Screenshot 13](screenshots/W4-M2/13.png)

### W4-M2 Screenshot 14

![W4-M2 Screenshot 14](screenshots/W4-M2/14.png)

### W4-M2 Screenshot 15

![W4-M2 Screenshot 15](screenshots/W4-M2/15.png)

### W4-M2 Screenshot 16

![W4-M2 Screenshot 16](screenshots/W4-M2/16.png)

### W4-M2 Screenshot 17

![W4-M2 Screenshot 17](screenshots/W4-M2/17.png)

### W4-M2 Screenshot 18

![W4-M2 Screenshot 18](screenshots/W4-M2/18.png)

### W4-M2 Screenshot 19

![W4-M2 Screenshot 19](screenshots/W4-M2/19.png)

### W4-M2 Screenshot 20

![W4-M2 Screenshot 20](screenshots/W4-M2/20.png)

### W4-M2 Screenshot 21

![W4-M2 Screenshot 21](screenshots/W4-M2/21.png)

### W4-M2 Screenshot 22

![W4-M2 Screenshot 22](screenshots/W4-M2/22.png)

### W4-M2 Screenshot 23

![W4-M2 Screenshot 23](screenshots/W4-M2/23.png)


## 8.3 W4-M3 — Information Exposure & Findings Evidence

These 10 screenshots form the W4-M3 evidence set. They support the information-exposure assessment and validated findings discussed in this report. Screenshots containing staff, shareholder, patient, financial, national-ID, phone-number, or other sensitive information must be redacted before public publication.

### W4-M3 Screenshot 1

![W4-M3 Screenshot 1](screenshots/W4-M3/1.png)

### W4-M3 Screenshot 2

![W4-M3 Screenshot 2](screenshots/W4-M3/2.png)

### W4-M3 Screenshot 3

![W4-M3 Screenshot 3](screenshots/W4-M3/3.png)

### W4-M3 Screenshot 4

![W4-M3 Screenshot 4](screenshots/W4-M3/4.png)

### W4-M3 Screenshot 5

![W4-M3 Screenshot 5](screenshots/W4-M3/5.png)

### W4-M3 Screenshot 6

![W4-M3 Screenshot 6](screenshots/W4-M3/6.png)

### W4-M3 Screenshot 7

![W4-M3 Screenshot 7](screenshots/W4-M3/7.png)

### W4-M3 Screenshot 8

![W4-M3 Screenshot 8](screenshots/W4-M3/8.png)

### W4-M3 Screenshot 9

![W4-M3 Screenshot 9](screenshots/W4-M3/9.png)

### W4-M3 Screenshot 10

![W4-M3 Screenshot 10](screenshots/W4-M3/10.png)


## 8.4 W4-M4 — Final Penetration Testing Report

W4-M4 is represented by this `README.md`.

It consolidates the **55 screenshots from M1–M3** together with:

1. Executive Summary
2. Scope and Methodology
3. Findings and Proof of Impact
4. Risk Ratings
5. Recommendations and Remediation
6. Evidence Organization
7. Evidence Integrity and Privacy
8. Problems Encountered and Solutions
9. Skills Practiced
10. Conclusion

**No `screenshots/W4-M4/` directory is required.**

---

# 9. Evidence Integrity and Privacy

Collected evidence files were verified using SHA-256 where applicable.
The evidence-verification workflow returned `OK` for the recorded
evidence set.

For the public GitHub repository:

-   Do not upload the exposed SQL database backup.
-   Do not publish raw confidential database contents.
-   Redact national IDs.
-   Redact phone numbers.
-   Redact salary information.
-   Redact patient information.
-   Redact unnecessary staff/shareholder PII.
-   Do not expose recovered passwords unnecessarily.
-   Preserve any required unredacted evidence only in an
    instructor-approved private submission location.

------------------------------------------------------------------------

# 10. Problems Encountered and Solutions

## 10.1 Continuity Between Week 3 and Week 4

**Problem:** Part of the Mediroza authentication assessment originated
in W3-OPTIONAL2, creating a risk that Week 3 evidence could be
incorrectly presented as new Week 4 evidence.

**Solution:** The Week 4 report explicitly documents the relationship to
W3-OPTIONAL2 while keeping Week 3 screenshots in the Week 3 repository.
Week 4 contains only its own evidence and references the earlier work as
background.

## 10.2 Distinguishing Setup from Confirmed Findings

**Problem:** Burp Suite screenshots include proxy, Repeater, and
Intruder setup screens.

**Solution:** Configuration/setup evidence is documented as methodology
only. No automated attack or successful credential compromise is claimed
unless supported by a demonstrated result.

## 10.3 Sensitive Database Evidence

**Problem:** The exposed SQL backup contained confidential information.

**Solution:** Testing stopped after sufficient proof of exposure was
obtained. The report summarizes the categories of exposed information
without reproducing unnecessary personal records.

## 10.4 Evidence Integrity

**Problem:** Security evidence must remain trustworthy and traceable.

**Solution:** SHA-256 hashes were used for collected evidence files
where applicable, and integrity verification returned successful
results.

## 10.5 Encrypted Document Validation

**Problem:** An automated password-recovery result alone should not be
treated as final proof.

**Solution:** Recovered passwords were manually verified against their
corresponding authorized protected documents.

------------------------------------------------------------------------

# 11. Skills Practiced

-   Web Application Penetration Testing
-   Burp Suite Community Edition
-   HTTP Request/Response Analysis
-   Proxy Interception
-   Burp Repeater
-   Burp Intruder Preparation
-   Authentication Security Review
-   Reconnaissance
-   Directory Indexing Assessment
-   Information Disclosure Analysis
-   Encrypted Document Security
-   Password Hash Extraction
-   Authorized Password Recovery
-   Manual Result Validation
-   Kali Linux
-   cURL
-   grep
-   SHA-256 Evidence Integrity
-   Risk Assessment
-   Vulnerability Reporting
-   Remediation Planning
-   Data Privacy
-   Evidence Handling
-   Professional Penetration Testing Documentation

------------------------------------------------------------------------

# 12. Conclusion

Week 4 demonstrated an end-to-end penetration-testing workflow combining
web application assessment, encrypted-document security,
information-exposure analysis, evidence validation, risk assessment, and
professional reporting.

W4-M1 extended the Mediroza web-authentication work begun during
W3-OPTIONAL2 and used Burp Suite Community Edition to inspect and
analyze the authorized web application.

W4-M2 demonstrated the security implications of weak document passwords
through the authorized NetworkWalks Hash Calculator and
password-recovery workflow. The exercise reinforced that encryption does
not compensate for weak password selection.

W4-M3 identified the most serious issue of the week: a publicly
accessible historical SQL database backup containing confidential
organizational information. Directory indexing and other exposed
resources increased reconnaissance value and contributed to the overall
security risk.

W4-M4 consolidated these activities into a professional
penetration-testing report with clear findings, evidence boundaries,
risk ratings, and actionable remediation.

The assessment reinforced several core security principles:

-   Sensitive backups must never be stored in public web directories.
-   Directory indexing should be disabled unless intentionally required.
-   Logs and legacy files must be protected from direct public access.
-   Strong passwords are essential even when files are encrypted.
-   Authentication systems should minimize information disclosure and
    resist automated abuse.
-   Automated tool output must be manually validated.
-   Evidence should be collected carefully, preserved with integrity,
    and reported without unnecessarily exposing sensitive information.
-   Penetration testing must always remain within explicit
    authorization.

------------------------------------------------------------------------

# Report Prepared By

**Georges Khoury**\
Cybersecurity & Ethical Hacking Intern\
**Batch:** B082 -- NetworkWalks\
**Week:** 04\
**Report:** W4-M4-FINAL

------------------------------------------------------------------------

## Project Status

  -----------------------------------------------------------------------
  Milestone                           Status
  ----------------------------------- -----------------------------------
  **W4-M1**                           ✅ Completed

  **W4-M2**                           ✅ Completed

  **W4-M3**                           ✅ Completed

  **W4-M4**                           ✅ Completed
  -----------------------------------------------------------------------

------------------------------------------------------------------------

**End of Report**
