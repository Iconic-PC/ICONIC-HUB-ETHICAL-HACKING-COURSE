# ICH-EH102 — LAB 12: RECONNAISSANCE-TO-ASSESSMENT WORKFLOW

**Engagement Type:** Internal Security Assessment  
**Client:** Meridian Retail Services Ltd. *(Fictional organization)*  
**Engagement Reference:** MRS-SEC-2026-012  
**Assessment Team:** Iconic Hub Cybersecurity  
**Target Environment:** Simulated customer-facing web server  
**Assessment Platform:** Kali Linux  
**Primary Tools:** Nmap and Greenbone Community Edition (GVM/OpenVAS)  
**Assessment Method:** Reconnaissance, Service Enumeration, Vulnerability Assessment, Vulnerability Scanning, Risk Analysis, and Reporting  
**Classification:** Confidential — Simulated Client Engagement

---

## 1. Executive Scenario

Meridian Retail Services Ltd. is a fictional e-commerce company preparing to launch an updated online shopping platform. The platform will allow customers to browse products, access accounts, and interact with online services.

Before the platform is approved for production, the company's IT manager requests an internal security assessment. The objective is to identify exposed services, investigate potential vulnerabilities, and provide recommendations that can be implemented before launch.

The IT manager has authorized Iconic Hub Cybersecurity to conduct a limited, non-destructive assessment of the designated staging server.

The assessment team has been provided with the following information:

- The target represents a staging web server.
- The system is accessible from an isolated assessment network.
- The server's exact service configuration has not been independently verified.
- The client wants to understand which services are exposed and whether known weaknesses may affect the system.
- The assessment must not disrupt services, access customer data, or attempt to exploit vulnerabilities.
- A written report is required before the client's production-readiness review.

For this simulated engagement, the local OWASP Broken Web Applications (BWA) virtual machine represents the client's staging server. It is a deliberately vulnerable training environment and is not an actual Meridian Retail Services asset.

**Engagement objective:** Determine what can be established about the target's exposed services and potential vulnerabilities, assess the significance of the findings, and deliver an evidence-based remediation report.

The engagement does not authorize exploitation, credential attacks, denial-of-service testing, or testing outside the approved target.

## 2. Business Objectives

The client has identified five business objectives for the assessment.

1. **Understand external exposure:** Identify the network services accessible on the designated target from the assessment network.
2. **Identify potential weaknesses:** Determine whether the discovered services may be affected by known vulnerabilities or insecure configurations.
3. **Evaluate findings:** Review scanner results and supporting evidence to distinguish confirmed observations from potential vulnerabilities.
4. **Prioritize remediation:** Identify which issues should be investigated or addressed first.
5. **Support the launch decision:** Provide a clear report that helps the IT manager decide what needs to be resolved or investigated before production deployment.

The assessment team must provide technical evidence for its conclusions. It must not declare the system secure merely because a scan returns few findings.

## 3. Rules of Engagement

Before beginning technical work, the assessment team documents the approved scope and testing restrictions.

### 3.1 Authorized target

| Item | Engagement detail |
|---|---|
| Client | Meridian Retail Services Ltd. — fictional |
| Target | Simulated staging web server |
| Target platform | Local OWASP BWA VM |
| Target IP | To be verified from the running lab VM |
| Assessment workstation | Kali Linux |
| Network | Isolated VirtualBox Host-Only or approved Internal Network |
| Assessment period | Defined by the engagement owner |
| Primary purpose | Identify exposed services and potential vulnerabilities |

The target IP must be verified at the beginning of the engagement. An example address must never be assumed to be the actual target address.

### 3.2 Authorized activities

The engagement permits:

- Reviewing supplied system documentation.
- Collecting relevant preliminary information.
- Conducting limited network discovery against the authorized target.
- Identifying ports and services with Nmap.
- Reviewing potential vulnerabilities associated with discovered services.
- Running an appropriately configured Greenbone scan against the authorized target.
- Analyzing scanner output and recording evidence.
- Preparing remediation recommendations.
- Producing a technical report and management summary.

### 3.3 Prohibited activities

The engagement does not permit:

