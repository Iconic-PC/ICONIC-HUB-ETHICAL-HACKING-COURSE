# Lab 1 — Introduction to Cybersecurity & Ethical Hacking

**Course Code:** ICH-EH101
**Course Title:** Cybersecurity Fundamentals & Ethical Hacking
**Provider:** Iconic Hub
**Week:** 1
**Lab:** 1

> **Secure. Build. Innovate.**
> *Where Web Development Meets Cybersecurity.*

---

## Course Focus

Cybersecurity Fundamentals and Introduction to Ethical Hacking


---

## Main Goal

By the end of Lab 1, students should be able to:

* Define cybersecurity and information security.
* Explain the fundamental security concepts.
* Explain the CIA Triad.
* Define threat, vulnerability, risk, asset, and security control.
* Explain what ethical hacking means.
* Distinguish ethical hacking from malicious/unauthorized hacking.
* Identify common types of hackers.
* Explain authorization, scope, and Rules of Engagement.
* Describe the basic penetration-testing methodology.
* Understand why ethical hackers must operate within legal and authorized boundaries.

---

# 1. WHAT IS CYBERSECURITY?

## Definition

Cybersecurity is the practice of protecting computers, networks, applications, devices, systems, and digital information from unauthorized access, misuse, alteration, disruption, destruction, or theft.

### In simpler terms

Cybersecurity is about protecting digital systems and information from people, software, or events that could cause harm.

Cybersecurity involves more than technical tools. It combines:

* People
* Processes
* Technology
* Policies
* Security controls
* Risk management
* Monitoring
* Incident response

### Example

Imagine a university has an online student portal containing:

* Student names
* Matriculation numbers
* Passwords
* Course registrations
* Examination results
* Financial information

The university needs cybersecurity to ensure:

* Unauthorized people cannot access the records.
* Students' results cannot be secretly modified.
* The system remains available to legitimate users.

---

# 2. WHY IS CYBERSECURITY IMPORTANT?

Organizations depend heavily on technology.

Businesses use computers and networks for:

* Banking
* Communication
* Customer management
* Payroll
* Manufacturing
* Cloud services
* Data storage
* Online transactions

If these systems are compromised, the organization may experience:

* Financial losses
* Data breaches
* Service disruption
* Privacy violations
* Reputation damage
* Legal consequences
* Loss of customer trust

### Example

Suppose an attacker gains access to an organization's customer database.

The attacker may:

* Steal customer information.
* Modify records.
* Delete data.
* Sell information.
* Use credentials for further attacks.

Cybersecurity attempts to prevent, detect, and respond to these situations.

---

# 3. INFORMATION SECURITY

Information security, often abbreviated as **InfoSec**, is the practice of protecting information and information systems from unauthorized access, use, disclosure, disruption, modification, or destruction.

Information can exist in many forms.

### Digital Information

* Databases
* Emails
* Documents
* Passwords
* Source code
* Cloud data

### Physical Information

* Printed documents
* Files
* Contracts
* Paper records

Therefore, information security is broader than simply protecting computers.

---

# 4. CYBERSECURITY VS INFORMATION SECURITY

These terms are related but not identical.

| Cybersecurity                                                        | Information Security                                                |
| -------------------------------------------------------------------- | ------------------------------------------------------------------- |
| Primarily concerned with digital systems and cyber threats           | Protects information regardless of its form                         |
| Focuses strongly on computers, networks, applications and devices    | Includes digital, physical and procedural protection                |
| Includes penetration testing, network security and endpoint security | Includes information classification, policies and physical security |
| Primarily associated with the digital environment                    | Broader information-protection discipline                           |

### Example

A firewall protecting a web server is a **cybersecurity control**.

A locked cabinet containing confidential printed documents is an **information-security control**.

---

# 5. THE CIA TRIAD

One of the most important concepts students must understand is the **CIA Triad**.

CIA stands for:

* **C — Confidentiality**
* **I — Integrity**
* **A — Availability**

These represent three fundamental objectives of information security.

---

# 6. CONFIDENTIALITY

Confidentiality means ensuring that information is accessible only to authorized individuals, systems, or processes.

The goal is to prevent unauthorized disclosure.

### Example

Suppose a university stores students' examination results.

Only authorized:

* Students
* Lecturers
* Administrators

should be able to access the appropriate information.

If an unauthorized student gains access to another student's result, confidentiality has been compromised.

### Controls That Support Confidentiality

