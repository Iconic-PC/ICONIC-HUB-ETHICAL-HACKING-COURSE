# LAB 4: NETWORKING FUNDAMENTALS

**Course:** Iconic Hub Ethical Hacking Course  
**Lab Code:** ICH-EH101  
**Topic:** Networking Fundamentals  
**Brand:** Iconic Hub — Secure. Build. Innovate.

---

## 1. Introduction to Computer Networking

A computer network is a group of two or more devices connected so that they can communicate and share resources.

These devices may include computers, smartphones, servers, printers, routers, switches, and other network-enabled equipment.

For example, when you connect your laptop to a Wi-Fi network, your laptop becomes part of a network. It can communicate with other permitted devices and access the internet through a router.

Networking is important in cybersecurity because computers, websites, applications, and servers communicate over networks. Ethical hackers must understand how this communication works to identify weaknesses and help secure systems.

### Why Is Networking Important in Cybersecurity?

Networking knowledge helps cybersecurity professionals to:

- Understand how computers communicate.
- Identify devices connected to a network.
- Understand IP addresses and ports.
- Recognize suspicious network traffic.
- Understand how attackers may attempt to access systems.
- Configure firewalls and other security controls.
- Analyze network traffic using tools such as Wireshark.
- Understand how data travels between clients and servers.
- Identify network misconfigurations and security weaknesses.

**Example:** If a company server is exposed to the internet with an unnecessarily open remote-access port, an attacker may attempt to exploit it. A cybersecurity professional needs networking knowledge to understand the exposure and recommend appropriate protection.

---

## 2. Basic Networking Terminologies

### 2.1 Network

A network is a collection of connected devices that communicate and share resources.

**Example:** Computers connected to the same office network.

### 2.2 Node

A node is any device or connection point that participates in a network.

**Examples:** Computers, printers, servers, routers, and smartphones.

### 2.3 Client

A client is a device or application that requests a service or resource from another system.

**Example:** Your web browser acts as a client when you open a website.

### 2.4 Server

A server is a computer or software application that provides services or resources to clients.

**Examples:**
- A web server delivers website content.
- A mail server handles email.
- A file server stores and shares files.
- A DNS server helps translate domain names into IP addresses.

### 2.5 IP Address

An IP address is a logical address used to identify an interface on an IP network and help route traffic to its destination.

**Example:** `192.168.1.10`

An IP address is not necessarily a permanent identifier for a physical device. Addresses may change, and one device can have multiple network interfaces or addresses.

### 2.6 MAC Address

A Media Access Control (MAC) address is a link-layer address commonly associated with a network interface.

Example format:

`00:1A:2B:3C:4D:5E`

MAC addresses are used for communication on a local network segment. Unlike IP addresses, which support routing between networks, MAC addresses are used for local link-layer delivery.

A MAC address can sometimes be changed or randomized, so it should not be treated as guaranteed proof of a device's identity.

### 2.7 Packet

A packet is a unit of data carried across a network. In everyday networking discussions, the term is often used generally for data being transmitted.

Different network layers use different terms, including frames, packets, and segments.

### 2.8 Protocol

A protocol is a set of rules that determines how devices communicate.

Examples include:

- **HTTP:** Transfers web content.
- **HTTPS:** HTTP protected by TLS encryption.
- **TCP:** Provides reliable, ordered delivery of a byte stream.
- **UDP:** Sends datagrams without TCP's built-in delivery guarantees.
- **DNS:** Resolves domain names and supports other types of name-related queries.
- **DHCP:** Automatically supplies network configuration to clients.
- **SSH:** Provides secure remote access.
- **ICMP:** Supports network control and diagnostic messages.

### 2.9 Bandwidth

Bandwidth refers to the maximum data-transfer capacity of a connection over a period of time.

It is commonly measured in bits per second, such as Mbps or Gbps.

### 2.10 Latency

Latency is the time it takes for data to travel from one point to another. Network latency is commonly measured in milliseconds.

### 2.11 Firewall

A firewall is a security control that monitors and filters network traffic according to configured rules.

A firewall can allow legitimate communication and block traffic that violates its policy.

### 2.12 Port

A port number identifies a particular service or application endpoint on a host when using transport protocols such as TCP or UDP.

Examples:

- TCP port 80: Commonly used for HTTP.
- TCP port 443: Commonly used for HTTPS.
- TCP port 22: Commonly used for SSH.
- UDP port 53: Commonly used for DNS queries, although DNS can also use TCP port 53.

A port is not the same thing as a physical connection. It is a logical communication endpoint.

---

## 3. Types of Computer Networks

### 3.1 PAN — Personal Area Network

A Personal Area Network connects devices around one person over a short distance.

**Example:** A smartphone connected to wireless earbuds through Bluetooth.

### 3.2 LAN — Local Area Network

A Local Area Network connects devices within a limited area, such as a home, laboratory, office, or school building.

**Example:** Computers connected to the same university computer laboratory network.

### 3.3 WLAN — Wireless Local Area Network

A WLAN is a local area network that uses wireless communication, typically Wi-Fi.

**Example:** Students connecting their laptops to a campus Wi-Fi network.

