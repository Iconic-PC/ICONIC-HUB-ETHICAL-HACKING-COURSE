# ICONIC HUB — ETHICAL HACKING COURSE

## NETWORKING FUNDAMENTALS, NMAP SCANNING AND DVWA INSTALLATION

**Course:** Ethical Hacking  
**Level:** Beginner  
**Practical Environment:** Kali Linux and a separate Linux virtual machine  
**Main Tools:** Linux networking commands, Nmap, Docker and DVWA  
**Brand:** Iconic Hub — Secure. Build. Innovate.

---

# 1. INTRODUCTION

Before an ethical hacker can test a computer or web application, they need to understand how computers communicate over a network.

An ethical hacker should understand:

- What a network is.
- What an IP address is.
- The difference between a private and public IP address.
- What a port is.
- How computers communicate using network protocols.
- How to identify network information using Linux commands.
- How to use Nmap to discover hosts, ports and services.
- How to install and access DVWA in a controlled laboratory.
- How to document findings from a security scan.

In this lesson, students will learn these concepts and practise them using their own virtual machines.

**Important rule:** Only scan systems you own or have explicit permission to test.

# 2. LEARNING OBJECTIVES

By the end of this lesson, students should be able to:

1. Explain basic networking concepts.
2. Identify a machine's IP address and default gateway.
3. Explain the difference between an IP address and a port.
4. Use basic Linux networking commands.
5. Explain what Nmap does.
6. Install and run DVWA in a lab environment.
7. Perform several types of Nmap scans against the lab target.
8. Identify open ports and services.
9. Save scan results for documentation.
10. Explain why Nmap results do not automatically prove that a vulnerability exists.

# 3. NETWORKING FUNDAMENTALS

## 3.1 What Is a Computer Network?

A computer network is a group of connected devices that communicate and share information.

Examples include:

- Computers connected to a school network.
- Phones connected to a Wi-Fi router.
- Computers communicating inside a bank.
- Web servers communicating with users over the internet.

Networks allow devices to exchange data, share resources and access services.

## 3.2 What Is an IP Address?

An IP address is a numerical address used to identify a network interface and help route data across a network.

For example:

`192.168.56.105`

This is an example of an IPv4 address commonly used in private networks.

Think of an IP address as the address of a building. It helps you identify where to send information.

A computer can have more than one IP address because it may have multiple network interfaces.

For example, a Kali VM may have:

- An address for internet access.
- A separate address for communication with another VM.
- A loopback address used by the computer to communicate with itself.

## 3.3 Types of IP Addresses

### Private IP addresses

Private IP addresses are used inside private networks, such as home networks, office networks and isolated virtual-machine laboratories.

The three main private IPv4 ranges are:

| Range | Example |
|---|---|
| `10.0.0.0/8` | `10.0.0.10` |
| `172.16.0.0/12` | `172.16.5.10` |
| `192.168.0.0/16` | `192.168.56.105` |

A private IP address is not directly routable across the public internet.

### Public IP addresses

A public IP address is used for internet routing. Internet-facing servers and routers may have public IP addresses.

Do not scan a public IP address just because it is visible online. Obtain authorization first.

### Loopback address

The IPv4 loopback address is:

`127.0.0.1`

It refers to the local computer.

If you run a command against `127.0.0.1` on Kali, you are testing Kali itself, not your DVWA target.

## 3.4 What Is a Subnet?

A subnet is a smaller logical network within a larger IP network.

Subnetting helps organize networks and determine which devices can communicate directly at the IP level.

For example, consider:

- Kali: `192.168.56.101`
- DVWA target: `192.168.56.105`

If both use the subnet mask `255.255.255.0`, they belong to the same `192.168.56.0/24` subnet.

The `/24` is called **CIDR prefix notation**. It means that the first 24 bits identify the network portion of the IPv4 address.

For this example:

- Network address: `192.168.56.0`
- Typical usable host addresses: `192.168.56.1` to `192.168.56.254`
- Broadcast address: `192.168.56.255`

