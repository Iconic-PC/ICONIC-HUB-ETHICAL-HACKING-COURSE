# ICONIC-HUB-ETHICAL-HACKING-COURSE

# ICH-EH102 — LAB 8: ACTIVE RECONNAISSANCE

**Course:** Iconic Hub Ethical Hacking Course  
**Lab Number:** 8  
**Stage:** ICH-EH102 — Reconnaissance and Vulnerability Assessment  
**Lab Duration:** 3 Hours  
**Topic:** Active Reconnaissance  
**Attacking Machine:** Kali Linux  
**Target Machine:** OWASP Broken Web Applications (BWA) VM  
**Primary Tool:** Nmap  
**Supporting Tools:** `ping`, `ip`, `ip route`, and `curl`  
**Tagline:** Secure. Build. Innovate.

---

# PART 1: INTRODUCTION

## 1.1 What Is Active Reconnaissance?

Active reconnaissance is the process of gathering information about a target by directly interacting with the target or its network services.

In ethical hacking, reconnaissance helps a tester understand the systems within an authorized assessment scope before moving to more detailed security testing.

During active reconnaissance, a tester may send network requests, check connectivity, perform host discovery, and conduct limited port scans to determine what a target exposes.

Unlike passive reconnaissance, which relies primarily on existing public information, active reconnaissance involves direct interaction with the target environment.

For this lab, students will use Kali Linux to investigate the OWASP Broken Web Applications VM inside an authorized VirtualBox lab.

### Simple example

Imagine that you are inspecting the security of a building.

During passive reconnaissance, you study the building's publicly available information.

During active reconnaissance, you perform an authorized inspection to determine whether the building is reachable and which entrances are accessible.

In cybersecurity, the equivalent might be checking whether a virtual machine responds to network requests and identifying which TCP ports accept connections.

### Examples of active reconnaissance

- Checking whether an authorized target responds to connectivity tests.
- Performing host discovery on an approved lab network.
- Checking selected TCP ports on an assigned target.
- Observing responses from an authorized web service.
- Recording which network services appear to be accessible.
- Comparing expected network exposure with observed results.

### Why is active reconnaissance important?

Active reconnaissance helps an ethical hacker:

1. Confirm which assigned systems appear reachable.
2. Identify exposed network ports.
3. Understand how a target responds to network requests.
4. Determine which systems require further investigation.
5. Reduce uncertainty before conducting more detailed assessments.
6. Document observable network exposure.
7. Prepare for service enumeration and vulnerability assessment.

**Important:** Active reconnaissance provides observations about a target. It does not automatically prove that the target contains a vulnerability.

## 1.2 Passive Reconnaissance vs Active Reconnaissance

| Passive reconnaissance | Active reconnaissance |
|---|---|
| Uses existing public information and records | Directly interacts with the target or its network services |
| May involve reviewing public DNS records | May involve checking target reachability |
| May involve examining public certificate records | May involve sending network discovery probes |
| Usually avoids probing the target's infrastructure | Generates traffic directed at the target or its network |
| Helps establish initial knowledge of the target | Helps confirm observable network characteristics |
| Example: reviewing public domain records | Example: running Nmap against the assigned OWASP VM |

The two approaches complement one another.

Passive reconnaissance can identify a possible target or provide background information. Active reconnaissance can then help verify specific technical observations, provided the tester has appropriate authorization.

## 1.3 Active Reconnaissance vs Enumeration

These activities are related, but they have different emphasis in this course.

**Active reconnaissance** focuses on direct interaction to discover target availability and basic network exposure.

**Service enumeration** focuses on collecting more detailed information about discovered services and resources.

For example:

- Active reconnaissance may identify that TCP port 80 is open.
- Service enumeration may investigate which web server is responding and what information it discloses.

The distinction is not absolute in professional practice. Port scanning and service identification can overlap. For teaching purposes, Lab 8 focuses on discovery and basic exposure, while Lab 9 develops the service-enumeration skills in more detail.

## 1.4 What Is a Network Host?

A network host is a device or system connected to a network that can communicate using network protocols.

Examples include:

- Desktop computers.
- Laptops.
- Virtual machines.
- Web servers.
- Database servers.
- Network appliances.
- Cloud-hosted systems.

In our lab, Kali Linux is the testing machine, while the OWASP VM is the target.

Both virtual machines need suitable network configurations before they can communicate.

A host can be connected to a network without responding to every discovery method. Firewalls, network configuration, and disabled services may affect what a tester observes.

