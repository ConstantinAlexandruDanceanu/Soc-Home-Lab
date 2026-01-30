# Scenario 3 – PowerShell Execution

## Objective
Observe how a basic PowerShell command executed by a local user
appears in the SIEM and what level of detail is available for analysis.

## Environment
- Windows 11 endpoint (real system)
- Wazuh SIEM
- Sysmon enabled on the endpoint

## Activity Performed
From an interactive user session, I executed a simple PowerShell command
without administrative privileges.

The goal was to generate a clear process creation event with command-line
visibility and parent process context.

## Detection
Wazuh detected the PowerShell execution on endpoint **ALEXD** through
Sysmon process creation events (Event ID 1).

The event included full command-line details, user context, and integrity level.

## Alert Details
- Executable: `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`
- Command line: `powershell -nop -c "Get-Process | Select-Object -First 5"`
- User: `Alex-D\dance`
- Integrity level: Medium
- Parent process: `powershell.exe`
- Detection source: Sysmon (Event ID 1)

## Analysis
This activity represents normal interactive PowerShell usage by a legitimate
local user.

The command itself is benign and commonly used for basic system inspection.
However, PowerShell is a dual-use tool and is frequently abused by attackers
for discovery, execution, and post-exploitation.

For this reason, command-line logging and parent-child process context are
important to determine intent.

In this case, no suspicious indicators were observed:
- no obfuscation or encoded commands
- no elevated privileges
- no suspicious child processes

## Analyst Decision
I classified this activity as **benign**.

No response or escalation was required.
The activity would become more relevant if correlated with additional
suspicious behavior in later stages.
