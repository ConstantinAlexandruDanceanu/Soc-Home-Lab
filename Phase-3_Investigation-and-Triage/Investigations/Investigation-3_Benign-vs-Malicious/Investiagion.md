# Incident 3 – Benign vs Malicious (PowerShell Enumeration)

Am analizat o alertă Wazuh generată de o execuție PowerShell care face enumerare locală
(Get-LocalUser, Get-Process, whoami). Tipul ăsta de activitate e “gri”: poate fi legitim
(troubleshooting/admin), dar apare și foarte des după compromitere (recon inițial).

---

## Dovezi (ce am avut în față)

### Wazuh
- Rule ID: 92027
- Description: Powershell process spawned powershell instance
- Level: 4
- MITRE: T1059.001 (PowerShell) – Execution
- Agent: ALEXD (003) | 192.168.100.3
- Timestamp (local): 2026-02-01 17:30:52 (+0200)

### Sysmon (din alertă)
- Event ID: 1 (Process Create)
- Image: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
- User: Alex-D\dance
- Integrity Level: Medium
- CommandLine:
  "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -Command "Get-LocalUser; Get-Process; whoami"
- Parent Image: powershell.exe (PowerShell spawning PowerShell)
- Hash (SHA256): 0FF6F2C94BC7E2833A5F7E16DE1622E5DBA70396F31C7D5F56381870317E8C46

---

## De ce e “gri”
Comanda rulează binare legitime și nu e obfuscată, dar face enumerare de conturi și procese,
care este un pas tipic după obținerea accesului inițial.

---

## Benign vs Malicious (pe ce mă bazez)

### Ce împinge spre benign
- PowerShell este din locația standard Windows.
- Command line este clar, lizibil, fără obfuscare.
- Nu apar argumente tipice de payload: -enc, IEX, DownloadString.
- User normal, Integrity Level: Medium (nu SYSTEM).
- Eveniment singular (firedtimes: 1).

### Ce împinge spre malicious / suspicious
- Enumerarea (Get-LocalUser + Get-Process + whoami) e recon clasic post-compromise.
- PowerShell spawning PowerShell e un pattern comun în lanțuri de execuție.
- Fără context suplimentar, nu pot exclude că e parte dintr-un flow mai mare.

---

## Decizie
Clasific acest eveniment ca **Suspicious (ambiguous)**, nu “benign” automat și nici “malicious confirmat”.

Motiv: există indicatori legitimi, dar și comportament de recon. Pentru a decide definitiv,
am nevoie de corelare pe intervalul imediat înainte/după.

---

## Ce aș corela mai departe (ca să închid cazul)
În jurul timestamp-ului (±5 minute) aș verifica:
- autentificări: 4624/4625
- procese lansate imediat după: cmd.exe, powershell.exe cu parametri diferiți, net.exe, sc.exe
- persistență: registry (Sysmon 12/13/14), scheduled tasks, servicii
- network: conexiuni outbound după execuție

Dacă nu există follow-up (network/persistence/alte procese), îl închid ca benign.
Dacă apare follow-up, îl tratez ca parte din activitate malițioasă și escaladez.

JSON COMPLET 