These examples assume a conventional `/24` IPv4 subnet.

You do not need to master subnet calculations before starting Nmap. For now, learn how to identify your IP address, subnet prefix and target address.

## 3.5 What Is a MAC Address?

A Media Access Control (MAC) address identifies a network interface at the data-link layer.

Example:

`08:00:27:12:34:56`

MAC addresses are commonly used for communication within a local network segment.

An IP address and a MAC address are not the same thing:

- IP address: used for IP-level addressing and routing.
- MAC address: used for local network-frame delivery.

## 3.6 What Is a Port?

A port is a numbered communication endpoint used by network services.

Think of an IP address as a building and a port as a particular door or service entrance inside the building.

Common examples include:

| Port | Common service |
|---|---|
| 22 | SSH |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 3306 | MySQL |
| 4280 | The host port used by our DVWA lab configuration |

Port numbers range from `0` to `65535`.

A port number alone does not guarantee which service is running. Nmap can help identify the service, but the result should be verified.

## 3.7 What Is a Protocol?

A protocol is a set of rules that devices follow when communicating.

Common examples:

- **TCP:** Provides a connection-oriented transport service.
- **UDP:** Provides a connectionless transport service.
- **HTTP:** Used for web communication.
- **HTTPS:** HTTP protected by TLS.
- **DNS:** Translates domain names into records such as IP addresses.
- **SSH:** Provides secure remote access.
- **ICMP:** Supports network-control and diagnostic messages.

# 4. BASIC LINUX NETWORKING COMMANDS

Run these commands in the Kali Linux terminal.

## 4.1 Display IP Addresses

```bash
ip addr
```

A shorter version is:

```bash
ip -br addr
```

The second command displays network interfaces and addresses in a compact format.

Look for interfaces such as `eth0`, `enp0s3` or `wlan0`. The actual interface name depends on the system.

To display IPv4 addresses only:

```bash
ip -4 addr
```

**Practical:** Record Kali's IP address and identify the interface connected to your lab network.

## 4.2 Display the Routing Table

```bash
ip route
```

This command shows routing information.

A line beginning with `default via` identifies the default gateway used for destinations that do not match a more specific route.

The default gateway is usually a router or another device that forwards traffic to other networks.

## 4.3 Test Connectivity with Ping

```bash
ping -c 4 127.0.0.1
```

This tests communication with the local machine.

To test your DVWA target, substitute its actual IP address:

```bash
ping -c 4 TARGET_IP
```

Replace `TARGET_IP` with the target's real address.

If ping fails, the machine might still be online. Firewalls, network configuration and ICMP filtering can prevent ping responses.

## 4.4 Trace a Network Route

```bash
traceroute TARGET_IP
```

If `traceroute` is not installed, Kali may require:

```bash
sudo apt update
sudo apt install -y traceroute
```

This command attempts to show the network hops toward a destination. A local VM-to-VM connection may show only a small number of hops.

## 4.5 Look Up a Domain Name

```bash
nslookup example.com
```

This asks a DNS resolver to look up a domain.

The example is for learning DNS lookup syntax. It is not permission to scan that website.

## 4.6 Display Network Connections

```bash
ss -tuln
```

This displays listening TCP and UDP sockets.

Common options:

- `-t`: TCP.
- `-u`: UDP.
- `-l`: Listening sockets.
- `-n`: Show numerical addresses and port numbers.

To include the process associated with a socket where permissions allow:

```bash
sudo ss -tulpn
```

# 5. INTRODUCTION TO NMAP

## 5.1 What Is Nmap?

Nmap stands for **Network Mapper**.

It is a network discovery and security-auditing tool used to identify hosts, examine ports and help determine which services are available.

Security professionals use Nmap to:

- Discover authorized lab hosts.
- Identify open, closed and filtered ports.
- Detect services and possible versions.
- Perform operating-system detection where supported.
- Record scan results.
- Support authorized security assessments.

Nmap does not automatically exploit a system, and an open port does not automatically mean that the system is vulnerable.

## 5.2 Install Nmap on Kali Linux