* Passwords
* Encryption
* Access control
* Multi-factor authentication
* Data classification
* Least privilege

---

# 7. INTEGRITY

Integrity means ensuring that information remains accurate, complete, trustworthy, and protected from unauthorized modification.

### Example

A student's examination result is:

```text
65%
```

An attacker changes it to:

```text
95%
```

without authorization.

The information's integrity has been compromised.

### Controls That Support Integrity

* Hashing
* Digital signatures
* Access controls
* Audit logs
* File integrity monitoring
* Database controls
* Version control

---

# 8. AVAILABILITY

Availability means ensuring that authorized users can access systems, services, and information when required.

### Example

Suppose an online banking application is attacked and becomes unavailable.

Customers cannot access their accounts even though their credentials are correct.

The system's availability has been compromised.

### Controls That Support Availability

* Backups
* Redundant servers
* Failover systems
* Load balancing
* Disaster recovery
* DDoS protection
* Monitoring
* UPS/power backup

---

# 9. CIA TRIAD EXAMPLE

Consider an online banking system.

| Principle       | Question                                      | Example                                                                 |
| --------------- | --------------------------------------------- | ----------------------------------------------------------------------- |
| Confidentiality | Who should be allowed to see this?            | Only authorized customers and employees can access account information. |
| Integrity       | Can unauthorized people change this?          | Unauthorized people cannot modify account balances.                     |
| Availability    | Can authorized users access this when needed? | Customers can access the banking service when they need it.             |

### Easy Way to Remember

**Confidentiality = Don't reveal it.**

**Integrity = Don't alter it improperly.**

**Availability = Don't make it inaccessible.**

---

# 10. WHAT IS AN ASSET?

An **asset** is anything valuable to an organization or individual that requires protection.

## Hardware

* Servers
* Computers
* Routers
* Switches
* Smartphones
* Storage devices

## Software

* Operating systems
* Applications
* Databases
* APIs

## Information

* Passwords
* Customer records
* Financial information
* Medical records
* Source code
* Intellectual property

## Human Resources

* Employees
* Administrators
* Security personnel

## Business Assets

* Reputation
* Customer relationships
* Business processes
* Intellectual property

### Example

For a bank:

> Customer account information is an asset.

For a university:

> Student academic records are an asset.

---

# 11. WHAT IS A THREAT?

A **threat** is anything that has the potential to cause harm to an asset, system, organization, or individual.

A threat does not necessarily mean that an attack has already happened.

### Examples

* Malware
* Cybercriminals
* Phishing
* Insider threats
* Hardware failure
* Fire
* Power failure
* Natural disasters
* DDoS attacks

---

# 12. THREAT ACTOR

A **threat actor** is a person, group, or organization that carries out or may carry out malicious activity.

Examples include:

* Cybercriminal groups
* Nation-state groups
* Hacktivists
* Malicious insiders
* Fraudsters
* Script users

### Example

Suppose an attacker wants to steal a company's database.

The attacker is the:

> **Threat actor**

The attempt to steal the database represents a:

> **Threat**

---

# 13. WHAT IS A VULNERABILITY?

A **vulnerability** is a weakness or flaw in a system, application, network, configuration, process, or human practice that could be exploited.

### Examples

* Outdated software
* Weak passwords
* Poor access control
* Misconfigured servers
* SQL injection
* Cross-site scripting
* Unnecessary open ports
* Default credentials
* Unpatched operating systems

### Example

A web application accepts user input without properly validating it.

That weakness could potentially allow an attacker to manipulate the application's behavior.

The weakness is the:

> **Vulnerability**

---

# 14. WHAT IS RISK?

Risk is the possibility that a threat will exploit a vulnerability and cause harm or loss.

A simplified formula is:

> **Risk = Likelihood × Impact**

Where:

### Likelihood

How likely is the event to occur?

### Impact

How serious would the consequences be?

### Example

A company's web server runs outdated software.

**Asset:** Web server

**Threat:** Attacker

**Vulnerability:** Outdated software

**Risk:** The attacker could exploit the software and gain unauthorized access.

---

# 15. WHAT IS A SECURITY CONTROL?

A security control is a safeguard or countermeasure used to prevent, detect, reduce, or respond to security risks.

Controls can be divided into several categories.

## Technical Controls

* Firewall
* Antivirus/EDR
* Encryption
* MFA
* IDS/IPS
* Access control

## Administrative Controls

