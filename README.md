# networkwalks-B082-week4-Phase1-2
NetworkWalks B082 Week 4 – Web Application Security, Burp Suite, Encryption, Vulnerability Assessment &amp; Penetration Testing (M1–M4)
# PENETRATION TESTING REPORT

## WEB APPLICATION SECURITY ASSESSMENT

### W4-M4-FINAL \| CYBERSECURITY \| NETWORKWALKS

  Field                 Details
  --------------------- -------------------------------------------
  **Pentester Name**    Georges Khoury
  **Program / Batch**   B082 -- NetworkWalks
  **Week**              04
  **Milestone**         M4 -- Detailed Penetration Testing Report
  **Target**            Mediroza General Hospital Web Application
  **Environment**       Kali Linux
  **M1 / M2 / M3**      Completed
  **M4**                Final Reporting

------------------------------------------------------------------------

## 1. Liability Disclaimer

The activities documented in this report were performed only within the
authorized NetworkWalks cybersecurity training scope. No destructive
modification of the target was performed. Findings are reported as
confirmed only when supported by collected evidence.

Sensitive information discovered during testing is summarized rather
than reproduced. PII and confidential records must be redacted from any
public GitHub version.

## 2. Executive Summary

Week 4 consisted of an authorized web-application security assessment of
the Mediroza General Hospital training target. The assessment identified
multiple information-exposure concerns.

The most significant confirmed issue was a publicly accessible
historical SQL database backup exposed through `/old/`. The backup
contained internal database structure and confidential staff/shareholder
information.

Directory indexing was also confirmed on `/patient/`, `/staff/`, and
`/old/`. The patient directory disclosed resources including `reports/`,
`download.php`, `error_log`, `login.php`, `logout.php`, and
`portal.php`. The staff directory exposed the internal staff login
resource.

**Overall Risk Rating: HIGH**

The High overall rating is driven primarily by the exposed confidential
database backup. Directory indexing and exposed internal resources
substantially increase reconnaissance value and compound the
information-disclosure risk.

## 3. Scope and Methodology

### 3.1 Authorized Scope

Testing focused on the authorized Mediroza web application and
web-accessible resources discovered during M1--M3, including public
content, `robots.txt`, patient/staff resources, legacy directories,
authentication interfaces, exposed files, and HTTP behavior.

### 3.2 Tools Used

  -----------------------------------------------------------------------
  Tool / Technique                    Purpose
  ----------------------------------- -----------------------------------
  **Kali Linux**                      Primary assessment environment

  **cURL**                            Retrieve HTTP responses, headers,
                                      and pages

  **grep**                            Search saved evidence for relevant
                                      content

  **sha256sum**                       Evidence-integrity verification

  **Web Browser**                     Manual validation and screenshots

  **Directory/resource review**       Identify exposed application
                                      resources

  **Authentication review**           Observe login interfaces and
                                      failure behavior
  -----------------------------------------------------------------------

### 3.3 Methodology

``` text
Authorized Scope
      ↓
Reconnaissance
      ↓
Resource Discovery
      ↓
Directory / Endpoint Review
      ↓
Authentication Review
      ↓
Exposure Validation
      ↓
Evidence Collection
      ↓
Integrity Verification
      ↓
Risk Analysis
      ↓
Remediation
```

### 3.4 Limitations

Testing remained within the educational scope. Sensitive records were
not modified. Public evidence must be redacted.

## 4. Findings and Proof of Exploitation

### 4.1 W4-01 --- Public Directory Indexing

**Severity: Medium**

Directory indexing was confirmed on:

``` text
/patient/
/staff/
/old/
```

The patient listing exposed resource names including:

``` text
reports/
download.php
error_log
login.php
logout.php
portal.php
```

The staff listing exposed `login.php`, while `/old/` exposed a
historical SQL backup.

**Impact:** Directory indexing accelerates reconnaissance, exposes
internal application structure, and may reveal forgotten or sensitive
resources.

**Remediation:** Disable automatic directory listing, restrict sensitive
directories, and remove obsolete files from the public web root.

------------------------------------------------------------------------

### 4.2 W4-02 --- Public Historical Database Backup

**Severity: Critical**

A historical SQL database backup was publicly discoverable through
`/old/`. Validation showed that the backup contained internal schema
information and confidential staff/shareholder records.

The public report intentionally does not reproduce the exposed personal
records.

**Potential Impact:** - Privacy breach - Targeted phishing/social
engineering - Exposure of internal organizational information -
Disclosure of database structure - Regulatory/reputational consequences

**Remediation:** Remove public backups immediately, store backups
outside the document root, encrypt them, enforce access control, review
access logs, and perform an incident/privacy review.

------------------------------------------------------------------------

### 4.3 W4-03 --- Public Application Error Log

**Severity: High**

The indexed `/patient/` directory exposed an `error_log` file.

**Impact:** Public logs may disclose internal paths, implementation
details, errors, debugging information, or other sensitive context.

**Remediation:** Move logs outside the public document root, deny direct
HTTP access, review the exposed log for sensitive data, and centralize
protected logging.

------------------------------------------------------------------------

### 4.4 W4-04 --- Internal Staff Authentication Interface Discoverable

**Severity: Low**

An internal staff authentication page was discoverable and identified
itself as:

``` text
Staff Login
Internal staff access only.
```

The form requested a Staff ID and password.

A public login interface is not automatically a vulnerability; the issue
here is its reconnaissance value when combined with directory indexing
and other exposed resources.

