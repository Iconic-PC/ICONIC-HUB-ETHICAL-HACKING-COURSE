# RECONNAISSANCE AND ENUMERATION COMMANDS

## 1. Network Reconnaissance

### `ip addr`
```bash
ip addr
```
**What it does:** Displays all network interfaces and their assigned IP addresses.

**Expected result:** Interface names, IPv4/IPv6 addresses, MAC addresses and interface status.

### `ip -br addr`
```bash
ip -br addr
```
**What it does:** Displays network interfaces and IP addresses in a simplified format.

**Expected result:**
```text
lo      UNKNOWN    127.0.0.1/8
eth0    UP         10.0.2.15/24
eth1    UP         192.168.56.106/24
```

### `ip route`
```bash
ip route
```
**What it does:** Displays the system's routing table.

**Expected result:** Default gateway, destination networks and network interfaces used for communication.

### `ip neigh`
```bash
ip neigh
```
**What it does:** Displays neighbouring devices discovered through ARP or IPv6 neighbour discovery.

**Expected result:** IP addresses, MAC addresses and neighbour states such as REACHABLE, STALE or FAILED.

### `hostname -I`
```bash
hostname -I
```
**What it does:** Displays IP addresses assigned to the local machine.

**Expected result:** One or more IP addresses.

### `arp -a`
```bash
arp -a
```
**What it does:** Displays the ARP cache containing known IP-to-MAC address mappings.

**Expected result:** IP addresses, MAC addresses and associated network interfaces.

### `ping -c 4 TARGET_IP`
```bash
ping -c 4 192.168.56.100
```
**What it does:** Sends four ICMP Echo Requests to test connectivity with a target.

**Expected result:** ICMP Echo Replies, response times and packet-loss statistics if the target responds.

### `traceroute TARGET_IP`
```bash
traceroute 192.168.56.100
```
**What it does:** Traces the network path between the local machine and the target.

**Expected result:** A list of intermediate network hops and their response times. Some hops may show `*` if they do not respond.

---

## 2. Host Discovery

### `nmap -sn`
```bash
nmap -sn 192.168.56.0/24
```
**What it does:** Discovers active hosts on a network without performing a port scan.

**Expected result:** Discovered IP addresses, host availability and MAC addresses where available.

### `nmap -sn TARGET_IP`
```bash
nmap -sn 192.168.56.100
```
**What it does:** Checks whether a specific target host appears reachable.

**Expected result:** Host status, such as `Host is up` or `Host seems down`.

### `arp-scan`
```bash
sudo arp-scan --localnet
```
**What it does:** Discovers devices on the local network using ARP requests.

**Expected result:** IP addresses, MAC addresses and hardware manufacturers where identifiable.

---

## 3. TCP Port Scanning

### Basic Nmap Scan
```bash
nmap TARGET_IP
```
**What it does:** Scans the most common 1,000 TCP ports by default.

**Expected result:** Open, closed or filtered ports and associated service names where identified.

### Full TCP Port Scan
```bash
nmap -p- TARGET_IP
```
**What it does:** Scans all TCP ports from 1 to 65535.

**Expected result:** A list of discovered open, closed or filtered TCP ports.

### Specific Port Scan
```bash
nmap -p 21,22,23,25,53,80,443,3306,8080 TARGET_IP
```
**What it does:** Scans only the specified ports.

**Expected result:** The status of each selected port.

### Fast Port Scan
```bash
nmap -F TARGET_IP
```
**What it does:** Performs a faster scan using a smaller list of commonly used ports.

**Expected result:** A list of discovered ports and their states.

### Open Ports Only
```bash
nmap --open TARGET_IP
```
**What it does:** Displays only ports that Nmap identifies as open or possibly open.

**Expected result:** A shorter report containing open-port findings.

---

## 4. Service and Version Enumeration

### Service Detection
```bash
nmap -sV TARGET_IP
```
**What it does:** Detects services running on discovered ports and attempts to identify their versions.

**Expected result:** Port numbers, service names, product names and version information where identifiable.

### Default Script Scan
```bash
nmap -sC TARGET_IP
```
**What it does:** Runs Nmap's default collection of NSE scripts against discovered services.

**Expected result:** Additional service information and configuration details depending on the scripts and target.

### Combined Service and Script Scan
```bash
nmap -sV -sC TARGET_IP
```
**What it does:** Combines service/version detection with default NSE scripts.

**Expected result:** Open ports, service versions and additional enumeration findings.

### Operating System Detection
```bash
sudo nmap -O TARGET_IP
```
**What it does:** Attempts to identify the target operating system using network fingerprinting.

**Expected result:** An estimated operating system, device type and sometimes OS version.

### Aggressive Detection
```bash
sudo nmap -A TARGET_IP
```
**What it does:** Enables OS detection, version detection, default scripts and traceroute.

**Expected result:** A more detailed report containing operating system estimates, services, script results and network route information.

---

## 5. UDP Enumeration

### UDP Port Scan
```bash
sudo nmap -sU TARGET_IP
```
**What it does:** Scans UDP ports on the target.

