# ICONIC-HUB-ETHICAL-HACKING-COURSE

# ICH-EH102 — LAB 9: SERVICE ENUMERATION

**Course:** Iconic Hub Ethical Hacking Course  
**Lab Number:** 9  
**Stage:** ICH-EH102 — Reconnaissance and Vulnerability Assessment  
**Duration:** 3 Hours  
**Topic:** Service Enumeration  
**Attacking Machine:** Kali Linux  
**Target Machine:** OWASP Broken Web Applications (BWA) VM  
**Primary Tool:** Nmap  
**Supporting Tools:** `curl`, `whatweb`, `nc`, `ip`, and Firefox  
**Tagline:** Secure. Build. Innovate.

---

# PART 1: INTRODUCTION TO SERVICE ENUMERATION

## 1.1 What Is Service Enumeration?

Service enumeration is the process of gathering detailed information about services running on a target system.

When a computer or server provides a service, it may disclose information about the software, version, configuration, protocols, or resources associated with that service.

An ethical hacker uses enumeration to understand what a target exposes and identify areas that may require further security assessment.

In this lab, students will use Kali Linux to investigate services exposed by the assigned OWASP BWA virtual machine.

The goal is to collect information, interpret the results, and document potential areas for further investigation without exploiting the target.

### Simple example

Imagine that a security officer discovers that a building has an accessible entrance.

The next step is to investigate what the entrance leads to, which department operates behind it, and what access controls are present.

In cybersecurity, discovering an open port is similar to discovering an accessible entrance.

Service enumeration goes further by investigating the service associated with that port.

For example:

- A port scan identifies TCP port 80 as open.
- Service detection suggests that an HTTP server is running.
- HTTP inspection reveals response headers or a default page.
- Further investigation may identify the software or configuration that needs review.

Each step provides additional information, but none automatically proves that the service is vulnerable.

## 1.2 Why Is Service Enumeration Important?

Service enumeration helps an ethical hacker:

1. Identify services exposed by a target.
2. Determine which services may be accessible to users or other systems.
3. Identify software products and version information when disclosed.
4. Understand which protocols a service uses.
5. Observe information returned by services.
6. Identify unnecessary or unexpected service exposure.
7. Prepare for a more focused vulnerability assessment.
8. Produce evidence-based security findings.

Without enumeration, a tester may know that a port is open but have insufficient information to understand what is running behind it.

## 1.3 Reconnaissance, Port Scanning, and Service Enumeration

These activities are related, but each answers a different question.

| Activity | Main question | Example |
|---|---|---|
| Reconnaissance | What information can I gather about the target? | Identifying the target IP address |
| Host discovery | Does the target appear reachable? | Running `nmap -sn` |
| Port scanning | Which ports are open, closed, or filtered? | Checking TCP ports 22, 80, and 443 |
| Service enumeration | What information do the discovered services disclose? | Identifying an HTTP server and inspecting its response |
| Vulnerability assessment | Are there weaknesses in the identified service or configuration? | Verifying whether a documented security issue applies |

The distinction is useful for learning, although professional security assessments may combine these activities.

### Example workflow

```text
Identify the authorized target
            |
            v
Perform reconnaissance
            |
            v
Discover reachable hosts
            |
            v
Scan selected ports
            |
            v
Enumerate discovered services
            |
            v
Verify potential security weaknesses
            |
            v
Document findings and recommendations
```

The objective of Lab 9 is the service-enumeration stage.

## 1.4 What Is a Network Service?

A network service is software that provides a function to other applications or computers through a network or communication interface.

Examples include:

- Web services that deliver websites and web applications.
- SSH services that provide remote command-line access.
- FTP services that transfer files.
- DNS services that resolve domain names.
- Database services that store and retrieve data.
- SMB services that support network file and printer sharing.
- Email services that send or receive messages.

A service commonly listens on a port, but not every service uses its standard port.

A web server can operate on TCP port 8080 instead of TCP port 80. A database service may also use a nonstandard port.

Therefore, the port number alone is not enough to identify the actual service.

## 1.5 What Is a Service Banner?

A service banner is information a service returns that may identify the software or provide details about the service.

Depending on its configuration, a service might disclose:

- Product name.
- Software version.
- Protocol information.
- Operating system clues.
- Server type.
- Configuration details.