## 1.5 What Is Host Discovery?

Host discovery is the process of determining which systems appear to be reachable on a network.

A discovery tool may send different types of probes and examine the responses.

For example, Nmap can use host-discovery techniques to determine whether a specified target appears to be up.

Host discovery does not necessarily involve scanning TCP ports.

Nmap's `-sn` option performs host discovery without a port scan.

Example:

```bash
nmap -sn 192.168.56.105
```

The address above is illustrative. Students must substitute the actual IP address of their assigned OWASP VM.

### Why is host discovery useful?

Host discovery helps a tester:

- Check whether the target appears reachable.
- Identify possible network configuration problems.
- Confirm whether a target responds to the discovery method used.
- Prepare for more focused network testing.

However, a host may be reachable even when it does not respond to a particular discovery probe.

A firewall may block ICMP or other discovery traffic while allowing TCP connections to specific services.

Therefore, a failed discovery result does not always prove that a machine is offline.

## 1.6 What Is an IP Address?

An Internet Protocol (IP) address identifies a network interface for communication using IP.

There are two commonly used IP versions:

- IPv4, such as `192.168.56.105`.
- IPv6, such as `2001:db8::10`.

The IPv4 address above is an illustrative private address.

In this lab, students must identify the actual address assigned to the OWASP VM rather than guessing it.

Kali Linux and the OWASP VM have their own network interfaces and may have different IP addresses.

The testing machine's address must not be confused with the target's address.

## 1.7 What Is a Network Port?

A network port is a numbered endpoint used by transport protocols such as TCP and UDP to support communication with services.

An IP address identifies a network interface, while a port helps identify the service endpoint being contacted.

Common TCP port associations include:

| Port | Common association |
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

These are common associations, not guarantees.

A service can use a nonstandard port, and the port number alone does not confirm the identity of the service.

## 1.8 What Is Port Scanning?

Port scanning is a technique used to determine the state of selected network ports on a target.

A scanner sends probes and examines responses to infer whether ports are open, closed, or filtered.

For this lab, students will use Nmap to inspect a limited set of TCP ports on the assigned OWASP VM.

Port scanning is useful because it helps identify which network services may be exposed.

However, a port scan does not automatically identify every application, establish that the application is vulnerable, or prove that the system has been compromised.

## 1.9 Understanding Port States

Nmap may report several port states.

### Open

A service is accepting connections on the port.

Example:

```text
80/tcp open http
```

This suggests that a service is accepting TCP connections on port 80.

It does not establish that the service is vulnerable.

### Closed

The target responds to the probe, but no service is listening on that port.

Example:

```text
443/tcp closed https
```

The service label is a common association for the port, not proof that HTTPS is running.

### Filtered

Nmap cannot determine confidently whether the port is open or closed because filtering or another network condition interferes with the probe.

Example:

```text
22/tcp filtered ssh
```

The port may be affected by a firewall or other network filtering mechanism.

### Why port states matter

A tester must interpret the result rather than merely record the word displayed by the tool.

An open port may be expected. A closed port may be normal. A filtered port may indicate network controls, but the result alone does not prove that those controls are configured correctly.

## 1.10 What Is Nmap?

Nmap is a network exploration and security auditing tool.

It can help a tester:

- Discover hosts.
- Identify open, closed, or filtered ports.
- Detect services and versions.
- Investigate network exposure.
- Save scan results for reporting.

Nmap is widely used in authorized network assessments and training environments.

### Useful options for this lab

| Option | Purpose |
|---|---|
| `-sn` | Perform host discovery without a port scan |
| `-p` | Specify which ports to scan |
| `-sT` | Perform a TCP connect scan |
| `-Pn` | Skip host discovery and treat the target as online for scanning |
| `-oN` | Save output in normal text format |
| `-v` | Increase the level of scan information displayed |

We will focus on `-sn`, `-p`, `-sT`, and `-oN`.

The `-Pn` option is included for explanation and troubleshooting. It should not be used to bypass the lab's scope restrictions.

## 1.11 What Is a TCP Connection?

TCP stands for Transmission Control Protocol.

TCP supports reliable, ordered communication between applications over IP networks.

A TCP connection is commonly established through a three-way handshake:

1. The client sends a SYN segment.
2. The server responds with SYN-ACK.
3. The client responds with ACK.

This process establishes the connection.

A TCP connect scan attempts to establish a TCP connection to the selected port. Nmap can use the outcome to determine whether the port appears open, closed, or filtered.

