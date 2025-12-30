# Incident Response Plan

Complete framework for detecting, responding to, and recovering from security incidents.

## Overview

This incident response plan follows the NIST Cybersecurity Framework and includes six key phases: Preparation, Identification, Containment, Eradication, Recovery, and Lessons Learned.

## Phase 1: Preparation

### Objectives
- Establish IR team and roles
- Define tools and processes
- Create playbooks and runbooks
- Maintain readiness

### Key Activities

**1. Establish Incident Response Team**
- **Incident Commander** - Leads the response, maintains timeline, ensures coordination
- **Security Lead** - Analyzes threats, determines severity, guides eradication
- **Infrastructure Lead** - Manages containment, recovery, system access
- **Legal/Compliance** - Advises on disclosure, regulatory notifications
- **Communications** - Manages internal/external messaging
- **Forensics Lead** - Collects evidence, preserves data

**2. Define Tools**
- SIEM platform (Splunk, ELK, Datadog)
- Ticketing system (Jira, GitHub Issues)
- Encrypted comms channel (Signal, Slack with admin oversight)
- Evidence storage (encrypted vault)
- Secure bastion/VPN access
- Forensic collection tools

**3. Create Playbooks**
- Ransomware response (see playbooks/ransomware_response.md)
- Data breach (see playbooks/data_breach_response.md)
- Credential compromise (see playbooks/credential_compromise.md)
- Insider threat (see playbooks/insider_threat.md)

**4. Training & Exercises**
- Quarterly tabletop simulations
- Annual full-scale exercises
- New hire security training
- Phishing drills

## Phase 2: Identification

### Detection Methods
1. **Automated Alerts** - SIEM, IDS/IPS, antivirus
2. **User Reports** - Email, help desk tickets
3. **Threat Feeds** - Security researcher notifications
4. **Anomaly Detection** - ML models, baseline deviations
5. **Audit Logs** - Unusual access patterns

### Initial Response (First 15 minutes)

**Upon Detection:**
1. Alert received → Create incident ticket
2. Assign unique ID: `INC-YYYYMMDD-XX`
3. Designate Incident Commander
4. Gather initial facts:
   - What system is affected?
   - What data might be involved? (Classify per DATA_CLASSIFICATION.md)
   - When was this detected?
   - Detection source?
   - Is it ongoing?
   - Current business impact?
5. Open secure comms channel
6. Begin timeline logging
7. Preserve evidence (no overwriting logs)
8. Determine severity (see SEVERITY_MATRIX.md)

### Severity Determination

```
SEV 1 (Critical)
- Major production impact
- Likely data breach
- Ransomware activity detected
- Response: All hands on deck, 24x7

SEV 2 (High)
- Significant system compromise
- Limited scope
- Sensitive data at risk
- Response: 1-hour engagement, IR team

SEV 3 (Medium)
- Limited impact
- No confirmed data access
- Good containment controls
- Response: Business hours, relevant teams

SEV 4 (Low)
- False positive
- No real impact
- Policy violation only
- Response: Backlog tracking
```

## Phase 3: Containment

### Short-term Containment (First 1-2 hours)

Goal: Stop the attack and prevent spread.

**Immediate Actions:**
1. **Isolate Affected Systems**
   - Take compromised system offline (network isolation, not hard shutdown)
   - Preserve volatile evidence before disconnect
   - Document state before changes

2. **Disable Compromised Accounts**
   - Reset passwords for affected accounts
   - Revoke active sessions
   - Revoke API tokens and keys
   - Disable SSH keys

3. **Block Attacker Access**
   - Block IP addresses at firewall
   - Revoke VPN credentials
   - Terminate suspicious sessions
   - Review active connections

4. **Stop Data Exfiltration**
   - Block egress to known C2 domains
   - Restrict cloud upload services
   - Enable DLP rules
   - Monitor outbound traffic

### Long-term Containment (Hours 2-24)

**Strategic Actions:**
1. **Patch & Harden**
   - Apply security patches to affected systems
   - Disable unnecessary services
   - Tighten firewall rules
   - Update IPS signatures

2. **Increase Monitoring**
   - Deploy additional monitoring
   - Create specific detection rules
   - Monitor for lateral movement
   - Alert on similar patterns

3. **Segment Network**
   - Isolate affected systems from production
   - Apply network segmentation
   - Restrict cross-segment traffic
   - Monitor inter-segment traffic