**Remediation:** Disable directory indexing, minimize unnecessary
technology disclosure, enforce MFA/rate limiting, and monitor
authentication attempts.

------------------------------------------------------------------------

### 4.5 W4-05 --- Application Structure Disclosure

**Severity: Medium**

Unauthenticated reconnaissance revealed patient, staff, legacy, report,
download, log, portal, and authentication-related resources.

**Impact:** The combined information provides a useful map of the
application and reduces attacker reconnaissance effort.

**Remediation:** Apply least privilege, remove legacy resources, protect
logs/backups, and conduct regular external attack-surface reviews.

## 5. Risk Rating

  -----------------------------------------------------------------------
  ID                Finding           Severity          Status
  ----------------- ----------------- ----------------- -----------------
  W4-01             Public directory  **Medium**        Confirmed
                    indexing                            

  W4-02             Public database   **Critical**      Confirmed
                    backup with                         
                    confidential                        
                    records                             

  W4-03             Public            **High**          Confirmed
                    application error                   exposure
                    log                                 

  W4-04             Internal staff    **Low**           Confirmed
                    login                               observation
                    discoverable                        

  W4-05             Application       **Medium**        Confirmed
                    structure                           
                    disclosure                          
  -----------------------------------------------------------------------

## 6. Recommendations and Remediation

1.  **Remove public database backups immediately.** Store backups
    outside the web root and protect/encrypt them.
2.  **Disable directory indexing.** For Apache, a defensive
    configuration may include `Options -Indexes`.
3.  **Protect application logs.** Logs should never be directly
    downloadable from the public site.
4.  **Review the exposed-data incident.** Determine what data was
    exposed, how long it was accessible, and whether notification
    obligations apply.
5.  **Harden authentication.** Use MFA, rate limiting, strong password
    policy, monitoring, and generic failure responses.
6.  **Remove legacy content.** Delete obsolete `/old/` resources and
    development artifacts from production.
7.  **Add deployment controls.** Prevent `.sql`, `.bak`, `.log`, `.zip`,
    `.tar`, `.gz`, `.old`, and temporary files from entering the public
    web root.
8.  **Perform recurring external reviews.** Check for directory listing,
    backups, logs, debug files, and access-control mistakes.

## 7. Conclusion

Week 4 demonstrated the complete penetration-testing workflow from
reconnaissance through validation and professional reporting.

The most serious confirmed issue was the publicly accessible historical
SQL backup containing confidential organizational information.
Additional findings included directory indexing, exposure of an
application error log, discoverable internal application resources, and
an exposed staff authentication interface.

The assessment reinforced that individually small information
disclosures can become substantially more valuable when combined. All
findings were documented within the authorized educational scope.

## 8. Evidence Collected

### 8.1 W4-M1

Store chronological screenshots under:

``` text
screenshots/W4-M1/
```

M1 evidence includes scope verification, HTTP reconnaissance,
`robots.txt`, patient/staff/old directory discovery, exposed resource
names, SQL backup discovery, and SHA-256 evidence-integrity
verification.

### 8.2 W4-M2

``` text
screenshots/W4-M2/
```

Insert the validated M2 screenshots in chronological order. Only
demonstrated findings should be marked confirmed.

### 8.3 W4-M3

``` text
screenshots/W4-M3/
```

Insert the validated M3 screenshots in chronological order. Only
demonstrated findings should be marked confirmed.

### 8.4 W4-M4

M4 contains the final report: Executive Summary, Scope & Methodology,
Findings & Proof, Risk Rating, and Recommendations & Remediation.

## 9. Evidence Handling and Privacy

For a public repository:

-   Redact national IDs, telephone numbers, salary information, and
    unnecessary personal data.
-   Do not upload the exposed SQL backup.
-   Do not publish raw confidential database contents.
-   Preserve unredacted evidence only in an approved private submission
    location if required.

## 10. Problems Encountered & Solutions

### Evidence Integrity

Collected evidence was hashed with SHA-256 and the recorded evidence
files returned `OK` during verification.

### Sensitive Evidence

Because the exposed backup contains confidential information, the
appropriate reporting approach is to preserve the original privately and
publish only redacted evidence.

## 11. Skills Practiced

-   Web Application Penetration Testing
-   Reconnaissance
-   Directory Indexing Assessment
-   Information Disclosure Analysis
-   Authentication Security Review
-   HTTP Analysis
-   Kali Linux
-   cURL
-   grep
-   SHA-256 Evidence Integrity
-   Risk Assessment
-   Vulnerability Reporting
-   Remediation Planning
-   Data Privacy
-   Security Documentation

## Recommended Repository Structure

``` text
networkwalks-B082-week4/
├── README.md
├── evidence/
│   └── redacted-or-nonsensitive-evidence-only
└── screenshots/
    ├── W4-M1/
    ├── W4-M2/
    ├── W4-M3/
    └── W4-M4/
```

## Report Prepared By

**Georges Khoury**\
Cybersecurity & Ethical Hacking Intern\
**Batch:** B082 -- NetworkWalks

## Project Information

-   **Program:** Cybersecurity Program at NetworkWalks
-   **Week:** 04
-   **M1:** Completed
-   **M2:** Completed
-   **M3:** Completed
-   **M4:** Detailed Penetration Testing Report
-   **Environment:** Kali Linux
-   **Repository:** GitHub

------------------------------------------------------------------------

**-End-**