For example, a service might return a response suggesting that it uses a particular version of an SSH server.

This information can help a tester determine which software documentation or security advisories to consult.

However, banners may be incomplete, misleading, customized, or deliberately hidden.

A banner is an observation, not definitive proof of the software's identity or vulnerability status.

## 1.6 What Is Service Version Detection?

Service version detection is a technique used to identify the software or product associated with an open network service.

Nmap supports service detection through the `-sV` option.

Example:

```bash
nmap -sV -p 22,80,443 192.168.56.105
```

This command asks Nmap to investigate the services on the selected ports of the specified target.

The address is illustrative. Students must use the actual IP address of their assigned OWASP VM.

Nmap may identify a product and version, identify only the service, or return incomplete information.

### Why is version detection useful?

Suppose a tester discovers a web server.

Knowing only that HTTP is available provides limited information.

If the tester can reliably identify the software and version, the tester can investigate:

- The product's official documentation.
- Supported versions.
- Known security advisories.
- Relevant vendor recommendations.
- Whether further verification is necessary.

The version alone does not establish that a vulnerability is present. The tester must also consider the configuration, installed patches, applicable conditions, and supporting evidence.

## 1.7 What Is HTTP?

HTTP stands for Hypertext Transfer Protocol.

It is an application-layer protocol used to transfer resources between clients and servers.

Web browsers commonly use HTTP to request web pages and other resources.

For example, when a user visits a website, the browser sends an HTTP request and the server returns an HTTP response.

A basic HTTP response may contain:

- A status code.
- Response headers.
- A response body.

### Common HTTP status codes

| Status code | Meaning |
|---|---|
| 200 | OK; the request succeeded |
| 301 | Moved permanently |
| 302 | Temporary redirect |
| 400 | Bad request |
| 401 | Authentication required or credentials are missing or invalid |
| 403 | The server refuses the request |
| 404 | The requested resource was not found |
| 405 | The request method is not allowed |
| 500 | Internal server error |
| 503 | Service unavailable |

These codes describe the response to a particular request. They do not independently establish whether an application is secure.

## 1.8 What Are HTTP Response Headers?

HTTP response headers provide metadata about a server response.

Headers may communicate information about content type, caching, redirection, cookies, and security-related browser behavior.

Examples include:

| Header | Purpose |
|---|---|
| `Server` | May disclose server software information |
| `Content-Type` | Identifies the media type of the response body |
| `Location` | Indicates a redirect destination |
| `Set-Cookie` | Requests that the browser store a cookie |
| `Strict-Transport-Security` | Instructs compatible browsers to use HTTPS for future requests under applicable conditions |
| `X-Content-Type-Options` | Can help prevent MIME-type sniffing |
| `Content-Security-Policy` | Defines browser-enforced restrictions on resource loading and related behaviors |
| `X-Frame-Options` | Provides a mechanism for controlling framing by other pages |

Not every application returns every header.

A missing header may warrant investigation, but it does not automatically establish a vulnerability. Its significance depends on the application's behavior, deployment, and threat model.

## 1.9 What Is HTTPS?

HTTPS is HTTP carried over a connection protected by TLS.

TLS stands for Transport Layer Security.

HTTPS helps protect data in transit by providing encryption and integrity protection, and it supports authentication of the server through certificate validation.

Commonly, HTTPS uses TCP port 443, although it can operate on other ports.

When investigating a web service, students should distinguish between:

- HTTP, which does not itself provide TLS protection.
- HTTPS, which uses TLS to protect the HTTP communication.

An open port 443 does not prove that the service is correctly configured or that every part of the application is secure.

Likewise, a service on port 80 may redirect users to HTTPS or may serve an application that needs further security review.

## 1.10 What Is WhatWeb?

WhatWeb is a web technology identification tool.

It can help identify technologies that a website appears to use, including:

- Web server software.
- Content management systems.
- Web frameworks.
- JavaScript libraries.
- Other recognizable technologies.

For example, WhatWeb may identify indicators associated with a particular web server or application framework.

The identification depends on the information exposed by the application and the tool's detection capabilities.

A technology fingerprint is a lead for investigation, not conclusive proof that a particular software version is installed.

For this lab, students should use WhatWeb only against the assigned OWASP VM.

## 1.11 What Is Netcat?

