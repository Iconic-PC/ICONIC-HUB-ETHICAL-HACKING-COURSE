# ICONIC HUB — ETHICAL HACKING COURSE
## LAB 7 ASSIGNMENT: Reconnaissance, Enumeration and Web Application Security Testing

**Course Code:** ICH-EH101  
**Level:** Beginner  
**Duration:** 3 hours  
**Total Marks:** 100  
**Testing Machine:** Kali Linux  
**Web Application Target 1:** DVWA running on Apache2  
**Target 2:** Separate OWASP Broken Web Applications (BWA) virtual machine  
**Primary Tools:** Nmap, Firefox, Linux networking commands and DVWA  
**Brand:** Iconic Hub — *Secure. Build. Innovate.*

---

## 1. Assignment Scenario

You have been assigned to work as a junior penetration tester for Iconic Hub. Your task is to conduct a basic security assessment of two intentionally vulnerable lab environments.

The first environment is **DVWA**, which was installed during a previous class and is already running on Apache2. You must verify that it is accessible and perform selected web application security tests.

The second environment is the **separate OWASP Broken Web Applications virtual machine** installed in VirtualBox. You must identify its IP address, confirm connectivity from Kali Linux, discover exposed ports and enumerate available services.

Your goal is to identify the services and application features exposed by the two environments, collect evidence, explain your findings and recommend appropriate security controls.

**Important:** Kali Linux is your testing machine. DVWA and the separate OWASP BWA virtual machine are the lab targets. Do not reinstall DVWA, scan public websites or test systems outside the authorized lab.

## 2. Learning Objectives

By completing this assignment, students should be able to:

1. Identify the testing machine and the two lab environments.
2. Determine the actual IP addresses of the machines involved.
3. Verify that DVWA and the OWASP BWA virtual machine are accessible.
4. Use Nmap for host discovery, port scanning and service detection.
5. Explain the difference between reconnaissance and enumeration.
6. Identify web application security weaknesses using DVWA.
7. Understand the purpose of OWASP application security guidance.
8. Capture genuine screenshots and preserve command output.
9. Distinguish confirmed observations from assumptions.
10. Write a short security assessment report with practical recommendations.

## 3. Rules and Scope

All testing must remain inside the authorized virtual laboratory.

- Use Kali Linux as the testing machine.
- Use the existing DVWA installation. Do not reinstall it.
- Use the separate OWASP BWA VM as the second target.
- Scan only the actual IP address of your OWASP BWA VM.
- Perform web vulnerability exercises only in DVWA's designated training environment.
- Do not attempt to attack the VirtualBox host, public websites or other students' devices.
- Do not use destructive payloads, delete data or attempt to disrupt services.
- Do not fabricate screenshots, scan results or findings.

**Before beginning:** Confirm that the OWASP VM is connected to the intended isolated lab network. Record the actual IP addresses rather than copying sample addresses from another student's report.

---

# PART A — LAB ENVIRONMENT AND CONNECTIVITY (15 MARKS)

### Task 1: Identify Your Lab Machines (5 marks)

On Kali Linux, open a terminal and run:

```bash
ip -br addr
```

Record the IPv4 address of the interface connected to your lab network.

Start the separate OWASP BWA VM and open its terminal or console. Run:

```bash
ip -br addr
```

If this command is unavailable, try:

```bash
ip addr
```

Record the OWASP VM's actual IPv4 address.

Complete the table in your report:

| Item | Your result |
|---|---|
| Testing machine | Kali Linux |
| Kali lab IP address | |
| DVWA access URL | |
| OWASP BWA VM IP address | |
| VirtualBox network type | |

Remember: DVWA is already running on Apache2 in the existing lab setup. Record the URL that actually works in your environment; do not assume every installation uses the same port or path.

### Task 2: Verify DVWA (5 marks)

1. Open Firefox on Kali Linux.
2. Visit your existing DVWA URL.
3. Confirm that the DVWA page loads.
4. Sign in using the lab credentials provided by your instructor, if required.
5. Locate the DVWA security-level setting and record the current level. Do not change it unless instructed.

**Evidence:** Capture a screenshot showing DVWA loaded in the browser. Avoid including passwords or other private credentials.

### Task 3: Verify the OWASP VM (5 marks)

From the Kali terminal, run:

```bash
ping -c 4 OWASP_TARGET_IP
```

Replace `OWASP_TARGET_IP` with the actual address of your OWASP BWA VM.

Record whether replies were received.

If ping fails, do not immediately conclude that the machine is offline. ICMP may be blocked. Check the VirtualBox network configuration and continue with the authorized checks your instructor has approved.

**Evidence:** Capture the command and its actual output.

---

# PART B — RECONNAISSANCE AND ENUMERATION OF OWASP BWA (25 MARKS)

