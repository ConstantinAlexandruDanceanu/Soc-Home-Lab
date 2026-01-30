
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


### Detection

The Wazuh SIEM detected a local discovery activity on the endpoint **ALEXD**.
A standard Windows LOLBin (`net1.exe`) was used to enumerate local administrator group membership.

### Alert Details
- Command executed: `net localgroup administrators`
- Executable: `C:\Windows\System32\net1.exe`
- User: `Alex-D\dance`
- Alert severity: Low (Level 3)

### Analysis
This activity represents local privilege enumeration.  
While the command is legitimate and was executed by an authenticated local user, similar behavior is commonly observed during early attacker reconnaissance phases.

No evidence of privilege escalation, persistence, or lateral movement was observed at this stage.

### Conclusion
The alert was classified as **benign but relevant**.  
Activity should be correlated with subsequent actions to determine intent.
