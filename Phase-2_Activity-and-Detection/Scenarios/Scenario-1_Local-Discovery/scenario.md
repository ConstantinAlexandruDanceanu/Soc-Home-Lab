# Scenario 1 – Local Discovery

## Scenario Overview
This scenario demonstrates local discovery activity executed by an interactive user
on a Windows endpoint. The objective is to observe how common enumeration commands
are detected and correlated within the SIEM from a SOC Tier 1 perspective.

## Environment
- Windows 11 endpoint (real system)
- Wazuh SIEM
- Sysmon enabled on the endpoint

## Activity Performed
Local enumeration commands were executed from an interactive command prompt session.

Observed commands:
- net users
- net localgroup administrators

Additional discovery-related commands were observed within the same session
(net stats, net time, net file, net share).

## Detection
The activity was detected by Wazuh through Sysmon process creation events (Event ID 1).
A standard Windows LOLBin (`net1.exe`) was used to enumerate local administrator group
membership on the endpoint **ALEXD**.

## Alert Details
- Command executed: `net localgroup administrators`
- Executable: `C:\Windows\System32\net1.exe`
- User: `Alex-D\dance`
- Alert severity: Low (Level 3)

## Correlation and Analysis
While each command is legitimate when executed individually, the sequence and timing
indicate systematic discovery behavior.

Such command patterns are commonly observed during early post-access reconnaissance
phases to assess user privileges and available system resources.

No evidence of privilege escalation, persistence, or lateral movement was observed
during this activity.

## Analyst Decision
The activity was monitored but not escalated.

Although the behavior aligns with discovery techniques, the lack of follow-up
malicious actions supports a **benign but relevant** classification.
Continued monitoring of the endpoint was recommended.