### 3.4 MAN — Metropolitan Area Network

A Metropolitan Area Network connects networks across a city or a large metropolitan area.

**Example:** An organization connecting several facilities throughout one city.

### 3.5 WAN — Wide Area Network

A Wide Area Network connects devices or smaller networks across large geographical areas.

**Example:** A company connecting offices in Lagos, Abuja, and Port Harcourt.

The internet is the largest and best-known example of an interconnected global network.

---

## 4. Common Network Devices

### 4.1 Router

A router forwards IP packets between different networks.

For example, a home router forwards traffic between the home network and the internet.

Many home routers also provide additional functions such as Wi-Fi, firewalling, DHCP, and network address translation (NAT).

### 4.2 Switch

A network switch connects devices within a local network. A typical Ethernet switch forwards frames using MAC addresses.

**Example:** An office switch connects computers, printers, and servers through Ethernet cables.

### 4.3 Modem

A modem provides the signal conversion or access function required by a particular internet connection technology.

The exact function depends on the technology used by the internet service provider.

### 4.4 Wireless Access Point

A wireless access point allows Wi-Fi devices to connect to a network.

Some routers include a built-in wireless access point.

### 4.5 Firewall

A firewall filters network traffic according to security rules. Firewalls may be implemented as dedicated appliances, software, cloud services, or features within other devices.

### 4.6 Network Interface Card

A Network Interface Card (NIC) is a hardware or virtual interface that allows a device to connect to a network.

Examples include an Ethernet adapter and a Wi-Fi adapter.

---

## 5. Network Topologies

Network topology describes how devices and connections are arranged.

### 5.1 Star Topology

In a star topology, devices connect to a central device, such as a switch or wireless access point.

**Advantages:**
- Easy to manage.
- Individual cable failures usually affect only the connected device.
- Easy to expand.

**Disadvantage:**
- Failure of the central device can disrupt the devices connected through it.

### 5.2 Bus Topology

In a bus topology, devices share a common communication backbone.

**Advantages:**
- Historically simple and inexpensive for small installations.

**Disadvantages:**
- A backbone failure can affect the network.
- Troubleshooting and performance become difficult as the network grows.

### 5.3 Ring Topology

In a ring topology, devices form a loop in which each device connects to neighboring devices.

**Advantage:**
- Some ring designs provide predictable communication behavior.

**Disadvantage:**
- A failure can disrupt communication if the topology lacks appropriate redundancy or protection.

### 5.4 Mesh Topology

In a mesh topology, devices have multiple interconnections.

**Advantages:**
- Can provide redundancy.
- Can offer alternative paths when a connection fails.

**Disadvantages:**
- Can be more expensive.
- May require more complex configuration and management.

### 5.5 Hybrid Topology

A hybrid topology combines two or more topology types.

**Example:** A company uses star topology inside its offices and connects office networks through a routed WAN.

---

## 6. The OSI Model

The Open Systems Interconnection (OSI) model describes network communication using seven conceptual layers.

The model helps cybersecurity professionals understand where protocols, devices, and security controls operate.

| Layer | Name | Main Function | Examples |
|---|---|---|---|
| 7 | Application | Provides network services to applications | HTTP, DNS, SMTP |
| 6 | Presentation | Represents, encodes, compresses, or encrypts data conceptually | Data encoding, encryption concepts |
| 5 | Session | Manages communication sessions conceptually | Session establishment and management |
| 4 | Transport | Supports end-to-end transport between applications | TCP, UDP |
| 3 | Network | Handles logical addressing and routing | IP, routers, ICMP |
| 2 | Data Link | Handles local link delivery and frames | Ethernet, Wi-Fi MAC, switches |
| 1 | Physical | Transmits raw bits through a physical or radio medium | Cables, connectors, radio signals |

The OSI model is a teaching model. Real-world protocol stacks do not always fit neatly into one layer.

### Why the OSI Model Matters in Cybersecurity

It helps analysts reason about different types of problems.

- **Layer 1:** Damaged cables or physical connectivity problems.
- **Layer 2:** Unauthorized devices, spoofing, or local network attacks.
- **Layer 3:** IP routing problems or suspicious IP traffic.
- **Layer 4:** Unexpected TCP or UDP ports and connections.
- **Layers 5–7:** Application sessions, insecure configurations, authentication problems, and web vulnerabilities.

Security controls can operate across multiple layers.

---

## 7. The TCP/IP Model

The TCP/IP model describes the protocol architecture used by modern internet communication.

A commonly taught version has four layers.

| TCP/IP Layer | Main Function | Examples |
|---|---|---|
| Application | Network services used by applications | HTTP, HTTPS, DNS, SSH, SMTP |
| Transport | Communication between application endpoints | TCP, UDP |
| Internet | IP addressing and routing | IPv4, IPv6, ICMP |
| Network Access | Communication over the local network medium | Ethernet, Wi-Fi |

### OSI vs TCP/IP

The OSI model has seven layers, while the common TCP/IP model has four.

The OSI model is useful for learning and troubleshooting. The TCP/IP model more directly represents the protocol suite used for internet communication.

---

## 8. IPv4 Addressing