- Scanning unrelated systems or public IP addresses.
- Attempting to exploit identified vulnerabilities.
- Attempting to obtain unauthorized access.
- Password guessing or credential attacks.
- Denial-of-service or stress testing.
- Modifying or deleting target data.
- Accessing unrelated personal or confidential information.
- Expanding the target scope without written authorization.

If the target becomes unstable, unexpected sensitive data is exposed, or an activity risks disrupting the environment, the assessment team must stop the affected activity and follow the engagement's escalation process.

## 4. Assessment Methodology

The assessment follows a structured workflow.

<escape>
Authorization and Scope
        |
        v
Review Available Information
        |
        v
Passive Reconnaissance
        |
        v
Active Reconnaissance
        |
        v
Service Enumeration
        |
        v
Vulnerability Assessment
        |
        v
Greenbone Vulnerability Scan
        |
        v
Correlate and Validate Findings
        |
        v
Prioritize Risks
        |
        v
Report Findings and Remediation
        |
        v
Client Remediation and Authorized Retesting
</escape>

The workflow is iterative. If evidence is incomplete, the assessment team may return to an earlier stage to investigate further, provided the additional activity remains within the approved scope.

The team must maintain a distinction between:

- **Observed facts:** Information directly supported by collected evidence.
- **Potential vulnerabilities:** Weaknesses suggested by available information but not sufficiently confirmed.
- **Confirmed findings:** Issues supported by sufficient evidence to establish that the weakness applies.
- **Limitations:** Conditions that prevent the team from reaching a reliable conclusion.

## 5. Phase One: Engagement Preparation

**Objective:** Establish the authorized target, prepare the assessment environment, and maintain an auditable record of the work.

### 5.1 Verify the assessment workstation

On Kali Linux, open a terminal and execute:

```bash
whoami
```

Confirm the operating system:

```bash
cat /etc/os-release
```

Review the network configuration:

```bash
ip addr
```

Review the routing table:

```bash
ip route
```

Record the assessment workstation's relevant IP address and network interface.

### 5.2 Verify the target

Start the local OWASP BWA VM.

Obtain the target's current IP address from its console or documented network configuration. Confirm that it belongs to the isolated lab network.

Do not assume the address from a previous session is still correct.

### 5.3 Create the engagement workspace

Create a structured directory for the engagement:

```bash
mkdir -p ~/MRS-SEC-2026-012/{scope,notes,evidence,scans,reports}
```

Navigate into the workspace:

```bash
cd ~/MRS-SEC-2026-012
```

Create the engagement record:

```bash
nano scope/engagement-scope.txt
```

Record the following:

```text
ENGAGEMENT REFERENCE: MRS-SEC-2026-012

CLIENT:
Meridian Retail Services Ltd. (fictional)

ASSESSMENT TEAM:
Iconic Hub Cybersecurity

ASSESSMENT OBJECTIVE:
Identify exposed services and assess potential vulnerabilities
in the designated staging-server simulation.

AUTHORIZED TARGET:
Local OWASP Broken Web Applications VM

TARGET IP:
[Record the verified IP address]

ASSESSMENT WORKSTATION:
Kali Linux

NETWORK CONFIGURATION:
[Record the actual isolated network mode]

AUTHORIZED ACTIVITIES:
Limited reconnaissance, service enumeration,
vulnerability assessment, vulnerability scanning,
evidence collection, and reporting.

EXCLUSIONS:
Exploitation, credential attacks, denial-of-service testing,
unapproved targets, and destructive testing.

ASSESSMENT DATE:
[Record the actual date]

LIMITATIONS:
[Record relevant limitations]
```

Save the document.

### 5.4 Establish evidence handling

Use the engagement directory for all notes, command output, and scan reports.

Assign each evidence item a unique identifier, such as:

- `E-001` — Target network configuration.
- `E-002` — Nmap service scan.
- `E-003` — Greenbone scan report.
- `E-004` — Selected vulnerability details.
- `E-005` — Risk prioritization record.

These identifiers are examples. Use them only for evidence actually collected.

**Deliverable:** Engagement scope record and organized evidence directory.

## 6. Phase Two: Passive Reconnaissance

**Objective:** Review available information before sending technical probes to the target.

The client has provided only a general description of the staging environment. The assessment team must document what is known and what remains unverified.

### 6.1 Review available documentation

