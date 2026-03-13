# How to Install & Use the OWASP ASVS Auditor Skill

## Prerequisites

- [Node.js](https://nodejs.org/) 18+ installed
- An AI agent that supports skills (GitHub Copilot, Claude Code, Gemini CLI, Cursor, etc.)
- Full read access to the target repository you want to audit

---

## Install

### Project-level (recommended for team audits)

```bash
npx skills add chiraleo2000/thai-gov-agent-skills@owasp-asvs-auditor
```

Installs the skill into `.agents/skills/` in your current project directory. Only active when working inside that project.

### Global (available in every project)

```bash
npx skills add chiraleo2000/thai-gov-agent-skills@owasp-asvs-auditor -g
```

### Specific agent only

```bash
# GitHub Copilot only
npx skills add chiraleo2000/thai-gov-agent-skills@owasp-asvs-auditor --agent copilot

# Claude Code only
npx skills add chiraleo2000/thai-gov-agent-skills@owasp-asvs-auditor --agent claude-code
```

---

## What Gets Installed

After installation, the skill directory contains:

```
owasp-asvs-auditor/
├── SKILL.md                          ← Skill definition (loaded by the agent)
├── owasp_asvs_auditor.agent.md       ← GitHub Copilot custom agent definition
├── assets/                           ← Bundled PDF references (5 files)
│   ├── OWASP_Application_Security_Verification_Standard_5.0.0_en.pdf
│   ├── OWASP_Developer_Guide-V4.1.9.pdf
│   ├── คู่มือทางเทคนิคเชิงลึก_ความมั่นคงปลอดภัยไซเบอร์_DevSecOpsV0.1.pdf
│   ├── มาตรฐานการรักษาความมั่นคงปลอดภัยเว็บไซต์ พ.ศ. 2568.pdf
│   └── มาตรฐานด้านการรักษาความมั่นคงปลอดภัยไซเบอร์ระบบคลาวน์ 2567.pdf
└── references/                       ← Supporting reference files (5 files)
    ├── REPORT-TEMPLATE.md
    ├── framework-defaults.md
    ├── severity-guidance.md
    ├── evidence-patterns.md
    └── th-gov-remediation-guide.md
```

---

## Use

Once installed, open your agent chat and run:

```
/owasp-asvs-auditor  audit the project at ./my-app
```

### Quick Start (Default Settings)

```
/owasp-asvs-auditor  run audit on ./my-project
```

Uses defaults: non-CII, PDPA controller, no cloud, bilingual output.

### Full Configuration

```
/owasp-asvs-auditor  audit ./my-app
agency: Ministry of Digital Economy
CII: CII
PDPA role: controller
cloud: public
language: bilingual
```

### Optional Parameters

| Parameter | Options | Default | Purpose |
|-----------|---------|---------|---------|
| Agency name | any string | `Thai Government Agency` | Appears in report header |
| CII classification | `CII` / `non-CII` | `non-CII` | Triggers Cybersecurity Act overlays (มาตรา 45, 56) |
| PDPA role | `controller` / `processor` / `both` / `none` | `controller` | Triggers PDPA overlays (มาตรา 26, 37, 40, 41) |
| Cloud deployment | `public` / `private` / `hybrid` / `on-premise` / `none` | `none` | Triggers Cloud Standard 2567 overlays |
| Report language | `en` / `th` / `bilingual` | `bilingual` | Output language for the report |

---

## How the Audit Works

The agent follows a strict 3-phase process:

### Phase 1 — Setup
- Detects the tech stack (Node.js, Python, Java, PHP, Go, Ruby, Rust)
- Loads all 5 bundled PDFs and 5 reference files
- Assesses Thai regulatory scope based on your CII/PDPA/Cloud settings

### Phase 2 — Evaluation (70 Items)
- Processes all 70 ASVS 5.0 Level 1 items in strict document order
- For each item, applies a 5-step Decision Tree:
  1. **Applicability** — Is this relevant to the tech stack?
  2. **Framework Defaults** — Does the framework handle this securely by default?
  3. **Verify Implementation** — Is a specific control verifiable in code?
  4. **Assign Severity** — How critical is the missing control?
  5. **Thai Law Overlay** — Which Thai laws are impacted?

### Phase 3 — Report Generation
- Fills in the report template with findings
- Sanitises output (removes any accidental secrets/PII)
- Writes the final report to disk

---

## Output

The skill produces a report file in the project root:

```
{project-name}-ASVS-L1-ThaiGov-audit-{YYYY-MM-DD}.md
```

The report contains:

- **Executive Summary** — pass/fail/needs-review/N/A counts
- **Verification Control Table** — all 70 ASVS L1 items with status and evidence
- **Findings Detail** — code-level evidence and remediation for every ❌ FAIL
- **Thai Regulatory Section** — findings mapped to PDPA / Cybersecurity Act / Cloud Standard 2567 / ETDA (bilingual)
- **Remediation Roadmap** — prioritised P0–P6 by severity and Thai law impact

---

## Manage

```bash
# List installed skills
npx skills list

# Check for updates
npx skills check

# Update to latest version
npx skills update

# Remove the skill
npx skills remove owasp-asvs-auditor
```

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Skill not found after install | Run `npx skills list` to verify installation path |
| Agent doesn't recognise the skill | Restart the agent or reload the workspace |
| PDFs not loading | Ensure the `assets/` directory is intact — re-install if needed |
| Thai characters display incorrectly | Ensure your terminal/editor supports UTF-8 |
| Report is `[PARTIAL]` | The agent ran out of context — re-run on a smaller codebase or specific subdirectory |

---

## Links

- **GitHub**: https://github.com/chiraleo2000/thai-gov-agent-skills
- **skills.sh**: https://skills.sh/chiraleo2000/thai-gov-agent-skills/owasp-asvs-auditor