IPv4 stands for Internet Protocol version 4.

An IPv4 address contains 32 bits, usually written as four decimal numbers separated by dots.

Example:

`192.168.1.10`

Each decimal number is called an **octet** because it represents eight bits.

An IPv4 address therefore contains four octets:

`192 . 168 . 1 . 10`

Each octet can have a value from 0 to 255.

### 8.1 Understanding Bits and Octets

A bit can have a value of 0 or 1.

An octet contains eight bits. The positions represent these decimal values:

`128, 64, 32, 16, 8, 4, 2, 1`

For example:

`11000000`

This equals:

`128 + 64 = 192`

Therefore, the first octet of `192.168.1.10` is 192.

You do not need to convert every address to binary immediately, but understanding that IPv4 contains 32 bits will help you understand subnetting.

### 8.2 Network Portion and Host Portion

An IPv4 address can be understood as having two logical portions:

- **Network portion:** Identifies the IP network.
- **Host portion:** Identifies an address within that network.

The subnet mask or CIDR prefix determines where the network portion ends and the host portion begins.

For example, with:

`192.168.1.10/24`

The `/24` means the first 24 bits belong to the network portion. The remaining 8 bits belong to the host portion.

The corresponding subnet mask is:

`255.255.255.0`

In this example:

- Network address: `192.168.1.0`
- Host address: `192.168.1.10`
- Broadcast address: `192.168.1.255`

For a conventional IPv4 subnet such as this `/24`, host addresses generally range from `192.168.1.1` to `192.168.1.254`.

---

## 9. IPv4 Address Classes: Class A, B, C, D and E

Before CIDR became the standard approach, IPv4 addressing commonly used a system called **classful addressing**.

Classful addressing divided IPv4 addresses into classes based mainly on their first octet.

This is important for understanding networking history, but modern networks generally use CIDR rather than relying on address classes.

### 9.1 Class A

- First-octet range: 1–126 for ordinary historical unicast examples.
- Historical default subnet mask: `255.0.0.0`
- Historical prefix: `/8`
- First 8 bits represented the network portion.
- Remaining 24 bits represented the host portion.

Example:

`10.1.2.3`

Under the historical classful model, this was a Class A address with a default `/8` network.

**Important:** `0.0.0.0/8` has special uses, and `127.0.0.0/8` is reserved for loopback. These are why the ordinary Class A range is often shown as 1–126.

### 9.2 Class B

- First-octet range: 128–191.
- Historical default subnet mask: `255.255.0.0`
- Historical prefix: `/16`
- First 16 bits represented the network portion.
- Remaining 16 bits represented the host portion.

Example:

`172.20.5.10`

Under the historical classful model, this was a Class B address with a default `/16` network.

### 9.3 Class C

- First-octet range: 192–223.
- Historical default subnet mask: `255.255.255.0`
- Historical prefix: `/24`
- First 24 bits represented the network portion.
- Remaining 8 bits represented the host portion.

Example:

`192.168.1.10`

Under the historical classful model, this was a Class C address with a default `/24` network.

### 9.4 Class D

- First-octet range: 224–239.
- Used for IPv4 multicast addressing.
- Not an ordinary classful unicast host-address class.

Multicast allows one sender to transmit traffic intended for a group of interested receivers.

### 9.5 Class E

- First-octet range: 240–255.
- Historically designated as reserved or experimental.
- Not an ordinary classful unicast host-address class.

The highest IPv4 address, `255.255.255.255`, is used as a limited broadcast address.

### Summary of Historical IPv4 Classes

| Class | First Octet | Historical Default Prefix | Historical Default Mask | General Purpose |
|---|---|---|---|---|
| A | 1–126 in ordinary examples | `/8` | `255.0.0.0` | Unicast addressing |
| B | 128–191 | `/16` | `255.255.0.0` | Unicast addressing |
| C | 192–223 | `/24` | `255.255.255.0` | Unicast addressing |
| D | 224–239 | Not applicable | Not applicable | Multicast |
| E | 240–255 | Not applicable | Not applicable | Reserved or special use |

**Remember:** Classful addressing is historical. Do not use the first octet alone to determine a modern network's subnet mask.

---

## 10. Understanding CIDR

CIDR stands for **Classless Inter-Domain Routing**.

CIDR allows networks to be described using a prefix length rather than assuming a fixed subnet mask based on Class A, B, or C.

CIDR notation places a slash after an IPv4 address, followed by the number of network bits.

Examples:

- `192.168.1.10/24`
- `192.168.1.10/26`
- `10.20.30.40/16`

The number after the slash tells you how many of the 32 bits belong to the network portion.

### 10.1 Understanding `/24`

Consider:

`192.168.1.10/24`

IPv4 contains 32 bits.

- Network bits: 24
- Host bits: 32 − 24 = 8

The subnet mask is:

`255.255.255.0`

The network address is:

`192.168.1.0`

The broadcast address is:

`192.168.1.255`

For a conventional `/24` subnet, the usable host range is:

`192.168.1.1 – 192.168.1.254`

### 10.2 Understanding `/26`

Consider:

`192.168.1.70/26`