Review any supplied:

- System description.
- Network diagram.
- Asset inventory.
- Application documentation.
- Software inventory.
- Known service requirements.
- Security configuration documentation.

In this simulation, the local OWASP BWA documentation can provide background about the training environment.

Do not assume that the simulated system's actual configuration matches every detail in the documentation.

### 6.2 Record preliminary observations

Create a file:

```bash
nano notes/passive-reconnaissance.md
```

Use the following structure:

```text
PASSIVE RECONNAISSANCE RECORD

Engagement:
MRS-SEC-2026-012

Information source:
[Identify the documentation reviewed]

Information obtained:
[Record relevant facts]

Potential areas requiring investigation:
[Record areas that need technical verification]

Unverified assumptions:
[Record assumptions that have not been established]

Limitations:
[Record missing documentation or unavailable information]
```

### 6.3 Distinguish facts from assumptions

For example:

- **Observed fact:** The engagement documentation identifies the target as a staging web server.
- **Observed fact:** The supplied VM documentation describes the target as an intentionally vulnerable training environment.
- **Unverified assumption:** The target has an outdated web server.
- **Unverified assumption:** A discovered service is accessible to the public.

The last two statements cannot be reported as facts without supporting evidence.

**Deliverable:** Passive reconnaissance record documenting known information, unknowns, and investigation priorities.

## 7. Phase Three: Active Reconnaissance

**Objective:** Establish whether the authorized target is reachable from the assessment workstation.

### 7.1 Confirm the target address

Verify the target's current IP address using the VM console or documented configuration.

Record it in the engagement scope.

### 7.2 Check basic reachability

Replace `TARGET_IP` with the verified address:

```bash
ping -c 4 TARGET_IP
```

Record the result.

A successful response indicates that the target responded to the ICMP requests. A failed response does not prove the system is offline because ICMP may be blocked.

### 7.3 Record the result

Create a short record in:

```text
notes/active-reconnaissance.md
```

Include:

- Target IP address.
- Date and time of the check.
- Command used.
- Observed result.
- Any limitations.

Do not expand the discovery process to other hosts unless they have been explicitly authorized.

**Deliverable:** Active reconnaissance record identifying the target and documenting its observed reachability.

## 8. Phase Four: Service Enumeration

**Objective:** Identify exposed ports and services on the authorized staging-server simulation.

The client wants to understand which network services may increase the system's attack surface.

### 8.1 Verify Nmap availability

Run:

```bash
nmap --version
```

If Nmap is not installed, install it through the configured Kali repositories:

```bash
sudo apt update
sudo apt install nmap
```

### 8.2 Conduct service detection

Run the following against the verified local target:

```bash
nmap -sV --version-light -T3 TARGET_IP
```

This command performs service/version detection using relatively light version probing.

### 8.3 Save the evidence

Save a normal-format output file:

```bash
nmap -sV --version-light -T3 \
-oN scans/nmap-services.txt TARGET_IP
```

Save an XML version for structured review:

```bash
nmap -sV --version-light -T3 \
-oX scans/nmap-services.xml TARGET_IP
```

### 8.4 Analyze the output

For every relevant result, record:

- Port number.
- Service name.
- Detected version, if available.
- Port state.
- Whether the service is expected, if that information is available.
- Whether further investigation is warranted.

Use this table in the assessment notes:

| Port | Service | Detected version | Assessment observation |
|---|---|---|---|
| Actual result | Actual result | Actual result or unknown | Evidence-based observation |

Populate the table from the real scan output.

### 8.5 Interpret the results

The presence of an open port establishes that a service was reachable under the conditions of the scan. It does not automatically establish that the service is insecure.

The assessment team should identify services that may warrant further investigation, such as:

- Services not listed in the supplied asset documentation.
- Services with version information that may correspond to known vulnerabilities.
- Services that appear unnecessary for the system's intended role.
- Services that may require access restrictions.
- Services for which the available evidence is incomplete.

Do not label a service as unnecessary or vulnerable without sufficient evidence.

**Deliverable:** Service enumeration record and saved Nmap output.

## 9. Phase Five: Vulnerability Assessment

**Objective:** Investigate whether the discovered services may be affected by known weaknesses.