Kali usually includes Nmap. Check first:

```bash
nmap --version
```

If it is missing:

```bash
sudo apt update
sudo apt install -y nmap
```

Confirm the installation:

```bash
nmap --version
```

## 5.3 Understand Nmap's Basic Syntax

The general format is:

```bash
nmap [OPTIONS] TARGET
```

- `nmap`: Starts the tool.
- `OPTIONS`: Specify the type of scan or information to collect.
- `TARGET`: The authorized IP address, hostname or network range.

Example:

```bash
nmap 192.168.56.105
```

This scans the target using Nmap's default TCP port-scanning behavior.

**Important:** `192.168.56.105` is an example. Use the actual IP address of your DVWA machine.

# 6. INSTALLING DVWA IN A LAB

## 6.1 What Is DVWA?

DVWA means **Damn Vulnerable Web Application**.

It is an intentionally vulnerable web application created for security training. It lets students learn how web application vulnerabilities work in a controlled environment.

DVWA can help demonstrate concepts such as:

- SQL injection.
- Cross-site scripting (XSS).
- Command injection.
- File inclusion.
- File upload security weaknesses.
- Authentication and session-management weaknesses.

DVWA is deliberately insecure. Do not expose it to the public internet or install it on a production server.

## 6.2 What Is OWASP?

OWASP stands for the **Open Worldwide Application Security Project**.

It is a nonprofit organization that provides resources, projects and guidance for improving application security.

The OWASP Top 10 is a widely used awareness resource describing important categories of web application security risks.

OWASP is not a single target machine with one IP address. It is an organization and security project ecosystem.

For this class, we will use DVWA as our practice target and OWASP resources to guide our understanding of web application security.

## 6.3 Lab Requirements

You need:

1. A computer capable of running virtual machines.
2. Kali Linux VM for testing.
3. A separate Linux VM for DVWA.
4. VirtualBox or another virtualization platform.
5. Internet access on the target VM during installation.
6. Enough disk space for Docker images and the lab.
7. An isolated virtual network connecting the two VMs.

A separate target VM is recommended because it helps students distinguish the testing machine from the target.

## 6.4 Step 1 — Configure the Virtual Machines

In VirtualBox:

1. Shut down both VMs.
2. Open the settings for the Kali VM.
3. Open **Network**.
4. Configure one adapter for a **Host-only Adapter**.
5. Select the same host-only network for the target Linux VM.
6. Start both VMs.
7. Confirm that each VM receives an address on that network.

Host-only networking allows the VMs to communicate with the host and other VMs on that host-only network, without directly exposing the lab to the public internet.

If the target needs internet access to download Docker and DVWA, temporarily add a second NAT adapter or use another controlled internet connection. Keep the lab-facing adapter on the same host-only network as Kali.

Do not rely on an internet-facing bridged network for this vulnerable application.

## 6.5 Step 2 — Install Docker on the Target VM

Run these commands on the separate Linux VM that will host DVWA, not on Kali.

For a Debian- or Ubuntu-based target, begin with:

```bash
sudo apt update
sudo apt install -y docker.io docker-compose git
```

Package names and versions may vary by Linux distribution. If Docker Compose is unavailable through the distribution packages, follow the official Docker installation instructions for your operating system.

Enable and start Docker:

```bash
sudo systemctl enable --now docker
```

Check its status:

```bash
sudo systemctl status docker
```

Check the installed Docker version:

```bash
sudo docker --version
```

Check which Compose command is available:

```bash
sudo docker compose version
```

If that command is unavailable, try:

```bash
sudo docker-compose --version
```

Use the Compose command that works on your system in the following steps.

## 6.6 Step 3 — Download DVWA

On the target VM, run:

```bash
git clone https://github.com/digininja/DVWA.git
```

Enter the downloaded directory:

```bash
cd DVWA
```

List the files:

```bash
ls -la
```

You should find the project's files, including its Docker Compose configuration.

If the clone fails, check your internet connection and try again.

## 6.7 Step 4 — Configure the DVWA Port

