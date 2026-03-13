# Thai Government Remediation Guide
# คู่มือการแก้ไขช่องโหว่สำหรับระบบภาครัฐไทย

---

## 1. PDPA — พ.ร.บ. คุ้มครองข้อมูลส่วนบุคคล พ.ศ. 2562

### มาตรา 26 — Sensitive Data (ข้อมูลอ่อนไหว)

**Trigger**: Personal data categorised as sensitive (health, biometric, religion, criminal record, union membership, political opinion, sexual orientation, disability, genetic data, Thai National ID).

**Required Remediation**:
1. Obtain explicit consent before collection/processing
2. Encrypt at rest using AES-256-GCM or equivalent
3. Apply field-level access control (not just table-level)
4. Enable audit logging for all access to sensitive fields
5. Implement data masking in non-production environments

**Thai National ID (เลขบัตรประชาชน) Specific**:
- Must NOT be stored in plain text
- Hash with HMAC-SHA256 for lookup, encrypt for display
- Mask display: `X-XXXX-XXXXX-XX-X` → show only last 4 digits
- Never use as primary key or session identifier

---

### มาตรา 37 — Security Measures (มาตรการรักษาความมั่นคงปลอดภัย)

**Required Remediation by ASVS Chapter**:

| ASVS Chapter | Remediation Action |
|--------------|-------------------|
| V1 (Injection) | Implement parameterised queries and output encoding for all personal data |
| V6 (Auth) | MFA for systems managing personal data of > 5,000 data subjects |
| V7 (Session) | Session timeout ≤ 30 minutes for personal data access screens |
| V11 (Crypto) | TLS 1.2+ for transit, AES-256 for rest, key rotation every 365 days |
| V14 (Data Protection) | Data classification, retention policies, purpose limitation controls |

---

### มาตรา 40 — Breach Notification (แจ้งเตือนเหตุละเมิดข้อมูล)

**Required Capabilities**:
1. Detect breach within 72 hours (V16 logging must enable this)
2. Notify PDPC within 72 hours of detection
3. Notify data subjects "without delay" if high risk
4. Maintain breach register with: timestamp, description, data types, number of records, containment actions

**Implementation Checklist**:
- [ ] Centralised logging (SIEM or equivalent)
- [ ] Alerting on anomalous data access patterns
- [ ] Pre-drafted notification templates (Thai/English)
- [ ] Designated Data Protection Officer (DPO) contact info
- [ ] Breach response runbook documented and tested

---

### มาตรา 41 — Data Subject Rights (สิทธิเจ้าของข้อมูล)

**Required Features**:
- Right to access: API/UI to export personal data (JSON/CSV)
- Right to correction: Allow data subjects to update their data
- Right to deletion: Soft-delete with 30-day retention, then hard-delete
- Right to portability: Machine-readable export format
- Right to object: Opt-out mechanism for data processing
- Response SLA: 30 days from receipt of request

---

## 2. Cybersecurity Act — พ.ร.บ. การรักษาความมั่นคงปลอดภัยไซเบอร์ พ.ศ. 2562

### มาตรา 45 — CII Security Standards

**Applies when**: `CII_CLASS = CII` (Critical Information Infrastructure)

**Baseline Requirements**:

| Control Area | Requirement |
|-------------|-------------|
| Access Control | Role-based access, least privilege, MFA for admin |
| Encryption | TLS 1.2+ transit, AES-256 at rest |
| Configuration | CIS Benchmark compliance or equivalent hardening |
| Vulnerability Management | Scan monthly, patch critical within 30 days |
| Network Security | Segmentation, WAF for public-facing applications |
| Change Management | Approved change process, rollback capability |

---

### มาตรา 56 — Incident Reporting

**Requirements**:
1. Report to NCSA within 24 hours of detection (not 72 hours like PDPA)
2. Maintain incident timeline with UTC+7 timestamps
3. Classify severity: Critical / High / Medium / Low
4. Provide root cause analysis within 30 days
5. Corrective actions implemented and verified

**Logging Requirements for Compliance**:
- Minimum 90-day online log retention
- Minimum 1-year archived log retention
- Include: who, what, when, where, outcome
- Tamper-evident log storage (append-only / signed)

---

## 3. Cloud Security Standard 2567 (มาตรฐานความมั่นคงปลอดภัยคลาวด์ 2567)

### Data Residency (การจัดเก็บข้อมูลในประเทศ)

**Rule**: Personal data of Thai citizens must be stored in Thailand or in
jurisdictions with adequate data protection laws (as determined by PDPC).

**Remediation**:
- Configure cloud provider region to `ap-southeast-1` (Singapore) or Thai region if available
- For AWS: use `ap-southeast-1` with S3 bucket policy restricting region
- For Azure: use `Southeast Asia` region
- For GCP: use `asia-southeast1`
- Document data flow diagram showing where data resides

### Encryption

| Layer | Requirement | Implementation |
|-------|-------------|----------------|
| Transit | TLS 1.2+ mandatory | Configure `min_tls_version = "1.2"` |
| At rest | AES-256-GCM | Enable provider-managed encryption + customer-managed keys for sensitive data |
| Key management | Rotate keys annually | Use cloud KMS with automatic rotation |

### Multi-tenancy Isolation

- Logical isolation at minimum (separate databases/schemas per tenant)
- Network isolation via VPC/VNET with security groups
- No shared service accounts across tenants

### Logging & Monitoring

- Cloud audit logs enabled (CloudTrail / Azure Monitor / Cloud Audit Logs)
- 90-day online retention minimum
- Export to SIEM for correlation
- Alert on: privilege escalation, data export > threshold, config changes

---

## 4. ETDA — สำนักงานพัฒนาธุรกรรมทางอิเล็กทรอนิกส์

### Electronic Transaction Standards

| Standard | Requirement | ASVS Mapping |
|----------|-------------|--------------|
| Digital Signature | Use certificates from ETDA-approved CA | V11 (Crypto) |
| Timestamp | RFC 3161 compliant timestamping | V16 (Logging) |
| Data Integrity | Hash chain or digital signature for critical records | V11 (Crypto) |
| Non-repudiation | Signed audit trail for financial/legal transactions | V16 (Logging) |

### e-Government Interoperability

- Use TH e-GIF standards for data exchange
- API must support Thai Government API Gateway standards
- OAuth 2.0 / OIDC for government SSO integration
- Support Thai National Digital ID (NDID) where applicable

---

## 5. Consolidated Remediation Priority Matrix

| Priority | Trigger | SLA | Remediation Action |
|----------|---------|-----|-------------------|
| P0 🔴 | PDPA มาตรา 26 breach + CII system | 24 hours | Isolate, encrypt, notify NCSA + PDPC |
| P1 🔴 | Auth bypass on any system | 48 hours | Emergency patch, credential reset |
| P2 🟠 | Missing encryption (transit or rest) | 1-2 weeks | Enable TLS, configure encryption at rest |
| P3 🟠 | Insufficient logging on CII | 1-2 weeks | Deploy centralised logging, configure alerts |
| P4 🟡 | PDPA มาตรา 41 rights not implemented | 1-3 months | Build data subject rights API/UI |
| P5 🟡 | Cloud Standard 2567 partial compliance | 1-3 months | Migrate to compliant region, configure controls |
| P6 🟢 | ETDA standards gaps | Next release | Implement digital signatures, timestamping |
