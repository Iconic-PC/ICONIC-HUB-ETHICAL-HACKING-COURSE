# ICH-EH101 — LAB 6: RECONNAISSANCE AND ENUMERATION

**Course:** Iconic Hub Ethical Hacking Course (ICH-EH101)  
**Lab Date:** Friday, 23 October 2026  
**Duration:** 3 Hours  
**Topic:** Reconnaissance and Enumeration  
**Attacking Machine:** Kali Linux  
**Target Machine:** OWASP VM 
**Tagline:** Secure. Build. Innovate.

---

# PART 1: INTRODUCTION

## 1.1 What Is Reconnaissance?

Reconnaissance is the process of gathering information about a target before performing a more detailed security assessment.

In ethical hacking, the target could be a computer, website, server, network, or web application.

Before testing a target, an ethical hacker needs to understand what they are working with. They may need to identify the target's IP address, determine whether it is reachable, and discover which network services are exposed.

### Simple example

Imagine that you want to inspect a building for security weaknesses.

Before inspecting its locks, you first identify the building, observe its entrances, and gather information about its layout.

In cybersecurity, reconnaissance serves a similar purpose. The ethical hacker gathers information to understand the target and plan the next stage of the assessment.

### Examples of reconnaissance activities

- Identifying the target's IP address.
- Checking whether the target is reachable.
- Discovering open network ports.
- Identifying services running on the target.
- Researching publicly available information within the assessment scope.
- Recording the technologies and services that may need further investigation.

### Why is reconnaissance important?

Reconnaissance helps an ethical hacker:

1. Understand the target before testing.
2. Identify systems and services within the authorized scope.
3. Avoid wasting time on irrelevant systems.
4. Plan further testing.
5. Maintain accurate documentation.
6. Reduce the risk of testing the wrong machine.

**Important:** Reconnaissance provides information. It does not automatically prove that a target has a vulnerability.

## 1.2 What Is Enumeration?

Enumeration is the process of gathering more specific information about a target's systems, ports, services, or resources.

After discovering that a target is reachable, a tester may investigate which ports are open, what services are listening on them, and which applications are available.

### Simple example

Suppose reconnaissance identifies a server with a web service accessible on TCP port 80.

Enumeration may help the tester determine:

- Whether the web service is responding.
- What service or product appears to be running.
- Whether the service provides a version identifier.
- Which additional authorized checks may be appropriate.

Enumeration is more focused than general information gathering because it seeks specific technical details.

### Examples of enumeration

- Identifying open TCP ports.
- Detecting services listening on those ports.
- Identifying service versions when they are disclosed.
- Checking available web pages on an authorized training server.
- Examining service responses for useful technical information.
- Recording the services that require further security review.

Enumeration does not necessarily involve exploiting a vulnerability. Its purpose is to understand what the target exposes.

## 1.3 Reconnaissance vs Enumeration

| Reconnaissance | Enumeration |
|---|---|
| Collects initial information about a target | Collects more specific technical information |
| Helps establish an understanding of the target | Helps identify details about ports, services, and resources |
| May include identifying the target's IP address | May include identifying which services use particular ports |
| Helps plan the assessment | Helps determine what to investigate next |
| Example: confirming the OWASP VM's IP address | Example: identifying the services exposed by the OWASP VM |

The two activities can overlap in practice. For this class, we will treat reconnaissance as the initial information-gathering stage and enumeration as the more detailed investigation of the discovered services.

## 1.4 Types of Reconnaissance

### A. Passive reconnaissance

Passive reconnaissance involves gathering information without directly probing the target's infrastructure.

Examples include:

- Reading public documentation.
- Reviewing publicly available company information.
- Studying published security advisories.
- Examining publicly accessible project documentation.

### B. Active reconnaissance

Active reconnaissance involves directly interacting with the target to obtain information.

Examples include:

- Checking connectivity to an authorized machine.
- Scanning ports on a lab VM.
- Identifying services running on a training server.
- Sending requests to an authorized web application.

In this lab, students will use active reconnaissance against the OWASP VM inside their authorized VirtualBox lab.

## 1.5 What Is an IP Address?

An Internet Protocol (IP) address is a numerical address used to identify a network interface and support communication across IP networks.

For example:

`192.168.56.105`

This is an example of a private IPv4 address, not necessarily the address of your OWASP VM.

In our lab, Kali Linux and the OWASP VM are separate machines. Each normally has its own IP address on the network used for the exercise.

We must identify the actual addresses before performing the scan.

## 1.6 What Is a Port?

A port is a numbered endpoint used by network services to communicate over protocols such as TCP and UDP.

