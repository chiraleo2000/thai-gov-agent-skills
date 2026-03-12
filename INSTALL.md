# How to Install & Use the OWASP ASVS Auditor Skill

## Prerequisites

- [Node.js](https://nodejs.org/) 18+ installed
- An AI agent that supports skills (GitHub Copilot, Claude Code, Gemini CLI, Cursor, etc.)

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

## Use

Once installed, open your agent chat and run:

```
/owasp-asvs-auditor  audit the project at ./my-app
```

**Optional parameters you can specify in your prompt:**

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
- **Thai Regulatory Section** — findings mapped to PDPA / Cybersecurity Act / Cloud Standard 2567 / ETDA (Thai language)
- **Remediation Roadmap** — prioritised by severity

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

## Links

- **GitHub**: https://github.com/chiraleo2000/thai-gov-agent-skills
- **skills.sh**: https://skills.sh/chiraleo2000/thai-gov-agent-skills/owasp-asvs-auditor