- Network bits: 26
- Host bits: 32 − 26 = 6
- Subnet mask: `255.255.255.192`

The total number of addresses is:

`2⁶ = 64`

For a conventional IPv4 subnet, two addresses are reserved for the network address and broadcast address.

Therefore:

`64 − 2 = 62 usable host addresses`

The address `192.168.1.70/26` belongs to the network:

`192.168.1.64/26`

Its conventional host range is:

`192.168.1.65 – 192.168.1.126`

Its broadcast address is:

`192.168.1.127`

The next `/26` network begins at `192.168.1.128`.

### 10.3 Why the Same IP Can Have Different Prefixes

Consider the address:

`192.168.1.10`

It may be configured as:

- `192.168.1.10/24`
- `192.168.1.10/25`
- `192.168.1.10/26`
- `192.168.1.10/27`

The IP address is the same, but the subnet prefix changes how the address is grouped into a network.

For example:

- With `/24`, the network is `192.168.1.0/24`.
- With `/25`, the network is `192.168.1.0/25`.
- With `/26`, the network is `192.168.1.0/26`.
- With `/27`, the network is `192.168.1.0/27`.

These examples use the same host address, but the network boundaries differ.

**Key lesson:** You cannot reliably determine a modern subnet mask from the first octet alone. You need the configured subnet mask or CIDR prefix.

---

## 11. Subnet Masks

A subnet mask identifies which bits of an IPv4 address represent the network portion and which represent the host portion.

Common subnet masks include:

| CIDR Prefix | Subnet Mask | Host Bits | Total Addresses | Conventional Usable Hosts |
|---|---|---:|---:|---:|
| `/8` | `255.0.0.0` | 24 | 16,777,216 | 16,777,214 |
| `/16` | `255.255.0.0` | 16 | 65,536 | 65,534 |
| `/24` | `255.255.255.0` | 8 | 256 | 254 |
| `/25` | `255.255.255.128` | 7 | 128 | 126 |
| `/26` | `255.255.255.192` | 6 | 64 | 62 |
| `/27` | `255.255.255.224` | 5 | 32 | 30 |
| `/28` | `255.255.255.240` | 4 | 16 | 14 |
| `/29` | `255.255.255.248` | 3 | 8 | 6 |
| `/30` | `255.255.255.252` | 2 | 4 | 2 |

The conventional usable-host calculation is:

`Usable hosts = 2^(host bits) − 2`

This calculation applies to ordinary IPv4 subnets where the network and broadcast addresses are reserved. Special cases include `/31` point-to-point links and `/32` single-address prefixes.

### Why Do Subnet Masks Matter?

Subnet masks help devices determine whether a destination is on the same local IP network or whether traffic should be sent to a router.

They also allow network administrators to divide address space into smaller networks for management, security, and efficient allocation.

---

## 12. Subnetting Explained

Subnetting is the process of dividing a larger IP network into smaller networks called subnets.

Organizations use subnetting to:

- Separate departments.
- Reduce unnecessary broadcast traffic.
- Organize IP addresses.
- Apply different security policies.
- Separate sensitive systems from ordinary user devices.
- Improve network management.

### Example: Dividing a `/24` Network into Four `/26` Networks

Suppose an organization has this network:

`192.168.1.0/24`

A `/24` contains 256 total IPv4 addresses.

The organization wants four smaller subnets.

Changing `/24` to `/26` borrows two additional bits for the network portion.

The number of subnets created is:

`2² = 4`

Each `/26` contains:

`2⁶ = 64 total addresses`

Each conventional `/26` subnet has 62 usable host addresses.

The four subnets are:

| Subnet | Network Address | Usable Host Range | Broadcast Address |
|---|---|---|---|
| 1 | `192.168.1.0/26` | `192.168.1.1 – 192.168.1.62` | `192.168.1.63` |
| 2 | `192.168.1.64/26` | `192.168.1.65 – 192.168.1.126` | `192.168.1.127` |
| 3 | `192.168.1.128/26` | `192.168.1.129 – 192.168.1.190` | `192.168.1.191` |
| 4 | `192.168.1.192/26` | `192.168.1.193 – 192.168.1.254` | `192.168.1.255` |

Notice that each new subnet starts 64 addresses after the previous subnet.

The network addresses are:

- `192.168.1.0`
- `192.168.1.64`
- `192.168.1.128`
- `192.168.1.192`

### The Block-Size Method

A quick way to find subnet boundaries is to calculate the block size in the octet where the subnet mask changes from 255.

For `/26`, the mask is:

`255.255.255.192`

Calculate:

`256 − 192 = 64`

Therefore, the subnet boundaries in the last octet are:

`0, 64, 128, 192`

Suppose you are given:

`192.168.1.70/26`

The last-octet subnet boundaries are 0, 64, 128, and 192.

Since 70 is between 64 and 127, the address belongs to:

- Network address: `192.168.1.64`
- First usable address: `192.168.1.65`
- Last usable address: `192.168.1.126`
- Broadcast address: `192.168.1.127`

This method is useful for introductory subnetting exercises.

---

## 13. Private and Public IPv4 Addresses