Because this scan attempts to establish connections, it directly interacts with the target and is an active reconnaissance technique.

## 1.12 What Is ICMP?

ICMP stands for Internet Control Message Protocol.

It supports network diagnostics and error reporting.

The `ping` utility commonly uses ICMP Echo Requests and Echo Replies to test connectivity.

For example:

```bash
ping -c 4 192.168.56.105
```

The `-c 4` option requests four ping attempts.

If replies are received, this provides evidence that the target responded to those requests.

If no replies are received, the cause may be that the target is offline, the network is misconfigured, or ICMP traffic is blocked.

**A failed ping is not conclusive proof that a host is offline.**

## 1.13 What Is an Authorized Testing Scope?

The testing scope identifies which systems, addresses, services, and activities are permitted during an assessment.

For this lab, the scope is the assigned OWASP VM and the specific exercises approved by the instructor.

Students must not assume that every system visible on the network is part of the assessment.

If a target IP address is uncertain, the student must stop and confirm it before scanning.

This is especially important when Kali Linux is connected to a network that includes other devices.

---

# PART 2: LAB ENVIRONMENT AND SAFETY

## 2.1 Our Virtual Lab Setup

The lab uses two virtual machines.

### Machine 1: Kali Linux

Kali Linux is the testing machine.

Students will use its terminal and installed networking tools to perform the authorized exercises.

### Machine 2: OWASP Broken Web Applications VM

The OWASP BWA VM is the target machine.

It contains intentionally vulnerable applications intended for security training.

The VM may expose several network services depending on its configuration.

Students must investigate only the assigned VM and must not assume that a particular service or port will be available.

### Intended communication flow

Kali Linux → Authorized VirtualBox network → OWASP VM

The two machines need suitable network connectivity for the exercises.

## 2.2 Recommended VirtualBox Network Configuration

For an isolated training lab, use a suitable Host-only Adapter or Internal Network.

Both VMs must be attached to the same intended virtual network.

A Host-only Adapter can permit communication between the host computer and attached VMs. An Internal Network is designed for communication among VMs attached to that same internal network.

Neither mode should be assumed to provide internet access automatically.

For this lab:

1. Shut down both VMs before changing network settings.
2. Open the network settings for each VM.
3. Confirm the intended adapter is enabled.
4. Ensure both VMs use the same appropriate virtual network.
5. Start the VMs.
6. Inspect their IP addresses.
7. Confirm that the assigned target address is correct before testing.

Do not change networking modes unnecessarily if the existing setup already works.

Do not bridge the vulnerable OWASP VM onto a public or ordinary production network for these exercises.

## 2.3 Authorization and Scope

Students must:

- Scan only the assigned OWASP VM.
- Confirm the actual target IP address.
- Avoid scanning public websites or unrelated systems.
- Avoid scanning the entire network unless the instructor has explicitly authorized a dedicated isolated lab subnet.
- Avoid exploitation and destructive actions.
- Avoid changing target configurations without permission.
- Save the actual results.
- Stop and ask the instructor if they are unsure about the target or network.

## 2.4 Pre-Lab Checklist

Before beginning, confirm:

- [ ] Kali Linux starts successfully.
- [ ] The OWASP VM starts successfully.
- [ ] Both VMs have suitable network adapters configured.
- [ ] The OWASP VM's actual IP address can be identified.
- [ ] Kali Linux has an appropriate IP address on the lab network.
- [ ] Nmap is installed on Kali.
- [ ] The target IP has been verified.
- [ ] A folder is available for saving evidence.
- [ ] The student understands the scope and safety rules.

---

# PART 3: PRACTICAL SESSION

## Practical 1 — Prepare the Lab Workspace

**Objective:** Create a dedicated directory for Lab 8 evidence and scan results.

### Step 1: Open Kali Linux

Start Kali Linux and log in.

Open a terminal.

### Step 2: Create the directory

Run:

```bash
mkdir -p ~/ICH-EH102/Lab8/screenshots
```

### Step 3: Create an evidence log

Run:

```bash
cd ~/ICH-EH102/Lab8
touch evidence-log.txt
```

### Step 4: Verify the directory

Run:

```bash
pwd
ls -la
```

The directory should resemble:

```text
Lab8/
├── evidence-log.txt
└── screenshots/
```

### Step 5: Record the date

Run:

```bash
date -u
```

Record the date and time in the lab report.

### Step 6: Capture evidence

Take a screenshot showing the directory structure.

Suggested filename:

`lab8_01_workspace_setup.png`

### Expected result

Students should have a dedicated location for saving scan output, screenshots, and notes.

---

## Practical 2 — Start Both Virtual Machines

**Objective:** Prepare Kali Linux and the OWASP VM for authorized active reconnaissance.

### Step 1: Open VirtualBox

Start Oracle VirtualBox on the host computer.

Locate the Kali Linux VM and the OWASP VM.

### Step 2: Start Kali Linux

Start Kali and log in.

Open a terminal.

### Step 3: Start the OWASP VM

Start the OWASP BWA VM.

Wait for it to finish booting.

### Step 4: Confirm the target identity

Verify that the VM is the assigned OWASP target.

Do not substitute another machine simply because it is easier to reach.

### Step 5: Review network settings if needed

If both machines already communicate correctly, leave their settings unchanged.

If connectivity fails, shut down the VMs and inspect the VirtualBox network adapters.

Confirm that both VMs are attached to the intended lab network.

### Step 6: Capture evidence

Suggested filename:

`lab8_02_virtualbox_lab_setup.png`

### Expected result

Both VMs should be running, and students should be able to identify the machine designated as the target.

---

## Practical 3 — Identify the IP Addresses of Both Machines

**Objective:** Identify the actual IP address of the target and distinguish it from Kali's address.

### Step 1: Inspect the OWASP VM

Open a terminal on the OWASP VM, if its interface provides one.

Run:

```bash
ip addr
```

The command displays network interfaces and their assigned addresses.

Look for:

- The interface connected to the lab network.
- Its operational state.
- An IPv4 address.
- The network prefix.

An illustrative address might be:

```text
192.168.56.105/24
```

This is only an example. Your target may have a different address.

### Step 2: Inspect Kali Linux

Switch to Kali and run:

```bash
ip addr
```

Identify the address associated with the interface connected to the lab network.

### Step 3: Inspect the routing table

On Kali, run:

```bash
ip route
```

This displays the system's routing table.

Look for the route associated with the network interface used to reach the target.

The output can help students understand how Kali determines where to send network traffic.

### Step 4: Record the addresses

Complete this table using the actual results.

| Field | Actual result |
|---|---|
| Kali interface | |
| Kali IPv4 address | |
| OWASP interface | |
| OWASP IPv4 address | |
| Network prefix | |
| Target identification verified? | |

### Step 5: Verify the target variable

On Kali, set the target address to the actual OWASP VM address.

For example:

```bash
TARGET="192.168.56.105"
```

Replace the example address with your actual target.

Check the variable:

```bash
printf '%s\n' "$TARGET"
```

Ensure that the displayed address matches the recorded target address.

### Step 6: Capture evidence

Suggested filenames:

- `lab8_03_owasp_ip_address.png`
- `lab8_04_kali_ip_address.png`
- `lab8_05_kali_routing_table.png`

### Expected result

Students should correctly identify the IP address of the assigned target and distinguish it from Kali's address.

---

## Practical 4 — Test Basic Connectivity

**Objective:** Use `ping` to determine whether the OWASP VM responds to ICMP Echo Requests.

### Step 1: Open Kali's terminal

Confirm that the `TARGET` variable contains the correct address.

Run:

```bash
printf '%s\n' "$TARGET"
```

### Step 2: Send four ping requests

Run:

```bash
ping -c 4 "$TARGET"
```

This sends four ICMP Echo Requests to the assigned target.

### Step 3: Observe the output

If the target responds, the output may contain replies showing the target IP and response time.

At the end, the command may display packet statistics, including the number of transmitted and received packets.

Record the actual results.

### Step 4: Interpret the results

If replies are received, the target responded to the ICMP requests.

If all requests time out, possible explanations include:

- The target is offline.
- The target IP is incorrect.
- The VMs are attached to different networks.
- The target interface is down.
- A firewall blocks ICMP.
- A network configuration issue prevents communication.

Do not immediately conclude that the target is offline.

### Step 5: Record the results

| Question | Actual observation |
|---|---|
| Did the target respond? | |
| Packets transmitted | |
| Packets received | |
| Packet loss | |
| Reported response time | |
| Reasonable conclusion | |

### Step 6: Save evidence

Suggested filename:

`lab8_06_ping_connectivity.png`

### Expected result

Students should understand how to use ping and interpret both successful and unsuccessful connectivity tests.

---

## Practical 5 — Perform Nmap Host Discovery

**Objective:** Use Nmap to determine whether the assigned target appears reachable without scanning its ports.

