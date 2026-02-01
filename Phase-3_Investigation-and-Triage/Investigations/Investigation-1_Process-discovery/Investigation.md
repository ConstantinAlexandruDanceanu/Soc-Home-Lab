Alertă Wazuh legată de o execuție PowerShell

Regula care a declanșat alerta este 92027 – „Powershell process spawned powershell instance”
(level 4, MITRE T1059.001 – Execution).

Primul lucru la care m-am uitat a fost command line-ul, pentru că de acolo îți dai seama
rapid dacă e ceva serios sau doar zgomot.

Comanda executată a fost:

"C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe"
-ExecutionPolicy Bypass
-NoProfile
-Command "Get-Process | Out-File C:\Users\Public\proc.txt"

ExecutionPolicy Bypass este un indicator suspect, dar în cazul ăsta comanda este clară,
lizibilă și nu este obfuscată. Nu apar argumente de tip -enc, IEX sau DownloadString.

Procesul părinte este tot powershell.exe, rulat din locația standard Windows.
Execuția s-a făcut în context de user normal, cu integrity level Medium.

Am verificat dacă există:
- conexiuni de rețea
- execuții ulterioare suspecte
- persistență (registry, scheduled tasks)

Nu apare nimic din astea.

Evenimentul principal este Sysmon Event ID 1 (Process Create).
Există și un Sysmon Event ID 11 care confirmă crearea fișierului
C:\Users\Public\proc.txt, ca rezultat direct al comenzii.

Pe baza acestor date, am tratat alerta ca benignă.
Decizia este bazată pe comportament: execuție unică, comandă explicită,
fără follow-up și fără impact ulterior.

Dacă aș fi văzut aceeași execuție pornită din Office, browser
sau corelată cu persistență ori trafic de rețea,
aș fi escaladat incidentul.


FULL JSON View! ! ! 
{
  "_index": "wazuh-alerts-4.x-2026.02.01",
  "_id": "z192GZwBHaLBAVJOuiPc",
  "_score": 1,
  "_source": {
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
          "parentProcessGuid": "{b637f13d-59a0-697f-893c-000000000d00}",
          "description": "Windows PowerShell",
          "logonGuid": "{b637f13d-a0af-6977-3707-060000000000}",
          "parentCommandLine": "\\\"C:\\\\Windows\\\\System32\\\\WindowsPowerShell\\\\v1.0\\\\powershell.exe\\\"",
          "processGuid": "{b637f13d-59c8-697f-923c-000000000d00}",
          "logonId": "0x60737",
          "parentProcessId": "24824",
          "processId": "14960",
          "currentDirectory": "C:\\\\WINDOWS\\\\system32\\\\",
          "utcTime": "2026-02-01 13:48:56.008",
          "hashes": "MD5=A97E6573B97B44C96122BFA543A82EA1,SHA256=0FF6F2C94BC7E2833A5F7E16DE1622E5DBA70396F31C7D5F56381870317E8C46,IMPHASH=AFACF6DC9041114B198160AAB4D0AE77",
          "parentImage": "C:\\\\Windows\\\\System32\\\\WindowsPowerShell\\\\v1.0\\\\powershell.exe",
          "company": "Microsoft Corporation",
          "commandLine": "\\\"C:\\\\Windows\\\\System32\\\\WindowsPowerShell\\\\v1.0\\\\powershell.exe\\\" -ExecutionPolicy Bypass -NoProfile -Command \\\"Get-Process | Out-File C:\\\\Users\\\\Public\\\\proc.txt\\\"",
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
          "message": "\"Process Create:\r\nRuleName: -\r\nUtcTime: 2026-02-01 13:48:56.008\r\nProcessGuid: {b637f13d-59c8-697f-923c-000000000d00}\r\nProcessId: 14960\r\nImage: C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe\r\nFileVersion: 10.0.26100.5074 (WinBuild.160101.0800)\r\nDescription: Windows PowerShell\r\nProduct: Microsoft® Windows® Operating System\r\nCompany: Microsoft Corporation\r\nOriginalFileName: PowerShell.EXE\r\nCommandLine: \"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe\" -ExecutionPolicy Bypass -NoProfile -Command \"Get-Process | Out-File C:\\Users\\Public\\proc.txt\"\r\nCurrentDirectory: C:\\WINDOWS\\system32\\\r\nUser: Alex-D\\dance\r\nLogonGuid: {b637f13d-a0af-6977-3707-060000000000}\r\nLogonId: 0x60737\r\nTerminalSessionId: 1\r\nIntegrityLevel: Medium\r\nHashes: MD5=A97E6573B97B44C96122BFA543A82EA1,SHA256=0FF6F2C94BC7E2833A5F7E16DE1622E5DBA70396F31C7D5F56381870317E8C46,IMPHASH=AFACF6DC9041114B198160AAB4D0AE77\r\nParentProcessGuid: {b637f13d-59a0-697f-893c-000000000d00}\r\nParentProcessId: 24824\r\nParentImage: C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe\r\nParentCommandLine: \"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe\" \r\nParentUser: Alex-D\\dance\"",
          "version": "5",
          "systemTime": "2026-02-01T13:48:56.0114859Z",
          "eventRecordID": "46555",
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
      "firedtimes": 4,
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
    "decoder": {
      "name": "windows_eventchannel"
    },
    "input": {
      "type": "log"
    },
    "@timestamp": "2026-02-01T13:48:56.348Z",
    "location": "EventChannel",
    "id": "1769953736.953712",
    "timestamp": "2026-02-01T15:48:56.348+0200"
  },
  "fields": {
    "timestamp": [
      "2026-02-01T13:48:56.348Z"
    ],
    "@timestamp": [
      "2026-02-01T13:48:56.348Z"
    ]
  }
}