IPv4 addresses can be used in different contexts.

### 13.1 Private IPv4 Addresses

Private IPv4 addresses are reserved for use inside private networks. They are not globally routed across the public internet.

The three main private IPv4 ranges are:

| Private Range | CIDR | Common Example |
|---|---|---|
| `10.0.0.0 – 10.255.255.255` | `10.0.0.0/8` | `10.1.1.5` |
| `172.16.0.0 – 172.31.255.255` | `172.16.0.0/12` | `172.20.1.10` |
| `192.168.0.0 – 192.168.255.255` | `192.168.0.0/16` | `192.168.1.10` |

These ranges are commonly used in home, school, office, and laboratory networks.

**Important:** Not every address beginning with `172` is private. The private range is only `172.16.0.0` through `172.31.255.255`.

### 13.2 Public IPv4 Addresses

Public IPv4 addresses are globally routable addresses assigned for communication across the internet, subject to routing and allocation policies.

A device using a private IPv4 address may access the internet through a router that performs Network Address Translation (NAT).

NAT allows multiple private devices to share a public IPv4 address in common home and office configurations.

However, NAT is not a replacement for a properly configured firewall or other security controls.

---

## 14. MAC Addresses and IP Addresses

IP addresses and MAC addresses serve different purposes.

| Feature | IP Address | MAC Address |
|---|---|---|
| Main role | Logical addressing and routing | Local link-layer delivery |
| Common example | `192.168.1.10` | `00:1A:2B:3C:4D:5E` |
| Layer in the OSI model | Layer 3 | Layer 2 |
| Can change? | Yes | Yes, in many systems |
| Used across routers? | IP addressing supports routing across networks | Ethernet MAC addresses normally change at each routed link |

When a computer sends traffic to another network, it normally sends the traffic to its default gateway at the local link layer. The router then forwards the IP packet toward its destination.

---

## 15. TCP and UDP

TCP and UDP are transport-layer protocols.

### 15.1 TCP — Transmission Control Protocol

TCP provides a reliable, ordered byte stream between communicating applications.

Its features include:

- Connection establishment.
- Reliable delivery.
- Retransmission of lost data.
- Ordered delivery of bytes.
- Flow control.

TCP is commonly used by web applications, email services, and secure remote administration protocols.

**Example:** HTTPS commonly uses TCP port 443 when HTTP is transported over TLS and TCP.

### 15.2 UDP — User Datagram Protocol

UDP sends datagrams without providing TCP's built-in guarantee of reliable, ordered delivery.

UDP has less protocol overhead and is useful for applications that benefit from low latency or implement their own reliability mechanisms.

Common uses include:

- DNS queries.
- Voice and video applications.
- Online gaming.
- Some streaming and real-time communication protocols.

### TCP vs UDP

| Feature | TCP | UDP |
|---|---|---|
| Connection setup | Yes | No TCP-style connection setup |
| Built-in reliable delivery | Yes | No |
| Ordered delivery | Yes | No |
| Common use | Web traffic, SSH, email | DNS queries, real-time traffic |
| Main consideration | Reliability and ordered delivery | Low overhead and application flexibility |

UDP is not inherently malicious, and TCP is not inherently secure. Both can carry legitimate or malicious traffic.

---

## 16. Common Network Ports and Protocols

A port number helps identify the service or application endpoint receiving traffic on a host.

The following are common default ports.

| Port | Protocol | Common Purpose |
|---|---|---|
| TCP 20/21 | FTP | File Transfer Protocol; data/control use depends on FTP mode |
| TCP 22 | SSH | Secure remote administration |
| TCP 23 | Telnet | Remote terminal access; traffic is not encrypted by default |
| TCP 25 | SMTP | Email transfer |
| TCP 53, UDP 53 | DNS | Domain Name System |
| UDP 67/68 | DHCP | IPv4 network configuration |
| TCP 80 | HTTP | Unencrypted web traffic |
| TCP 110 | POP3 | Email retrieval |
| TCP 143 | IMAP | Email access |
| TCP 443 | HTTPS over TLS | Secure web traffic |
| TCP 445 | SMB | Windows file and printer sharing |
| TCP 3389 | RDP | Remote Desktop Protocol |
| UDP 123 | NTP | Network time synchronization |

Port assignments can vary by configuration, and a service may use non-default ports.

**Cybersecurity note:** A port being open does not automatically mean a system is vulnerable. Analysts must determine which service is listening, whether it is required, how it is configured, and whether it is exposed to the appropriate network.

---

## 17. DNS — Domain Name System

DNS translates domain names into information used by networked systems, commonly including IP addresses.

People find names such as:

`example.com`

easier to remember than numerical IP addresses.

DNS allows a client to query for the address associated with a domain name.

### Basic DNS Resolution

A simplified DNS resolution process is:

1. A user enters a domain name in a browser.
2. The computer checks relevant local caches and configuration.
3. If needed, the computer queries a configured DNS resolver.
4. The resolver obtains the required answer from its cache or by querying other DNS servers.
5. The answer is returned to the client.
6. The browser uses the returned address to connect to the destination.

The exact process depends on caching, DNS configuration, and the type of query.