### Step 1: Confirm the target address

Run:

```bash
echo "$TARGET"
```

Check that the address is correct.

### Step 2: Perform host discovery

Run:

```bash
nmap -sn "$TARGET"
```

### Step 3: Understand the command

- `nmap` starts the network discovery tool.
- `-sn` performs host discovery without a port scan.
- `"$TARGET"` specifies the assigned target IP address.

Nmap uses discovery methods appropriate to the target and network conditions.

### Step 4: Interpret the output

Nmap may report that the host is up.

If it reports the host is down, that result does not conclusively establish that the target is offline.

The discovery probes may not have received a response.

### Step 5: Compare ping and Nmap results

Complete this table.

| Test | Result |
|---|---|
| Ping response | |
| Nmap host-discovery result | |
| Did both tests agree? | |
| What might explain any difference? | |

### Step 6: Capture evidence

Suggested filename:

`lab8_07_nmap_host_discovery.png`

### Expected result

Students should be able to distinguish a host-discovery operation from a port scan and explain what its results establish.

---

## Practical 6 — Scan Selected TCP Ports

**Objective:** Identify the state of selected TCP ports on the assigned OWASP VM.

This practical moves beyond host discovery and examines specific network endpoints.

### Step 1: Confirm the target

Run:

```bash
printf '%s\n' "$TARGET"
```

Check the IP address against the lab record.

### Step 2: Run a limited TCP connect scan

Run:

```bash
nmap -sT -p 22,80,443,3306,8080 "$TARGET"
```

This command checks a limited set of TCP ports on the assigned target.

### Step 3: Understand the options

- `-sT` requests a TCP connect scan.
- `-p` specifies the ports to examine.
- `"$TARGET"` specifies the authorized target.

A TCP connect scan attempts to establish TCP connections to the selected ports.

### Step 4: Interpret the results

Illustrative output:

```text
PORT     STATE     SERVICE
22/tcp   closed    ssh
80/tcp   open      http
443/tcp  closed    https
3306/tcp filtered  mysql
8080/tcp open      http-proxy
```

**This is example output only. It is not a prediction of your OWASP VM's actual results.**

Your target may return different port states, and the reported service labels may not accurately identify the running applications.

### Step 5: Record the actual results

| Port | Actual state | Reported service | Interpretation |
|---|---|---|---|
| 22 | | | |
| 80 | | | |
| 443 | | | |
| 3306 | | | |
| 8080 | | | |

Include only ports and results that were actually observed.

### Step 6: Answer the following questions

1. Which selected ports were reported open?
2. Which were reported closed?
3. Were any ports reported filtered?
4. What might the open ports be used for?
5. Does an open port automatically mean the service is vulnerable?
6. What additional information would be needed to identify the services accurately?

### Step 7: Save evidence

Suggested filename:

`lab8_08_selected_tcp_ports.png`

### Expected result

Students should be able to run a limited TCP port scan, document port states, and explain the limitations of the output.

---

## Practical 7 — Compare Discovery and Port Scanning

**Objective:** Understand how host discovery differs from port scanning.

### Step 1: Review the host-discovery command

```bash
nmap -sn "$TARGET"
```

This performs host discovery without a port scan.

### Step 2: Review the TCP scan command

```bash
nmap -sT -p 22,80,443,3306,8080 "$TARGET"
```

This checks selected TCP ports.

### Step 3: Compare the purpose of each command

Complete the table.

| Question | Host discovery | Selected-port scan |
|---|---|---|
| Primary objective | | |
| Does it scan the selected TCP ports? | | |
| What type of information does it provide? | | |
| What does the result not prove? | | |

### Step 4: Explain the difference

Students should explain that host discovery determines whether a target appears reachable using the selected discovery method.

A port scan investigates the state of particular ports.

These operations answer different questions and should not be treated as interchangeable.

### Step 5: Capture evidence

Suggested filename:

`lab8_09_discovery_vs_port_scan.png`

### Expected result

Students should understand the purpose of each technique and know when to use it during an authorized assessment.

---

## Practical 8 — Save Scan Results to Files

**Objective:** Preserve the results of active reconnaissance for later analysis and reporting.

### Step 1: Confirm the output directory

Run:

```bash
mkdir -p ~/ICH-EH102/Lab8
cd ~/ICH-EH102/Lab8
```

### Step 2: Save the host-discovery results

Run:

```bash
nmap -sn -oN host-discovery.txt "$TARGET"
```

