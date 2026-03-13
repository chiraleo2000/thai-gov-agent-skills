# Evidence Format Patterns# Evidence Format Patterns


























































6. **FAIL items need TWO fields** — both `evidence` (what's wrong) and optionally `thai_law` (legal impact)5. **PASS items need proof** — point to the specific file, config, or framework default4. **N/A items need evidence** — explain WHY it's not applicable using `feature:` or `techstack:`3. **Line numbers are 1-based** — reference the specific line where the control/issue exists2. **Paths are relative** — always use paths relative to TARGET_REPO root1. **No free-form text** — every evidence field must match one of the patterns above## Rules---```Thai Law: thai_law:PDPA:มาตรา_26, thai_law:PDPA:มาตรา_37```Multiple Thai law violations for one finding:```Thai Law: thai_law:PDPA:มาตรา_37Evidence: missing:encryption_at_rest_for_personal_data```When a FAIL finding also triggers a Thai law violation, combine them:## Combining Evidence with Thai Law---| Multiple files | `file:<path1>:<line1>, file:<path2>:<line2>` | Control verified across multiple files | `file:src/auth.py:10, file:src/middleware.py:25` || Large file note | `sampled:<path>:first500+last100` | File exceeds 500 lines | `sampled:src/app.js:first500+last100` || Monorepo prefix | `[<component>] <evidence>` | Multi-component repository | `[api-service] file:src/auth.go:15` ||--------|---------|-------------|---------|| Format | Pattern | When to Use | Example |## Modifier Formats---| Thai law ref | `thai_law:<act>:มาตรา_<n>` | Thai law violation annotation | `thai_law:PDPA:มาตรา_37` || Unclear evidence | `unclear:<path>:<line>` | Evidence inconclusive | `unclear:src/middleware.js:120` || Bypass found | `bypass:<path>:<line>` | Framework default bypassed | `bypass:src/api/auth.py:88` || Missing control | `missing:<description>` | Required control is absent | `missing:rate_limiting_on_login` || Tech stack N/A | `techstack:<expected>:not_in_use` | Requirement not applicable to tech stack | `techstack:java:not_in_use` || Feature absent | `feature:<name>:not_utilized` | Feature doesn't exist in codebase | `feature:file_upload:not_utilized` || Config evidence | `config:<path>:<key>=<value>` | Control verified via configuration | `config:settings.py:SESSION_COOKIE_SECURE=True` || Framework default | `framework:<name>:<feature>` | Satisfied by framework secure default | `framework:django:CSRF_middleware` || Code location | `file:<path>:<line>` | Control found in source code | `file:src/auth/login.py:42` ||--------|---------|-------------|---------|| Format | Pattern | When to Use | Example |## Primary Evidence Formats---No free-form text is permitted in evidence fields.All evidence fields in the Verification Control Table must use one of these exact formats.
All evidence fields in the Verification Control Table must use one of these exact formats.
No free-form text is permitted in evidence fields.

---

## Primary Evidence Formats

| Format | Pattern | When to Use | Example |
|--------|---------|-------------|---------|
| Code location | `file:<path>:<line>` | Control found in source code | `file:src/auth/login.py:42` |
| Framework default | `framework:<name>:<feature>` | Satisfied by framework secure default | `framework:django:CSRF_middleware` |
| Config evidence | `config:<path>:<key>=<value>` | Control verified via configuration | `config:settings.py:SESSION_COOKIE_SECURE=True` |
| Feature absent | `feature:<name>:not_utilized` | Feature doesn't exist in codebase | `feature:file_upload:not_utilized` |
| Tech stack N/A | `techstack:<expected>:not_in_use` | Requirement not applicable to tech stack | `techstack:java:not_in_use` |
| Missing control | `missing:<description>` | Required control is absent | `missing:rate_limiting_on_login` |
| Bypass found | `bypass:<path>:<line>` | Framework default bypassed | `bypass:src/api/auth.py:88` |
| Unclear evidence | `unclear:<path>:<line>` | Evidence inconclusive | `unclear:src/middleware.js:120` |
| Thai law ref | `thai_law:<act>:มาตรา_<n>` | Thai law violation annotation | `thai_law:PDPA:มาตรา_37` |

---

## Modifier Formats

| Format | Pattern | When to Use | Example |
|--------|---------|-------------|---------|
| Monorepo prefix | `[<component>] <evidence>` | Multi-component repository | `[api-service] file:src/auth.go:15` |
| Large file note | `sampled:<path>:first500+last100` | File exceeds 500 lines | `sampled:src/app.js:first500+last100` |
| Multiple files | `file:<path1>:<line1>, file:<path2>:<line2>` | Control verified across multiple files | `file:src/auth.py:10, file:src/middleware.py:25` |

---

## Combining Evidence with Thai Law

When a FAIL finding also triggers a Thai law violation, combine them:

```
Evidence: missing:encryption_at_rest_for_personal_data
Thai Law: thai_law:PDPA:มาตรา_37
```

Multiple Thai law violations for one finding:

```
Thai Law: thai_law:PDPA:มาตรา_26, thai_law:PDPA:มาตรา_37
```

---

## Rules

1. **No free-form text** — every evidence field must match one of the patterns above
2. **Paths are relative** — always use paths relative to TARGET_REPO root
3. **Line numbers are 1-based** — reference the specific line where the control/issue exists
4. **N/A items need evidence** — explain WHY it's not applicable using `feature:` or `techstack:`
5. **PASS items need proof** — point to the specific file, config, or framework default
6. **FAIL items need TWO fields** — both `evidence` (what's wrong) and optionally `thai_law` (legal impact)
