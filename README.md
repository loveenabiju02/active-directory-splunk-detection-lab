# Active Directory & Splunk SOC Detection Lab

## Project Overview

This project is a hands-on cybersecurity lab designed to explore Active Directory administration, Windows security event monitoring, and SIEM-based threat detection using Splunk.

The objective is to simulate common security events, analyse Windows logs, develop detection rules using Splunk Search Processing Language (SPL), and understand how a Security Operations Centre (SOC) monitors and investigates suspicious activities.

## Lab Environment

- **Virtualization:** Oracle VirtualBox
- **Operating System:** Windows Server
- **Directory Service:** Active Directory Domain Services (AD DS)
- **Domain:** cyberlab.local
- **SIEM:** Splunk Enterprise (planned)

## Project Progress

### Phase 1 – Active Directory Setup
- Configured a Windows Server virtual machine.
- Installed Active Directory Domain Services and DNS.
- Created the `cyberlab.local` domain.
- Created a dedicated organisational unit and test user.

### Phase 2 – Windows Security Auditing
- Configured Advanced Audit Policies.
- Enabled auditing for logon events, account management, security group management, and directory service changes.
- Generated a failed logon event (Event ID 4625).
- Started analysing Windows Security Event Logs.

### Upcoming Phases
- Install and configure Splunk Enterprise.
- Collect and analyse Windows Security Events.
- Develop SPL queries for suspicious activities.
- Create detection rules and SOC alerts.
- Build a security monitoring dashboard.

## Skills Demonstrated

- Active Directory Administration
- Windows Event Log Analysis
- Security Auditing
- SIEM Monitoring
- Splunk SPL
- Detection Engineering
- SOC Alert Investigation

*This is a learning project conducted in an isolated virtual lab environment.*