Netcat, commonly invoked as `nc`, is a network utility that can read from and write to network connections.

Depending on the implementation, it can help test connectivity and interact with TCP or UDP services.

In service enumeration, Netcat can sometimes be used to observe a service's initial response or send a basic protocol request.

For example:

```bash
nc -nv -w 3 192.168.56.105 22
```

This attempts a connection to TCP port 22 and limits the wait time.

The example IP address must be replaced with the actual assigned target.

Some services return a banner immediately. Others wait for a protocol-specific request or disclose very little information.

Netcat does not automatically identify a vulnerability, and it should not be used to send arbitrary commands to unfamiliar services.

## 1.12 What Is Service Exposure?

Service exposure describes how a service is made reachable through a network.

A service may be exposed to:

- The local computer only.
- Other computers on a private network.
- A specific internal network segment.
- The public internet.

The appropriate exposure depends on the service's purpose and security requirements.

For example, an internal database may not need to accept connections from every workstation or external network.

A security tester should determine whether observed exposure matches the intended design.

**Important:** An accessible service is not automatically an unnecessary service. Its business purpose, intended users, network placement, and access controls must be considered.

## 1.13 What Is the Difference Between Enumeration and Exploitation?

Enumeration collects information about a target.

Exploitation attempts to use a vulnerability or weakness to produce an effect that should not be possible under the intended security model.

Examples of enumeration include:

- Identifying an HTTP service.
- Observing response headers.
- Recording a disclosed software version.
- Determining which selected ports are open.

Examples of exploitation include attempting to bypass authorization, execute unauthorized commands, or gain access through a confirmed weakness.

This lab focuses on enumeration.

Students must not exploit services or attempt unauthorized access.

---

# PART 2: LAB ENVIRONMENT AND SAFETY

## 2.1 Lab Machines

### Machine 1: Kali Linux

Kali is the testing machine.

Students will use its terminal and networking tools to investigate the target.

### Machine 2: OWASP Broken Web Applications VM

The OWASP BWA VM is the authorized target.

It contains intentionally vulnerable applications designed for training.

The services available depend on the VM configuration and network settings.

Students must record what they actually observe rather than assume that every example in this lesson will appear on their target.

## 2.2 Network Configuration

Both VMs should use the intended isolated lab network.

A suitable VirtualBox Host-only Adapter or Internal Network can be used, depending on the existing configuration.

Before scanning:

1. Confirm both VMs are running.
2. Inspect their network interfaces.
3. Identify the target IP address.
4. Confirm the target is the assigned OWASP VM.
5. Verify that the intended lab network is functioning.

Do not connect the vulnerable target to a public network merely to make the practical work.

## 2.3 Authorization and Scope

Students must:

- Enumerate only the assigned OWASP VM.
- Use only the confirmed target IP address.
- Avoid scanning unrelated systems.
- Avoid testing public websites.
- Avoid password guessing or brute-force attempts.
- Avoid modifying services or target configurations without permission.
- Avoid uploading files or executing commands on the target.
- Preserve accurate evidence.
- Stop and ask the instructor if they are uncertain about the target.

## 2.4 Pre-Lab Checklist

- [ ] Kali Linux is running.
- [ ] OWASP BWA is running.
- [ ] Both VMs use the intended lab network.
- [ ] The target IP address has been confirmed.
- [ ] Nmap is installed.
- [ ] Firefox is available.
- [ ] `curl` is available.
- [ ] WhatWeb is available, or the instructor has approved an alternative.
- [ ] The Lab 9 evidence directory has been created.
- [ ] Students understand the authorized scope.

---

# PART 3: PRACTICAL SESSION

## Practical 1 — Prepare the Lab Workspace

**Objective:** Create a dedicated directory for Lab 9 results.

### Step 1: Open Kali Linux

Start Kali Linux and open a terminal.

### Step 2: Create the directory

Run:

```bash
mkdir -p ~/ICH-EH102/Lab9/screenshots
```

### Step 3: Create an evidence log

Run:

```bash
cd ~/ICH-EH102/Lab9
touch evidence-log.txt
```

### Step 4: Verify the directory

Run:

```bash
pwd
ls -la
```

### Step 5: Record the date

Run:

```bash
date -u
```

Record the date and time in the report.

### Step 6: Capture evidence

