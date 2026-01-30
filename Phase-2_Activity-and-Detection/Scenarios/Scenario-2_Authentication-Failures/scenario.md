# Scenario 2 – Failed Authentication (Local)

## Objective
I wanted to see how a simple failed local authentication attempt appears in the SIEM
and what information is available for analysis from a SOC Tier 1 perspective.

## Environment
- Windows 11 endpoint (real system)
- Wazuh SIEM
- Windows Security logs collected
- Sysmon enabled (not the main source for this scenario)

## Activity Performed
From an interactive user session, I executed the following command:

`runas /user:testuser cmd`

An incorrect password was intentionally entered.

After the failed attempt, the command prompt closed immediately.
No visible error message was displayed on the endpoint.

## Detection
Wazuh detected the failed authentication attempt using Windows Security logs
on endpoint **ALEXD**.

The event was visible in the SIEM even though no clear feedback was shown locally.

## Alert Details
- Event ID: 4625  
- Target user: `testuser`  
- Initiating user: `Alex-D\dance`  
- Logon type: 2 (Interactive)  
- Authentication package: Negotiate  
- Process involved: `svchost.exe`  
- Result: Logon failure  

## Analysis
This activity represents a failed interactive logon attempt.

From an analyst perspective, this type of event can be caused by:
- simple user error
- incorrect credentials
- misconfiguration
- early credential access attempts

In this case, the attempt was isolated and intentional.
No repeated failures or follow-up activity were observed.

## Limitations
The `runas` command does not show a visible error message after a failed authentication,
as the process exits immediately.

Because of this, confirmation of the failure relies entirely on log analysis rather than endpoint output.

## Analyst Decision
I classified this activity as **benign but security-relevant**.

No escalation was required.
The endpoint should remain monitored in case similar failures begin to occur repeatedly or alongside other suspicious activity.
