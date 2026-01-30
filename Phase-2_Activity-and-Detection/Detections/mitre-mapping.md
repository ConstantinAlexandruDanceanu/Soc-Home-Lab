

## Observed Techniques

### T1087 – Account Discovery
Observed during local discovery activity on the Windows endpoint.
Commands used to enumerate local users or groups generated visibility in the SIEM.

### T1059.001 – PowerShell
PowerShell execution was logged during controlled command execution.
PowerShell is a common administrative tool but is also frequently abused by attackers.

### T1546.011 – Application Shimming
Triggered during testing of process execution related to system binaries.
Mapped for awareness only, no persistence was established.

### T1105 – Ingress Tool Transfer (Observed Behavior)
File creation activity in temporary directories matched behavior commonly associated with tool staging.
No actual tool transfer occurred during testing.

### T1110 – Brute Force (Observed Behavior)
Multiple failed authentication attempts from an external host were logged.
Activity remained limited and did not result in successful authentication.

### T1046 – Network Service Discovery
External network connections from Kali Linux toward the Windows endpoint generated network authentication events.
This behavior is consistent with basic service discovery.