{
  "_index": "wazuh-alerts-4.x-2026.02.01",
  "_id": "01_UGZwBHaLBAVJOGCQv",
  "_version": 1,
  "_score": null,
  "_source": {
    "input": {
      "type": "log"
    },
    "agent": {
      "ip": "192.168.100.3",
      "name": "ALEXD",
      "id": "003"
    },
    "manager": {
      "name": "wazuh"
    },
    "data": {
      "win": {
        "eventdata": {
          "originalFileName": "PowerShell.EXE",
          "image": "C:\\\\Windows\\\\System32\\\\WindowsPowerShell\\\\v1.0\\\\powershell.exe",
          "product": "Microsoft® Windows® Operating System",
          "parentProcessGuid": "{b637f13d-71a8-697f-533e-000000000d00}",
          "description": "Windows PowerShell",
          "logonGuid": "{b637f13d-a0af-6977-3707-060000000000}",
          "parentCommandLine": "\\\"C:\\\\Windows\\\\System32\\\\WindowsPowerShell\\\\v1.0\\\\powershell.exe\\\"",
          "processGuid": "{b637f13d-71ac-697f-573e-000000000d00}",
          "logonId": "0x60737",
          "parentProcessId": "7904",
          "processId": "25468",
          "currentDirectory": "C:\\\\WINDOWS\\\\system32\\\\",
          "utcTime": "2026-02-01 15:30:52.034",
          "hashes": "MD5=A97E6573B97B44C96122BFA543A82EA1,SHA256=0FF6F2C94BC7E2833A5F7E16DE1622E5DBA70396F31C7D5F56381870317E8C46,IMPHASH=AFACF6DC9041114B198160AAB4D0AE77",
          "parentImage": "C:\\\\Windows\\\\System32\\\\WindowsPowerShell\\\\v1.0\\\\powershell.exe",
          "company": "Microsoft Corporation",
          "commandLine": "\\\"C:\\\\Windows\\\\System32\\\\WindowsPowerShell\\\\v1.0\\\\powershell.exe\\\" -NoProfile -Command \\\"Get-LocalUser; Get-Process; whoami\\\"",
          "integrityLevel": "Medium",
          "fileVersion": "10.0.26100.5074 (WinBuild.160101.0800)",
          "user": "Alex-D\\\\dance",
          "terminalSessionId": "1",
          "parentUser": "Alex-D\\\\dance"
        },
        "system": {
          "eventID": "1",
          "keywords": "0x8000000000000000",
          "providerGuid": "{5770385f-c22a-43e0-bf4c-06f5698ffbd9}",
          "level": "4",
          "channel": "Microsoft-Windows-Sysmon/Operational",
          "opcode": "0",
          "message": "\"Process Create:\r\nRuleName: -\r\nUtcTime: 2026-02-01 15:30:52.034\r\nProcessGuid: {b637f13d-71ac-697f-573e-000000000d00}\r\nProcessId: 25468\r\nImage: C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe\r\nFileVersion: 10.0.26100.5074 (WinBuild.160101.0800)\r\nDescription: Windows PowerShell\r\nProduct: Microsoft® Windows® Operating System\r\nCompany: Microsoft Corporation\r\nOriginalFileName: PowerShell.EXE\r\nCommandLine: \"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe\" -NoProfile -Command \"Get-LocalUser; Get-Process; whoami\"\r\nCurrentDirectory: C:\\WINDOWS\\system32\\\r\nUser: Alex-D\\dance\r\nLogonGuid: {b637f13d-a0af-6977-3707-060000000000}\r\nLogonId: 0x60737\r\nTerminalSessionId: 1\r\nIntegrityLevel: Medium\r\nHashes: MD5=A97E6573B97B44C96122BFA543A82EA1,SHA256=0FF6F2C94BC7E2833A5F7E16DE1622E5DBA70396F31C7D5F56381870317E8C46,IMPHASH=AFACF6DC9041114B198160AAB4D0AE77\r\nParentProcessGuid: {b637f13d-71a8-697f-533e-000000000d00}\r\nParentProcessId: 7904\r\nParentImage: C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe\r\nParentCommandLine: \"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe\" \r\nParentUser: Alex-D\\dance\"",
          "version": "5",
          "systemTime": "2026-02-01T15:30:52.0463302Z",
          "eventRecordID": "47244",
          "threadID": "8032",
          "computer": "Alex-D",
          "task": "1",
          "processID": "6512",
          "severityValue": "INFORMATION",
          "providerName": "Microsoft-Windows-Sysmon"
        }
      }
    },
    "rule": {
      "firedtimes": 1,
      "mail": false,
      "level": 4,
      "description": "Powershell process spawned powershell instance",
      "groups": [
        "sysmon",
        "sysmon_eid1_detections",
        "windows"
      ],
      "mitre": {
        "technique": [
          "PowerShell"
        ],
        "id": [
          "T1059.001"
        ],
        "tactic": [
          "Execution"
        ]
      },
      "id": "92027"
    },
    "location": "EventChannel",
    "decoder": {
      "name": "windows_eventchannel"
    },
    "id": "1769959852.2043822",
    "timestamp": "2026-02-01T17:30:52.267+0200"
  },
  "fields": {
    "timestamp": [
      "2026-02-01T15:30:52.267Z"
    ]
  },
  "sort": [
    1769959852267
  ]
}
asta e json 
