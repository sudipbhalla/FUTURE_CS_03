# FUTURE_CS_03 - API Security Risk Analysis

**Author:** Sudip Bhalla
**Program:** Future Interns - Cyber Security Internship, Task 3
**Date:** 11 October 2026

A read-only API security risk analysis of public demo APIs, written as a client-style report for a SaaS company.

## Report

[API_Security_Risk_Analysis_Report.pdf](report/API_Security_Risk_Analysis_Report.pdf)

## Targets

| API | Activity |
|---|---|
| JSONPlaceholder (https://jsonplaceholder.typicode.com) | Live read-only testing |
| ReqRes (https://reqres.in) | Documentation review only (no requests sent) |

## Tools used

- Insomnia 13.3.1 (API requests and response inspection)
- Browser DevTools, Network tab (response headers, HTTPS and CORS checks)
- reportlab (PDF report generation)

## Scope and ethics

- Only public demo APIs built for testing and learning were used.
- Read-only requests only: three GET requests, one safe POST to a service that simulates writes, and one HTTP-to-HTTPS redirect check.
- No exploitation, bypass attempts, brute-forcing, credential guessing, flooding or load testing.
- Rate limiting was assessed from response headers only.
- No login or magic-link requests were sent to ReqRes.
- Severity ratings describe the impact **if the same behavior existed in a production SaaS API**. The demo APIs return fake data and are not vulnerable in their intended design.

## Methodology

1. Sent requests with no Authorization header and confirmed no auth was configured.
2. Reviewed status codes, bodies and headers in Insomnia and browser DevTools.
3. Compared single-object requests to check for sequential IDs and visible ownership checks.
4. Checked HTTPS enforcement by requesting the plain `http://` address.
5. Sent one POST with wrong types, script-like text and an unexpected field to observe validation.
6. Reviewed the ReqRes credential model (private key, public key, session token) from its documentation.
7. Mapped findings to the OWASP API Security Top 10 (2023) and rated each as Low / Medium / High.

## Findings summary

| ID | Finding | Severity (if production) |
|---|---|---|
| F1 | Unauthenticated access to user data | High |
| F2 | Excessive data exposure | High |
| F3 | Sequential IDs, no visible ownership check | High |
| F4 | No pagination or response size limit | Medium |
| F5 | Extremely high rate limit | Medium |
| F6 | Missing HSTS, CSP, X-Frame-Options | Medium |
| F7 | Technology disclosure (`x-powered-by`) | Low |
| F8 | Conflicting cache directives | Low |
| F9 | No input validation | Medium |
| I1 | CORS policy not fully assessed | Informational |

The report also lists positive controls (HTTPS enforced, `nosniff`, rate limit headers) and a design-level review of ReqRes authentication.

## Repository structure

```
FUTURE_CS_03/
├── README.md
├── report/
│   └── API_Security_Risk_Analysis_Report.pdf
└── screenshots/
    ├── 01_users_list_unauthenticated.png
    ├── 02_user1_data_exposure.png
    ├── 03a_response_headers.png
    ├── 03b_response_headers.png
    ├── 04_posts_unauthenticated.png
    ├── 05_cors_headers.png
    └── 06_input_validation.png
```

## References

- OWASP API Security Top 10: https://github.com/OWASP/API-Security
- API Security Checklist: https://github.com/shieldfy/API-Security-Checklist
- Public APIs list: https://github.com/public-apis/public-apis
