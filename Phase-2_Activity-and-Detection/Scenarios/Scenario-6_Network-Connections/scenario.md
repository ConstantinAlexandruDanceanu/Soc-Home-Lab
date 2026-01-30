# Scenario 6 – Network Connections (External)

## Objective
See how a basic network connection coming from an external system is logged on a Windows endpoint and detected by Wazuh.

The focus was on visibility and understanding the logs, not on exploitation.

## Environment
- Windows 11 endpoint with Wazuh agent
- Wazuh SIEM
- Kali Linux (external host)
- Sysmon enabled on the endpoint

## Activity
From the Kali Linux machine, I initiated simple network connections toward the Windows endpoint.

Commands executed from Kali:
- nc -vn 192.168.100.3 445
- smbclient -L //192.168.100.3 -N

These actions simulate an external system attempting to access a network service without valid credentials.

## Detection
On the Windows endpoint, the activity generated authentication-related security events.

Wazuh detected failed network logon attempts associated with the Kali IP address.

Observed details:
- External source IP (Kali)
- Network logon type
- NTLM authentication attempts
- Authentication failures only

## Analysis
External authentication attempts over the network can be an early sign of enumeration or brute-force activity.

In this case, the activity was controlled and initiated by me for testing purposes.
No successful authentication occurred and no follow-up activity was observed.

## Verdict
Benign external network activity.  
Alert was valid, but no escalation was required.
