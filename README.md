# 🇹🇭 Thai Government Agent Skills

AI agent skills for Thai government application security — OWASP ASVS 5.0 Level 1 audit with full compliance mapping to Thai law.

## Skills

### `owasp-asvs-auditor`

Expert Application Security Auditor for Thai Government systems. Conducts systematic OWASP ASVS 5.0 Level 1 audits with compliance mapping to:

- **PDPA** (พ.ร.บ. คุ้มครองข้อมูลส่วนบุคคล พ.ศ. 2562)
- **Cybersecurity Act** (พ.ร.บ. การรักษาความมั่นคงปลอดภัยไซเบอร์ พ.ศ. 2562)
- **Thailand Cloud Security Standard 2567** (ETDA/DGA)
- **ETDA e-Transactions Act**
- **DevSecOps Government Manual (2568–2569)**

#### What It Does

1. Reads all 70 ASVS 5.0 Level 1 requirements from the bundled PDF
2. Evaluates each item against the target codebase using a 5-step Decision Tree
3. Cross-references every finding against Thai laws (PDPA, Cybersecurity Act, Cloud Standard 2567, ETDA)
4. Produces a bilingual (Thai/English) audit report with evidence-based classification

Each of the 70 items is classified as:
- ✅ **PASS** — proof of control (file:line, config value, or framework default)
- ❌ **FAIL** — missing control or verified bypass, with severity + Thai law annotation
- ⚠️ **NEEDS_REVIEW** — insufficient evidence for automated determination
- ⚪ **N/A** — not applicable to the detected tech stack

#### Install

```bash
# Install for all agents (project-level)
npx skills add chiraleo2000/thai-gov-agent-skills@owasp-asvs-auditor

# Install globally
npx skills add chiraleo2000/thai-gov-agent-skills@owasp-asvs-auditor -g

# Install for a specific agent
npx skills add chiraleo2000/thai-gov-agent-skills@owasp-asvs-auditor --agent copilot
```

#### Bundled Assets

| File | Purpose |
|------|---------|
| `OWASP_Application_Security_Verification_Standard_5.0.0_en.pdf` | ASVS 5.0 canonical source (70 L1 requirements) |
| `OWASP_Developer_Guide-V4.1.9.pdf` | OWASP Developer Guide — secure development reference |
| `มาตรฐานด้านการรักษาความมั่นคงปลอดภัยไซเบอร์ระบบคลาวน์ 2567.pdf` | Thailand Cloud Security Standard 2567 |
| `มาตรฐานการรักษาความมั่นคงปลอดภัยเว็บไซต์ พ.ศ. 2568.pdf` | Website Security Standard 2568 |
| `คู่มือทางเทคนิคเชิงลึก_ความมั่นคงปลอดภัยไซเบอร์_DevSecOpsV0.1.pdf` | DevSecOps Technical Deep-Dive Manual |

#### Reference Files

| File | Purpose |
|------|---------|
| `references/REPORT-TEMPLATE.md` | Mandatory audit report skeleton with `{{PLACEHOLDER}}` tokens |
| `references/framework-defaults.md` | Secure defaults for Django, Express, Spring Boot, Laravel, Rails, etc. |
| `references/severity-guidance.md` | Severity classification per ASVS chapter + Thai law escalation rules |
| `references/evidence-patterns.md` | Strict evidence format patterns (no free-form text) |
| `references/th-gov-remediation-guide.md` | Thai-specific remediation: PDPA, Cybersecurity Act, Cloud Standard, ETDA |

#### Usage

Once installed, invoke the skill in your agent chat:

```
/owasp-asvs-auditor  run audit on ./my-project
```

The agent will evaluate all 70 ASVS 5.0 L1 items in strict order, classifying each as ✅ PASS / ❌ FAIL / ⚠️ NEEDS_REVIEW / ⚪ N/A, then produce a bilingual (Thai/English) audit report with Thai law violation annotations.

**Optional parameters:**

| Parameter | Options | Default |
|-----------|---------|---------|
| Agency name | any string | `Thai Government Agency` |
| CII classification | `CII` / `non-CII` | `non-CII` |
| PDPA role | `controller` / `processor` / `both` / `none` | `controller` |
| Cloud deployment | `public` / `private` / `hybrid` / `on-premise` | `none` |
| Report language | `en` / `th` / `bilingual` | `bilingual` |

**Example prompt:**

```
/owasp-asvs-auditor  audit ./my-app
agency: Ministry of Digital Economy
CII: CII
PDPA role: controller
cloud: public
language: bilingual
```

#### Output Report

```
{project}-ASVS-L1-ThaiGov-audit-{YYYY-MM-DD}.md
```

Contains:
- Executive summary with pass/fail/review/N/A breakdown
- 70-row verification control table with status and evidence
- Detailed findings with code-level evidence for ❌ FAIL items
- Thai regulatory compliance section (PDPA / Cybersec Act / Cloud Std / ETDA) in Thai
- Prioritised remediation roadmap (P0–P6 by severity and Thai law impact)

---

## Discover More

🔗 [skills.sh/chiraleo2000/thai-gov-agent-skills](https://skills.sh/chiraleo2000/thai-gov-agent-skills/owasp-asvs-auditor)

---

*Based on OWASP ASVS 5.0.0 (May 2025) — CC BY-SA 4.0*  
*Extended for Thai Government compliance*  
*พัฒนาสำหรับมาตรฐานความมั่นคงปลอดภัยของระบบสารสนเทศภาครัฐไทย*  
*Version 1.1.1 | March 2026*
