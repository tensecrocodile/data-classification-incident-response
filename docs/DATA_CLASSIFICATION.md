# Data Classification Framework

Complete guide to classifying data across your organization with clear examples and handling rules.

## Classification Levels Overview

### 1. Public Data

**Definition:** Information that is intentionally and safely released to the public with no confidentiality requirements.

**Characteristics:**
- No business sensitivity
- No regulatory restrictions
- Safe for public disclosure
- Already publicly available or intended for release

**Examples:**
- Marketing website content
- Published blog posts and articles
- Press releases
- Open-source code
- Public API documentation
- Company branding materials
- General job postings

**Handling Requirements:**
- Version control in public repositories
- Integrity verification via Git commit signatures
- No authentication required for access
- Read-only in production systems
- Standard change management process

**Storage Locations:**
- Public GitHub/GitLab repositories
- Public websites and CDNs
- Any publicly accessible platform

**Sharing Rules:**
- Can be shared widely
- No restrictions on distribution
- Ensure authenticity and integrity

---

### 2. Internal Data

**Definition:** Business information for internal use only, with no confidentiality requirements but limited to authorized staff.

**Characteristics:**
- Non-sensitive operational data
- For internal staff use only
- No regulatory restrictions
- Not meant for external parties

**Examples:**
- Internal process documentation and SOPs
- Team meeting notes (non-confidential)
- Non-confidential project plans
- Internal metrics and dashboards
- HR policies (non-salary information)
- Internal communications and memos
- Anonymous usage statistics

**Handling Requirements:**
- Single Sign-On (SSO) authentication required
- Access restricted to company email accounts
- External sharing prohibited without explicit NDA
- Basic access logging and audit trail
- No distribution outside organization
- Internal drives or secure wikis only

**Storage Locations:**
- Internal document management systems
- Private repositories with SSO access
- Internal wikis and knowledge bases
- Company intranet

**Sharing Rules:**
- Only to org-managed accounts
- Log all access
- No external sharing without NDA
- Basic access review quarterly

**Access Control:**
```
Default: ALLOW company employees
Required: Company email (@yourcompany.com)
External: Requires written NDA
```

---

### 3. Confidential Data

**Definition:** Sensitive business information that could cause harm if disclosed. Requires strong protection measures.

**Characteristics:**
- Business-critical information
- Could damage business if leaked
- Could harm customer relationships
- Requires need-to-know access
- Regulatory handling may apply

**Examples:**
- Customer contracts and agreements
- Security runbooks and incident procedures
- Non-public financial information
- Product roadmaps
- API configurations and endpoints
- Vendor pricing and agreements
- System architecture documentation
- Business strategies
- Customer data (non-PII)
- Third-party integration details

**Handling Requirements:**
- Multi-factor authentication (MFA) mandatory
- Encryption at rest (AES-256 or equivalent)
- Encryption in transit (TLS 1.2+)
- Need-to-know access control
- Individual access approval required
- No personal email accounts
- No unsecured cloud storage
- Data Loss Prevention (DLP) rules enforced
- Download logging and monitoring
- Quarterly access reviews
- Watermarking on exports

**Storage Locations:**
- Encrypted internal drives
- Private repositories with MFA
- Secure document management systems
- Ticketing systems with audit trails
- Encrypted backup systems

**Sharing Rules:**
- Only to specific named individuals
- Requires manager approval
- NDA required for external parties
- Log all access and downloads
- Monitor for unauthorized sharing
- Watermark or mark as "CONFIDENTIAL"

**Access Control:**
```
Default: DENY all access
Required: Individual approval + NDA
Authentication: MFA mandatory
Monitoring: Log all access
Review: Quarterly
```

---

### 4. Highly Confidential Data

**Definition:** Mission-critical or regulated data where compromise causes severe damage. Maximum security controls required.

**Characteristics:**
- Extreme business sensitivity
- Regulatory/legal requirements
- Personal identifiable information (PII)
- Production system access credentials
- Severe consequences if compromised
- Very limited authorized personnel