Think of an IP address as the address of a building and a port as a particular entrance associated with a service.

Common TCP ports include:

| Port | Common use |
|---|---|
| 21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP3 |
| 139 | NetBIOS session service |
| 143 | IMAP |
| 443 | HTTPS |
| 445 | SMB |
| 3306 | MySQL |
| 8080 | Alternative HTTP or web application port |

These are common associations, not guarantees. A service can use a different port, and an open port alone does not establish which service is running.

## 1.7 What Is Nmap?

Nmap is a network exploration and security auditing tool. It can help a tester discover hosts, inspect port states, and identify services in an authorized environment.

For this lab, Nmap will be run from Kali Linux against the OWASP VM.

Useful options include:

- `-sn`: Host discovery without a port scan.
- `-p`: Select particular ports.
- `-sV`: Attempt to identify services and versions.
- `-oN`: Save scan output in normal text format.

The exact results depend on the target's configuration and the network environment.

## 1.8 What Does an Open, Closed, or Filtered Port Mean?

**Open:** A service is accepting connections on the port.

**Closed:** The target responds, but no service is listening on that port.

**Filtered:** Network filtering or another condition prevents Nmap from confidently determining whether the port is open or closed.

An open port is not automatically a vulnerability. A legitimate web server must listen on a port to serve web pages. Security depends on the service, its configuration, exposure, access controls, and other factors.

---

# PART 2: LAB ENVIRONMENT AND SAFETY

## 2.1 Our Virtual Lab Setup

The lab contains two separate virtual machines.

**Machine 1: Kali Linux**

Kali is the attacking and testing machine. Students will use its terminal, Nmap, and browser to interact with the target.

**Machine 2: OWASP VM**

The OWASP VM is the target machine. It was imported or configured in VirtualBox using the existing VMDK file.

The intended flow is:

Kali Linux → Authorized virtual network → OWASP VM

The OWASP VM's IP address must be discovered before testing.

## 2.2 Authorization and Scope

The OWASP VM is our authorized target for this lab. Do not assume that any other machine on the network is included in the assessment.

Students must:

- Scan only the assigned OWASP VM.
- Confirm the target IP address before running Nmap.
- Avoid scanning public websites or unrelated computers.
- Avoid attempting destructive actions.
- Record their actual findings.
- Stop and ask the instructor if they are unsure which IP address belongs to the target.

## 2.3 Pre-Lab Checklist

Before starting:

- [ ] VirtualBox is installed and working.
- [ ] Kali Linux starts successfully.
- [ ] The OWASP VM starts successfully.
- [ ] Both VMs have suitable network adapters configured.
- [ ] The target IP can be identified.
- [ ] Nmap is installed on Kali.
- [ ] Students have a place to save screenshots and notes.

Do not reinstall the OWASP VM simply because the IP address is unknown. First inspect its current configuration.

---

# PART 3: PRACTICAL SESSION

# Practical 1 — Start Both Virtual Machines

**Objective:** Prepare Kali Linux and the OWASP target VM for testing.

### Step 1: Open VirtualBox

Start Oracle VirtualBox on the host computer.

Locate the Kali Linux VM and the OWASP VM.

### Step 2: Start Kali Linux

Start the Kali VM and log in.

Open a terminal window.

### Step 3: Start the OWASP VM

Start the OWASP VM using the existing virtual disk.

Log in to the target VM if necessary.

Wait for it to finish booting.

### Step 4: Check the network settings

If both machines are already communicating, leave their settings unchanged.

If they cannot communicate:

1. Shut down both VMs.
2. Open the settings of each VM in VirtualBox.
3. Select **Settings → Network**.
4. Inspect the active network adapter.
5. For an isolated lab, configure both machines to use the same suitable Host-only Adapter or the same Internal Network.
6. Make sure the selected network has a suitable IP configuration.
7. Start both VMs and check their addresses again.

A Host-only Adapter and an Internal Network are not identical. Host-only networking can also allow communication with the host, whereas an Internal Network is designed for communication among VMs attached to that same internal network.

Do not switch networking modes unnecessarily if the current setup already works.

### Expected result

Both VMs should start, and students should be able to inspect the network configuration of each machine.

**Evidence:** Take a screenshot showing both VMs running in VirtualBox.

Suggested filename: `lab6_01_virtualbox_lab_setup.png`

---

# Practical 2 — Find the OWASP VM's IP Address

**Objective:** Identify the target machine's actual IPv4 address.

This is the first step before scanning. We must not guess the target address.

### Step 1: Open the OWASP VM terminal

