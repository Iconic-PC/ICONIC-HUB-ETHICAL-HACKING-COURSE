# ICH-EH101 — LAB 6 ASSIGNMENT
## Reconnaissance and Enumeration of the OWASP Virtual Machine

**Institution:** Iconic Hub  
**Course:** ICH-EH101 — Ethical Hacking  
**Assignment:** Lab 6  
**Topic:** Reconnaissance and Enumeration  
**Target:** OWASP Virtual Machine running in VirtualBox  
**Testing Machine:** Kali Linux  
**Total Marks:** 100  
**Submission:** Practical report with screenshots and analysis

---

## 1. Assignment Objective

The purpose of this assignment is to assess your ability to gather information about an authorized target, discover its exposed network services, interpret technical results, and document your findings professionally.

You are required to use Kali Linux to investigate the OWASP virtual machine provided for this class.

You must perform the practical exercises yourself and submit genuine screenshots showing your commands and results.

## 2. Scenario

You have been assigned the role of a junior penetration tester at Iconic Hub. Your supervisor has provided an OWASP virtual machine for an internal security assessment.

Before any vulnerability testing begins, your supervisor requires you to perform reconnaissance and enumeration to understand the target's network presence and exposed services.

Your task is to conduct the initial assessment, record your findings, and prepare a report explaining what you discovered.

**Authorized scope:** Only the OWASP VM provided for this lab. Do not scan unrelated machines, public websites, or systems outside the authorized lab environment.

---

## 3. Practical Tasks

### Task 1: Identify the target and document the lab environment — 10 marks

1. Start Kali Linux and the OWASP VM in VirtualBox.
2. Open a terminal inside the OWASP VM.
3. Run:

   ```bash
   ip addr
   ```

4. Identify and record the OWASP VM's actual IPv4 address.
5. Run the same command inside Kali Linux.
6. Record Kali's IP address separately.
7. Briefly explain the purpose of each machine in the lab.

**Required evidence:** Screenshots showing the IP configuration of both VMs.

**Questions to answer:**
- What is the OWASP VM's IPv4 address?
- What is Kali's IPv4 address?
- Why is it important not to confuse the target IP with the testing machine's IP?

### Task 2: Test connectivity — 10 marks

From Kali Linux, run the following command, replacing `TARGET_IP` with the actual OWASP VM address:

```bash
ping -c 4 TARGET_IP
```

Record the output and determine whether the target responds to ICMP requests.

If the command fails, investigate the VM's network configuration and document what you found. Do not fabricate a successful result.

**Required evidence:** A screenshot showing the command and its output.

**Questions to answer:**
- Did the target respond?
- What does a successful ping indicate?
- Does a failed ping necessarily mean the target is offline? Explain.

### Task 3: Perform host discovery — 10 marks

From Kali, run:

```bash
nmap -sn TARGET_IP
```

Replace `TARGET_IP` with the correct target address.

Record the result and explain what Nmap reports about the target's availability.

**Required evidence:** A screenshot of the Nmap command and output.

**Questions to answer:**
- What is the purpose of the `-sn` option?
- Did Nmap identify the host as up?
- How is host discovery different from port scanning?

### Task 4: Scan selected TCP ports — 20 marks

Run the following command against the authorized OWASP VM:

```bash
nmap -p 21,22,23,25,53,80,110,139,143,443,445,3306,8080 TARGET_IP
```

Record the results in a table.

| Port | State | Reported service | Your interpretation |
|---|---|---|---|
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |

Include only the ports and results actually reported by your scan.

**Required evidence:** A screenshot showing the port scan and results.

**Questions to answer:**
- Which ports were reported as open?
- Which were closed or filtered, if any?
- What is the difference between an open, closed, and filtered port?
- Does an open port automatically indicate a vulnerability? Explain.

### Task 5: Perform service and version detection — 15 marks

Run:

```bash
nmap -sV -p 21,22,23,25,53,80,110,139,143,443,445,3306,8080 TARGET_IP
```

Document any service names or version details Nmap reports.

| Port | Service identified | Version information | Notes |
|---|---|---|---|
| | | | |
| | | | |
| | | | |

If Nmap does not identify a version, write **"Not identified"** rather than guessing.

