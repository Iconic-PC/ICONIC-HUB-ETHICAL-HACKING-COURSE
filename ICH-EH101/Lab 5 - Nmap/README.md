# ICONIC HUB — ETHICAL HACKING COURSE
## LAB 5 ASSIGNMENT: NETWORK RECONNAISSANCE AND NMAP SCANNING

**Course Code:** ICH-EH101  
**Level:** Beginner  
**Total Marks:** 100  
**Practical Environment:** Kali Linux, DVWA and the separate OWASP virtual machine  
**Primary Tool:** Nmap  
**Brand:** Iconic Hub — *Secure. Build. Innovate.*

---

## 1. Assignment Scenario

You are a junior ethical hacker working in the Iconic Hub training laboratory. Your instructor has provided two lab environments:

1. **DVWA:** An intentionally vulnerable web application already installed and running on Apache2.
2. **OWASP virtual machine:** A separate virtual machine connected to your VirtualBox lab network.

Your task is to use Kali Linux to identify the targets, test network connectivity, scan authorized ports, identify available services and confirm whether the DVWA web application can be reached.

You must document your findings using actual Nmap output and screenshots.

**Important:** Do not reinstall DVWA. Use the existing installation. The OWASP target is the separate lab VM, not the public OWASP website. Only scan the assigned virtual machines.

## 2. Learning Objectives

By completing this assignment, you should be able to:

- Identify the IP addresses of Kali Linux and both lab targets.
- Test connectivity between virtual machines.
- Explain the difference between an IP address and a port.
- Use Nmap for host discovery and TCP port scanning.
- Identify services and attempt service-version detection.
- Verify access to DVWA through a browser.
- Save scan results to text files.
- Interpret Nmap results accurately.
- Explain why an open port does not automatically mean a vulnerability exists.

---

## PART A — NETWORK IDENTIFICATION (15 MARKS)

### Task 1: Identify the IP Addresses (8 marks)

On Kali Linux, run:

```bash
ip -br addr
```

On the OWASP virtual machine, open its terminal and run:

```bash
ip -br addr
```

Identify and record the IPv4 address of each machine's lab network interface.

Complete this table:

| Machine | Actual IP address |
|---|---|
| Kali Linux | |
| DVWA host, if separate from OWASP | |
| OWASP virtual machine | |

If DVWA is running on the OWASP virtual machine, record that fact rather than inventing a separate DVWA host address.

### Task 2: Test Connectivity (7 marks)

From Kali Linux, test connectivity to the OWASP VM:

```bash
ping -c 4 OWASP_TARGET_IP
```

Replace `OWASP_TARGET_IP` with the actual address of your OWASP VM.

If DVWA is hosted on a different machine, test that host separately:

```bash
ping -c 4 DVWA_TARGET_IP
```

Record the results and explain whether replies were received.

If ping fails, explain why this alone does not prove the machine is offline.

---

## PART B — NMAP SCANNING OF THE OWASP VM (30 MARKS)

### Task 3: Host Discovery (5 marks)

Run:

```bash
nmap -sn OWASP_TARGET_IP
```

Record whether Nmap reports the target as up.

Explain what the `-sn` option does.

### Task 4: Basic Port Scan (10 marks)

Run:

```bash
nmap OWASP_TARGET_IP
```

Record the ports, states and service names shown in your results.

Use this table:

| Port | State | Service reported | Interpretation |
|---|---|---|---|
| | | | |
| | | | |
| | | | |

Explain the difference between:

- **Open:** A service appears to be accepting connections.
- **Closed:** The target responds, but no service is listening on that port.
- **Filtered:** Nmap cannot reliably determine the port state because of filtering or other network behaviour.

### Task 5: Service and Version Detection (10 marks)

Run:

```bash
nmap -sV OWASP_TARGET_IP
```

Record the services and any version information reported by Nmap.

Explain:

1. What the `-sV` option does.
2. Why service identification is useful during a security assessment.
3. Why a reported version should be verified before making a vulnerability claim.

### Task 6: Scan Selected Ports (5 marks)

Run:

```bash
nmap -p 22,80,443,4280 OWASP_TARGET_IP
```

This checks the listed TCP ports only. Port 4280 is included because it is used by the DVWA configuration described in the Lab 5 lesson; it may not be open on the OWASP VM.

