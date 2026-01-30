# Scenario 3 – PowerShell Execution

## Objective
Observe and analyze the execution of a PowerShell process from an interactive
user context and how it is detected by the SIEM.

## Environment
- Windows 11 endpoint (real system)
- Wazuh SIEM
- Sysmon enabled on the endpoint

## Planned Activity
Execute a simple PowerShell command from a non-administrative user context
to generate a process creation event with command-line visibility.

## Expected Outcome
- Sysmon Event ID 1 (Process Create)
- PowerShell process and command line visible in Wazuh
- User and parent process context available for analysis

## Detection

The Wazuh SIEM detected the execution of a PowerShell process on the Windows
endpoint ALEXD. The activity was captured through Sysmon process creation
events (Event ID 1), providing full command-line visibility.

## Alert Details

- Executable: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
- Command line: powershell -nop -c "Get-Process | Select-Object -First 5"
- User: Alex-D\dance
- Integrity level: Medium
- Parent process: powershell.exe
- Detection source: Sysmon (Event ID 1)

## Analysis

This activity represents interactive PowerShell usage by a legitimate local
user. The command executed is benign and commonly used for system inspection.

However, PowerShell is a dual-use tool frequently abused by attackers during
initial discovery and execution phases. Command-line logging and parent-child
process context are critical for distinguishing between normal administrative
activity and malicious behavior.

In this case, no obfuscation, encoded commands, elevated privileges, or
suspicious child processes were observed.

## Conclusion

The alert was classified as benign.
No immediate response or escalation was required.

The activity should be monitored and correlated with subsequent events,
especially if followed by persistence mechanisms, privilege escalation, or
network-based actions.
