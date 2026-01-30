
# Scenario 4 – Parent / Child Process Execution

## Objective
Observe how a simple parent-child process relationship looks in the SIEM
when one process launches another from an interactive user session.

The focus was on visibility and context, not on simulating malicious behavior.

## Environment
- Windows 11 endpoint (real system)
- Wazuh SIEM
- Sysmon enabled on the endpoint

## Activity Performed
From an interactive Command Prompt session, I launched PowerShell to execute a basic command.

Command executed:
`cmd.exe /c powershell.exe -NoProfile -Command "Get-Date"`

This was done to generate a clear parent-child process relationship.

## Detection
Wazuh detected the activity through a Sysmon process creation event (Event ID 1).

The event clearly showed:
- `cmd.exe` as the parent process
- `powershell.exe` as the child process

## Alert Details
- Parent process: `C:\Windows\System32\cmd.exe`
- Child process: `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`
- User: `Alex-D\dance`
- Execution context: Interactive user session
- Detection source: Sysmon Process Create (Event ID 1)

## Analysis
Parent-child process relationships like this are very common in normal user and administrative activity.

At the same time, similar execution chains are frequently used by attackers to run scripts
or launch follow-on commands while blending in with legitimate behavior.

In this case, the command was simple, readable, and manually executed by me.
No additional suspicious activity followed.

## Analyst Decision
I classified this event as **informational but relevant**.

No escalation was required.
Parent-child relationships should always be evaluated together with the command content
and any subsequent activity on the endpoint.