Suggested filename:

`lab9_01_workspace_setup.png`

### Expected result

Students should have a directory in which to preserve their scan output and screenshots.

---

## Practical 2 — Confirm the Target IP Address

**Objective:** Verify the IP address of the assigned OWASP VM.

### Step 1: Inspect the OWASP VM

On the target VM, run:

```bash
ip addr
```

Identify the interface connected to the lab network and record its IPv4 address.

### Step 2: Inspect Kali

On Kali, run:

```bash
ip addr
```

Record Kali's lab-network address.

### Step 3: Set the target variable

Replace the example address with the actual address of the OWASP VM.

```bash
TARGET="192.168.56.105"
```

### Step 4: Verify the variable

Run:

```bash
printf '%s\n' "$TARGET"
```

Confirm that the displayed address matches the target record.

### Step 5: Check connectivity

Run:

```bash
ping -c 4 "$TARGET"
```

Record whether the target responds.

A failed ping does not necessarily mean that the target is offline.

### Step 6: Capture evidence

Suggested filenames:

- `lab9_02_target_ip.png`
- `lab9_03_connectivity_test.png`

### Expected result

Students should know the target address and be ready to perform authorized service discovery.

---

## Practical 3 — Identify Open Ports and Services

**Objective:** Identify which selected TCP ports are open and investigate the services associated with them.

### Step 1: Perform a limited port scan

Run:

```bash
nmap -sT -p 21,22,23,25,53,80,110,139,143,443,445,3306,8080 "$TARGET"
```

This checks selected TCP ports on the assigned target.

### Step 2: Review the output

Nmap may display results similar to:

```text
PORT     STATE     SERVICE
22/tcp   open      ssh
80/tcp   open      http
443/tcp  closed    https
```

This is illustrative output only.

Your target may produce different results.

### Step 3: Identify candidate services

Record the open ports and their reported service labels.

Do not treat the labels as definitive identification of the software.

### Step 4: Complete the table

| Port | State | Reported service | Requires further investigation? |
|---|---|---|---|
| | | | |
| | | | |
| | | | |

Include only the results actually returned by Nmap.

### Step 5: Capture evidence

Suggested filename:

`lab9_04_initial_port_inventory.png`

### Expected result

Students should have a list of the exposed ports that will guide the rest of the enumeration process.

---

## Practical 4 — Perform Service Version Detection with Nmap

**Objective:** Use Nmap's service detection capability to gather more detailed information about selected open ports.

### Step 1: Understand the purpose

A basic port scan identifies port states and provides common service associations.

Service detection attempts to identify the software responding on open ports.

### Step 2: Select the ports

Review the actual scan results from Practical 3.

Select only ports that were reported open and that are relevant to the authorized exercise.

For example, if ports 22, 80, and 8080 were reported open, run:

```bash
nmap -sV -p 22,80,8080 "$TARGET"
```

**Important:** Adjust the port list to match the real scan results. Do not assume these ports are open.

### Step 3: Understand the command

- `nmap` starts Nmap.
- `-sV` enables service and version detection.
- `-p` specifies the ports to investigate.
- `"$TARGET"` identifies the authorized target.

Service detection sends additional probes to the selected services.

### Step 4: Examine the output

Nmap may report information such as:

- Service name.
- Product name.
- Version information.
- Additional service details.

Some services may not disclose their version.

### Step 5: Interpret the results

Record the information exactly as reported.

Do not guess a version when the output is incomplete.

If Nmap identifies a product and version, that information can help guide later research.

However, the result may require verification.

### Step 6: Complete the service inventory

| Port | State | Product or service | Version reported | Confidence or limitations |
|---|---|---|---|---|
| | | | | |
| | | | | |
| | | | | |

### Step 7: Capture evidence

Suggested filename:

`lab9_05_nmap_service_detection.png`

### Expected result

Students should understand how Nmap service detection differs from a basic port scan and how to record its limitations.

---

## Practical 5 — Investigate HTTP Response Headers

**Objective:** Examine the response headers of a web service on the authorized target.

Perform this practical only if the target provides an HTTP service on the selected port.

### Step 1: Identify the HTTP port

Review the Nmap results.

If HTTP is available on TCP port 80, the base URL may be:

```text
http://TARGET_IP/
```

If the HTTP service is on port 8080, the URL may be:

