Wazuh Server
- Deployed as a VM and configured to access the Wazuh dashboard.

Windows 11 Endpoint
- Wazuh agent installed and pointed to the Wazuh server.
- Sysmon installed to improve endpoint visibility.

Sysmon Integration
- Sysmon event channel is collected by the Wazuh agent.
- Verified that Sysmon-related events are visible in the Wazuh dashboard.