The official Docker Compose configuration may bind the web service to the local loopback interface, using a mapping similar to:

```yaml
127.0.0.1:4280:80
```

That configuration is accessible from the target VM itself but is not normally reachable directly from Kali over the network.

For the isolated host-only lab, edit the Compose file and change that mapping to:

```yaml
4280:80
```

This maps TCP port `4280` on the target VM to port `80` inside the DVWA container.

To edit the file, you can use a terminal editor such as:

```bash
nano compose.yml
```

If the configuration file has a different name, list the files and open the actual Compose file provided by the repository.

Find the existing port mapping and change only the relevant mapping. Save and exit the editor.

**Security warning:** Binding the port to all interfaces can make DVWA reachable from every network interface on the target. Only do this when the target is isolated on your controlled lab network. Never forward this port from your router or expose DVWA to the public internet.

## 6.8 Step 5 — Start DVWA

If the Docker Compose plugin is installed, run:

```bash
sudo docker compose up -d
```

If your system uses the standalone Compose command, run:

```bash
sudo docker-compose up -d
```

Check the container status:

```bash
sudo docker compose ps
```

If you are using standalone Compose, substitute `docker-compose` for `docker compose`.

If the container fails to start, inspect its logs:

```bash
sudo docker compose logs
```

The exact service names and container status depend on the current Compose configuration.

## 6.9 Step 6 — Open DVWA on the Target VM

Open a browser on the target Linux VM and visit:

```text
http://127.0.0.1:4280
```

Follow the DVWA setup page to initialize its database, using the installation instructions provided by the project.

The standard default training login is commonly:

- Username: `admin`
- Password: `password`

If the login does not work, check the setup page and current project documentation instead of repeatedly guessing credentials.

Once setup is complete, sign in and confirm that the DVWA home page loads.

## 6.10 Step 7 — Find the Target VM's IP Address

On the DVWA target VM, run:

```bash
ip -4 addr
```

Or:

```bash
ip -br addr
```

Identify the IPv4 address assigned to the host-only network interface.

For example, the target might have:

```text
192.168.56.105
```

This is only an example address. Your actual address may be different.

Write down the real address. We will refer to it as `TARGET_IP`.

## 6.11 Step 8 — Test DVWA from Kali

On Kali, check Kali's own network configuration:

```bash
ip -br addr
```

Then test connectivity to the target:

```bash
ping -c 4 TARGET_IP
```

Replace `TARGET_IP` with the actual target address.

Open Firefox on Kali and visit:

```text
http://TARGET_IP:4280
```

For example, if the target address really is `192.168.56.105`, visit:

```text
http://192.168.56.105:4280
```

If DVWA loads from Kali, the basic lab network is working.

If it does not load, check the following:

- Both VMs are connected to the same host-only network.
- You used the target VM's IP address, not Kali's address.
- The DVWA container is running.
- The port mapping is `4280:80`.
- The target firewall allows the lab connection.
- The browser URL uses the correct port.

# 7. PRACTICAL NMAP SCANNING

In the following commands, replace `TARGET_IP` with the actual IP address of the DVWA host.

For a concrete example only, assume the target is `192.168.56.105`.

## Scan 1 — Basic TCP Port Scan

```bash
nmap TARGET_IP
```

Example:

```bash
nmap 192.168.56.105
```

**Purpose:** Check Nmap's default selection of TCP ports on the target.

Students should record:

- The target IP address.
- The ports reported.
- Their reported states.
- The service names Nmap associates with those ports.

## Scan 2 — Check Whether a Host Is Up

```bash
nmap -sn TARGET_IP
```

Example:

```bash
nmap -sn 192.168.56.105
```

The `-sn` option performs host discovery without a port scan.

It helps determine whether Nmap can detect the target as reachable. Firewalls and host-discovery behavior can affect the result.

## Scan 3 — Scan the DVWA Web Port

```bash
nmap -p 4280 TARGET_IP
```

Example:

```bash
nmap -p 4280 192.168.56.105
```