This saves the normal text output to `host-discovery.txt`.

### Step 3: Save the selected-port scan

Run:

```bash
nmap -sT -p 22,80,443,3306,8080 -oN selected-ports.txt "$TARGET"
```

This saves the selected-port scan results to `selected-ports.txt`.

### Step 4: Read the saved results

Run:

```bash
cat host-discovery.txt
```

Then:

```bash
cat selected-ports.txt
```

Compare the saved results with the terminal output from the earlier practicals.

### Step 5: Confirm that the files exist

Run:

```bash
ls -l ~/ICH-EH102/Lab8
```

You should see the output files and any other files saved in the directory.

### Step 6: Understand why saving results matters

Saved scan output helps students:

- Support their findings with evidence.
- Review results after the practical.
- Compare observations.
- Prepare a technical report.
- Explain what was observed during the assessment.

Students should not alter saved evidence to make results appear more successful.

### Step 7: Capture evidence

Suggested filenames:

- `lab8_10_saved_host_discovery.png`
- `lab8_11_saved_port_scan.png`

### Expected result

Students should be able to preserve and retrieve Nmap output for reporting.

---

## Practical 9 — Inspect an Authorized Web Service

**Objective:** Observe the response of a web service identified during the scan.

This practical is conditional on a web service being available on the assigned OWASP VM.

It is not necessary to force the exercise if the expected web service is unavailable.

### Step 1: Review the port-scan results

Look for an open port commonly associated with HTTP, such as TCP port 80 or 8080.

The port number alone does not guarantee that an HTTP service is running.

### Step 2: Construct the appropriate URL

If the assigned target has an HTTP service on port 80, the URL may be:

```text
http://TARGET_IP/
```

If the service is on port 8080, the URL may be:

```text
http://TARGET_IP:8080/
```

Replace `TARGET_IP` with the actual target IP address.

Use the port identified in your lab results and only interact with the assigned target.

### Step 3: Open Firefox in Kali

Enter the appropriate URL in Firefox.

Observe whether a page loads and record the response.

The page may display a training application, a default server page, or another response depending on the VM configuration.

### Step 4: Record your observations

| Field | Observation |
|---|---|
| Target IP | |
| Port used | |
| URL accessed | |
| Did the page load? | |
| Page or application observed | |
| Any error message | |
| What does the result establish? | |
| What remains unknown? | |

### Step 5: Optional basic HTTP check

If `curl` is available, students may make a basic request to the assigned web service.

For example:

```bash
curl -I "http://$TARGET/"
```

This requests the response headers for the specified URL.

If the service uses port 8080, use:

```bash
curl -I "http://$TARGET:8080/"
```

A server may not support the HTTP HEAD method, so an error response does not necessarily mean that the web application is unavailable.

Do not submit passwords, session cookies, or other sensitive information during this exercise.

### Step 6: Capture evidence

Suggested filename:

`lab8_12_authorized_web_service.png`

### Expected result

Students should understand how browser observations and basic HTTP responses can complement network discovery.

---

# PART 4: ANALYSIS AND REPORTING

## 4.1 What Should Students Conclude?

After completing the practicals, students should be able to answer:

1. What is the actual IP address of the assigned OWASP VM?
2. What IP address is assigned to Kali on the lab network?
3. Did the target respond to ping?
4. What did Nmap host discovery report?
5. Which selected TCP ports were open, closed, or filtered?
6. Did the discovery and port-scan results differ?
7. Was a web service accessible?
8. What evidence supports each observation?
9. What remains unknown?
10. What additional investigation might be appropriate in a later lab?

Students must not claim that a service is vulnerable simply because its port is open.

A port scan is an observation of network exposure, not a complete security assessment.

## 4.2 How to Write a Finding

A good active reconnaissance finding contains:

**Finding ID:** A unique identifier, such as AR-01.

**Title:** A short description of the observation.

**Observation:** What the command or tool actually reported.

**Evidence:** The screenshot or saved output supporting the observation.

**Interpretation:** What the result reasonably means.

**Limitation:** What the result does not establish.

**Recommended follow-up:** What further authorized investigation may be appropriate.

### Example finding

**Finding ID:** AR-01

**Title:** TCP port 80 responded as open on the assigned OWASP VM.

**Observation:** Nmap reported TCP port 80 as open during the selected-port scan.

**Evidence:** The saved Nmap output contains the corresponding port state.

**Interpretation:** A service accepted TCP connections on port 80 during the scan.

