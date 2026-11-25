# Mediroza General Hospital -- Week 4 Penetration Testing

**Program:** Networkwalks -- Batch B083, Week 4\
**Assessment type:** Black-box web application penetration test\
**Target:** `https://medirozahospital.com`\
**Date:** 25 November 2026

## Overview

This repository contains documentation and evidence notes from the Week
4 black-box penetration-testing assignment. Testing was limited to the
authorized target and the assignment rules of engagement.

## Scope and restrictions

-   Target domain: `medirozahospital.com`
-   No social engineering.
-   No denial-of-service testing.
-   Do not test systems outside the authorized scope.
-   Handle all report files, credentials, cookies, session identifiers,
    and personal information as confidential.
-   Include only synthetic/test data that the assignment explicitly
    authorizes.

## Work completed / observations

### Reconnaissance

-   DNS A record observed: `199.188.201.16`
-   Name servers observed: `dns1.namecheaphosting.com`,
    `dns2.namecheaphosting.com`
-   Web responses identified LiteSpeed; PHP version disclosure
    (`PHP/8.2.36`) appeared on some login responses.
-   TLS certificate validation succeeded. A TLS 1.3 connection using
    `TLS_AES_256_GCM_SHA384` was observed.
-   Nmap reported some TLS 1.2 CBC and SHA-1-based cipher suites; this
    is recorded as a hardening observation, not a confirmed exploit.

### Patient Portal observation

-   Login endpoint observed: `POST /patient/login.php`
-   Form parameters observed: `username` and `password`
-   Tester-entered username: `admin' --`
-   Tester reported that the portal then displayed the restricted **My
    Lab Reports** page.
-   The page showed three encrypted pathology-report entries.

**Assessment note:** This behaviour is consistent with an SQL-injection
authentication bypass, but the backend SQL query/root cause has not been
independently verified. Confirm with a controlled authorized retest
before treating the implementation cause as proven.

### Encrypted PDF assessment

The tester reported completing the PDF password-recovery stage. The
supplied screenshots show two pathology report documents. Passwords and
patient/clinical details must not be included in this repository or
public posts.

### Remaining / evidence-dependent objective

The assignment also includes investigating staff salary and shareholder
information. Add a result only when supported by authorized evidence. Do
not claim this objective is complete based on the current README.

## Suggested evidence structure

``` text
evidence/
├── E01-recon/
├── E02-patient-login/
├── E03-report-listing/
├── E04-pdf-encryption/
└── E05-further-discovery/
```

Before adding evidence, redact: - Patient names, IDs, dates of birth,
and clinical results - Passwords and credentials - `Cookie`,
`Set-Cookie`, session IDs, and authorization tokens - Any information
not required to demonstrate the finding

## Tools used

-   Burp Suite
-   Nmap
-   `whois`
-   `dig` / `nslookup`
-   `curl`
-   OpenSSL

## Key remediation recommendations

1.  Use parameterized SQL queries / prepared statements.
2.  Enforce server-side authentication and per-record authorization for
    every report request.
3.  Use least-privilege database accounts and generic login errors.
4.  Use strong, unique PDF passwords and distribute them through a
    separate secure channel.
5.  Review authentication and report-download logs.
6.  Review TLS cipher configuration and HTTP security headers.
7.  Retest after remediation.

## Disclaimer

This document is an assignment summary, not a certification of complete
security. Findings are limited to the observations and evidence
collected during the authorized assessment. Do not use these techniques
against systems without explicit written authorization.
