# Data Classification & Incident Response System

> Enterprise-grade data classification framework and incident response system with comprehensive documentation, playbooks, and automated tooling for data security compliance.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

## 📋 Table of Contents

- [Overview](#overview)
- [Data Classification Map](#data-classification-map)
- [Incident Response Framework](#incident-response-framework)
- [Getting Started](#getting-started)
- [Documentation](#documentation)
- [Tools & Automation](#tools--automation)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

This repository provides a complete, production-ready framework for:

✅ **Data Classification** - Standardized 4-level classification with clear handling rules  
✅ **Incident Response** - NIST-aligned IR processes with playbooks and automation  
✅ **Compliance** - SOC 2, ISO 27001, and regulatory requirements built-in  
✅ **Security Controls** - DLP, encryption, access controls, and audit logging  
✅ **Automation** - Python tools, detection rules, and response workflows  

## 🏛️ Data Classification Map

Four standardized classification levels with explicit handling rules:

### Public
**Description:** Information safe to disclose externally  
**Examples:** Website content, published docs, marketing materials  
**Controls:**
- No login required
- Integrity checks via Git/CI
- Read-only in production
- Version controlled

**Storage:** Public repos, websites  
**Sharing:** Anywhere; ensure integrity

---

### Internal
**Description:** Non-sensitive business data for staff and trusted contractors  
**Examples:** Internal SOPs, team metrics, non-confidential roadmaps  
**Controls:**
- SSO required
- No external sharing without NDA
- Access logging enabled
- Internal drives only

**Storage:** Internal drives, private repos with SSO/MFA  
**Sharing:** Only to org-managed accounts; basic access logging

---

### Confidential
**Description:** Data that could harm business or customers if leaked  
**Examples:** Customer contracts, security runbooks, non-public financials, API configurations  
**Controls:**
- Need-to-know access only
- MFA mandatory for all users
- Encryption at rest and in transit
- No personal email or public cloud storage
- DLP rules on downloads/shares
- Quarterly access reviews

**Storage:** Encrypted drives, private repos with SSO/MFA, secure ticketing  
**Sharing:** Only to named individuals/groups; NDA for external parties

---

### Highly Confidential
**Description:** Regulated or mission-critical data; compromise = severe damage  
**Examples:** Production DB exports, API keys, tokens, encryption keys, PII datasets  
**Controls:**
- Only security/infra leadership access
- Hardware-backed MFA required
- Isolated vault or secrets manager
- Strict logging and quarterly access reviews
- No local storage
- Secure workstation/bastion access only
- Dual approval for key operations

**Storage:** Secrets manager, HSM, encrypted evidence store  
**Sharing:** Only via secure channels (VPN, E2EE messaging)

---

## 🚨 Incident Response Framework

NIST-aligned incident response with 6 phases:

### 1. **Preparation**
- Maintain contact list: Security lead, Infra, Legal/Compliance, Comms, On-call Engineering
- Define tools: SIEM/logs, ticketing, secure comms, evidence store, VPN/bastion
- Establish playbooks and runbooks
- Run quarterly tabletop exercises

### 2. **Identification**
- **Trigger:** Alert, user report, or anomaly detection
- **Immediate steps:**
  - Assign Incident Commander (IC)
  - Capture: What system, data class, when, detection source, current impact
  - Open incident ticket with unique ID (INC-YYYYMMDD-XX)
  - All comms reference this ID

### 3. **Containment**
**Short-term:**
- Isolate affected hosts/accounts
- Revoke tokens, disable accounts
- Block data exfiltration paths
- Restrict VPN and suspicious IPs

**Long-term:**
- Move workloads to clean environment
- Apply firewall rules
- Tighten access on affected systems

### 4. **Eradication**
- Remove malware, backdoors, and unauthorized access
- Patch vulnerabilities
- Rotate all impacted secrets and credentials
- Verify systems clean via scans and manual checks

### 5. **Recovery**
- Restore from backups if needed
- Monitor closely with heightened logging for 30 days
- Gradually re-enable services per business priority
- Test and validate before returning to production

### 6. **Lessons Learned**
- Post-incident review within 72 hours
- Document: Root cause, timeline, impact, root factors
- Update controls: Detections, playbooks, hardening, training
- Share findings with relevant teams

---

## 📊 Severity Matrix

| **Severity** | **Description** | **Examples** | **Response Target** |
|---|---|---|---|
| **Sev 1** – Critical | Major production impact; high regulatory risk | Ransomware on prod DB, widespread account takeover, active key abuse | IC + full team; 24x7 response |
| **Sev 2** – High | Significant impact on subset of services; sensitive data at risk | Compromise of one service account, confirmed phishing with access | 1 hour; IR team engaged |
| **Sev 3** – Medium | Limited impact; no confirmed sensitive data access | Malware blocked, suspicious login with MFA success | Business hours; owner involved |
| **Sev 4** – Low | Informational; no real impact | Vuln scan findings, false positives, policy violations | Backlog tracking only |

---

## 🚀 Getting Started

### Prerequisites
- Python 3.8+
- Git
- Access to SIEM/logging system (optional)
- Ticketing system (Jira, GitHub Issues, or similar)

### Installation

```bash
# Clone the repository
git clone https://github.com/tensecrocodile/data-classification-incident-response.git
cd data-classification-incident-response

# Install dependencies
pip install -r requirements.txt

# Configure for your environment
cp config/config.template.yaml config/config.yaml
# Edit config/config.yaml with your org details
```

### Quick Start

1. **Define your data:** Review `docs/DATA_CLASSIFICATION.md`
2. **Set up contacts:** Update `config/contacts.yaml` with your team
3. **Test a playbook:** Run `python tools/simulate_incident.py`
4. **Review logs:** Check `logs/` for incident evidence

---

## 📚 Documentation

### Core Documentation
- **[DATA_CLASSIFICATION.md](docs/DATA_CLASSIFICATION.md)** - Detailed classification guide with examples and mapping
- **[INCIDENT_RESPONSE.md](docs/INCIDENT_RESPONSE.md)** - Complete IR process and phase details
- **[SEVERITY_MATRIX.md](docs/SEVERITY_MATRIX.md)** - Incident triage and severity determination
- **[PLAYBOOKS.md](docs/PLAYBOOKS.md)** - Response workflows for common incident types
- **[DLP_RULES.md](docs/DLP_RULES.md)** - Data Loss Prevention configuration

### Configuration
- **[CONFIG.md](docs/CONFIG.md)** - Environment setup and tool integration
- **[CONTACTS.yaml](config/contacts.yaml)** - Team contact list and escalation paths
- **[SIEM_INTEGRATION.md](docs/SIEM_INTEGRATION.md)** - Logging and alerting setup

---

## 🔧 Tools & Automation

### Python Tools

```bash
# Data classification validator
python tools/classify_data.py --file mydata.csv

# Incident simulator (for testing)
python tools/simulate_incident.py --severity Sev2 --type credential-theft

# Evidence collector (for actual incidents)
python tools/collect_evidence.py --incident INC-20250101-01 --host prod-server-01

# Report generator
python tools/generate_report.py --incident INC-20250101-01 --output pdf
```

### Detection Rules
- YARA rules for malware detection
- Sigma rules for SIEM (Splunk, ELK, Datadog)
- Custom detection logic in `detections/`

### Automation
- Ansible playbooks for rapid containment
- Terraform for secure infrastructure
- GitHub Actions for CI/CD security checks

---

## 📁 Directory Structure

```
.
├── README.md                    # This file
├── docs/
│   ├── DATA_CLASSIFICATION.md   # Classification framework
│   ├── INCIDENT_RESPONSE.md     # IR process details
│   ├── PLAYBOOKS.md             # Incident response playbooks
│   ├── DLP_RULES.md             # DLP configuration
│   ├── SIEM_INTEGRATION.md      # Logging integration
│   └── SEVERITY_MATRIX.md       # Triage guidelines
├── config/
│   ├── config.template.yaml     # Configuration template
│   ├── contacts.yaml            # Team contacts and escalation
│   ├── dlp_rules.yaml           # DLP rule definitions
│   └── alert_rules.yaml         # Detection rules
├── tools/
│   ├── classify_data.py         # Data classification tool
│   ├── collect_evidence.py      # Evidence collection
│   ├── simulate_incident.py     # IR testing and simulation
│   └── generate_report.py       # Incident report generation
├── playbooks/
│   ├── ransomware_response.md   # Ransomware IR playbook
│   ├── data_breach_response.md  # Data breach playbook
│   ├── credential_compromise.md # Credential theft response
│   └── insider_threat.md        # Insider threat playbook
├── detections/
│   ├── yara_rules.yar           # YARA detection rules
│   ├── sigma_rules.yml          # SIEM detection rules
│   └── custom_detections.py     # Custom logic
├── logs/
│   └── incidents/               # Incident logs and evidence
├── templates/
│   ├── incident_ticket.md       # Issue template
│   ├── ir_checklist.md          # IR phase checklist
│   └── post_mortem.md           # Post-incident review template
├── requirements.txt             # Python dependencies
├── LICENSE                      # MIT License
└── .gitignore                   # Git ignore rules
```

---

## 🔐 Security Best Practices

1. **Never commit secrets** - Use `.gitignore` and `git-secrets` hooks
2. **Secure communication** - Use encrypted channels for incident discussions
3. **Access control** - Limit repo access to authorized personnel only
4. **Audit logging** - Log all access and changes to sensitive documents
5. **Regular updates** - Keep playbooks and tools current
6. **Testing** - Run tabletop exercises and simulations quarterly

---

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/my-improvement`)
3. Make your changes
4. Add tests and documentation
5. Submit a pull request

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

---

## 📞 Support & Contact

- **Security Issues:** Do not open public issues. Email security@yourorg.com
- **General Questions:** Create an issue or discussion
- **Documentation Feedback:** Pull requests welcome

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- NIST Cybersecurity Framework
- SANS Institute IR methodology
- ISO/IEC 27001 standards
- Community security best practices

---

**Last Updated:** December 2025  
**Maintained by:** Data Security Team