**Purpose:** Check the specific host port mapped to DVWA's web service.

Possible states include:

- **Open:** A service is accepting connections on that port.
- **Closed:** The host is reachable, but no service is listening on that port.
- **Filtered:** Nmap cannot determine whether the port is open because filtering or network behavior prevents a clear result.

If port `4280` is open, that is consistent with the expected DVWA setup, but it does not prove that every part of the application is functioning correctly.

## Scan 4 — Detect Services and Versions

```bash
nmap -sV -p 4280 TARGET_IP
```

Example:

```bash
nmap -sV -p 4280 192.168.56.105
```

The `-sV` option attempts to identify the service and its version.

This can help a security tester understand what is exposed. Version detection may not identify the exact version, so verify important findings against the application or system itself.

## Scan 5 — Scan All TCP Ports

```bash
nmap -p- TARGET_IP
```

Example:

```bash
nmap -p- 192.168.56.105
```

The `-p-` option scans all TCP port numbers from `1` to `65535`.

This takes longer than checking a small selection of ports.

To combine a full TCP port scan with service detection:

```bash
nmap -sV -p- TARGET_IP
```

Use this against your own lab target and allow the scan to finish.

## Scan 6 — TCP Connect Scan

```bash
nmap -sT -p 4280 TARGET_IP
```

The `-sT` option uses the operating system's TCP connect mechanism to test the selected port.

This is useful when a raw-packet SYN scan is not available or when you want to learn about TCP connection-based scanning.

## Scan 7 — SYN Scan

```bash
sudo nmap -sS -p 4280 TARGET_IP
```

The `-sS` option requests a TCP SYN scan. It typically requires elevated privileges.

This scan examines TCP connection responses without normally completing a full TCP connection to each open port.

A SYN scan is not automatically undetectable. Network monitoring and host security tools may still record or detect it.

## Scan 8 — Operating-System Detection

```bash
sudo nmap -O TARGET_IP
```

The `-O` option attempts to estimate the target's operating system using network responses.

Results are estimates, not guaranteed facts. Virtualization, firewalls and limited responses can reduce accuracy.

For a containerized DVWA installation, the exposed network behavior may not reveal the container's operating system accurately. Nmap is scanning the reachable host and service, not magically identifying every detail inside the container.

## Scan 9 — Detect Services and Attempt OS Detection

```bash
sudo nmap -sV -O TARGET_IP
```

This combines service/version detection with operating-system detection.

Students should compare the results and note which details are identified confidently and which remain uncertain.

## Scan 10 — Common TCP Ports

```bash
nmap --top-ports 100 TARGET_IP
```

This scans the 100 most common TCP ports in Nmap's port-frequency database.

It is useful for a quicker initial assessment, but it can miss services running on less common ports.

## Scan 11 — Scan Specific Ports

```bash
nmap -p 22,80,443,4280 TARGET_IP
```

This checks only the listed TCP ports.

The example includes common SSH and web ports as well as the port used in our DVWA lab.

If a port is closed, that is not necessarily a problem. The result depends on which services the target is intended to provide.

## Scan 12 — Save the Scan Results

```bash
nmap -sV -p 4280 -oN dvwa-scan.txt TARGET_IP
```

The `-oN` option saves results in normal text format.

Read the saved file with:

```bash
cat dvwa-scan.txt
```

Keeping scan output is important for evidence, reporting and comparison between scans.

## Scan 13 — Save Results in XML Format

```bash
nmap -sV -p 4280 -oX dvwa-scan.xml TARGET_IP
```

XML output is useful when scan results need to be processed by compatible reporting tools.

## Scan 14 — Combine Multiple Scan Options

```bash
sudo nmap -sS -sV -O -p 4280 TARGET_IP
```

This combines:

- SYN scanning.
- Service/version detection.
- Operating-system detection.
- A scan of the selected port.

A combined scan may provide more information, but not every option is necessary for every task. Students should understand each option instead of copying long commands without knowing what they do.

# 8. HOW TO INTERPRET NMAP RESULTS