* Security policies
* Security training
* Incident-response procedures
* Risk assessments
* Password policies

## Physical Controls

* Door locks
* Security guards
* CCTV
* Biometric systems
* Security gates

---

# 16. PUTTING THE CONCEPTS TOGETHER

This is an important example for students.

## Scenario

A company has an internet-facing web server running outdated software.

### Asset

Web server.

### Threat Actor

Cybercriminal.

### Threat

Attempt to compromise the server.

### Vulnerability

Outdated software.

### Risk

The attacker could exploit the vulnerability and gain unauthorized access.

### Controls

* Patch management
* Firewall
* IDS/IPS
* Vulnerability scanning
* Access control
* Monitoring

The relationship can be remembered as:

```text
Asset → Vulnerability → Threat → Risk → Control
```

---

# 17. WHAT IS ETHICAL HACKING?

Now we move into the main subject.

**Ethical hacking** is the authorized practice of identifying, analyzing, and testing security weaknesses in systems, networks, applications, or devices to help improve their security.

An ethical hacker uses techniques similar to those used by attackers, but operates with:

* Authorization
* Defined scope
* Rules
* Documentation
* Professional responsibility

### Simple Definition

> **Ethical hacking is hacking with permission for the purpose of improving security.**

---

# 18. WHY DO ORGANIZATIONS HIRE ETHICAL HACKERS?

Organizations may perform penetration testing to discover security weaknesses before malicious attackers exploit them.

Ethical hackers may help organizations:

* Identify vulnerabilities
* Test security controls
* Validate security configurations
* Assess exposure
* Demonstrate potential impact
* Recommend remediation
* Verify that vulnerabilities have been fixed

### Basic Idea

Instead of waiting for a criminal to discover a vulnerability:

> The organization authorizes a security professional to look for it first.

---

# 19. ETHICAL HACKER VS MALICIOUS HACKER

The technical skills can overlap.

The critical difference is **authorization and purpose**.

| Ethical Hacker             | Malicious Hacker                                      |
| -------------------------- | ----------------------------------------------------- |
| Has authorization          | Does not have authorization                           |
| Works within defined scope | Acts outside approved scope                           |
| Tests security             | Attempts to exploit systems for unauthorized purposes |
| Documents findings         | May conceal activity                                  |
| Reports vulnerabilities    | May steal, alter or destroy information               |
| Helps improve security     | Causes or attempts to cause harm                      |

---

# 20. AUTHORIZATION

Authorization is permission to perform a specific security activity against a defined target.

This is one of the most important concepts in the course.

Suppose a company says:

> "You are authorized to test our web application at test.example.com."

That does not automatically mean the ethical hacker can:

* Attack unrelated domains.
* Attack employee personal accounts.
* Attack third-party services.
* Attack other company systems outside the agreed scope.

The permission has boundaries.

---

# 21. SCOPE

Scope defines what the security tester is allowed and not allowed to test.

A penetration-testing scope may specify:

## In Scope

* IP addresses
* Domains
* Applications
* APIs
* Servers

## Out of Scope

* Production systems
* Third-party infrastructure
* Employee devices
* Specific sensitive systems

It may also specify:

* Testing dates
* Allowed techniques
* Testing windows
* Contact information
* Emergency procedures

---

# 22. RULES OF ENGAGEMENT

Rules of Engagement (**RoE**) define how an authorized security assessment should be performed.

They may specify:

* Who authorized the test
* What systems can be tested
* When testing can occur
* What techniques are permitted
* What techniques are prohibited
* How evidence should be handled
* How incidents should be reported
* What to do if a critical vulnerability is discovered

The purpose is to prevent the security test itself from causing unnecessary damage.

---

# 23. RESPONSIBLE DISCLOSURE

When a security researcher discovers a vulnerability, responsible disclosure generally means reporting it appropriately rather than exploiting or publicly exposing it irresponsibly.

A typical process might involve:

1. Discover vulnerability.
2. Validate it safely.
3. Document evidence.
4. Notify the appropriate organization.
5. Give the organization an opportunity to investigate and remediate.
6. Coordinate any later disclosure where appropriate.

---

# 24. TYPES OF HACKERS

## White Hat

A security professional who performs authorized security testing.

**Example:**

A penetration tester hired by a company.

## Black Hat

An individual who conducts unauthorized malicious activity.

Examples may include:

* Data theft
* Unauthorized access
* Malware deployment
* Fraud