**Limitation:** The result alone does not identify the application conclusively or establish that the service contains a vulnerability.

**Recommended follow-up:** Investigate the service in the service-enumeration lab.

Students must replace this example with their actual observations.

## 4.3 Recommended Lab Report Structure

Students should submit a report titled:

**Lab 8: Active Reconnaissance**

The report should contain:

1. Introduction.
2. Objectives.
3. Lab environment.
4. Scope and authorization.
5. Tools used.
6. Target IP identification.
7. Network configuration observations.
8. Connectivity-test results.
9. Nmap host-discovery results.
10. Selected-port scan results.
11. Authorized web-service observations, if applicable.
12. Evidence screenshots.
13. Findings and interpretation.
14. Limitations.
15. Conclusion.

## 4.4 Evidence Requirements

Each screenshot must:

- Show the relevant command or application.
- Contain enough context to identify the activity.
- Be readable.
- Support a finding discussed in the report.
- Be saved with a meaningful filename.
- Avoid exposing unrelated personal information or secrets.

Students should also retain the actual text output of their scans.

A screenshot alone may not provide enough detail to reproduce or verify a technical observation.

## 4.5 Suggested Evidence Checklist

- [ ] VirtualBox lab setup.
- [ ] OWASP VM IP address.
- [ ] Kali Linux IP address.
- [ ] Kali routing table.
- [ ] Ping connectivity test.
- [ ] Nmap host discovery.
- [ ] Selected TCP port scan.
- [ ] Saved Nmap results.
- [ ] Web-service observation, if applicable.
- [ ] Completed findings table.

---

# PART 5: TROUBLESHOOTING

## 5.1 The Target Does Not Respond to Ping

Possible causes include:

- The target is powered off.
- The target IP address is incorrect.
- The VMs are attached to different virtual networks.
- The target interface is down.
- ICMP traffic is blocked.

Recommended steps:

1. Confirm that both VMs are running.
2. Run `ip addr` on each machine.
3. Verify the target address.
4. Inspect the VirtualBox network settings.
5. Confirm that both machines use the intended lab network.
6. Retry the connectivity test.

Do not assume the target is offline simply because ping fails.

## 5.2 Nmap Reports the Host as Down

Possible causes include incorrect addressing, connectivity problems, or blocked discovery probes.

First, verify the target address and network configuration.

If the instructor confirms that the target is correct and a discovery probe is being blocked, the instructor may demonstrate the use of `-Pn` against that exact authorized target.

For example:

```bash
nmap -Pn -sT -p 80 "$TARGET"
```

This skips host discovery and attempts to scan the specified TCP port.

Use it only when the target is verified and the scan is within the lab scope. It does not repair network connectivity or prove that the host is online.

## 5.3 Nmap Is Not Installed

Check availability:

```bash
command -v nmap
```

If it is missing, ask the instructor whether installation is permitted.

On Kali Linux, Nmap can typically be installed with:

```bash
sudo apt update
sudo apt install nmap
```

## 5.4 All Selected Ports Are Closed

Possible explanations include:

- The selected services are not running.
- The target VM has a different configuration.
- The selected ports do not match the exposed services.
- The wrong IP address was used.

Verify the target and the lab configuration before drawing a conclusion.

Do not invent open ports to complete the assignment.

## 5.5 Ports Are Reported as Filtered

Filtering may prevent Nmap from determining whether a port is open or closed.

Possible causes include firewall rules, network filtering, or other conditions that prevent a clear response.

Record the observed state and explain the uncertainty.

A filtered port is not proof of a vulnerability or proof that the firewall is correctly configured.

## 5.6 The Web Application Does Not Load

Possible causes include:

- The selected port is not serving HTTP.
- The URL uses the wrong port.
- The web service is not running.
- The target is unreachable.
- The service expects HTTPS instead of HTTP.
- The service returned an error.

Check the actual scan results and verify the intended URL.

Do not assume that a web application must be available simply because the target VM has booted.

## 5.7 The Scan Results Differ From Another Student's Results

Possible causes include different target configurations, different target addresses, timing, or different network settings.

Students should confirm that they tested the correct target and used the same authorized parameters.

Record actual results rather than copying another student's output.

---

# PART 6: REVIEW QUESTIONS

Answer in your own words.

