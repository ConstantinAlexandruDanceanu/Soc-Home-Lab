# Scenario 1 – Local Discovery

## Scenario Overview
In this scenario, I looked at how basic local discovery commands executed by a user appear in the SIEM.
The goal was to understand what kind of visibility these commands generate and how they would look from a SOC Tier 1 point of view.

## Environment
- Windows 11 endpoint (real system)
- Wazuh SIEM
- Sysmon enabled on the endpoint

## Activity Performed
From an interactive command prompt session, I ran a few common local enumeration commands.

Commands observed:
- net users
- net localgroup administrators

During the same session, I also noticed other discovery-related commands such as
`net stats`, `net time`, `net file`, and `net share`.

## Detection
Wazuh detected this activity through Sysmon process creation events (Event ID 1).
The enumeration was performed using a standard Windows binary (`net1.exe`) on the endpoint **ALEXD**.

## Alert Details
- Command executed: `net localgroup administrators`
- Executable: `C:\Windows\System32\net1.exe`
- User: `Alex-D\dance`
- Alert severity: Low (Level 3)

## Analysis
Individually, these commands are normal administrative actions.
However, when executed together and close in time, they form a clear discovery pattern.

This type of behavior is often seen early in an attack lifecycle, when an attacker is
trying to understand user privileges and system configuration.
In this case, no further activity such as privilege escalation or persistence was observed.

## Analyst Decision
I classified this activity as **benign but relevant**.
No escalation was required, but continued monitoring of the endpoint was considered appropriate.