```text
http://TARGET_IP:8080/
```

Replace `TARGET_IP` with the actual address.

### Step 2: Request the response headers

If the service is on port 80, run:

```bash
curl -I "http://$TARGET/"
```

If the service is on port 8080, run:

```bash
curl -I "http://$TARGET:8080/"
```

Use the correct port for your target.

### Step 3: Understand the command

`curl` is a command-line tool for transferring data using protocols such as HTTP and HTTPS.

The `-I` option requests response headers using the HTTP HEAD method.

The server may not support HEAD requests, so an error does not necessarily mean that the web service is unavailable.

### Step 4: Examine the output

An illustrative response might look like:

```text
HTTP/1.1 200 OK
Content-Type: text/html
Server: ExampleServer
```

This example is not an expected result for your VM.

Your server may return a different status code and different headers.

### Step 5: Record the observations

| Field | Actual observation |
|---|---|
| URL tested | |
| HTTP status code | |
| Server header, if present | |
| Content-Type | |
| Other relevant headers | |
| What does the response establish? | |
| What remains unknown? | |

### Step 6: Understand the limitations

The `Server` header may be absent, modified, or inaccurate.

The presence or absence of a security header should be evaluated in context.

Do not label the application vulnerable solely because a header is missing.

### Step 7: Capture evidence

Suggested filename:

`lab9_06_http_response_headers.png`

### Expected result

Students should be able to retrieve and interpret basic HTTP response headers.

---

## Practical 6 — Inspect an HTTP Response in the Browser

**Objective:** Compare command-line HTTP observations with the response visible in a browser.

### Step 1: Open Firefox in Kali

Launch Firefox.

### Step 2: Enter the target URL

Use the correct URL based on the port identified in the earlier scan.

For example:

```text
http://192.168.56.105/
```

This is an example only.

### Step 3: Observe the response

Record:

- Whether the page loads.
- The page title, if visible.
- The application or landing page shown.
- Any redirects.
- Any error messages.
- Any technology information disclosed by the page.

### Step 4: Compare with the curl results

Consider these questions:

1. Did the browser and curl both reach the service?
2. Did the browser display a page?
3. Did the HTTP status and headers provide additional information?
4. Did the browser redirect to another URL?
5. What information remains unconfirmed?

### Step 5: Capture evidence

Suggested filename:

`lab9_07_browser_service_observation.png`

### Expected result

Students should understand that browser observations and command-line responses provide complementary evidence.

---

## Practical 7 — Identify Web Technologies with WhatWeb

**Objective:** Use WhatWeb to identify technologies that an authorized web application appears to use.

This practical is conditional on WhatWeb being installed and an HTTP service being available.

### Step 1: Check whether WhatWeb is installed

Run:

```bash
command -v whatweb
```

If a path is returned, the command is available.

If it is missing, ask the instructor before installing it.

On Kali Linux, installation may be possible using:

```bash
sudo apt update
sudo apt install whatweb
```

Installation requires permission to modify the lab machine and suitable package access.

### Step 2: Run WhatWeb against the assigned target

If the application is on port 80, run:

```bash
whatweb "http://$TARGET/"
```

If the application is on port 8080, run:

```bash
whatweb "http://$TARGET:8080/"
```

Use only the actual port identified during the authorized lab scan.

### Step 3: Examine the results

WhatWeb may identify technologies such as:

- Web server software.
- Content management systems.
- Application frameworks.
- JavaScript libraries.
- Other recognizable components.

The result depends on what the application exposes.

### Step 4: Record the results

| Technology or indicator | WhatWeb output | Confidence or limitation |
|---|---|---|
| Web server | | |
| Framework or application | | |
| Other technology | | |
| Other observation | | |

Only record technologies that WhatWeb actually reports.

### Step 5: Compare the results

Compare WhatWeb's output with:

- Nmap service detection.
- HTTP response headers.
- Browser observations.

Do the tools appear to agree?

If they disagree, record the difference rather than selecting the result you prefer.

### Step 6: Capture evidence

Suggested filename:

`lab9_08_whatweb_results.png`

### Expected result

Students should learn basic web technology fingerprinting and understand why automated identification requires verification.

---

## Practical 8 — Observe a Service Banner with Netcat

**Objective:** Observe the initial response from a selected TCP service when it provides a readable banner.