A scan may produce output resembling the following:

```text
PORT     STATE SERVICE
4280/tcp open  http
```

This is an illustrative example, not a guaranteed scan result.

Interpretation:

- `4280/tcp`: TCP port 4280.
- `open`: A service appears to be accepting connections.
- `http`: Nmap identifies the service as HTTP.

If you see:

```text
4280/tcp closed
```

The target may be reachable, but the port is not accepting connections at the time of the scan.

If you see:

```text
4280/tcp filtered
```

A firewall or other network behavior may be preventing Nmap from determining the port's state.

Do not conclude that a target is secure just because Nmap reports no open ports. A scan can miss services because of filtering, scan limitations, network conditions or the chosen ports.

# 9. NMAP SCANNING DOES NOT EQUAL WEB APPLICATION TESTING

This distinction is important.

Nmap can help identify the exposed port and web service on the DVWA host. It does not, by itself, prove that DVWA has SQL injection, XSS or another application vulnerability.

A typical authorized workflow is:

1. Identify the target's IP address.
2. Confirm that the target is reachable.
3. Scan the relevant ports.
4. Identify the exposed services.
5. Open DVWA in a browser.
6. Perform application-security exercises within the lab.
7. Record the evidence and explain the findings.

For the application-security part of the course, students should use DVWA's intentionally vulnerable features and the relevant OWASP guidance.

# 10. TROUBLESHOOTING

## Problem 1: DVWA Does Not Load

Check the container status on the target:

```bash
sudo docker compose ps
```

Check the logs:

```bash
sudo docker compose logs
```

If using standalone Compose, replace `docker compose` with `docker-compose`.

Confirm that the target port is listening:

```bash
sudo ss -tuln
```

## Problem 2: Kali Cannot Reach the Target

On both VMs, run:

```bash
ip -br addr
```

Confirm that both VMs have addresses on the same host-only network.

Test the target from Kali:

```bash
ping -c 4 TARGET_IP
```

Remember that ping can fail even when a target is online.

## Problem 3: Nmap Reports the Port as Closed

Check:

1. Whether the DVWA container is running.
2. Whether the port mapping is correct.
3. Whether the target IP address is correct.
4. Whether the target service is listening.
5. Whether a firewall is blocking the connection.

## Problem 4: Nmap Reports the Port as Filtered

Check the host firewall and VirtualBox network configuration.

Do not disable every firewall as a first response. Understand which rule is blocking the connection and make only the changes needed for the isolated lab.

## Problem 5: Docker Compose Command Does Not Work

Try:

```bash
sudo docker compose version
```

If unavailable, try:

```bash
sudo docker-compose --version
```

Use the installed version's command syntax. If neither is available, consult the official Docker installation instructions for your Linux distribution.

# 11. PRACTICAL LAB ASSIGNMENT

## Title: Network Discovery and Service Identification Using Nmap

**Objective:** Identify the reachable DVWA target, examine its exposed web port and document the results.

### Part A — Network Identification

1. Display the Kali IP address.
2. Display the DVWA target IP address.
3. Record both addresses.
4. Explain which machine is the tester and which is the target.
5. Confirm that both machines use the intended lab network.

### Part B — Nmap Scanning

Run the following commands against your authorized DVWA target:

```bash
nmap -sn TARGET_IP
```

```bash
nmap TARGET_IP
```

```bash
nmap -p 4280 TARGET_IP
```

```bash
nmap -sV -p 4280 TARGET_IP
```

```bash
nmap -p- TARGET_IP
```

```bash
nmap -sV -p 4280 -oN dvwa-scan.txt TARGET_IP
```

Replace `TARGET_IP` with the actual target IP address in every command.

### Part C — Analysis

Answer the following questions:

1. What is an IP address?
2. What is a port?
3. What is the difference between an IP address and a port?
4. What does Nmap do?
5. What does the `-sn` option do?
6. What does the `-sV` option do?
7. What does the `-p-` option do?
8. What is the difference between an open port and a filtered port?
9. Why did we scan the DVWA host instead of scanning OWASP as an organization?
10. Why does an open HTTP port not automatically prove a web application vulnerability?
11. Why should DVWA not be exposed to the public internet?
12. What information did you learn from your scan?

