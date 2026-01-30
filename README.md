# SOC Home Lab – Entry-Level Project

This repository documents a personal SOC-style home lab built to understand
how endpoint activity is collected, detected, and analyzed using a SIEM.

The lab focuses on visibility, alert interpretation, and basic analyst decision-making,
from an entry-level SOC Analyst perspective.

---

## What This Lab Is About

The main goal of this lab is to move beyond theory and understand:
- how real Windows activity appears in logs
- how alerts are generated in a SIEM
- how to decide whether activity is benign or requires attention

The lab is built step by step and documented as it evolves, reflecting how
a junior analyst would approach learning and validation in a real SOC environment.

---

## Environment Overview

- **Windows 11 endpoint** (real system)
- **Wazuh SIEM**
- **Sysmon** for enhanced endpoint visibility
- **Kali Linux** used to simulate external network activity

All actions performed in the lab are controlled and intentional.

---

## Project Structure

The lab is organized into phases, each focusing on a different aspect of SOC work:

- **Phase 1 – Environment and Visibility**  
  Building the environment and validating that logs reach the SIEM.

- **Phase 2 – Activity and Detection**  
  Executing common actions and observing how they are detected and logged.

- **Phase 3 – Investigation and Triage**  
  Analyzing alerts, correlating events, and making basic analyst decisions.

- **Phase 4 – Response and Hardening**  
  Documenting response considerations, limitations, and potential improvements.

Each phase includes documentation and screenshots to show both the actions taken
and the resulting visibility in the SIEM.

---

## Perspective

This project is intentionally scoped for an **entry-level SOC Analyst** role.

It does not attempt to demonstrate advanced exploitation, threat hunting,
or incident response.
Instead, it focuses on:
- understanding alerts
- building context
- learning when *not* to escalate

---

## Status

Work in progress.

The lab is continuously updated as new scenarios are added and existing ones
are refined.

---

## Why This Lab Exists

This project exists to demonstrate:
- hands-on interaction with a SIEM
- basic security analysis skills
- structured thinking and documentation
- a realistic learning approach aligned with SOC Tier 1 responsibilities