**Expected result:** UDP port states such as open, closed, open|filtered or filtered.

### Top 20 UDP Ports
```bash
sudo nmap -sU --top-ports 20 TARGET_IP
```
**What it does:** Scans the 20 most commonly used UDP ports.

**Expected result:** The states of the selected UDP ports and service information where identifiable.

---

## 6. DNS Reconnaissance

### NSLookup
```bash
nslookup example.com
```
**What it does:** Queries DNS to resolve a domain name.

**Expected result:** The DNS server used and the IP address associated with the domain, if available.

### DIG Basic Query
```bash
dig example.com
```
**What it does:** Performs a DNS lookup for a domain.

**Expected result:** DNS response status, answer records, query time and DNS server information.

### MX Record Lookup
```bash
dig example.com MX
```
**What it does:** Retrieves the domain's mail exchange records.

**Expected result:** Mail server hostnames and their priority values.

### NS Record Lookup
```bash
dig example.com NS
```
**What it does:** Retrieves the domain's authoritative name server records.

**Expected result:** Names of the domain's configured DNS name servers.

### A Record Lookup
```bash
dig example.com A
```
**What it does:** Retrieves IPv4 address records for a domain.

**Expected result:** One or more IPv4 addresses.

### TXT Record Lookup
```bash
dig example.com TXT
```
**What it does:** Retrieves text records associated with a domain.

**Expected result:** Published TXT information, which may include SPF, verification and other domain records.

### ANY Record Query
```bash
dig example.com ANY
```
**What it does:** Requests available DNS information for a domain using the ANY query type.

**Expected result:** A response containing records the DNS server chooses to provide. Many servers restrict or limit ANY responses.

---

## 7. Web Server Enumeration

### WhatWeb
```bash
whatweb http://TARGET_IP
```
**What it does:** Identifies technologies used by a web application.

**Expected result:** Web server information, frameworks, content management systems and other identifiable technologies.

### HTTP Header Inspection
```bash
curl -I http://TARGET_IP
```
**What it does:** Sends an HTTP HEAD request to retrieve response headers.

**Expected result:** HTTP status code, server information, content type and other response headers.

### Detailed HTTP Request
```bash
curl -v http://TARGET_IP
```
**What it does:** Displays detailed information about an HTTP connection and response.

**Expected result:** Connection details, request headers, response headers and server response information.

### Nikto Web Server Scan
```bash
nikto -h http://TARGET_IP
```
**What it does:** Checks a web server for known security issues, outdated components and potentially exposed files.

**Expected result:** Potential vulnerabilities, insecure configurations and interesting server resources. Findings require manual verification.

---

## 8. Directory and File Enumeration

### Gobuster
```bash
gobuster dir -u http://TARGET_IP -w /usr/share/wordlists/dirb/common.txt
```
**What it does:** Uses a wordlist to discover directories and files on a web server.

**Expected result:** Discovered paths, HTTP status codes and response sizes.

### Dirb
```bash
dirb http://TARGET_IP
```
**What it does:** Performs web directory and file discovery using a wordlist.

**Expected result:** Potentially accessible directories and files with their HTTP response codes.

### Feroxbuster
```bash
feroxbuster -u http://TARGET_IP
```
**What it does:** Performs recursive content discovery on a web server.

**Expected result:** Discovered directories, files, URLs and their HTTP response codes.

---

## 9. SMB Enumeration

### SMB Port Scan
```bash
nmap -p 139,445 TARGET_IP
```
**What it does:** Checks whether the common SMB-related ports are open.

**Expected result:** Port states and service information for ports 139 and 445.

### Enum4linux
```bash
enum4linux -a TARGET_IP
```
**What it does:** Performs information gathering against SMB services.

**Expected result:** Potential usernames, workgroup information, domain details, shares and other available SMB information.

### SMB Share Enumeration
```bash
smbclient -L //TARGET_IP/ -N
```
**What it does:** Attempts to list available SMB shares without supplying a password.

**Expected result:** Accessible share names and share types, if anonymous listing is permitted.

---

## 10. FTP Enumeration

### FTP Service Detection
```bash
nmap -p 21 -sV TARGET_IP
```
**What it does:** Checks port 21 and attempts to identify the FTP service and version.

**Expected result:** FTP port status, service name and version information where identifiable.

### FTP Anonymous Access Check
```bash
nmap -p 21 --script ftp-anon TARGET_IP
```
**What it does:** Checks whether anonymous FTP access is enabled.

**Expected result:** Information indicating whether anonymous login is allowed and potentially accessible directory listings.

### FTP Connection
```bash
ftp TARGET_IP
```
**What it does:** Starts an FTP client connection to the target.

**Expected result:** FTP connection status and a login prompt if the service is accessible.

---

## 11. SSH Enumeration

### SSH Service Detection
```bash
nmap -p 22 -sV TARGET_IP
```
**What it does:** Identifies the SSH service and attempts to determine its version.