1. Define active reconnaissance.
2. Why is active reconnaissance important in ethical hacking?
3. Explain the difference between passive and active reconnaissance.
4. Explain the difference between active reconnaissance and service enumeration.
5. What is a network host?
6. What is host discovery?
7. What is an IP address?
8. Why must the target IP address be verified before scanning?
9. What is a network port?
10. What is port scanning?
11. What is Nmap?
12. What does the `-sn` option do?
13. What does the `-p` option specify?
14. What does the `-sT` option do?
15. What does the `-oN` option do?
16. What is the purpose of the `ping` command?
17. What is ICMP?
18. Why does a failed ping not necessarily mean that a host is offline?
19. What is the difference between an open, closed, and filtered port?
20. Why does an open port not automatically prove that a vulnerability exists?
21. Why should students save their scan results?
22. Why must Kali Linux and the OWASP VM have suitable network connectivity?
23. What should a student do if Nmap reports that the host is down?
24. What is the purpose of an authorized testing scope?
25. Why should students not scan unrelated machines?
26. What information should a professional active reconnaissance finding contain?
27. How does active reconnaissance prepare a tester for service enumeration?
28. What is the difference between an observed result and an interpretation?
29. Why must students report their actual scan results?
30. Explain two ethical responsibilities that apply during active reconnaissance.

---

# PART 7: TAKE-HOME ASSIGNMENT

## Assignment Title: Active Reconnaissance Report

**Total Marks: 20**

### Scenario

You are a junior penetration tester assigned to investigate an intentionally vulnerable virtual machine in a controlled lab.

Your objective is to determine whether the assigned target appears reachable, identify selected exposed TCP ports, and document the results accurately.

You are not authorized to exploit the target or scan unrelated systems.

### Task 1 — Document the lab environment

Record:

- The testing machine.
- The target machine.
- The VirtualBox network configuration.
- Kali's actual lab-network IP address.
- The target's actual IP address.
- The authorized scope.

### Task 2 — Perform connectivity testing

Use ping to check whether the target responds.

Record the command, actual output, packet statistics, and a reasonable interpretation.

If the target does not respond, document the troubleshooting steps taken.

### Task 3 — Perform host discovery

Run Nmap host discovery against the assigned target.

Record the command and the actual result.

Explain what the result establishes and what it does not prove.

### Task 4 — Scan selected TCP ports

Perform the limited TCP port scan from Practical 6.

Record the actual state of each selected port.

Explain the difference between open, closed, and filtered ports.

### Task 5 — Save scan results

Save the host-discovery and selected-port scan outputs to text files.

Include evidence showing that the files exist.

### Task 6 — Inspect a web service

If an appropriate web service is available, inspect it using Firefox or the basic HTTP request demonstrated in the lab.

Record the observed response.

If no web service is available on the selected ports, explain the result rather than inventing one.

### Task 7 — Produce a findings table

Include at least three findings supported by actual evidence, where the available results permit.

For each finding, include:

- Finding ID.
- Observation.
- Evidence.
- Interpretation.
- Limitation.
- Suggested follow-up.

### Task 8 — Write a conclusion

Explain:

1. Whether the target responded to the tests performed.
2. Which selected ports were observed as open, closed, or filtered.
3. Which results require additional investigation.
4. What the scans do not establish.
5. Why authorization and scope were important.

### Submission requirements

Submit a report containing:

- Introduction.
- Objectives.
- Lab environment and scope.
- Commands used.
- Actual scan results.
- Evidence screenshots.
- Findings and interpretation.
- Limitations.
- Conclusion.

Do not submit invented scan output or present example output from these notes as your own results.

### Marking scheme

| Assessment area | Marks |
|---|---:|
| Lab environment and scope | 2 |
| Correct identification of target and IP addresses | 3 |
| Connectivity and host-discovery exercises | 4 |
| Selected-port scanning and interpretation | 4 |
| Evidence and findings | 4 |
| Report quality and ethical compliance | 3 |
| **Total** | **20** |

---

# PART 8: CONCLUSION

Active reconnaissance allows an ethical hacker to gather information by directly interacting with an authorized target or its network services.

In this lab, students learned how to identify a target's IP address, test connectivity, perform Nmap host discovery, scan selected TCP ports, inspect an available web service, and save results for reporting.

They also learned that network observations must be interpreted carefully.

A failed ping does not necessarily mean that a machine is offline. A port reported as open does not automatically indicate a vulnerability. A filtered port does not conclusively reveal the firewall's configuration.

Professional reconnaissance requires accurate evidence, a clear understanding of limitations, and strict adherence to the authorized testing scope.

**End of Lab 8 — Active Reconnaissance**

**Iconic Hub — Secure. Build. Innovate.**
