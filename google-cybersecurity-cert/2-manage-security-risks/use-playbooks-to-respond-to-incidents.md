# Course 2 — Play It Safe: Manage Security Risks

## Module 4: Use Playbooks to Respond to Incidents

---

### Overview
This module is about what a security team actually *does* when something goes wrong, and how they do it consistently. The core tool is the **playbook** — a predefined guide for responding to an incident — and the core model is the **six-phase incident response** process it's built around. It also covers how playbooks work alongside SIEM and SOAR tools, and why a team's makeup matters as much as its tooling.

**Incident response** is an organization's quick attempt to **identify an attack, contain the damage, and correct the effects** of a security breach. Doing that well under pressure is almost impossible to improvise — which is the whole reason playbooks exist.

---

### Playbooks
A **playbook** is a manual that provides details about any operational action — in practice, a **predefined, step-by-step guide** for how to respond to a specific type of situation or incident. It takes the thinking that would otherwise happen in a panic and works it out ahead of time.

**Why they matter:**
- **Consistency** — every responder handles the same incident the same correct way, no matter their experience level.
- **Speed** — no one is figuring out the steps from scratch while an attack is live.
- **Fewer mistakes** — nothing critical gets skipped or forgotten under pressure.
- **Repeatable and auditable** — the response can be reviewed and improved afterward.

Playbooks are **living documents** — they get refined after each incident as the team learns what worked and what didn't (which is exactly what the post-incident phase feeds back into). The same idea extends beyond incidents to responding to **threats, risks, and vulnerabilities** in a structured, repeatable way.

---

### The 6 Phases of an Incident Response Playbook
The phases run in order, and a good playbook walks the responder through each one:

| # | Phase | What happens |
|---|-------|--------------|
| 1 | **Preparation** | Put procedures, tools, and training in place *before* an incident so the team can respond effectively when one hits. |
| 2 | **Detection and Analysis** | Detect the incident and analyze it — confirm it's real, then determine its scope and severity. |
| 3 | **Containment** | Limit the damage and stop the incident from spreading further. |
| 4 | **Eradication and Recovery** | Fully remove the threat (malware, compromised accounts, etc.) and restore affected systems to normal operation. |
| 5 | **Post-incident activity** | Review what happened — document it, run a lessons-learned / post-mortem, and improve processes and the playbook itself. |
| 6 | **Coordination** | Report and share information with the right stakeholders throughout the entire process. |

---

### How Playbooks Work with SIEM and SOAR
These three tie Module 3 and Module 4 together into one response pipeline:

1. **SIEM** collects logs, then **detects and alerts** on suspicious activity.
2. The analyst pulls up the **relevant playbook** and follows it to respond to that alert — validate it, assess severity, contain, and escalate as needed.
3. **SOAR (Security Orchestration, Automation, and Response)** tools can **automate** those playbook steps, executing the response with less manual effort and speeding everything up.

So the flow is: **logs → SIEM (detect & alert) → playbook (how to respond) → SOAR (automate the response).** The playbook is the bridge between an alert firing and a correct response happening.

---

### Diversity of Perspectives on a Security Team
A team's ability to spot and respond to threats depends heavily on *who* is on it. Diverse backgrounds, experiences, and viewpoints strengthen security because different people notice different risks — a team that all thinks alike shares the same **blind spots**, and attackers exploit blind spots. Diversity of perspective improves problem-solving, sharpens decision-making, and closes gaps that a more uniform team would miss entirely.