**Expected result:** Port 22 status, SSH service information and version details where identifiable.

### SSH Verbose Connection
```bash
ssh -v username@TARGET_IP
```
**What it does:** Attempts an SSH connection while displaying detailed connection and negotiation information.

**Expected result:** Connection status, SSH protocol details, key exchange information and authentication results.

---

## 12. Vulnerability Scanning and Identification

### Nmap Vulnerability Scripts
```bash
nmap --script vuln TARGET_IP
```
**What it does:** Runs NSE scripts categorized for vulnerability detection against discovered services.

**Expected result:** Potential vulnerabilities and security-related findings associated with the target's exposed services. Results must be verified.

### SearchSploit
```bash
searchsploit SERVICE_NAME VERSION
```
**What it does:** Searches the local Exploit-DB database for publicly documented exploits associated with a service and version.

**Expected result:** Matching exploit references, descriptions and available exploit file paths.

---

## 13. HTTP Method Enumeration

### HTTP Options Request
```bash
curl -X OPTIONS -i http://TARGET_IP
```
**What it does:** Sends an OPTIONS request to identify HTTP methods advertised by the server for the requested resource.

**Expected result:** HTTP response headers and potentially an `Allow` header listing supported methods.

### HTTP Response Inspection
```bash
curl -v http://TARGET_IP
```
**What it does:** Displays detailed HTTP request and response information.

**Expected result:** Request details, response status, headers and connection information.

---

## 14. SSL/TLS Enumeration

### Nmap SSL/TLS Cipher Enumeration
```bash
nmap -p 443 --script ssl-enum-ciphers TARGET_IP
```
**What it does:** Examines supported SSL/TLS protocols and cipher suites on port 443.

**Expected result:** Supported TLS versions, cipher suites and security-related observations.

### SSLScan
```bash
sslscan TARGET_IP
```
**What it does:** Examines SSL/TLS configuration on a target server.

**Expected result:** Supported protocols, cipher suites, certificate information and potential configuration weaknesses.

---

## 15. Network Packet Analysis

### Capture Packets on eth0
```bash
sudo tcpdump -i eth0
```
**What it does:** Captures packets passing through the `eth0` interface.

**Expected result:** Live packet information, including source and destination addresses, protocols and ports where available.

### Capture Packets on eth1
```bash
sudo tcpdump -i eth1
```
**What it does:** Captures packets passing through the `eth1` interface.

**Expected result:** Live traffic observed on the selected interface.

### Capture on All Interfaces
```bash
sudo tcpdump -i any
```
**What it does:** Captures packets from all supported network interfaces.

**Expected result:** Traffic from available interfaces, with interface information where supported.

### Display Numeric Addresses
```bash
sudo tcpdump -i eth1 -nn
```
**What it does:** Captures traffic without resolving IP addresses or port numbers into names.

**Expected result:** Numeric source and destination IP addresses, ports and packet details.

### Capture HTTP Port Traffic
```bash
sudo tcpdump -i eth1 port 80
```
**What it does:** Captures packets involving TCP or UDP port 80.

**Expected result:** Packets matching the specified port filter. HTTP normally uses TCP port 80.

### Capture Traffic from a Specific Host
```bash
sudo tcpdump -i eth1 host TARGET_IP
```
**What it does:** Captures packets to or from a specified IP address.

**Expected result:** Network packets involving the specified target.

---

## 16. Saving Reconnaissance Results

### Create a Results Directory
```bash
mkdir -p ~/Recon-Results
```
**What it does:** Creates a directory for storing reconnaissance results.

**Expected result:** A directory named `Recon-Results` in the user's home directory.

### Save Normal Nmap Output
```bash
nmap -sV TARGET_IP -oN ~/Recon-Results/services.txt
```
**What it does:** Performs service detection and saves the normal output to a text file.

**Expected result:** A file named `services.txt` containing the scan results.

### Save Multiple Nmap Output Formats
```bash
nmap -sV -sC TARGET_IP -oA ~/Recon-Results/full-scan
```
**What it does:** Saves the scan results in normal, XML and grepable formats.

**Expected result:**
- `full-scan.nmap`
- `full-scan.xml`
- `full-scan.gnmap`

### View Saved Results
```bash
cat ~/Recon-Results/services.txt
```
**What it does:** Displays the contents of the saved scan file.

**Expected result:** The previously saved Nmap scan results.

---

## 17. Important Notes

- Replace `TARGET_IP` with the authorised target's IP address.
- Replace `SERVICE_NAME` and `VERSION` with the actual service and version identified during enumeration.
- Replace `example.com` with the domain being assessed.
- Some commands require root privileges and should be run with `sudo`.
- Some tools may need to be installed before use.
- Nmap scan results can vary depending on firewalls, filtering, network conditions and target configurations.
- An open port does not automatically mean a vulnerability exists.
- A tool reporting a possible vulnerability does not establish that the target is exploitable.
- Always document findings and verify results before drawing security conclusions.

**Documentation principle:** Record the target, date, command executed, observed output, and security relevance of each finding.
