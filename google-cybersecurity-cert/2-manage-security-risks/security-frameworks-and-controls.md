## Module 2: Security Frameworks and Controls

---

### Overview
This module covers the tools and models security teams use to actually manage risk: **frameworks** (the plans), **controls** (the safeguards), the **CIA triad** (the model behind every security decision), and two key industry resources — the **NIST CSF** and **OWASP**. It closes with the **security audit** — the process of checking all of this against a set of expectations.

---

### Security Frameworks
**Guidelines used for building plans to help mitigate risks and threats to data and privacy.** A framework gives an organization a repeatable structure instead of improvising security from scratch.

Frameworks are used to:
1. **Identify and document** security goals.
2. **Set guidelines** to achieve those goals.
3. **Implement** strong security processes.
4. **Monitor and communicate** results.

---

### Security Controls
**Safeguards designed to reduce specific security risks.** If frameworks are the plan, controls are the actual mechanisms that carry it out. Three main categories:

| Type | What it is | Examples |
|------|------------|----------|
| **Physical** | Protects the physical environment and hardware | Locks, gates, badge/key-card readers, CCTV, security guards |
| **Technical** | Technology-based safeguards built into systems | Encryption, firewalls, authentication/MFA, antivirus, IDS/IPS |
| **Administrative** | Policies, procedures, and people-focused measures | Security policies, procedures, training, access guidelines |

---

### Frameworks vs. Controls — the Relationship
These two are constantly confused, so the distinction is worth nailing:

- A **framework** provides the structure and guidance — *what* needs protecting and *why*.
- A **control** is a specific safeguard you put in place — *how* you actually reduce the risk.

They work as a pair: a framework might set the goal "protect sensitive customer data," and the control that satisfies it is "encrypt that data at rest and in transit." Frameworks without controls are just intentions; controls without a framework are scattered and hard to measure.

---

### The CIA Triad
A foundational model that informs how organizations consider risk when setting up systems and security policies. Nearly every security decision is a tradeoff among these three:

- **Confidentiality** — only authorized users can access specific assets or data. *(Supported by encryption, access controls, authentication.)*
- **Integrity** — data is correct, authentic, and reliable; it hasn't been tampered with.
- **Availability** — data is accessible to those who are authorized to access it, when they need it.

---

### NIST Cybersecurity Framework (CSF)
A **voluntary** framework of standards, guidelines, and best practices to manage cybersecurity risk. Widely used as a common language across industries.

The CSF is organized around **core functions**. Note this is the **2.0** version, which has **6 functions** — the original five plus **Govern**, added at the center to tie the others to business strategy:

| Function | What it covers |
|----------|----------------|
| **Govern** | Establishing, overseeing, and improving the org's cybersecurity strategy, policies, roles, and risk management so they align with business goals and regulations. *(Central function — informs the other five.)* |
| **Identify** | Managing cybersecurity risk and understanding its effect on the org's people and assets. |
| **Protect** | Implementing policies, procedures, training, and tools to mitigate cybersecurity threats. |
| **Detect** | Identifying potential security incidents and improving monitoring to increase the speed and efficiency of detection. |
| **Respond** | Using proper procedures to contain, neutralize, and analyze incidents, then implement process improvements. |
| **Recover** | Returning affected systems to normal operation after an incident. |

> **Related:** **NIST SP 800-53** is a separate, more detailed framework — a unified set of controls for protecting information systems within the U.S. federal government.

---

### OWASP & Security Principles
**OWASP** (Open Web Application Security Project, now Open *Worldwide* Application Security Project) is a non-profit focused on improving software security. It publishes widely-used security principles for building and defending systems:

- **Minimize the attack surface area** — reduce the number of entry points (attack vectors) an attacker can use.
- **Principle of least privilege** — give users the minimum access they need to do their job, nothing more.
- **Defense in depth** — layer multiple controls so that if one fails, others still protect the asset.
- **Separation of duties** — split critical tasks across multiple people so no single person can abuse the system.
- **Keep security simple** — avoid unnecessary complexity; complicated systems are harder to secure and audit.
- **Fix security issues correctly** — find the root cause of a vulnerability, fix it, and test the fix rather than patching symptoms.
- **Establish secure defaults** — the out-of-the-box configuration should be the most secure option.
- **Fail securely** — when a control fails, it should default to a safe state (deny access rather than grant it).
- **Don't trust services** — don't implicitly trust third-party services or partners; verify them.
- **Avoid security by obscurity** — don't rely on secrecy (hidden code, secret algorithms) as your only line of defense.

---

### Security Audits
A **review of an organization's security controls, policies, and procedures against a set of expectations.** Audits surface gaps, verify compliance, and show where the security posture needs work.

**Two types:**
- **External audit** — performed by a third party outside the organization.
- **Internal audit** — performed by the organization's own team (the focus of this module).

**Key elements of an internal audit:**
1. **Establish the scope and goals** — which assets, policies, and systems are being assessed, and why.
2. **Conduct a risk assessment** — identify potential threats and vulnerabilities to those assets.
3. **Complete a controls assessment** — review existing controls across **administrative, technical, and physical** categories.
4. **Assess compliance** — check whether the org meets the regulations and standards that apply to it (e.g., GDPR, PCI DSS, HIPAA).
5. **Communicate results** — report findings, risks, and recommended fixes to stakeholders.

---

### Glossary — Key Terms
(CIA components and CSF core functions are defined in their sections above.)

- **Asset** — an item perceived as having value to an organization.
- **Attack vectors** — the pathways attackers use to penetrate security defenses.
- **Authentication** — the process of verifying who someone is.
- **Authorization** — granting access to specific resources in a system.
- **Biometrics** — unique physical characteristics that can be used to verify a person's identity.
- **Encryption** — converting data from a readable format to an encoded format.
- **Risk** — anything that can impact the confidentiality, integrity, or availability of an asset.
- **Security posture** — an organization's ability to manage its defense of critical assets and data, and to react to change.
- **Threat** — any circumstance or event that can negatively impact assets.