Click inside the OWASP VM and open its terminal.

If the target is a graphical Linux VM, use its terminal application. If it is a command-line VM, log in to the console.

### Step 2: Run the command

```bash
ip addr
```

### What does this command do?

The `ip addr` command displays network interfaces and their assigned addresses.

Look for:

- The name of the network interface.
- Whether the interface is up.
- An IPv4 address.
- The associated subnet prefix.

Example output might include:

```text
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
    inet 192.168.56.105/24
```

This is illustrative output only. Your interface may be named `eth0`, `enp0s3`, or something else, and your address may differ.

In this example, `192.168.56.105` would be the interface's IPv4 address. The `/24` indicates the network prefix length.

### Step 3: Identify the correct address

Look for an address assigned to the active network interface connected to your lab network.

Do not select:

- `127.0.0.1`, which is the loopback address.
- An unrelated interface address.
- An address belonging to Kali rather than the OWASP VM.

If the target has more than one network interface, ask the instructor which interface belongs to the lab network.

### Step 4: Record the result

Complete the table:

| Field | Actual result |
|---|---|
| Target name | OWASP VM |
| Target interface | |
| Target IPv4 address | |
| Network prefix | |
| Interface status | |

### Step 5: Inspect Kali's address

Switch to the Kali VM and run:

```bash
ip addr
```

Record Kali's lab-network address separately.

| Machine | IPv4 address |
|---|---|
| Kali Linux | |
| OWASP VM | |

The addresses should be compatible with the configured network. If they are not, connectivity may fail.

### Step 6: Capture evidence

Take a screenshot of the OWASP VM's `ip addr` output and another of Kali's output.

Suggested filenames:

- `lab6_02_owasp_ip_address.png`
- `lab6_03_kali_ip_address.png`

### Expected learning outcome

Students should know the difference between the attacking machine's IP address and the target machine's IP address.

---

# Practical 3 — Check Connectivity Between Kali and OWASP

**Objective:** Determine whether Kali can reach the OWASP VM.

### Step 1: Open Kali's terminal

Use the target IP address you recorded in Practical 2.

For example, if the actual target address is `192.168.56.105`, run:

```bash
ping -c 4 192.168.56.105
```

Replace the example with your actual target IP.

### Explanation

- `ping` sends ICMP Echo Requests.
- `-c 4` requests four attempts.
- The IP address identifies the target you want to check.

### Step 2: Interpret the results

If the target responds, you may see replies containing the target IP and a response time.

This suggests that the target is reachable over ICMP.

If you see messages such as `Destination Host Unreachable` or repeated timeouts, communication may not be working.

Possible causes include:

- The target VM is powered off.
- The VMs are connected to different virtual networks.
- The target's network interface is down.
- The target does not have the expected IP address.
- A firewall or network setting blocks ICMP.

**Important:** A failed ping does not prove that the target is offline. Some systems block ICMP while still providing other services.

### Step 3: Troubleshoot carefully

If ping fails:

1. Confirm both VMs are running.
2. Run `ip addr` again on each VM.
3. Confirm the correct target IP.
4. Check the VirtualBox network adapter settings.
5. Confirm both VMs use the intended lab network.
6. Retry the ping.

Do not begin a broad scan while you are uncertain which machine is the target.

### Step 4: Record the result

| Question | Result |
|---|---|
| Did the target respond to ping? | |
| Were any packets lost? | |
| What was the reported response time, if available? | |
| What conclusion can you reasonably draw? | |

### Step 5: Save evidence

Suggested filename: `lab6_04_owasp_connectivity.png`

### Expected learning outcome

Students should understand basic connectivity testing and why the absence of a ping response does not necessarily mean a machine is offline.

---

# Practical 4 — Perform Initial Host Discovery

**Objective:** Use Nmap to check whether the assigned OWASP VM appears reachable.

### Step 1: Confirm the target

Before running the command, confirm that you have copied the OWASP VM's IP address correctly.

For this exercise, we will use a shell variable to avoid repeatedly typing the address.

Replace the example address below with your actual OWASP VM IP:

```bash
TARGET=192.168.56.105
```

Check the value:

```bash
echo "$TARGET"
```

The displayed address should match the OWASP VM's recorded IP.

### Step 2: Run Nmap host discovery

```bash
nmap -sn "$TARGET"
```

### Explanation

- `nmap` launches the network scanning tool.
- `-sn` performs host discovery without a port scan.
- `"$TARGET"` supplies the IP address stored in the variable.

Nmap may report that the host is up. Depending on the network and scanning method, it may also report that the host appears down.

