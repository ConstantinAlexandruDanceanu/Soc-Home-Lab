
Scenario 1 – Local Discovery

Objective:
Observe and analyze local discovery activity executed by an interactive user
and how it is detected by the SIEM.

Environment:
- Windows 11 endpoint (real system)
- Wazuh SIEM
- Sysmon enabled on the endpoint

Planned Activity:
Local enumeration commands executed from an interactive user session.

Expected Outcome:
- Sysmon process creation events (Event ID 1)
- Discovery-related alerts visible in Wazuh
