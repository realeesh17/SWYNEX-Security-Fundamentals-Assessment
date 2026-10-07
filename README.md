# SWYNEX-Security-Fundamentals-Assessment

Security assessment of an authorized OWASP Juice Shop lab using Nmap and OWASP ZAP.

---

## Overview

This repository contains my **Task 1: Security Fundamentals Assessment** for the SWYNEX Cyber Security Internship.

I assessed an intentionally vulnerable web application (**OWASP Juice Shop**) running in my own isolated lab. I used reconnaissance and passive scanning to find security weaknesses, then documented each one with its affected component, risk, evidence, and recommended mitigation.

> All testing was done only on a local, self-owned training environment (`127.0.0.1`). No external or unauthorized system was tested.

---

## Full Report

**[Read the full security assessment report](report/Security-Assessment-Report.md)**

---

## Summary of Findings

| # | Finding | Risk | Confidence |
|---|---------|------|------------|
| 1 | Missing Anti-clickjacking Header | Medium | Medium |
| 2 | Content Security Policy (CSP) Header Not Set | Medium | High |
| 3 | X-Content-Type-Options Header Missing | Low | Medium |

**Result:** 2 Medium-risk and 1 Low-risk finding. All are HTTP security-header misconfigurations that can be fixed with simple server settings.

---

## Lab Environment

| Component | Purpose |
|-----------|---------|
| VMware | Isolated virtual lab |
| Kali Linux | Security testing platform |
| Docker | Runs OWASP Juice Shop |
| Nmap | Port and service reconnaissance |
| cURL | HTTP header analysis |
| OWASP ZAP | Passive web security scanning |

---

## Methodology

1. **Setup:** Deployed OWASP Juice Shop in Docker inside a Kali Linux VM.
2. **Reconnaissance:** Used Nmap to confirm TCP port `3000` was open on `127.0.0.1`.
3. **HTTP analysis:** Used `curl -I` to inspect response headers.
4. **Passive scanning:** Used OWASP ZAP to find missing security headers.
5. **Documentation:** Recorded each finding with evidence, risk rating, and mitigation.

---

## Recommended Fixes

```http
X-Frame-Options: SAMEORIGIN
Content-Security-Policy: default-src 'self'
X-Content-Type-Options: nosniff
```

The CSP should be adjusted to the application's real script, style, and image needs before use in production.

---

## Repository Structure

```text
SWYNEX-Security-Fundamentals-Assessment/
├── README.md
├── report/
│   └── Security-Assessment-Report.md
└── evidence/
    ├── 01-juice-shop-running.png
    ├── 02-nmap-reconnaissance.png
    ├── 03-http-header-analysis.png
    ├── 04-zap-anti-clickjacking.png.png
    ├── 05-zap-csp.png
    └── 6-zap-x-content-type-options.png.png
```

---

## Skills Demonstrated

- Network reconnaissance with Nmap
- HTTP security-header analysis
- Passive vulnerability scanning with OWASP ZAP
- Risk rating using confidence, CWE, and WASC references
- Evidence collection and professional security reporting
- Working ethically inside an authorized scope

---

## Author

**Rakesh Babriya**
Computer Science Engineering student (IoT, Blockchain, Cybersecurity)

- GitHub: [realeesh17](https://github.com/realeesh17)
- LinkedIn: [rakeshbabriya](https://www.linkedin.com/in/rakeshbabriya)

---

## Disclaimer

This project is for **educational purposes only**. Only test systems you own or have explicit written permission to test.