### Step 3: Interpret the output

If Nmap reports the target is up, it has received a response sufficient for its host-discovery determination.

This is useful information, but it does not establish that every service on the target is accessible.

If the host appears down:

1. Verify the address.
2. Check the VMs' network configuration.
3. Confirm connectivity.
4. Ask the instructor for help before proceeding.

### Step 4: Record the result

| Field | Observation |
|---|---|
| Target IP | |
| Host-discovery result | |
| Command used | `nmap -sn "$TARGET"` |
| Interpretation | |

### Step 5: Capture evidence

Suggested filename: `lab6_05_nmap_host_discovery.png`

### Expected learning outcome

Students should be able to distinguish identifying a reachable host from identifying the services running on that host.

---

# Practical 5 — Scan Common TCP Ports

**Objective:** Identify the state of selected TCP ports on the OWASP VM.

### Step 1: Scan common ports

Run:

```bash
nmap -p 21,22,23,25,53,80,110,139,143,443,445,3306,8080 "$TARGET"
```

This checks a selected set of common TCP ports.

The `-p` option tells Nmap which ports to check.

This command is limited to the specified ports. It does not scan every possible TCP port.

### Step 2: Read the output

Nmap may show columns similar to:

```text
PORT     STATE    SERVICE
22/tcp   open     ssh
80/tcp   open     http
443/tcp  closed   https
```

This is an example for teaching purposes, not a prediction of your VM's actual output.

Interpret the states as follows:

- `open`: A service is accepting connections on that port.
- `closed`: The target responded, but no service is listening on that port.
- `filtered`: Nmap could not determine the port's state confidently because filtering or another network condition interfered.

### Step 3: Explain the service column

The `SERVICE` column contains common service associations for the port numbers. At this stage, these names may be based on the standard port assignment rather than a confirmed identification of the actual running software.

For example, port 80 is commonly associated with HTTP, but a service on a nonstandard port could also provide HTTP.

That is why we will perform service detection next.

### Step 4: Record the open ports

Complete this table using your real scan output.

| Port | State | Reported service | Notes |
|---|---|---|---|
| | | | |
| | | | |
| | | | |
| | | | |

Include only the results you actually observed.

### Step 5: Save evidence

Suggested filename: `lab6_06_owasp_common_ports.png`

### Expected learning outcome

Students should understand that a port scan identifies port states, but the service name displayed beside a port is not always proof of the actual software running there.

---

# Practical 6 — Identify Services and Versions

**Objective:** Use Nmap service detection to obtain more information about the services exposed by the OWASP VM.

### Step 1: Run service detection on selected ports

Use:

```bash
nmap -sV -p 21,22,23,25,53,80,110,139,143,443,445,3306,8080 "$TARGET"
```

### Explanation

- `-sV` asks Nmap to probe open ports to identify the service and, where possible, its version.
- `-p` limits the scan to the selected ports.
- `"$TARGET"` identifies the assigned OWASP VM.

Service detection sends additional probes. In this lab, it must remain limited to the authorized target.

### Step 2: Interpret the output

Depending on the target, Nmap may report service names and possibly product or version details.

For example, it might identify a service as SSH or HTTP. If it identifies a product version, record the version exactly as displayed.

Do not invent a version when the tool does not identify one.

### Step 3: Understand the limitations

Service detection can be incomplete or inaccurate. Some services hide version information, and some applications use nonstandard ports.

A reported version is a lead for further investigation, not proof of a vulnerability.

Before concluding that a version is vulnerable, a tester must verify the product, version, configuration, applicable advisory, and other relevant conditions.

### Step 4: Complete the service inventory

| Port | State | Identified service | Version reported, if any |
|---|---|---|---|
| | | | |
| | | | |
| | | | |

### Step 5: Capture evidence

Suggested filename: `lab6_07_owasp_service_detection.png`

### Expected learning outcome

Students should understand how service detection provides more detailed information than a basic port scan and why the results require careful interpretation.

---

# Practical 7 — Inspect the Target's Web Application

**Objective:** Examine the web service on the OWASP VM through a browser.

Perform this practical only if the scan identifies an open web port or the instructor confirms the correct application URL.

### Step 1: Identify the web port

Review the Nmap results.

If port 80 is open, the application may be available at:

```text
http://TARGET_IP/
```

Replace `TARGET_IP` with the actual address of the OWASP VM.

If the service is on port 8080, the address may instead be:

```text
http://TARGET_IP:8080/
```

Use the port shown in your actual results.

### Step 2: Open Kali Firefox