**Required evidence:** A screenshot of the service-detection results.

**Questions to answer:**
- What does the `-sV` option do?
- Did service detection provide more information than the basic port scan?
- Why should a reported version be verified before concluding that a vulnerability exists?

### Task 6: Inspect an exposed web service — 10 marks

If your scan identifies an open web port, open Firefox in Kali and visit the target using the appropriate address.

For example, if port 80 is open:

```text
http://TARGET_IP/
```

If the web service is running on port 8080, use:

```text
http://TARGET_IP:8080/
```

Replace the example IP with the actual OWASP VM address.

Record the page or response you observe.

**Required evidence:** A screenshot of the browser showing the target address and the page returned.

**Questions to answer:**
- Was a web page accessible?
- Which port did you use?
- What did the page reveal about the target?
- What can you not conclude from the page alone?

If no web service is available on the scanned ports, document that result and ask your instructor for guidance.

### Task 7: Save scan results and interpret your findings — 10 marks

Create a folder for your assignment:

```bash
mkdir -p ~/ICH-EH101/Lab6_Assignment
```

Save the selected-port scan:

```bash
nmap -p 21,22,23,25,53,80,110,139,143,443,445,3306,8080 -oN ~/ICH-EH101/Lab6_Assignment/port-scan.txt TARGET_IP
```

Save the service-detection scan:

```bash
nmap -sV -p 21,22,23,25,53,80,110,139,143,445,443,3306,8080 -oN ~/ICH-EH101/Lab6_Assignment/service-scan.txt TARGET_IP
```

Check that your files exist:

```bash
ls -l ~/ICH-EH101/Lab6_Assignment
```

Review the results and write a short analysis that distinguishes:

- What you observed.
- What the observations suggest.
- What remains unconfirmed.
- What you would investigate next within the authorized scope.

**Required evidence:** A screenshot showing the saved output files and at least one saved scan result.

### Task 8: Explain reconnaissance and enumeration — 5 marks

Answer in your own words:

1. What is reconnaissance?
2. What is enumeration?
3. Give two differences between them.
4. Explain how the practical exercises demonstrated both activities.

---

## 4. Final Report Requirements

Submit one organized report using the following headings:

1. Cover page.
2. Introduction.
3. Objectives.
4. Lab environment and authorized scope.
5. Target IP identification.
6. Connectivity testing.
7. Host discovery.
8. Port-scanning results.
9. Service-detection results.
10. Web-service observations.
11. Findings and analysis.
12. Limitations.
13. Conclusion.
14. Screenshot appendix.

Your report must explain the results rather than simply pasting terminal output.

## 5. Screenshot Requirements

Submit at least six genuine screenshots:

- Kali IP configuration.
- OWASP VM IP configuration.
- Connectivity test.
- Nmap host discovery.
- Nmap port scan.
- Nmap service detection.
- Web application, if available.
- Saved scan files.

You may include more than six screenshots. Ensure the commands, target address, and relevant output are readable.

## 6. Marking Scheme

| Assessment area | Marks |
|---|---:|
| Target identification and lab documentation | 10 |
| Connectivity testing | 10 |
| Host discovery | 10 |
| TCP port scanning | 20 |
| Service and version detection | 15 |
| Web-service inspection | 10 |
| Saved results and interpretation | 10 |
| Explanation of reconnaissance and enumeration | 5 |
| Report quality, evidence, and responsible testing | 10 |
| **Total** | **100** |

## 7. Submission Rules

- Perform all scans from Kali Linux against the assigned OWASP VM.
- Replace every `TARGET_IP` placeholder with the actual target IP.
- Submit your own results and screenshots.
- Do not copy another student's report.
- Do not invent open ports, service versions, or scan results.
- If a command fails, document the error and explain your troubleshooting.
- Do not claim to have discovered a vulnerability solely because a port is open.
- Do not scan public systems or unrelated machines.

## 8. Final Reflection

Conclude your report by answering:

**"Based on your reconnaissance and enumeration results, what have you learned about the OWASP VM, and what additional information would you need before conducting a vulnerability assessment?"**

Your answer should refer to your actual findings and demonstrate that you understand the difference between gathering information and proving a security weakness.

**End of Assignment**
