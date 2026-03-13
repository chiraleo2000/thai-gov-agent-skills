# Severity Classification Guidance

Reference for Decision Tree Step 4 — Assign severity to ❌ FAIL items.

---

## Severity Levels

| Level | Label | Description | SLA (Recommended) |
|-------|-------|-------------|--------------------|
| 🔴 | **Critical** | Immediate exploitation risk; data breach likely; complete auth/authz bypass | Fix within 24–48 hours |
| 🟠 | **High** | Significant security weakness; exploitation requires minimal skill | Fix within 1–2 weeks |
| 🟡 | **Medium** | Moderate risk; exploitation requires specific conditions or chained attacks | Fix within 1–3 months |
| 🟢 | **Low** | Minor weakness; defence-in-depth improvement; limited exploitation potential | Fix within next release cycle |

---

## ASVS Chapter Baseline Severity

| Chapter | Baseline Severity | Rationale |
|---------|-------------------|-----------|
| V1 — Encoding & Sanitization | High | Injection attacks (SQLi, XSS) can lead to full compromise |
| V2 — Validation & Business Logic | Medium–High | Logic flaws can bypass controls; severity depends on context |
| V3 — Web Frontend Security | Medium | Client-side protections; mitigated by server-side controls |
| V4 — API & Web Service | High | API exposure can leak data at scale |
| V5 — File Handling | High | File uploads can enable RCE, path traversal |
| V6 — Authentication | **Critical** | Auth bypass = full system compromise |
| V7 — Session Management | High | Session hijacking leads to account takeover |
| V8 — Authorization | **Critical** | Authz bypass = access to any resource |
| V9 — Self-contained Tokens | High | Token manipulation can impersonate users |
| V10 — OAuth & OIDC | High | OAuth misconfiguration enables account takeover |
| V11 — Cryptography | **Critical** | Weak crypto = data exposure at rest |
| V12 — Secure Communication | High | TLS failures expose data in transit |
| V13 — Configuration | High | Misconfig can expose secrets, enable debug access |
| V14 — Data Protection | **Critical** | PII/personal data exposure has legal consequences |
| V15 — Secure Coding & Architecture | Medium | Architectural weaknesses; usually not directly exploitable |
| V16 — Security Logging | High | Missing logging prevents breach detection |
| V17 — WebRTC | Medium | Limited attack surface in most applications |

---

## Thai Law Severity Escalation Rules

### CII Escalation (Cybersecurity Act พ.ร.บ. ไซเบอร์ฯ)

When `CII_CLASS = CII`, escalate severity **one level up** for failures in:
- V6 (Authentication) — มาตรา 45 requires strong access controls
- V7 (Session Management) — มาตรา 45
- V8 (Authorization) — มาตรา 45
- V11 (Cryptography) — มาตรา 45 mandates encryption
- V12 (Secure Communication) — มาตรา 45 (TLS mandatory)
- V13 (Configuration) — มาตรา 45 (secure configuration)
- V16 (Logging) — มาตรา 56 (incident detection and 24-hour reporting)

### PDPA Escalation

When personal data is involved in the finding:
- V1 (Injection) → +PDPA มาตรา 37 if personal data in injected payload
- V4 (API) → +PDPA มาตรา 37 for API data exposure affecting personal data
- V5 (Files) → +PDPA มาตรา 37 for personal data in uploaded/downloaded files
- V6 (Auth) → +PDPA มาตรา 37 (access to personal data)
- V7 (Session) → +PDPA มาตรา 37
- V11 (Crypto) → +PDPA มาตรา 37 (encryption of personal data at rest)
- V14 (Data Protection):
  - Thai National ID → +PDPA มาตรา 26 (sensitive data) + มาตรา 37
  - Breach detection missing → +PDPA มาตรา 40 (72-hour breach notification)
  - Data subject rights → +PDPA มาตรา 41
- V16 (Logging) → +PDPA มาตรา 40 (breach detection capability)

### Cloud Standard 2567 Escalation

When `CLOUD_DEPLOY ≠ none` and `≠ on-premise`:
- V12 findings → TLS 1.2/1.3 mandatory
- V11 findings → AES-256-GCM required for data at rest
- V16 findings → 90-day log retention required
- V14 findings → Data residency checks (data must remain in Thailand or approved jurisdictions)

---

## Severity Decision Matrix

```
Is authentication/authorization completely bypassable?
├─ YES → 🔴 Critical
└─ NO
   ├─ Can attacker access other users' data without privilege?
   │  ├─ YES → 🔴 Critical
   │  └─ NO → continue
   ├─ Can attacker execute arbitrary code or commands?
   │  ├─ YES → 🔴 Critical
   │  └─ NO → continue
   ├─ Can attacker read/modify sensitive data (PII, credentials)?
   │  ├─ YES → 🟠 High
   │  └─ NO → continue
   ├─ Is the weakness exploitable without user interaction?
   │  ├─ YES → 🟠 High
   │  └─ NO → continue
   ├─ Does exploitation require chaining multiple vulnerabilities?
   │  ├─ YES → 🟡 Medium
   │  └─ NO → continue
   ├─ Is this a defence-in-depth improvement only?
   │  ├─ YES → 🟢 Low
   │  └─ NO → 🟡 Medium (default)
```
