# Scenario 5 – File Impact

## Objective
I wanted to see how file creation activity triggered by PowerShell is logged
and detected by the SIEM.

The focus was on understanding what visibility exists when a process creates
files in common temporary locations.

## Environment
- Windows 11 endpoint (real system)
- Wazuh SIEM
- Sysmon enabled on the endpoint

## Activity Performed
From an interactive PowerShell session, I executed a simple command to create
a file in the user Temp directory.

Command executed:
`New-Item -Path $env:TEMP\lab_file_impact.txt -ItemType File`

This action was meant to generate a clear file creation event.

## Detection
Wazuh detected the file creation through Sysmon file creation events
(Event ID 11).

Observed details:
- Process: `powershell.exe`
- File created: `lab_file_impact.txt`
- Path: `C:\Users\dance\AppData\Local\Temp`
- User: `Alex-D\dance`
- Alert level: Medium

## Analysis
File creation in temporary directories is common for legitimate scripts and
applications, but it is also frequently used by malware for staging or payload
execution.

In this case, the file creation was expected and manually initiated.
No additional suspicious activity followed, such as execution of the file
or persistence mechanisms.

## Analyst Decision
I classified this activity as **benign but relevant**.

File creation events in temporary locations should always be reviewed together
with process context, parent-child relationships, and any follow-up actions
to accurately determine intent.


