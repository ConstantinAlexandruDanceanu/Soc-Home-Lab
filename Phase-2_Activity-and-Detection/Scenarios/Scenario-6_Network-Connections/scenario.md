
## Scenario 6 – Network Connections (External)

### Objective
Observe and detect external network connection attempts originating from a Kali Linux host towards a Windows endpoint.

### Environment
- Windows 11 endpoint
- Wazuh SIEM
- Sysmon enabled
- Kali Linux attacker VM

### Activity
From the Kali Linux system, network connections were initiated towards the Windows endpoint using SMB-related services.

Commands executed:
- nc -vn 192.168.100.3 445
- smbclient -L //192.168.100.3 -N

### Detection
The Wazuh SIEM detected multiple authentication-related security events on the Windows endpoint.

Observed events:
- Event ID: 4625 (Failed logon)
- Logon Type: 3 (Network)
- Authentication Package: NTLM
- Source Workstation: KALI
- Target User: ANONYMOUS LOGON / kali

### Analysis
The activity represents an external network authentication attempt against an exposed SMB service.
No successful authentication occurred and no follow-up activity was detected.

### Conclusion
The activity was classified as low severity and monitored only.
Such behavior is commonly observed during network enumeration or service discovery phases.