This phase uses the service information gathered during enumeration and any available system documentation.

### 9.1 Select areas for investigation

Select up to three services or observations from the actual Nmap output that warrant further review.

For each one, ask:

1. What software or service was identified?
2. Is the version information reliable?
3. Is the service expected to be available?
4. Is there an applicable vendor security advisory?
5. Does the available evidence establish that the vulnerability applies?
6. What additional validation is necessary?

### 9.2 Research relevant security advisories

Use authoritative sources where applicable:

- Software vendor security advisories.
- National Vulnerability Database: https://nvd.nist.gov/
- CVE Program: https://www.cve.org/
- Official product documentation.

Record the advisory or CVE identifier only when it is relevant to the observed product and version.

A CVE match based on incomplete version information should be treated as a lead for investigation, not automatic proof of vulnerability.

### 9.3 Document the assessment

Create:

```text
notes/vulnerability-assessment.md
```

Use the following structure:

```text
FINDING REFERENCE:
[Unique identifier]

OBSERVATION:
[What was directly observed]

AFFECTED SERVICE:
[Service and version, if established]

EVIDENCE:
[Reference to command output, scan result, or documentation]

POTENTIAL WEAKNESS:
[Describe the suspected security issue]

RELEVANT CVE OR ADVISORY:
[Identifier and source, if applicable]

ASSESSMENT STATUS:
[Confirmed, potential, or not established]

CONFIDENCE:
[Explain confidence and its basis]

REMAINING UNCERTAINTIES:
[Information that is missing]

RECOMMENDED NEXT STEP:
[Appropriate investigation or remediation action]
```

### 9.4 Maintain evidence-based language

Use precise statements in the report:

- “Nmap identified the service as…” when describing observed output.
- “The service may be affected by…” when applicability is not confirmed.
- “The available evidence supports…” when describing the basis for a conclusion.
- “Further validation is required…” when the evidence is incomplete.

Do not report that a system is exploitable unless the evidence establishes that conclusion and the engagement explicitly permits the necessary testing.

**Deliverable:** Preliminary vulnerability assessment record.

## 10. Phase Six: Vulnerability Scanning with Greenbone

**Objective:** Use Greenbone Community Edition to identify potential vulnerabilities in the authorized target and correlate its findings with the service enumeration results.

### 10.1 Confirm scanner readiness

Check that Greenbone is available and initialized.

If required, review the relevant setup status using the commands supported by the installed Kali package, such as:

```bash
sudo gvm-check-setup
```

If the scanner or feeds are not ready, document the limitation. Do not represent an incomplete scan as a completed assessment.

### 10.2 Create or verify the target

In the Greenbone interface, locate the target configuration.

Confirm that the target references the verified local training VM.

Use a descriptive target name, for example:

`MRS-SEC-2026-012-Staging-Simulation`

Confirm the address and scan scope before saving.

### 10.3 Create or verify the scan task

Create a scan task associated with the authorized target.

Select an appropriate standard scan configuration supported by the installed version.

Before starting the task, confirm:

- The target address is correct.
- The scan configuration is appropriate.
- The scanner is ready.
- The task does not include unrelated systems.

### 10.4 Run the scan

Start the task and monitor its status.

Allow the scan to finish if possible. Scan duration depends on the target, configuration, feed readiness, and available resources.

If the task fails or remains incomplete, record the status and relevant error information.

### 10.5 Review the results

For each meaningful finding, record:

- Finding name.
- Target host.
- Affected port or service, if provided.
- Reported severity.
- CVE identifier, if available.
- Supporting scanner evidence.
- Suggested remediation.
- Validation status.

Do not treat every reported result as a confirmed vulnerability.

### 10.6 Preserve the report

Export the report in an available format and save it in the engagement directory, for example:

```text
scans/greenbone-report.pdf
```

Use the actual extension of the exported file.

Record the task name, scan date, target, configuration, and completion status.

**Deliverable:** Greenbone scan report or documented scanner limitation.

## 11. Phase Seven: Correlate and Validate Findings

**Objective:** Determine what the combined evidence establishes and identify unresolved issues.

The assessment team must compare Nmap service information with Greenbone's reported findings.

### 11.1 Compare the evidence

Use a correlation table:

| Finding reference | Nmap evidence | Greenbone evidence | Assessment conclusion |
|---|---|---|---|
| F-001 | Actual observation | Actual observation | Supported conclusion |
| F-002 | Actual observation | Actual observation | Supported conclusion |
| F-003 | Actual observation | Actual observation | Supported conclusion |

Include only findings that actually exist in the collected evidence.

### 11.2 Investigate discrepancies

Possible discrepancies include:

- Nmap detects a service, but Greenbone reports no related vulnerability.
- Greenbone reports a potential issue that cannot be established from the available service information.
- The tools identify different service versions.
- A finding references a vulnerability whose applicability is uncertain.
- The scan did not complete or did not have sufficient feed coverage.

Record the discrepancy and the next step required to resolve it.

### 11.3 Understand false positives and false negatives

A **false positive** is a finding that does not represent the suspected vulnerability in the target's actual circumstances.

A **false negative** occurs when a vulnerability exists but is not detected.

The assessment team should consider incomplete feeds, service identification errors, configuration differences, and limited scanner coverage when interpreting results.

A clean report does not prove the absence of vulnerabilities.

### 11.4 Assign an assessment status

Use a consistent classification:

- **Confirmed:** Sufficient evidence establishes that the reported weakness applies.
- **Potential:** Evidence suggests a weakness, but applicability requires further validation.
- **Not established:** The available evidence is insufficient to determine whether the weakness exists.
- **Not applicable:** Reliable evidence establishes that the finding does not apply to the assessed system.
- **Not assessed:** The activity needed to evaluate the issue was outside the scope or was not completed.

Use “confirmed” only when the available evidence genuinely supports that status.

**Deliverable:** Correlation table and documented assessment status for each reported finding.

## 12. Phase Eight: Risk Analysis and Prioritization

**Objective:** Help the client determine which findings should be addressed or investigated first.

Severity ratings are useful, but they do not fully represent business risk.

### 12.1 Evaluate each finding

Consider:

- Reported severity and scoring method.
- Reliability of the evidence.
- Exposure of the affected service.
- Importance of the asset.
- Potential confidentiality, integrity, or availability impact.
- Available mitigating controls.
- Whether the finding is confirmed or potential.
- Remediation urgency and operational constraints.

### 12.2 Prepare the prioritization table

| Finding | Severity | Confidence | Business impact | Recommended priority |
|---|---|---|---|---|
| Actual finding | Actual rating | Evidence-based | Potential consequence | Justified priority |

### 12.3 Explain the priority

For example, a confirmed weakness in a service that is reachable by untrusted users and supports sensitive transactions may deserve urgent attention.

By contrast, a scanner finding that is not yet validated may first require confirmation before the client makes a major remediation decision.

The priority must be supported by the evidence and the business context. Do not assign a critical severity simply to make the report appear more serious.

**Deliverable:** A prioritized list of findings with a clear justification for each recommendation.

## 13. Phase Nine: Prepare the Client Report

**Objective:** Deliver a concise, actionable report to Meridian Retail Services Ltd.

The report should communicate the assessment results to both technical staff and management.

Create:

```text
reports/final-security-assessment.md
```

Use the following report structure.

### 13.1 Cover Information

**Security Assessment Report**

- Client: Meridian Retail Services Ltd. *(fictional)*
- Engagement reference: MRS-SEC-2026-012
- Assessment team: Iconic Hub Cybersecurity
- Target: Simulated staging web server
- Assessment date: Actual date of testing
- Classification: Confidential — Simulated Client Engagement

### 13.2 Executive Summary

Summarize:

- Why the assessment was conducted.
- Which system was assessed.
- What methods were used.
- The most important findings supported by the evidence.
- The recommended next steps.

The summary should be understandable to a manager who does not use Nmap or Greenbone.

### 13.3 Scope and Limitations

Document:

- The authorized target and address.
- Activities performed.
- Activities excluded.
- Assessment period.
- Network limitations.
- Any incomplete or unavailable scanning activities.

### 13.4 Methodology

Describe the actual workflow followed:

1. Engagement preparation.
2. Passive reconnaissance.
3. Active reconnaissance.
4. Service enumeration.
5. Vulnerability assessment.
6. Vulnerability scanning.
7. Correlation and validation.
8. Risk prioritization.

