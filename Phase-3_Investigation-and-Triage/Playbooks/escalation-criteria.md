# Escalation Criteria – When to Escalate an Incident

This document defines the clear conditions under which an incident
should be escalated from triage to incident response
or a higher level of analysis.

Escalation is based on observed behavior,
not on assumptions or external context.

---

## 1. Authentication (High Priority)

Immediate escalation if any of the following are observed:
- a clear **4625 → 4624** sequence
- successful authentication after repeated failures
- login from an unusual context (time, user, method)

This type of activity indicates a potential compromise.

---

## 2. Post-Authentication Activity

Escalate if, after a successful login, the following are observed:
- enumeration commands (whoami, net, sc, powershell)
- attempts to escalate privileges
- multiple executions within a short time window

Even if privilege escalation does not succeed,
the observed intent is sufficient to justify escalation.

---

## 3. Persistence

Automatic escalation if any of the following are detected:
- registry run keys
- scheduled tasks
- newly created services
- changes that survive a system reboot

Persistence indicates confirmed impact
and requires immediate response.

---

## 4. Network Activity

Escalate if any of the following occur:
- outbound network traffic immediately after execution
- connections to unknown IP addresses or domains
- beaconing or repetitive communication patterns

Process-to-network correlation is a strong indicator of malicious activity.

---

## 5. Volume and Pattern

Escalate if:
- events are repetitive
- multiple users or systems are affected
- activity indicates automation

Even without immediate success,
the pattern alone justifies escalation.

---

## 6. When Not to Escalate Immediately

Do not escalate if:
- the event is isolated
- there is no follow-up activity
- no observable impact is present
- the activity can be legitimately explained

In these cases, the event is documented and monitored.

---

## 7. Final Principle

Escalation decisions are based on:
- **impact**
- **intent**
- **correlation**

Not on:
- the number of alerts
- initial severity level
- assumptions.