### Task 4: Host Discovery (5 marks)

Run:

```bash
nmap -sn OWASP_TARGET_IP
```

This performs host discovery without a normal port scan.

Record:

- The target IP address.
- Whether Nmap reports the host as up.
- Any relevant notes about the result.

Explain in your own words what the `-sn` option does.

### Task 5: Basic Port Scan (7 marks)

Run:

```bash
nmap OWASP_TARGET_IP
```

Record the ports Nmap reports and their states.

Create a results table:

| Port | State | Service reported by Nmap | Your interpretation |
|---|---|---|---|
| | | | |
| | | | |
| | | | |

Add more rows if necessary. Use your own results rather than copying the example from the lesson.

Explain the difference between an **open**, **closed** and **filtered** port.

### Task 6: Service and Version Detection (7 marks)

Run:

```bash
nmap -sV OWASP_TARGET_IP
```

The `-sV` option attempts to identify services and their versions on discovered ports.

Record at least three relevant observations from your actual output, where available. For each observation, explain what it might mean for a security tester.

If fewer than three services are detected, report the actual result and explain why you should not invent additional findings.

### Task 7: Investigate the Web Service (6 marks)

If Nmap identifies an HTTP or HTTPS service:

1. Open Firefox on Kali Linux.
2. Enter the appropriate address for the actual service.
3. Include the port number in the URL when it is not the default for that protocol.
4. Record the page title or application name if identifiable.
5. Note any login page, application information or visible security warning.

Example URL format:

```text
http://OWASP_TARGET_IP:PORT
```

This is a format example only. Substitute the real target address and discovered port.

Do not assume that every open web port is vulnerable. Record only what you can verify.

**Evidence:** Capture the browser page and the relevant Nmap result.

---

# PART C — DVWA WEB APPLICATION SECURITY TESTING (30 MARKS)

This section uses the existing DVWA application. Perform the exercises within DVWA only.

### Task 8: Review the DVWA Environment (5 marks)

Open DVWA and record:

- The page or module you are working with.
- The configured DVWA security level.
- The purpose of the module.
- One security risk the module is designed to demonstrate.

Do not change the security level unless your instructor authorizes it. Different security levels can produce different results.

### Task 9: SQL Injection Demonstration (10 marks)

**Objective:** Understand how unsafe handling of user input can affect a database query.

1. In DVWA, open the **SQL Injection** module.
2. Read the input instructions shown by the application.
3. Use the ordinary demonstration input first, if one is provided.
4. Observe the result displayed by the application.
5. In the authorized DVWA field, try this simple test input:

```text
1' OR '1'='1
```

6. Submit the input and observe the response.
7. Compare the response with an ordinary input.
8. Record what changed and whether the result supports the conclusion that input handling may be vulnerable.

If the payload does not work, record that result. The configured security level, application version or input handling may affect the outcome. Do not claim a successful injection unless your evidence supports it.

Answer these questions:

- What is SQL injection?
- Why can concatenating untrusted input into a database query be dangerous?
- What did DVWA display before and after your test?
- How could parameterized queries help prevent SQL injection?

**Evidence:** Capture screenshots of the ordinary input and the test result. Do not include unrelated sensitive information.

### Task 10: Cross-Site Scripting (XSS) Demonstration (10 marks)

**Objective:** Understand how an application can become vulnerable when it returns untrusted input to a browser without appropriate output handling.

1. In DVWA, open the **XSS (Reflected)** module.
2. Identify the input field.
3. Submit a harmless ordinary value, such as:

```text
Hello Iconic Hub
```

4. Observe how the application displays it.
5. In the same DVWA module, try the following harmless demonstration payload:

```html
<script>alert('ICH-EH101')</script>
```

6. Observe whether the browser displays an alert or whether the input is rejected or shown as text.
7. Record the result accurately.

If the payload does not execute, do not assume the application is secure. The module's security level or output handling may affect the result.

Answer these questions:

- What is cross-site scripting?
- What is the difference between reflected XSS and stored XSS?
- Did your test input execute, get rejected or appear as text?
- How can context-appropriate output encoding reduce XSS risk?

**Evidence:** Capture the input and the actual result. If an alert appears, capture it without including unrelated browser tabs or private information.

### Task 11: Relate Your Findings to OWASP Guidance (5 marks)

Use the OWASP guidance supplied or approved by your instructor to explain how SQL injection and XSS relate to secure application development.

For each issue, record:

| Issue | Potential impact | Recommended defence |
|---|---|---|
| SQL injection | | |
| Cross-site scripting | | |

Your answer must connect the risk to the evidence you collected. Do not state that the separate OWASP BWA VM has the same vulnerability simply because DVWA demonstrates it.

---