## Grey Hat

Someone whose activities may fall between traditional white-hat and black-hat categories, particularly when they access or test systems without authorization but may not have an explicitly malicious objective.

**Important:**

Lack of malicious intent does not automatically make unauthorized access legal or ethical.

## Script User

A person who relies heavily on existing tools or scripts without necessarily understanding the underlying techniques.

They may use publicly available tools with limited technical understanding.

## Hacktivist

An individual or group that uses hacking techniques in connection with ideological, political, or social causes.

## Insider

A person with legitimate access who misuses that access or accidentally creates a security problem.

---

# 25. WHAT IS PENETRATION TESTING?

Penetration testing, or **pentesting**, is an authorized security assessment in which testers simulate aspects of real-world attacks to identify and demonstrate security weaknesses.

A penetration test generally involves:

1. Planning
2. Reconnaissance
3. Scanning
4. Enumeration
5. Vulnerability analysis
6. Controlled exploitation
7. Post-exploitation assessment
8. Evidence collection
9. Reporting
10. Remediation/retesting

---

# 26. BASIC ETHICAL HACKING METHODOLOGY

This will become the backbone of your entire one-month course.

## Phase 1 — Planning and Authorization

Before touching the target:

* Obtain permission.
* Define scope.
* Establish rules.
* Identify contacts.
* Determine testing windows.

↓

## Phase 2 — Reconnaissance

Collect information about the target.

↓

## Phase 3 — Scanning

Identify:

* Hosts
* Ports
* Services
* Technologies

↓

## Phase 4 — Enumeration

Gather more detailed information about exposed services.

↓

## Phase 5 — Vulnerability Analysis

Identify weaknesses and assess their significance.

↓

## Phase 6 — Exploitation

Within the authorized lab/scope, safely demonstrate whether a vulnerability can actually be exploited.

↓

## Phase 7 — Post-Exploitation

Determine what access or impact could follow, without exceeding the agreed scope.

↓

## Phase 8 — Reporting

Document:

* Vulnerability
* Evidence
* Impact
* Risk
* Remediation

↓

## Phase 9 — Remediation and Retesting

The organization fixes the issue.

The tester can then verify whether the vulnerability has been resolved.

---

# 27. COMMON CYBERATTACK CATEGORIES

Students should understand the major categories before learning specific tools.

## Malware

Malicious software such as:

* Viruses
* Worms
* Trojans
* Ransomware
* Spyware
* RATs

## Phishing

Attempts to manipulate users into:

* Revealing credentials
* Opening malicious files
* Clicking malicious links
* Performing unauthorized actions

## Password Attacks

Examples include:

* Brute-force attacks
* Password spraying
* Credential stuffing
* Dictionary attacks
* Credential theft

## Web Application Attacks

Examples include:

* SQL injection
* Cross-site scripting
* Broken access control
* File-upload vulnerabilities
* Command injection
* SSRF

## Network Attacks

Examples include:

* Man-in-the-middle attacks
* ARP spoofing
* DNS-related attacks
* Packet interception
* Unauthorized network access

## Denial-of-Service

Attempts to make a service unavailable to legitimate users.

---

# 28. COMMON DEFENSIVE CONTROLS

| Attack              | Example Controls                                  |
| ------------------- | ------------------------------------------------- |
| Malware             | EDR, antivirus, patching                          |
| Phishing            | Email filtering, MFA, awareness training          |
| Password attacks    | MFA, strong passwords, rate limiting              |
| SQL injection       | Parameterized queries, input validation           |
| XSS                 | Output encoding, input validation, CSP            |
| Network attacks     | Firewalls, IDS/IPS, segmentation                  |
| DDoS                | Rate limiting, traffic filtering, DDoS mitigation |
| Unauthorized access | MFA, access control, least privilege              |

---

# 29. PRINCIPLE OF LEAST PRIVILEGE

Least privilege means giving a user, application, or system only the permissions necessary to perform its legitimate function.

### Example

A normal employee doesn't need administrator privileges on every server.

Instead:

```text
Employee → Required business permissions

Administrator → Administrative permissions
```

This reduces the damage that could occur if an account becomes compromised.

---

# 30. DEFENSE IN DEPTH

Defense in depth means using multiple layers of security instead of depending on a single security control.

### Example

```text
Internet
    ↓
Firewall
    ↓
Network Segmentation
    ↓
Authentication
    ↓
Application Security
    ↓
Endpoint Security
    ↓
Logging & Monitoring
```