**Examples:**
- Production database backups with customer PII
- Encryption keys and master passwords
- API keys and authentication tokens
- Private signing keys
- Full customer datasets with PII
- Incident investigation evidence
- Payment card data
- Social security numbers
- Medical records
- Security vulnerability details
- Zero-day exploit information

**Handling Requirements:**
- Hardware-backed MFA (FIDO2 keys)
- Only on approved secured workstations
- Encrypted vault or secrets manager (HashiCorp Vault, AWS KMS, etc.)
- Strict role-based access control (RBAC)
- Dual approval for any access
- Comprehensive audit logging
- No local storage allowed
- Access via VPN and bastion hosts only
- Monthly access reviews (not quarterly)
- Immutable evidence trail
- Break-glass procedures documented
- Segregated network or air-gapped systems

**Storage Locations:**
- Hardware Security Modules (HSM)
- Encrypted vault systems
- Isolated secure storage
- Cold backup in secure facility
- Never on personal devices

**Sharing Rules:**
- Only to security/infra leadership
- Executive approval required
- Only via secure channels (VPN + E2EE)
- No email transmission
- Secure messenger only (Signal, Wire)
- Dual approval for any export
- Track every access and export

**Access Control:**
```
Default: DENY all access
Required: Dual approval + break-glass procedure
Authentication: Hardware-backed MFA
Monitoring: Real-time logging + alerts
Review: Monthly + unexpected access alerts
Network: VPN + bastion only
```

---

## Data Classification Decision Tree

```
┌─ Can this be safely shared publicly?
│  YES → PUBLIC
│  NO ↓
├─ Is this for internal business operations only?
│  YES → INTERNAL
│  NO ↓
├─ Could disclosure cause significant business or customer harm?
│  YES → CONFIDENTIAL
│  NO → INTERNAL
│
├─ Is this PII, credentials, encryption keys, or mission-critical data?
│  YES → HIGHLY CONFIDENTIAL
│  NO ↓
├─ Are there regulatory/legal requirements (GDPR, PCI-DSS, HIPAA, etc.)?
│  YES → HIGHLY CONFIDENTIAL
│  NO ↓
└─ Is this production system data, secrets, or incident evidence?
   YES → HIGHLY CONFIDENTIAL
   NO → CONFIDENTIAL (unless answered YES to INTERNAL above)
```

---

## Classification Responsibilities

### Data Owners
- Classify all data in their purview
- Review classifications quarterly
- Approve access requests
- Communicate classification to team

### Data Custodians (IT/Security)
- Implement controls per classification
- Monitor and log access
- Audit compliance quarterly
- Update tools and processes

### All Employees
- Understand classification of their data
- Follow handling rules
- Report misclassified data
- Participate in training

---

## Escalation Workflow

If unsure of classification:

1. **Default to higher classification** - When in doubt, classify higher
2. **Ask data owner** - Contact department owner
3. **Consult security team** - Email security@yourcompany.com
4. **Document the decision** - Record why a classification was chosen

---

## Reclassification

Data can be reclassified when:
- Business circumstances change
- Data is no longer sensitive (e.g., after embargo period)
- New regulatory requirements apply
- Data maturity changes (e.g., after public release)

**Process:**
1. Document reason for reclassification
2. Get data owner approval
3. Notify security team
4. Update storage location if needed
5. Update access controls
6. Log in data classification inventory

---

## Compliance Mapping

| Regulation | Data Type | Minimum Classification |
|---|---|---|
| GDPR | Personal data | Highly Confidential |
| PCI-DSS | Payment card data | Highly Confidential |
| HIPAA | Health information | Highly Confidential |
| SOC 2 | Customer data | Confidential |
| ISO 27001 | Any sensitive data | Confidential |

---

## Related Policies

- [Incident Response Plan](INCIDENT_RESPONSE.md)
- [Data Retention Policy](../config/retention_policy.yaml)
- [Access Control Policy](../config/access_control.yaml)
- [DLP Rules](DLP_RULES.md)