## Phase 4: Eradication

### Remove All Traces of Attack

**1. Malware Removal**
- Full system scan with updated signatures
- Manual review of suspicious processes
- Check persistence mechanisms (cron, services, registry)
- Remove backdoors and rootkits

**2. Patch Vulnerabilities**
- Apply all relevant security patches
- Patch before restoration
- Test patches on isolated system first
- Document all patches applied

**3. Credential Rotation**
- Change all passwords for affected accounts
- Rotate API keys and tokens
- Rotate SSH keys
- Update service accounts
- Reset privileged access credentials

**4. Verification**
- Run complete malware scans
- Review logs for suspicious activity
- Verify patches are applied
- Test affected systems in isolation

## Phase 5: Recovery

### Restore Systems to Production

**1. Backup Verification**
- Confirm clean backup exists (pre-compromise)
- Verify backup integrity
- Test backup restoration
- Validate backup contains no malware

**2. System Restoration**
- Restore from clean backups
- Apply all security patches
- Reinstall critical updates
- Restore configurations
- Verify functionality

**3. Production Return**
- Restore in order of criticality
- Start with non-customer-facing systems
- Monitor closely for 30 days
- Maintain elevated alerting
- Daily status reviews

**4. Data Validation**
- Verify data integrity
- Check for data corruption
- Confirm no data loss
- Validate all backups

## Phase 6: Lessons Learned

### Post-Incident Review (within 72 hours)

**1. Incident Summary**
- Date and time of incident
- Systems and data affected
- Root cause
- Duration and impact

**2. Timeline**
- Detection time
- Notification time
- Investigation time
- Containment time
- Eradication time
- Recovery time

**3. Root Cause Analysis**
- Primary attack vector
- Contributing factors
- Gaps in controls
- Why detection was delayed

**4. Improvements**
- Process improvements
- Detection rule updates
- Playbook updates
- Tool upgrades
- Training needs

**5. Action Items**
- Owner assignment
- Priority
- Due date
- Status tracking

### Stakeholder Communication
- Report to leadership
- Legal and compliance notification
- Customer communication (if applicable)
- Regulatory notification (if required)
- Public disclosure (if warranted)

## Incident Ticket Template

```
INC-YYYYMMDD-XX
Title: [Brief description]
Severity: SEV X
Status: [Identification/Containment/Eradication/Recovery]
Data Classification: [Public/Internal/Confidential/Highly Confidential]

Detection Time: [UTC time]
Reporting: [Who reported it]
IC: [Incident Commander name]
Affected Systems: [List]
Affected Data: [Describe]

Initial Assessment:
[What happened]
[Data at risk]
[Current impact]
[Ongoing?]

Actions Taken:
[Chronological list]

Phase: [Current phase]
Next Steps: [What's next]
Timeline Target: [Expected containment time]
```

## Escalation Path

```
SEV 4 (Low)
→ Assign to on-call security
→ Create ticket
→ Resolve within 5 business days

SEV 3 (Medium)
→ Notify Security Lead
→ Activate IR core team
→ Resolve within 24 hours

SEV 2 (High)
→ Notify Security Lead + Infra Lead
→ Activate full IR team
→ CTO aware
→ Resolve within 4 hours

SEV 1 (Critical)
→ Notify all IR team immediately
→ Notify CEO/Board
→ Legal/Compliance engaged
→ 24x7 response until contained
```

## Communication Plan

**Internal:**
- Incident ticket (updated continuously)
- Secure Slack channel for IR team
- Daily stand-ups for Sev 1/2
- Post-incident report

**External (if applicable):**
- Customer notification (within 72 hours if data breach)
- Regulatory notification (per legal guidance)
- Press statement (if public)
- Vendor communication (if 3rd party)

## Tools & Resources

- SIEM: [Your SIEM tool]
- Ticketing: [Your ticketing system]
- Secure Comms: [Signal/Wire/Slack]
- Evidence Storage: [Your vault]
- Forensics Tools: [list]
- Legal Contact: [name/email]
- Insurance: [provider/policy]

## Related Documents

- [Data Classification](DATA_CLASSIFICATION.md)
- [Severity Matrix](SEVERITY_MATRIX.md)
- [DLP Rules](DLP_RULES.md)
- [Playbooks](../playbooks/)
- [Contacts](../config/contacts.yaml)