Record the actual results and explain why a port might be closed or filtered.

---

## PART C — NMAP AND DVWA (25 MARKS)

### Task 7: Verify DVWA in Firefox (5 marks)

Open Firefox on Kali Linux and visit the actual DVWA URL.

If DVWA is hosted on a separate machine using port 4280, the URL may follow this format:

```text
http://DVWA_TARGET_IP:4280
```

Use the actual address and port for your installation.

Capture a screenshot showing DVWA loading successfully. If it does not load, document the problem rather than claiming success.

### Task 8: Scan the DVWA Web Port (10 marks)

Identify the host running DVWA and its actual web port.

For a DVWA host using port 4280, run:

```bash
nmap -p 4280 DVWA_TARGET_IP
```

Then run:

```bash
nmap -sV -p 4280 DVWA_TARGET_IP
```

If your existing DVWA installation uses a different host port, substitute that port in both commands. If DVWA and OWASP BWA are the same host, clearly state that in your report and avoid presenting them as two separate machines.

Record the port state and service information.

Explain whether your scan results agree with what you observed in Firefox.

### Task 9: Save Your Nmap Results (10 marks)

Create a folder for your assignment:

```bash
mkdir -p ~/ICH-EH101/Lab5_Assignment
cd ~/ICH-EH101/Lab5_Assignment
```

Save your OWASP VM scan:

```bash
nmap -oN owasp-scan.txt OWASP_TARGET_IP
```

Save service-detection results:

```bash
nmap -sV -oN owasp-services.txt OWASP_TARGET_IP
```

Save your DVWA port scan, substituting the actual target and port:

```bash
nmap -p 4280 -oN dvwa-scan.txt DVWA_TARGET_IP
```

Check that the files exist:

```bash
ls -l
```

View one of the saved files:

```bash
cat owasp-scan.txt
```

If DVWA uses a different port, change `4280` to the correct port before running the command.

---

## PART D — ANALYSIS AND INTERPRETATION (15 MARKS)

Answer all the following questions in your own words.

1. What is Nmap, and what is it used for? (2 marks)
2. What is the difference between an IP address and a port? (2 marks)
3. What does `nmap -sn TARGET_IP` do? (2 marks)
4. What does `nmap -sV TARGET_IP` attempt to identify? (2 marks)
5. What is the difference between an open and a filtered port? (2 marks)
6. Why might DVWA load in Firefox even if a scan of the wrong port shows it as closed? (2 marks)
7. Why does an open web port not automatically prove that a vulnerability exists? (3 marks)

---

## PART E — SCREENSHOTS AND REPORT (15 MARKS)

### Required Evidence

Include genuine screenshots showing:

1. Kali's IP address.
2. The OWASP VM's IP address.
3. The connectivity test.
4. The OWASP host-discovery result.
5. The basic Nmap scan.
6. The service-version scan.
7. DVWA loaded in Firefox.
8. The DVWA port scan.
9. The saved Nmap output files.

Add a brief caption beneath each screenshot explaining what it demonstrates.

### Report Structure

Use the following structure:

1. **Title page:** Name, course code, Lab 5 title and submission date.
2. **Objective:** Explain the purpose of the practical.
3. **Lab environment:** Identify Kali, DVWA and the OWASP VM, including actual IP addresses and the network setup.
4. **Methodology:** List the commands you ran and explain their purposes.
5. **Results:** Present the scan tables and browser verification.
6. **Analysis:** Answer the questions in Part D.
7. **Recommendations:** Suggest appropriate actions for unnecessary exposed services or insecure configurations identified by your evidence.
8. **Conclusion:** Summarize what you learned.
9. **Evidence:** Include your labelled screenshots and reference the saved output files.

## 3. Marking Scheme

| Component | Marks |
|---|---:|
| Part A — Network identification | 15 |
| Part B — OWASP Nmap scanning | 30 |
| Part C — DVWA scanning and saved output | 25 |
| Part D — Analysis and interpretation | 15 |
| Part E — Evidence and report | 15 |
| **Total** | **100** |

## 4. Submission Instructions

Submit one report containing your answers, scan tables and screenshots. Include the saved Nmap text files as supporting evidence.

Use the real IP addresses and results from your own laboratory. Do not copy another student's output or invent services that your scan did not detect.

**Iconic Hub — Secure. Build. Innovate.**