### Part D — Evidence

Capture screenshots showing:

- The IP address of Kali.
- The IP address of the DVWA target.
- DVWA loading in a browser.
- The basic Nmap scan.
- The scan of port 4280.
- The service/version detection result.
- The saved scan output.

Ensure that screenshots show the actual output from your own lab. Do not fabricate results.

# 12. REPORT TEMPLATE

Students should submit a short report using the following structure.

## A. Title

Network Discovery and Service Identification Using Nmap

## B. Objective

State what you intended to discover about your lab target.

## C. Lab Environment

Record:

- Testing machine: Kali Linux.
- Target machine: Linux VM running DVWA.
- Testing tool: Nmap.
- Target IP address: [insert actual address].
- Target port: 4280/TCP.

## D. Methodology

List the commands you ran and briefly explain what each command was intended to do.

## E. Findings

Use a table such as:

| Test | Result | Interpretation |
|---|---|---|
| Host discovery | [Your result] | [Your explanation] |
| Basic port scan | [Your result] | [Your explanation] |
| Port 4280 scan | [Your result] | [Your explanation] |
| Service detection | [Your result] | [Your explanation] |
| Full TCP port scan | [Your result] | [Your explanation] |

## F. Evidence

Insert screenshots of your real lab results.

## G. Conclusion

Summarize what you learned about IP addresses, ports, services and Nmap. Distinguish between what the scan confirmed and what remains unverified.

# 13. REVIEW QUESTIONS AND ANSWERS

**1. What is Nmap?**

Nmap is a network discovery and security-auditing tool used to identify hosts, examine ports and help detect network services.

**2. What is DVWA?**

DVWA is an intentionally vulnerable web application used for learning and practising web application security in a controlled environment.

**3. What is OWASP?**

OWASP is the Open Worldwide Application Security Project, which provides resources and projects for improving application security.

**4. Which IP address should we scan in this practical?**

The actual IP address of the Linux VM hosting DVWA, using a network connection that Kali can reach.

**5. What is the purpose of port 4280 in this lab?**

It is the host port used by the selected DVWA Docker configuration to expose the web application.

**6. What does `nmap -sV TARGET_IP` do?**

It attempts to identify the services and their versions on the target's scanned ports.

**7. What does `nmap -p- TARGET_IP` do?**

It scans all TCP ports from 1 through 65535.

**8. Does an open port automatically mean a vulnerability exists?**

No. An open port indicates that a service is accepting connections. Further investigation is needed to assess its security.

**9. Why should students use an isolated lab network?**

It limits unintended exposure and helps keep deliberately vulnerable software away from public and production networks.

**10. What is the difference between Nmap and DVWA?**

Nmap is a network scanning tool. DVWA is a deliberately vulnerable web application used as a training target.

# 14. KEY TAKEAWAYS

By completing this lesson, students should understand that:

- Networking knowledge is fundamental to ethical hacking.
- IP addresses identify network interfaces and help direct traffic.
- Ports identify communication endpoints used by services.
- Linux commands can reveal network configuration and connectivity.
- Nmap helps identify reachable hosts, ports and services.
- DVWA provides a controlled environment for web application security exercises.
- OWASP provides security guidance; it is not a single machine to scan.
- An open port does not automatically mean a vulnerability exists.
- Scan findings must be verified and documented accurately.
- All testing must be authorized and performed within defined scope.

# 15. CONCLUSION

Networking is one of the foundations of ethical hacking. Before testing a service, an ethical hacker must know where the target is, how to reach it and which services are exposed.

In this lesson, students learn basic networking commands, install DVWA in an isolated virtual environment and use Nmap to examine the target's network exposure.

These skills prepare students for later lessons involving web application testing, vulnerability assessment, evidence collection and security reporting.

**Iconic Hub — Secure. Build. Innovate.**