If one layer fails, another layer may still prevent or detect the attack.

---

# 31. THE ETHICAL HACKER'S MINDSET

An ethical hacker should think:

> **"What can go wrong?"**

rather than:

> **"What tool can I run?"**

When looking at a system, ask:

### 1. What am I protecting?

**Asset**

### 2. Who could attack it?

**Threat actor**

### 3. What weaknesses exist?

**Vulnerabilities**

### 4. What could happen?

**Impact**

### 5. How likely is it?

**Risk**

### 6. How can it be prevented or detected?

**Security controls**

### 7. How can I safely test it?

**Authorized security testing**

---

# 32. IMPORTANT RULES YOU SHOULD KNOW AS A HACKER

### Rule 1 — Never test a system without authorization.

### Rule 2 — Know your scope before testing.

### Rule 3 — Do not access data you do not need.

### Rule 4 — Do not intentionally damage systems.

### Rule 5 — Do not perform tests against third parties unless explicitly authorized.

### Rule 6 — Preserve evidence.

### Rule 7 — Document what you do.

### Rule 8 — Report vulnerabilities responsibly.

### Rule 9 — Use isolated labs for learning and experimentation.

### Rule 10 — Never assume that "I was only testing" automatically makes unauthorized activity acceptable.

---

# 33. LAB 1 PRACTICAL EXERCISE

Don't start Lab 1 with Metasploit.

Give students a scenario-based security exercise first.

## Scenario

A university operates an online student portal.

The portal contains:

* Student profiles
* Course registrations
* Examination results
* Payment information

A security assessment discovers that the server is running outdated software.

Ask students to identify:

### Question 1

What is the asset?

**Answer:**

Student portal and the information it contains.

### Question 2

Who could be the threat actor?

**Answer:**

An unauthorized attacker/cybercriminal, for example.

### Question 3

What is the vulnerability?

**Answer:**

Outdated software.

### Question 4

What is the threat?

**Answer:**

An attacker attempting to exploit the vulnerable system.

### Question 5

What is the risk?

**Answer:**

Unauthorized access, data compromise, service disruption, or other impacts depending on the vulnerability.

### Question 6

Which CIA components could be affected?

Potentially all three:

* **Confidentiality** → unauthorized access to student data.
* **Integrity** → unauthorized modification of records.
* **Availability** → disruption of the portal.

### Question 7

Name security controls.

Possible answers:

* Patch management
* Firewall
* Access control
* MFA
* Vulnerability scanning
* IDS/IPS
* Monitoring
* Backups

---

# 34. LAB 1 CLASSROOM DISCUSSION

Ask the students these questions.

### Question 1

If you discover a vulnerability on Facebook, can you immediately exploit it because you are studying cybersecurity?

**Expected answer:**

No.

Studying cybersecurity does not automatically give you authorization to test someone else's system.

### Question 2

If you own a virtual machine and intentionally make it vulnerable, can you practice penetration testing against it?

**Answer:**

Yes, because you control the environment.

### Question 3

What makes ethical hacking different from unauthorized hacking?

**Answer:**

Authorization, scope, rules, and legitimate security purpose.

### Question 4

Is every vulnerability a risk?

**Answer:**

Not necessarily.

Risk depends on factors such as likelihood and impact.

### Question 5

Can a system have good confidentiality but poor availability?

**Answer:**

Yes.

For example, a database may be properly protected from unauthorized users but be unavailable because of a hardware failure or DDoS attack.

---

# Lab 1 Completion Checklist

Before moving to Lab 2, students should be able to explain:

* [ ] Cybersecurity
* [ ] Information Security
* [ ] Cybersecurity vs Information Security
* [ ] CIA Triad
* [ ] Confidentiality
* [ ] Integrity
* [ ] Availability
* [ ] Asset
* [ ] Threat
* [ ] Threat Actor
* [ ] Vulnerability
* [ ] Risk
* [ ] Security Control
* [ ] Ethical Hacking
* [ ] Authorization
* [ ] Scope
* [ ] Rules of Engagement
* [ ] Responsible Disclosure
* [ ] Types of Hackers
* [ ] Penetration Testing
* [ ] Ethical Hacking Methodology
* [ ] Common Cyberattack Categories
* [ ] Defensive Controls
* [ ] Least Privilege
* [ ] Defense in Depth
* [ ] Ethical Hacker's Mindset

---
