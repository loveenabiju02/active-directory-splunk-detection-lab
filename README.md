# Active Directory & Splunk SOC Detection Lab


## Project Overview

This project involved building a small Windows Active Directory environment and integrating it with Splunk Cloud to practise Security Operations Centre (SOC) monitoring, Windows security event analysis, and basic threat detection.

The lab was designed to simulate common identity-related security events, collect Windows Event Logs, investigate account and group activity, and configure an alert to identify repeated failed login attempts.

## Objectives

- Set up a Windows Server Active Directory environment.
- Configure Splunk Universal Forwarder to collect Windows Event Logs.
- Forward security-related logs to Splunk Cloud.
- Investigate authentication, account management, and group membership events.
- Develop a basic detection rule for repeated failed login attempts.
- Configure and validate a Splunk alert.

## Lab Environment

| Component | Configuration |
|---|---|
| Host OS | Windows 11 Home |
| Virtualization | Oracle VirtualBox 7.2.8 |
| Server OS | Windows Server |
| Domain | CYBERLAB |
| Domain Controller | AD-DC01 |
| Test Account | testuser |
| SIEM | Splunk Cloud |
| Log Forwarder | Splunk Universal Forwarder |
| Log Sources | Windows Security, System and Application logs |

## Lab Architecture

**Windows Server / Active Directory (AD-DC01)**  
↓  
**Splunk Universal Forwarder**  
↓  
**Splunk Cloud – Main Index**  
↓  
**SPL Searches, Event Investigation and Alerting**

## Implementation

### 1. Active Directory Configuration

- Configured Windows Server as an Active Directory Domain Controller.
- Created the `CYBERLAB` domain.
- Created an organisational unit named `Lab_Users`.
- Created a test user account named `testuser`.
- Generated controlled authentication and account-management activity for log analysis.

### 2. Splunk Cloud Setup

- Obtained access to a Splunk Cloud environment for the lab.
- Identified the Splunk Cloud receiving endpoint and configured the required SSL connection details.
- Used the Splunk Cloud environment as the central platform for receiving, searching, and analysing Windows Event Logs.
- Identified the `main` index as the destination for the forwarded events.
- Verified access to the Splunk Cloud search interface before configuring log forwarding.

### 3. Splunk Universal Forwarder Integration

- Installed Splunk Universal Forwarder on the Windows Server.
- Configured forwarding to the Splunk Cloud receiving endpoint using SSL.
- Configured collection of:
  - Windows Security Event Logs
  - Windows System Event Logs
  - Windows Application Event Logs
- Verified configuration and confirmed that events were being received in the Splunk Cloud `main` index.

### 4. Windows Security Event Investigation

The following Windows Security Event IDs were investigated:

| Event ID | Activity | Investigation |
|---|---|---|
| 4624 | Successful logon | Examined successful logon activity and identified observed logon types. |
| 4625 | Failed logon | Analysed failed authentication attempts and tested a repeated-failure detection rule. |
| 4720 | User account created | Investigated account creation, including the creation of `testuser`. |
| 4738 | User account changed | Examined account modification activity. |
| 4728 | Member added to global security group | Reviewed privileged group membership changes, including Domain Admins. |
| 4732 | Member added to local security group | Investigated local group membership changes, including Administrators. |
| 4725 | User account disabled | Checked for account-disable activity; none was observed in the available dataset. |
| 4726 | User account deleted | Checked for account-deletion activity; none was observed in the available dataset. |
| 4740 | User account locked out | Checked for account-lockout activity; none was observed in the available dataset. |

### 5. Detection Rule – Repeated Failed Logins

**Detection objective:** Identify repeated failed login attempts associated with an account within a five-minute period.

The following SPL query was used:

```spl
index=main EventCode=4625
| bin _time span=5m
| stats count by _time, Account_Name
| where count >= 5
```

**Detection logic:**
- Collect Windows failed-logon events (Event ID 4625).
- Group events into five-minute time windows.
- Count failed attempts by account.
- Identify accounts with five or more failed attempts in a time window.

**Alert name:** Brute Force – Multiple Failed Logins

**Alert configuration:**
- Scheduled execution: Every minute
- Search window: Previous five minutes
- Threshold: Five or more failed attempts

The alert was successfully triggered during controlled testing using repeated incorrect login attempts against the test account.

This validated the detection workflow; it does not represent evidence of a real-world brute-force attack.

## Key Findings

- Successfully integrated Windows Server event logging with Splunk Cloud.
- Observed successful and failed authentication events.
- Identified different Windows logon types, including interactive, network, service, and workstation-unlock activity.
- Investigated user account creation and modification events.
- Examined privileged group membership activity involving Domain Admins and local Administrators.
- Successfully configured and validated a Splunk alert for repeated failed login attempts.
- No account lockout, account disablement, or account deletion events were observed in the available dataset.

## Screenshots

## Screenshots

### 1. Splunk Event Ingestion – Windows Logs
![Windows Logs](screenshots/WINDOWS%20LOGS.png)

### 2. Failed Login Investigation – Event ID 4625
![Failed Logins](screenshots/FAILED%20LOGINS.png)

### 3. Brute Force Detection – Triggered Alert
![Triggered Alerts](screenshots/TRIGGERED%20ALERTS.png)

## Skills Demonstrated

- Active Directory administration and user management
- Windows Server configuration
- Splunk Cloud SIEM monitoring
- Splunk Search Processing Language (SPL)
- Windows Security Event Log analysis
- Authentication and account activity monitoring
- Privileged group membership investigation
- Basic detection engineering and alert configuration
- Security event investigation and log analysis

## Tools and Technologies

`Splunk Cloud` · `Splunk Universal Forwarder` · `SPL` · `Windows Server` · `Active Directory` · `Windows Event Viewer` · `Oracle VirtualBox`

## Limitations

- This was a controlled, single-domain lab environment rather than a production enterprise network.
- Detection was based on a basic failed-login threshold and was not designed to eliminate false positives.
- Some Windows event fields, including account details in successful logon events, were not consistently extracted during the investigation.
- Findings are limited to the events collected during the lab exercises.


## Conclusion

This project provided practical experience in building an Active Directory environment, forwarding Windows Event Logs to a SIEM platform, investigating security events, and configuring a basic detection alert in Splunk.

It strengthened my understanding of Windows authentication, account management, privileged group activity, SIEM monitoring, and the detection workflow used in SOC environments.

---

**Project Type:** Hands-on Cybersecurity / SOC Lab  
**Focus Areas:** SIEM Monitoring · Windows Security · Active Directory · Detection Engineering