### Common DNS Record Types

| Record | Purpose |
|---|---|
| A | Maps a name to an IPv4 address |
| AAAA | Maps a name to an IPv6 address |
| CNAME | Creates an alias for another domain name |
| MX | Identifies mail servers for a domain |
| NS | Identifies authoritative name servers |
| TXT | Stores text associated with a DNS name, often used for verification and email security policies |

### DNS Security Risks

DNS-related security issues may include:

- DNS spoofing or cache poisoning.
- Misconfigured DNS records.
- Unauthorized changes to DNS settings.
- DNS tunneling used to carry data through DNS queries.
- Phishing through deceptive domain names.

Security teams can reduce risk through secure DNS administration, access controls, monitoring, and appropriate DNS security measures.

---

## 18. DHCP — Dynamic Host Configuration Protocol

DHCP automatically provides network configuration to clients.

Depending on the network, DHCP may provide:

- An IP address.
- A subnet mask or prefix.
- A default gateway.
- DNS resolver addresses.
- Lease duration and other configuration options.

Without DHCP, an administrator may need to configure network settings manually.

### Simplified DHCP Process

A common DHCPv4 exchange is known as DORA:

1. **Discover:** The client searches for a DHCP server.
2. **Offer:** A server offers network configuration.
3. **Request:** The client requests the offered configuration.
4. **Acknowledgment:** The server confirms the lease.

This exchange can vary depending on whether the client already has a lease or is renewing one.

### DHCP Security Risks

A rogue DHCP server could provide incorrect network settings, potentially directing clients through an attacker-controlled gateway or DNS resolver.

Possible protections include:

- Using trusted DHCP servers.
- Restricting physical access to network equipment.
- Applying DHCP snooping on supported switches.
- Monitoring unexpected changes in network configuration.

---

## 19. HTTP and HTTPS

HTTP stands for Hypertext Transfer Protocol. It is used to transfer web requests and responses.

HTTPS is HTTP protected by Transport Layer Security (TLS).

### HTTP

HTTP without TLS does not provide transport encryption. Sensitive information sent over an unprotected HTTP connection may be exposed to someone who can observe the traffic.

### HTTPS

HTTPS uses TLS to provide encryption in transit and help authenticate the server.

It protects data against many forms of network eavesdropping and tampering when configured correctly.

However, HTTPS does not guarantee that a website is trustworthy, free of vulnerabilities, or safe to use. A malicious website can also use HTTPS.

### Example

When a user opens an online banking website, HTTPS helps protect information transmitted between the browser and the website.

The website must still use secure authentication, access controls, application security, and appropriate server configuration.

---

## 20. Default Gateway

A default gateway is the router or other next-hop device a host uses when it needs to send traffic outside its local IP network and has no more specific route.

Example configuration:

- IP address: `192.168.1.10`
- Subnet mask: `255.255.255.0`
- Default gateway: `192.168.1.1`
- DNS resolver: `192.168.1.1`

In this example, `192.168.1.1` is the default gateway and also happens to be the DNS resolver. These roles do not have to be provided by the same device.

### Why Is the Default Gateway Important?

If the default gateway is missing or incorrectly configured, the device may still communicate with some local devices but fail to reach other networks or the internet.

A wrong or malicious gateway configuration can also redirect traffic, so gateway settings are important in network troubleshooting and security.

---

## 21. Basic Network Troubleshooting Commands

Cybersecurity professionals and network administrators use command-line tools to inspect network configuration and diagnose connectivity problems.

Commands differ between Windows and Linux.

### 21.1 Windows Commands

Open Command Prompt or PowerShell.

**Display IP configuration:**

```powershell
ipconfig
```

Shows basic network configuration.

**Display detailed IP configuration:**

```powershell
ipconfig /all
```

Shows additional details, including MAC addresses, DHCP information, and DNS settings.

**Test connectivity to a destination:**

```powershell
ping 8.8.8.8
```

Tests whether ICMP echo requests receive replies from the destination.

A missing reply does not necessarily prove the destination is offline because a firewall or network policy may block ICMP.

**Display the route toward a destination:**

```powershell
tracert example.com
```

Shows the path discovered toward the destination, subject to network and device responses.

**Display active connections and listening ports:**

```powershell
netstat -ano
```

Displays network connections, listening endpoints, and associated process IDs where available.

### 21.2 Linux Commands

Open a terminal.

**Display network interfaces and IP addresses:**

```bash
ip addr
```

**Display the routing table:**

```bash
ip route
```

**Test connectivity:**

```bash
ping -c 4 8.8.8.8
```

Sends four ICMP echo requests on systems that support this option.

**Display the route toward a destination:**

```bash
traceroute example.com
```

The `traceroute` utility may need to be installed. Some systems provide alternatives such as `tracepath`.

**Display listening TCP and UDP sockets:**

```bash
ss -tuln
```

Shows listening sockets and associated protocol information. Options and output can vary by system.

**Display the MAC address and interface details:**

```bash
ip link
```

### Important Note

Use diagnostic commands on your own devices and networks or where you have explicit authorization. A troubleshooting command is not automatically an attack, but the purpose and target of an activity matter.