This practical is optional and should be performed only against a suitable service on the assigned OWASP VM.

### Step 1: Identify an appropriate port

Review the service inventory.

If TCP port 22 is open and the instructor approves the exercise, students can observe its initial response.

### Step 2: Connect to the service

Run:

```bash
nc -nv -w 3 "$TARGET" 22
```

This attempts a connection to TCP port 22.

The `-w 3` option sets a short timeout for connection or I/O operations, depending on the Netcat implementation.

### Step 3: Observe the output

A service may display a banner identifying its protocol or software.

For example, an SSH service may return a line beginning with:

```text
SSH-2.0-
```

This is an illustrative protocol identifier, not proof of any particular SSH product or version.

### Step 4: Record the result

| Field | Observation |
|---|---|
| Target IP | |
| Port | |
| Connection successful? | |
| Banner received? | |
| Information disclosed | |
| Limitations | |

### Step 5: Exit the connection

If the service remains connected, use `Ctrl+C` to stop the Netcat session.

Do not send arbitrary input or commands to the service.

### Step 6: Capture evidence

Suggested filename:

`lab9_09_service_banner.png`

### Expected result

Students should understand that some services disclose protocol or software information through banners, while others do not.

---

## Practical 9 — Save Enumeration Results

**Objective:** Preserve evidence from service enumeration so the results can be reviewed and reported.

### Step 1: Open the Lab 9 directory

Run:

```bash
cd ~/ICH-EH102/Lab9
```

### Step 2: Save Nmap service-detection results

Use the actual authorized target and the relevant ports identified earlier.

For example:

```bash
nmap -sV -p 22,80,8080 -oN service-detection.txt "$TARGET"
```

Adjust the port list to match your actual results.

### Step 3: Save the HTTP header results

If the target serves HTTP on port 80, run:

```bash
curl -I "http://$TARGET/" -o http-headers.txt
```

If the service is on port 8080, run:

```bash
curl -I "http://$TARGET:8080/" -o http-headers.txt
```

Use the correct port for your target.

If the server does not support HEAD requests, record that limitation and use the browser observation or an instructor-approved alternative.

### Step 4: Read the saved results

Run:

```bash
cat service-detection.txt
```

Then:

```bash
cat http-headers.txt
```

### Step 5: Confirm the files exist

Run:

```bash
ls -l ~/ICH-EH102/Lab9
```

### Step 6: Review the evidence

Check that the saved results match the actual observations.

Do not alter the output to remove unexpected findings or create results that were never observed.

### Step 7: Capture evidence

Suggested filenames:

- `lab9_10_saved_service_detection.png`
- `lab9_11_saved_http_headers.png`

### Expected result

Students should be able to preserve and retrieve service-enumeration evidence.

---

# PART 4: ANALYSIS AND INTERPRETATION

## 4.1 What Should Students Understand?

By the end of the practical session, students should be able to explain:

1. Which selected ports were reported open.
2. Which services Nmap identified.
3. Whether product or version information was disclosed.
4. What information the HTTP headers revealed.
5. Whether WhatWeb identified any web technologies.
6. Whether a service returned a readable banner.
7. Where the tools agreed or disagreed.
8. What evidence supports each observation.
9. What remains uncertain.
10. Which observations warrant further investigation.

## 4.2 Understanding Information Disclosure

Information disclosure occurs when a system reveals information that should not be available to a particular audience or that creates unnecessary security risk.

Examples that may warrant investigation include:

- Unnecessary software version disclosure.
- Internal system details in error messages.
- Debug information visible to unauthorized users.
- Sensitive configuration information returned in responses.
- Unintended exposure of administrative interfaces.

Not every disclosed detail is a vulnerability.

The tester must determine what information was exposed, who could access it, whether the disclosure matters to the threat model, and whether there is supporting evidence of security impact.

## 4.3 Distinguishing an Observation from a Finding

An observation is something the tester directly saw or measured.

A finding is a documented result that includes interpretation and supporting evidence.

For example:

**Observation:** Nmap reported TCP port 80 as open.

**Interpretation:** A service accepted TCP connections on port 80 during the scan.

**Limitation:** The result does not identify the service conclusively or prove that it is vulnerable.

**Follow-up:** Investigate the service and its responses in more detail.