Clearly identify any stage that was not completed.

### 13.5 Technical Findings

For each finding, use this template:

**Finding ID:** F-001

**Title:** [Accurate finding title]

**Affected asset:** [Actual target]

**Affected service:** [Actual service, if established]

**Severity:** [Reported severity or assessed rating]

**Assessment status:** [Confirmed, potential, not established, not applicable, or not assessed]

**Evidence:** [Relevant Nmap output, Greenbone result, or advisory]

**Description:** [What the evidence establishes]

**Potential impact:** [Reasonable consequence if the weakness applies]

**Recommendation:** [Specific corrective action]

**Validation required:** [Additional evidence or follow-up action]

**Evidence references:** [E-001, E-002, and so on]

### 13.6 Risk Prioritization

Summarize the findings in priority order and explain the reasoning.

### 13.7 Remediation Plan

For each actionable finding, recommend an appropriate measure, such as:

- Applying a relevant security update.
- Disabling an unnecessary service.
- Restricting access to a service.
- Correcting an insecure configuration.
- Replacing unsupported software.
- Validating a potential false positive.
- Performing an authorized follow-up assessment.

Do not prescribe a specific patch or configuration change unless it is relevant to the actual finding.

### 13.8 Limitations

Document anything that may affect the completeness or reliability of the assessment, including:

- Incomplete scans.
- Unavailable feed data.
- Missing software inventory.
- Service identification limitations.
- Findings that could not be validated.
- Excluded testing methods.

### 13.9 Conclusion

Summarize the security observations, key uncertainties, and actions recommended before the simulated platform proceeds to production review.

The conclusion must not claim that the system is secure or compromised unless the evidence supports that statement.

### 13.10 References and Evidence

List relevant vendor advisories, CVE references, scan reports, and evidence identifiers.

Keep the report concise, accurate, and focused on actions the client can take.

**Deliverable:** Final technical security assessment report.

## 14. Phase Ten: Client Handover and Retesting Plan

**Objective:** Define what happens after the report has been delivered.

The assessment team provides the client with:

- The final report.
- The evidence supporting each finding.
- The list of prioritized recommendations.
- Outstanding validation requirements.
- A proposed retesting plan.

The client is responsible for approving and implementing remediation.

Once remediation has been completed, the client may authorize a follow-up assessment to verify whether the affected weakness has been addressed.

For example, if a finding concerns an exposed service, an approved retest may check whether that service is still reachable. If a finding concerns a specific software vulnerability, additional checks may be required to establish whether the corrective action resolved the issue.

Retesting is not automatically authorized by the original engagement. The scope and permitted methods must be confirmed before follow-up testing begins.

**Deliverable:** Remediation and retesting recommendations.

## 15. Evidence Register

Maintain an evidence register that links each report statement to its supporting material.

| Evidence ID | Evidence item | Purpose |
|---|---|---|
| E-001 | Engagement scope record | Establishes the authorized target and limits |
| E-002 | Network configuration record | Documents the assessment environment |
| E-003 | Nmap output | Supports service enumeration observations |
| E-004 | Greenbone report | Supports vulnerability scanning findings |
| E-005 | Advisory or CVE reference | Supports vulnerability research |
| E-006 | Finding correlation table | Documents comparison and assessment conclusions |
| E-007 | Final report | Consolidates findings and recommendations |

These are proposed identifiers. Remove any entry for which no evidence was collected, and update the descriptions to match the actual files.

Store evidence in the engagement directory:

```text
MRS-SEC-2026-012/
├── scope/
│   └── engagement-scope.txt
├── notes/
│   ├── passive-reconnaissance.md
│   ├── active-reconnaissance.md
│   └── vulnerability-assessment.md
├── scans/
│   ├── nmap-services.txt
│   ├── nmap-services.xml
│   └── greenbone-report.pdf
├── evidence/
│   └── evidence-register.md
└── reports/
    └── final-security-assessment.md
```

The exported Greenbone report may use another format. Keep the filename extension consistent with the actual file.

### Evidence Integrity

The assessment team should:

- Preserve original scan output.
- Avoid editing evidence to change its meaning.
- Record the date and source of each important item.
- Reference evidence IDs in the report.
- Protect credentials and sensitive information.
- Share the report only through the agreed channel.