---

## 22. Introduction to Network Traffic Analysis

Network traffic analysis is the process of examining communications between devices to understand normal activity, investigate problems, and identify potential threats.

A network analyst may examine:

- Source and destination IP addresses.
- Source and destination ports.
- Protocols.
- Connection attempts.
- DNS requests.
- HTTP requests and responses.
- Unusual traffic volume.
- Repeated connection failures.
- Communication with suspicious destinations.

### Wireshark

Wireshark is a network protocol analyzer that captures and displays network traffic.

It helps users inspect protocol behavior and investigate network events.

Common display filters include:

```text
dns
```

Displays DNS packets.

```text
tcp
```

Displays TCP packets.

```text
udp
```

Displays UDP packets.

```text
icmp
```

Displays ICMP packets.

```text
ip.addr == 192.168.1.10
```

Displays packets where the specified IPv4 address appears as a source or destination.

```text
tcp.port == 443
```

Displays TCP packets with source or destination port 443.

These are Wireshark **display filters**. They filter packets already captured or loaded into Wireshark; they do not themselves start a capture.

### Ethical and Legal Considerations

Only capture and analyze network traffic when you have permission to do so.

Network captures may contain personal information, credentials, session tokens, and other sensitive data. Store evidence securely and avoid sharing unnecessary sensitive information.

---

## 23. Common Network Attacks

Understanding common attacks helps cybersecurity professionals recognize threats and recommend suitable controls.

### 23.1 DoS and DDoS Attacks

A Denial-of-Service (DoS) attack attempts to make a system or service unavailable.

A Distributed Denial-of-Service (DDoS) attack uses multiple systems or sources to overwhelm a target.

**Possible defenses:**
- Traffic filtering.
- Rate limiting.
- DDoS mitigation services.
- Capacity planning.
- Monitoring and incident response procedures.

### 23.2 Man-in-the-Middle Attack

A man-in-the-middle attack occurs when an attacker positions themselves between communicating parties to observe or manipulate traffic.

**Possible defenses:**
- Proper TLS configuration.
- Certificate validation.
- Secure Wi-Fi.
- Strong authentication.
- Protection against unauthorized network changes.

### 23.3 ARP Spoofing

ARP spoofing involves sending forged Address Resolution Protocol messages on a local IPv4 network to associate an IP address with an incorrect MAC address.

It may allow an attacker to redirect local traffic.

**Possible defenses:**
- Dynamic ARP Inspection where supported.
- Appropriate switch configuration.
- Network segmentation.
- Monitoring unusual ARP activity.

### 23.4 DNS Spoofing

DNS spoofing involves providing false DNS information or manipulating DNS resolution to direct users to an unintended destination.

**Possible defenses:**
- Secure DNS administration.
- DNSSEC validation where appropriate.
- Monitoring DNS changes.
- Protecting resolver access and configuration.

### 23.5 Port Scanning

Port scanning identifies which ports respond on a target system.

Administrators and authorized security testers use scanning to understand exposed services. Attackers may also scan systems before attempting further attacks.

**Possible defenses:**
- Disable unnecessary services.
- Restrict access to required ports.
- Apply firewall rules.
- Monitor suspicious scanning behavior.
- Keep exposed services updated.

Port scanning results alone do not prove that a system is vulnerable.

### 23.6 Packet Sniffing

Packet sniffing involves capturing and examining network traffic.

It is a legitimate diagnostic and security-analysis activity, but unauthorized capture may expose confidential information.

**Possible defenses:**
- Encrypt sensitive communications.
- Restrict access to network equipment.
- Secure Wi-Fi.
- Apply appropriate network segmentation and monitoring.

---

## 24. Network Security Best Practices

Organizations can reduce network risks by applying layered security controls.

### 24.1 Use Strong Authentication

Use strong, unique passwords and multifactor authentication where supported, especially for administrative and remote access.

### 24.2 Apply Firewall Rules

Allow only the network traffic needed for legitimate business operations.

Review rules regularly and remove unnecessary exposure.

### 24.3 Update Systems

Install security updates for operating systems, network equipment, applications, and services.

### 24.4 Disable Unnecessary Services

Every exposed service can introduce additional risk. Disable services that are not required and restrict access to those that must remain available.

### 24.5 Use Network Segmentation

Separate systems into appropriate network segments.

For example, guest Wi-Fi should not automatically have unrestricted access to internal company servers.

### 24.6 Monitor Network Activity

Monitor network connections, authentication events, DNS requests, firewall logs, and unusual traffic patterns.

### 24.7 Secure Wireless Networks

Use modern Wi-Fi security settings, strong credentials, updated equipment, and appropriate separation between guest and internal networks.

### 24.8 Encrypt Sensitive Communication

Use appropriately configured secure protocols such as HTTPS and SSH instead of unencrypted alternatives when possible.

### 24.9 Back Up Important Data

Maintain tested backups and protect them against unauthorized access and destructive attacks.

### 24.10 Apply Least Privilege

Give users, devices, and applications only the permissions they need.

Network access should be limited according to business requirements and risk.

---

## 25. Practical Lab Exercises