A strong report keeps observed facts separate from assumptions.

## 4.4 Example Service Inventory

The following is a blank template. Students must complete it with their actual results.

| Port | State | Identified service | Version | Additional observations |
|---|---|---|---|---|
| | | | | |
| | | | | |
| | | | | |
| | | | | |

## 4.5 Example Finding

**Finding ID:** SE-01

**Title:** Web service identified on an exposed TCP port

**Observation:** Nmap reported TCP port 80 as open and identified an HTTP service.

**Evidence:** The saved Nmap service-detection output contains the port and service information.

**Interpretation:** A service accepting TCP connections on port 80 was detected. Further inspection can help establish what application is responding.

**Limitation:** The scan does not establish whether the application contains a vulnerability.

**Recommended follow-up:** Review the service response, identify the application where possible, and determine whether any additional security assessment is justified.

Students must replace this example with a finding based on their own evidence.

## 4.6 Recommended Lab Report Structure

Students should submit a report titled:

**Lab 9: Service Enumeration**

Include:

1. Introduction.
2. Objectives.
3. Lab environment.
4. Scope and authorization.
5. Tools used.
6. Target identification.
7. Initial port inventory.
8. Nmap service-detection results.
9. HTTP response-header observations.
10. Web technology identification, if performed.
11. Service banner observations, if performed.
12. Evidence screenshots.
13. Service inventory table.
14. Findings and interpretation.
15. Limitations.
16. Conclusion.

## 4.7 Evidence Requirements

Students should submit:

- Screenshots showing the relevant commands and output.
- Saved Nmap service-detection results.
- HTTP response-header results, if collected.
- WhatWeb results, if available.
- A completed service inventory.
- Findings supported by actual evidence.

Evidence must be readable and must not contain unnecessary passwords, session tokens, or unrelated sensitive information.

---

# PART 5: TROUBLESHOOTING

## 5.1 Nmap Does Not Identify the Service Version

Possible reasons include:

- The service does not disclose version information.
- The service uses a nonstandard protocol or port.
- The target is filtering probes.
- The service returned insufficient information.
- The detected service is not one Nmap can identify confidently.

Recommended actions:

1. Confirm that the port is open.
2. Check that the selected target is correct.
3. Review the actual output.
4. Record the service as unidentified or partially identified.
5. Do not invent a product name or version.

An unidentified service is a valid result.

## 5.2 curl Cannot Connect

Possible causes include:

- The target is unreachable.
- The selected port is incorrect.
- The service is not running.
- The service uses HTTPS instead of HTTP.
- The network is filtering the connection.

Confirm the actual target and port before retrying.

If HTTPS is indicated by the service evidence, the instructor may demonstrate an appropriate HTTPS request against the assigned target.

## 5.3 The Server Does Not Return a Server Header

Some servers do not disclose their software identity in the `Server` header.

Others return generic or modified values.

Record the header as absent or inconclusive.

Do not conclude that the server is secure or insecure solely because the header is missing.

## 5.4 WhatWeb Returns Limited Information

WhatWeb relies on observable indicators and its detection capabilities.

A website may reveal very little information.

Check that the URL and port are correct, and compare the results with the Nmap output and browser observations.

Do not treat limited fingerprinting results as a failed lab.

## 5.5 Netcat Does Not Display a Banner

Some services do not send a banner immediately.

Others expect a protocol-specific request before returning a response.

If no banner appears, record that observation and stop the connection.

Do not send arbitrary commands to force a response.

## 5.6 Different Tools Produce Different Results

Different tools use different methods and may observe different aspects of a service.

For example, Nmap may identify an HTTP service, while WhatWeb identifies a possible framework and the HTTP response contains a generic server header.

These results are not necessarily contradictory.

Record what each tool reported and explain the limitations of each result.

---

# PART 6: REVIEW QUESTIONS

Answer in your own words.

