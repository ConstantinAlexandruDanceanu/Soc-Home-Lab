# Triage Process – SOC Analyst Notes

This document describes how I perform triage on alerts
in Wazuh, with the goal of quickly deciding whether an event
is noise, suspicious, or requires escalation.

I do not treat every alert as a separate incident.
The primary objective is identifying patterns.

---

## 1. Initial Alert Review

When an alert appears, I first look at:
- rule.id and description
- event type (auth, process, file, network)
- frequency (single vs repetitive)
- involved agent / user

The initial question is:
> Is this an isolated event or part of a sequence?

---

## 2. Alert Grouping

If I observe:
- the same rule
- the same user
- the same source
- a short time window

I treat the alerts as **a single incident**,
not as separate events.

---

## 3. Analysis Order (Important)

I always follow the same order:

1. **Authentication**
   - Are there 4625 events (failures)?
   - Is there a 4624 event (success)?
   - Is there a fail → success sequence?

2. **Process Execution**
   - Which process is running?
   - With what parameters?
   - Who started it?

3. **Post-Login Activity**
   - Enumeration activity?
   - Privilege escalation attempts?
   - Persistence mechanisms?

4. **Network**
   - Any outbound connections?
   - Unusual IP addresses?
   - Communication immediately after execution?

---

## 4. Initial Classification

After triage, I classify the alert as:
- **Benign** – explainable activity, no pattern
- **Suspicious** – ambiguous activity requiring correlation
- **Malicious** – clear pattern or confirmed impact

If there is insufficient data, I do not force a verdict.

---

## 5. Importance of Negative Evidence

I explicitly verify the absence of:
- successful authentication
- persistence mechanisms
- network activity
- subsequent process execution

The lack of these elements is documented
and influences the final decision.

---

## 6. Triage Outcome

At the end of triage, I decide one of the following:
- close the alert as benign
- document it as suspicious and monitor
- escalate it for advanced investigation

The purpose of triage is speed and clarity,
not full investigation.