1. Start Firefox on Kali.
2. Enter the correct target URL.
3. Wait for the response.
4. Record what page or application appears.

The page might be a landing page, a training application, a web server default page, or another response. Do not assume a particular page will appear before testing.

### Step 3: Record observations

Answer these questions:

1. Did the page load?
2. What application or page did you observe?
3. Which port did you use?
4. Was the connection HTTP or HTTPS?
5. Did the page identify the application or its technology?
6. What additional information would you need before making a security conclusion?

### Step 4: Record evidence

Suggested filename: `lab6_08_owasp_web_application.png`

Do not submit passwords, session cookies, or other sensitive information in screenshots.

### Expected learning outcome

Students should understand how browser observations complement the results of network enumeration.

---

# Practical 8 — Save Scan Results to a File

**Objective:** Preserve Nmap output for reporting and comparison.

### Step 1: Create a folder for Lab 6

On Kali, run:

```bash
mkdir -p ~/ICH-EH101/Lab6
```

This creates the directory if it does not already exist.

### Step 2: Save the selected-port scan

Run:

```bash
nmap -p 21,22,23,25,53,80,110,139,143,443,445,3306,8080 -oN ~/ICH-EH101/Lab6/owasp-ports.txt "$TARGET"
```

### Explanation

- `-oN` saves the results in normal text format.
- The path specifies the output file.
- `"$TARGET"` is the target address.

### Step 3: Save service-detection results

Run:

```bash
nmap -sV -p 21,22,23,25,53,80,110,139,143,443,445,3306,8080 -oN ~/ICH-EH101/Lab6/owasp-services.txt "$TARGET"
```

### Step 4: Read the saved results

Display the port-scan file:

```bash
cat ~/ICH-EH101/Lab6/owasp-ports.txt
```

Display the service-detection file:

```bash
cat ~/ICH-EH101/Lab6/owasp-services.txt
```

### Step 5: Confirm that the files exist

Run:

```bash
ls -l ~/ICH-EH101/Lab6
```

You should see the files you created.

### Expected learning outcome

Students should understand why saving command output is useful for reporting, repeatability, and evidence management.

---

# PART 4: ANALYSIS AND REPORTING

## 4.1 What Should Students Conclude?

After completing the practicals, students should be able to answer the following:

- What is the OWASP VM's IP address?
- Was the target reachable?
- Which selected TCP ports were open, closed, or filtered?
- Which services were identified?
- Was a web application reachable?
- Which findings are confirmed observations?
- What remains unknown?

Students must not conclude that the target is vulnerable merely because a port is open or a service version appears in the scan.

A vulnerability requires further verification using appropriate evidence and authorized testing.

## 4.2 How to Write a Finding

A good finding contains:

**Observation:** What the tool or browser showed.

**Evidence:** The screenshot, saved output, or other record that supports the observation.

**Interpretation:** What the result reasonably means.

**Limitation:** What the result does not prove.

### Example

**Observation:** Nmap reported TCP port 80 as open on the OWASP VM.

**Evidence:** The saved Nmap output shows `80/tcp open http`.

**Interpretation:** A service accepting connections on TCP port 80 was detected.

**Limitation:** This observation does not establish that the service is vulnerable.

Students should replace the example with their actual results.

## 4.3 Recommended Lab Report Structure

Submit a report titled **Lab 6: Reconnaissance and Enumeration** containing:

1. Introduction.
2. Objectives.
3. Lab environment.
4. Tools used.
5. Target IP identification.
6. Connectivity test.
7. Nmap host-discovery results.
8. Port-scan results.
9. Service-detection results.
10. Web application observations.
11. Screenshots and evidence.
12. Findings and analysis.
13. Limitations.
14. Conclusion.

---

# PART 5: REVIEW QUESTIONS

Answer in your own words.

1. Define reconnaissance.
2. Define enumeration.
3. Explain two differences between reconnaissance and enumeration.
4. What is passive reconnaissance?
5. What is active reconnaissance?
6. What is an IP address?
7. Why must we identify the target IP before scanning?
8. What is a network port?
9. What is Nmap?
10. What does `nmap -sn` do?
11. What does the `-p` option specify?
12. What does `-sV` attempt to identify?
13. What is the meaning of an open port?
14. What is the difference between a closed and filtered port?
15. Why does an open port not automatically prove that a vulnerability exists?
16. Why do Kali and the OWASP VM need suitable network connectivity?
17. Why should scan results be saved?
18. Why are screenshots important in a technical report?
19. What should a student do if the target does not respond to ping?
20. Why must ethical hackers remain within the authorized scope?