1. Define service enumeration.
2. Why is service enumeration important in ethical hacking?
3. Explain the difference between port scanning and service enumeration.
4. What is a network service?
5. What is a service banner?
6. What is service version detection?
7. What does the `-sV` option do in Nmap?
8. Why should a tester investigate an open port further?
9. What is HTTP?
10. What is HTTPS?
11. What is TLS?
12. What are HTTP response headers?
13. What information might the `Server` header disclose?
14. What does the HTTP status code 200 mean?
15. What is the difference between HTTP status codes 401 and 403?
16. What is WhatWeb used for?
17. What is Netcat?
18. Why might a service fail to disclose a banner?
19. What is service exposure?
20. Why is an exposed service not automatically a vulnerability?
21. Why can a detected software version be insufficient to prove that a vulnerability exists?
22. What should a tester do if Nmap cannot identify a service?
23. Why should service-enumeration output be saved?
24. What is the difference between an observation and a finding?
25. What evidence should be included in a service-enumeration report?
26. Why might different enumeration tools return different results?
27. What is information disclosure?
28. Give two examples of service information that may warrant security review.
29. Explain why authorization is necessary before enumerating a system.
30. How does service enumeration prepare a tester for vulnerability assessment?

---

# PART 7: TAKE-HOME ASSIGNMENT

## Assignment Title: Service Enumeration and Exposure Assessment

**Total Marks: 20**

### Scenario

You are a junior penetration tester assigned to examine services exposed by an intentionally vulnerable virtual machine in a controlled training environment.

Your task is to identify the services that can be observed, record information disclosed by those services, and document areas requiring further investigation.

You are not authorized to exploit vulnerabilities, attempt password guessing, or scan unrelated systems.

### Task 1 — Identify the target

Record:

- Testing machine.
- Target machine.
- Actual target IP address.
- Network configuration.
- Authorized scope.

### Task 2 — Perform service detection

Use Nmap to identify the services associated with selected open ports.

Record the command and the actual output.

Explain which services were identified and which remained uncertain.

### Task 3 — Investigate a web service

If an HTTP service is available, collect its response headers.

Record the status code, relevant headers, and any disclosed technology information.

If no suitable web service is available, document the limitation.

### Task 4 — Perform web technology identification

If WhatWeb is installed and an authorized web service is available, run the tool against the assigned target.

Record the actual results.

Explain why automated technology identification may require verification.

### Task 5 — Record service information

Complete a service inventory containing:

- Port.
- State.
- Identified service.
- Product or version, if reported.
- Additional observations.
- Limitations.

### Task 6 — Document findings

Prepare at least three evidence-supported findings where the available results permit.

Each finding must include:

- Finding ID.
- Title.
- Observation.
- Evidence.
- Interpretation.
- Limitation.
- Recommended follow-up.

Do not invent findings to meet the requested number. If the target provides insufficient evidence for three distinct findings, explain this and submit all supported findings.

### Task 7 — Write a conclusion

Explain:

1. Which services were identified.
2. What information the services disclosed.
3. Which observations require further investigation.
4. What the evidence does not prove.
5. Why authorization and accurate reporting matter.

### Submission requirements

Submit a report containing:

- Introduction.
- Objectives.
- Lab environment and scope.
- Tools and commands used.
- Actual service-detection results.
- HTTP or web technology observations, if applicable.
- Service inventory.
- Evidence screenshots.
- Findings and recommendations.
- Limitations.
- Conclusion.

Do not submit example outputs from these notes as if they were generated by your own testing.

### Marking scheme

| Assessment area | Marks |
|---|---:|
| Lab environment and authorization | 2 |
| Correct use of service detection | 4 |
| Service inventory and interpretation | 4 |
| HTTP or web technology investigation | 3 |
| Evidence-supported findings | 4 |
| Report quality and ethical compliance | 3 |
| **Total** | **20** |

---

# PART 9: CONCLUSION

Service enumeration helps ethical hackers understand what network services are exposed and what information those services disclose.

In this lab, students learned how to identify services and versions with Nmap, inspect HTTP response headers with curl, examine web technologies with WhatWeb, and observe service banners where available.

They also learned that enumeration results have limitations. A reported service version may be incomplete or inaccurate, an HTTP header may not reveal the actual software, and an exposed service does not automatically indicate a vulnerability.

The goal is to collect reliable evidence, interpret it carefully, and identify areas that warrant further investigation.

The next lab, **Lab 10: Vulnerability Assessment**, will build on the service inventory created in this lesson. Students will learn how to investigate potential weaknesses, assess their relevance, and distinguish confirmed findings from unverified indicators.

**End of Lab 9 — Service Enumeration**

**Iconic Hub — Secure. Build. Innovate.**