Complete these exercises on your own computer or in an authorized lab environment.

### Exercise 1: Inspect Your IP Configuration

**On Windows:**

```powershell
ipconfig /all
```

**On Linux:**

```bash
ip addr
ip route
```

Record the following:

1. Your active network interface.
2. Your IPv4 address, if assigned.
3. Your subnet mask or CIDR prefix.
4. Your default gateway, if configured.
5. Your DNS resolver information, if available.
6. Your network interface's MAC address.

Do not publish your complete network details or other sensitive configuration information publicly.

### Exercise 2: Test Connectivity

Run:

**Windows:**

```powershell
ping 8.8.8.8
```

**Linux:**

```bash
ping -c 4 8.8.8.8
```

Record whether replies were received.

Explain why a failed ping does not necessarily mean that the destination is offline.

### Exercise 3: Investigate DNS Resolution

Run:

**Windows:**

```powershell
nslookup example.com
```

**Linux:**

```bash
nslookup example.com
```

If `nslookup` is unavailable on your Linux system, use an installed DNS lookup utility such as `dig`.

Record the domain queried, the DNS server used if displayed, and the returned address information.

Remember that DNS responses may change because of caching, geographic distribution, and configuration.

### Exercise 4: Identify Listening Services

**Windows:**

```powershell
netstat -ano
```

**Linux:**

```bash
ss -tuln
```

Identify any listening endpoints displayed by the command.

Do not assume that every listening port is a vulnerability. Determine which services are expected and whether access is appropriately restricted.

### Exercise 5: CIDR Practice

Answer the following questions.

1. How many bits are in an IPv4 address?
2. How many bits are in one IPv4 octet?
3. What subnet mask corresponds to `/24`?
4. What subnet mask corresponds to `/26`?
5. How many total addresses are in a `/26`?
6. How many conventional usable host addresses are in a `/26`?
7. What is the network address of `192.168.1.70/26`?
8. What is its broadcast address?
9. What is the conventional usable host range for that subnet?
10. Why can the address `192.168.1.10` be configured with different CIDR prefixes?

---

## 26. Review Questions

Answer these questions to confirm your understanding.

1. What is a computer network?
2. Explain the difference between a client and a server.
3. What is the difference between an IP address and a MAC address?
4. What are the main differences between a router and a switch?
5. List the seven layers of the OSI model.
6. What is the purpose of the TCP/IP model?
7. What does an IPv4 address identify?
8. What is the difference between IPv4 and IPv6?
9. Explain the historical purpose of IPv4 address classes.
10. Why is classful addressing considered historical?
11. What does `/24` mean in CIDR notation?
12. Why should you not determine a modern subnet mask from the first octet alone?
13. What is subnetting, and why do organizations use it?
14. Explain the difference between private and public IPv4 addresses.
15. What is the role of a default gateway?
16. What is the difference between TCP and UDP?
17. What is DNS, and why is it important?
18. What is DHCP, and what does DORA stand for?
19. What is the difference between HTTP and HTTPS?
20. How can a firewall improve network security?
21. What information can Wireshark reveal?
22. Name three network attacks and one defense against each.
23. Why is network segmentation important?
24. Why must network testing be authorized?

---

## 27. Key Takeaways

By the end of this lab, you should understand that:

- A computer network connects devices for communication and resource sharing.
- Network devices such as routers, switches, and access points perform different functions.
- The OSI and TCP/IP models help explain network communication.
- IPv4 addresses contain 32 bits arranged into four octets.
- IP addresses and MAC addresses serve different purposes.
- Historical IPv4 classes A, B, and C used default `/8`, `/16`, and `/24` prefixes respectively.
- Modern networks use CIDR, which allows flexible network prefixes.
- A subnet mask or CIDR prefix determines the network and host portions of an IPv4 address.
- Subnetting divides a larger network into smaller networks.
- Private IPv4 addresses are used inside private networks and are not globally routed on the public internet.
- TCP and UDP have different transport characteristics.
- DNS resolves domain names, while DHCP provides network configuration.
- HTTPS protects HTTP communication using TLS when correctly configured.
- Network diagnostic tools help troubleshoot connectivity and inspect system configuration.
- Wireshark helps analysts examine network traffic.
- Firewalls, updates, segmentation, encryption, monitoring, and strong authentication improve network security.
- All security testing must be conducted on systems and networks you own or have explicit authorization to assess.

---

## 28. Conclusion

Networking fundamentals are essential for anyone pursuing cybersecurity or ethical hacking. Before testing a system's security, a professional needs to understand how devices communicate, how IP addresses and ports work, how traffic is routed, and how network services are configured.

Understanding IPv4 addressing, historical address classes, CIDR, and subnetting is particularly important because these concepts help explain how devices are grouped into networks and how traffic reaches its destination. The key distinction is that address classes belong to the historical classful model, while modern networks use subnet masks and CIDR prefixes to define network boundaries.

With these fundamentals, learners can progress toward network scanning, traffic analysis, firewall configuration, vulnerability assessment, and other ethical hacking activities in controlled and authorized environments.

**Iconic Hub — Secure. Build. Innovate.**