# PART D — SAVE EVIDENCE AND WRITE FINDINGS (15 MARKS)

### Task 12: Save Your Nmap Results (5 marks)

On Kali, create an assignment folder:

```bash
mkdir -p ~/ICH-EH101/Lab7_Assignment
cd ~/ICH-EH101/Lab7_Assignment
```

Save a basic scan of the OWASP target:

```bash
nmap -oN owasp-basic-scan.txt OWASP_TARGET_IP
```

Save the service-detection results:

```bash
nmap -sV -oN owasp-service-scan.txt OWASP_TARGET_IP
```

Replace the placeholder with the actual target IP in each command.

Confirm that the files exist:

```bash
ls -l
```

View the saved results:

```bash
cat owasp-basic-scan.txt
cat owasp-service-scan.txt
```

Do not edit the original scan output to make results appear more successful.

### Task 13: Produce a Findings Summary (5 marks)

Write at least **three findings** based on your lab work. These may include verified network observations, an exposed service, a web application behaviour or a demonstrated weakness in DVWA.

For each finding, provide:

1. Finding title.
2. Target or module affected.
3. Evidence.
4. Potential impact.
5. Recommended mitigation.

Use this table:

| Finding | Evidence | Potential impact | Recommendation |
|---|---|---|---|
| Finding 1 | | | |
| Finding 2 | | | |
| Finding 3 | | | |

A finding may be an observation rather than a confirmed vulnerability. Label it correctly.

### Task 14: Explain the Limits of Your Testing (5 marks)

Answer the following:

1. Does an open port prove that a vulnerability exists?
2. Does a successful DVWA SQL injection demonstration prove that the OWASP BWA VM has SQL injection?
3. Does a failed ping prove that the target is offline?
4. Why should a tester preserve original scan results and screenshots?
5. What additional testing would be needed before declaring a real application secure?

---

# PART E — REPORT AND PRESENTATION (15 MARKS)

Prepare a report with the following sections.

## 1. Cover Page

Include:

- Iconic Hub — Ethical Hacking Course
- Lab 7: Reconnaissance, Enumeration and Web Application Security Testing
- Student's name
- Student ID, if applicable
- Submission date

## 2. Objective

Briefly explain the purpose of assessing DVWA and the separate OWASP BWA virtual machine.

## 3. Lab Environment

Identify Kali Linux, the existing DVWA installation, the OWASP BWA VM, the lab network and the tools used.

Include the actual target IP address and DVWA URL.

## 4. Methodology

List the commands and browser-based tests you performed. Explain what each was intended to determine.

## 5. Findings

Present the Nmap results and DVWA test results. Distinguish between confirmed observations, possible risks and unverified assumptions.

## 6. Evidence

Insert genuine screenshots showing:

- Kali's lab IP address.
- The OWASP BWA VM's IP address.
- DVWA loading in Firefox.
- The Nmap host-discovery result.
- The Nmap port-scan result.
- The service-detection result.
- The SQL injection demonstration.
- The XSS demonstration.
- The saved scan files.

Ensure each screenshot has a short caption explaining what it proves.

## 7. Recommendations

Suggest practical controls, including:

- Restricting unnecessary network services.
- Applying security updates.
- Using parameterized database queries.
- Applying appropriate output encoding.
- Validating input as part of a broader secure-development approach.
- Restricting vulnerable training applications to isolated lab networks.

## 8. Conclusion

Summarize what you learned and explain the difference between network reconnaissance, service enumeration and web application testing.

## 9. Submission Checklist

- [ ] Both lab environments are identified correctly.
- [ ] Actual IP addresses and the DVWA URL are recorded.
- [ ] Nmap commands and results are included.
- [ ] DVWA SQL injection and XSS tests are documented.
- [ ] Screenshots show real results.
- [ ] At least three findings are discussed.
- [ ] Recommendations relate to the findings.
- [ ] Limitations are acknowledged.
- [ ] The report is clearly organized.

---

# MARKING GUIDE — 100 MARKS

| Assessment area | Marks |
|---|---:|
| Part A — Environment and connectivity | 15 |
| Part B — Reconnaissance and enumeration | 25 |
| Part C — DVWA security testing | 30 |
| Part D — Evidence and findings | 15 |
| Part E — Report quality and presentation | 15 |
| **Total** | **100** |

## Submission Requirements

Submit one report containing the required answers, tables and screenshots, together with the saved Nmap output files.

Use your own words. Copied commands are acceptable when properly executed and explained; copied results are not acceptable as evidence of your own testing.

**Final reminder:** A professional ethical hacker does not invent findings. If a scan or test produces an unexpected result, document what happened, explain possible causes and state what remains unverified.

**Iconic Hub — Secure. Build. Innovate.**