For a more formal engagement, the team may also record cryptographic hashes of evidence files and maintain an access log.

## 16. Operational Decision Points

A real assessment requires decisions when results are incomplete or unexpected.

### Situation A: The target is unreachable

**Action:** Verify the target address and approved network configuration. Record the issue and pause target-specific testing until reachability can be established.

### Situation B: Nmap identifies an unfamiliar service

**Action:** Record the service and investigate its identity, purpose, and relevant security documentation. Do not assume that the service is malicious or vulnerable.

### Situation C: Greenbone reports a high-severity issue

**Action:** Review the evidence, affected service, severity basis, and applicability. If the finding is credible and potentially significant, communicate it through the engagement's agreed escalation process.

### Situation D: Nmap and Greenbone disagree

**Action:** Check whether both results refer to the same target, whether each scan completed, and what evidence supports the discrepancy. Document unresolved uncertainty.

### Situation E: The scanner produces no findings

**Action:** Verify scan completion, target selection, scanner readiness, and coverage. Report the actual result and its limitations. Do not conclude that the system has no vulnerabilities.

### Situation F: The target becomes unstable

**Action:** Stop the activity that may be contributing to the instability, notify the engagement owner, preserve relevant logs, and resume only when authorized.

### Situation G: Sensitive information is encountered unexpectedly

**Action:** Stop unnecessary collection, avoid copying or distributing the information, preserve only what the engagement policy permits, and notify the authorized contact.

## 17. Final Deliverables

At the end of the engagement, Iconic Hub Cybersecurity should be able to provide the following deliverables, to the extent supported by completed work.

| Deliverable | Description |
|---|---|
| Scope record | Authorized target, permitted methods, and exclusions |
| Reconnaissance record | Relevant preliminary and reachability observations |
| Service inventory | Nmap-discovered ports and services |
| Vulnerability assessment record | Potential weaknesses and supporting research |
| Greenbone report | Scan results or a documented scanner limitation |
| Findings correlation | Comparison of service and vulnerability evidence |
| Risk prioritization | Findings ordered by justified priority |
| Remediation recommendations | Actions the client should consider |
| Final assessment report | Consolidated findings, limitations, and next steps |
| Retesting plan | Proposed verification after remediation, subject to new authorization |

The assessment team must not present an unavailable scan, unverified finding, or incomplete phase as completed work.

## 18. Engagement Completion Criteria

The engagement is ready for handover when:

- [ ] The authorized target and scope have been documented.
- [ ] The assessment workstation and target have been verified.
- [ ] Reconnaissance activities have been recorded.
- [ ] Nmap service enumeration has been performed or its limitation documented.
- [ ] Potential vulnerabilities have been assessed using available evidence.
- [ ] Greenbone results have been reviewed or the scanning limitation documented.
- [ ] Findings have been correlated and assigned appropriate assessment statuses.
- [ ] Risk priorities have been justified.
- [ ] Remediation recommendations have been documented.
- [ ] Evidence has been organized and referenced.
- [ ] The final report accurately describes the work performed.
- [ ] Sensitive information has been handled appropriately.
- [ ] Outstanding issues and retesting requirements have been identified.

## 19. Professional Standard

The quality of a security assessment is not measured by the number of vulnerabilities reported. It is measured by the reliability of the evidence, the accuracy of the conclusions, the clarity of the risk analysis, and the usefulness of the recommendations.

A professional assessment team does not exaggerate findings, treat scanner output as unquestionable truth, or claim to have completed activities that were not performed.

Every conclusion must be traceable to evidence, and every recommendation should address an identified risk or clearly stated uncertainty.

## 20. Engagement Conclusion

The simulated Meridian Retail Services engagement demonstrates how an authorized security assessment moves from initial information gathering to service enumeration, vulnerability investigation, scanning, evidence correlation, risk prioritization, and client reporting.

The final product is not simply a list of open ports or scanner alerts. It is an evidence-based assessment that explains what was observed, what remains uncertain, which issues deserve attention, and what the client should do next.

**The engagement is complete when the assessment team has delivered an accurate, traceable report and documented the next steps required by the client—not merely when the tools have finished running.**
