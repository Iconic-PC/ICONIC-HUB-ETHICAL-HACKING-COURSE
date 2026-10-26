# ICONIC-HUB-ETHICAL-HACKING-COURSE

# ICH-EH102 — LAB 7: PASSIVE RECONNAISSANCE

**Course:** Iconic Hub Ethical Hacking Course  
**Lab Number:** 7  
**Stage:** ICH-EH102 — Reconnaissance and Vulnerability Assessment  
**Duration:** 3 Hours  
**Topic:** Passive Reconnaissance  
**Primary Environment:** Kali Linux  
**Tools:** Web browser, `whois`, `dig`, `nslookup`, public RDAP services, certificate transparency logs, and search engines  
**Target:** An instructor-approved domain or an organization selected for a public-information research exercise  
**Tagline:** Secure. Build. Innovate.

---

# PART 1: INTRODUCTION

## 1.1 What Is Passive Reconnaissance?

Passive reconnaissance is the process of gathering information about a target using publicly available information and existing records, without directly probing the target's infrastructure.

In ethical hacking, reconnaissance is an important stage of a security assessment. Before a tester examines a system for weaknesses, they need to understand the target, identify relevant information, and establish what requires further investigation.

Passive reconnaissance allows an ethical hacker to learn about a target before conducting direct technical tests.

For example, an ethical hacker may investigate:

- Public information about an organization.
- Its official website and published documentation.
- Publicly recorded domain information.
- DNS records available through public DNS infrastructure.
- Publicly visible TLS certificates.
- Published information about technologies and services.
- Public security advisories relevant to identified technologies.

The goal is to build an initial picture of the target using information that is already publicly accessible.

### Simple example

Imagine that you are preparing to inspect a building's security.

Before visiting the building, you study its publicly available address, official website, published opening hours, and publicly available architectural information.

You have gathered information without entering the building or testing its locks.

Passive reconnaissance follows a similar principle in cybersecurity.

An ethical hacker studies existing information to understand the target and plan an assessment.

### Why is passive reconnaissance important?

Passive reconnaissance helps an ethical hacker:

1. Understand a target before performing technical tests.
2. Identify relevant domains and publicly documented assets.
3. Discover information that may help define an assessment scope.
4. Reduce unnecessary direct interaction with target systems.
5. Identify potential security concerns for later verification.
6. Prepare a more focused and efficient assessment.
7. Record useful evidence and information sources.

**Important:** Passive reconnaissance does not automatically prove that a system is vulnerable. It provides information that may guide further investigation.

## 1.2 What Is the Difference Between Reconnaissance and Passive Reconnaissance?

Reconnaissance is the broader process of gathering information about a target.

Passive reconnaissance is one approach to reconnaissance.

Another approach is active reconnaissance, which involves directly interacting with the target's infrastructure to obtain information.

For this course, we will distinguish the two as follows:

| Activity | Passive reconnaissance | Active reconnaissance |
|---|---|---|
| Review an organization's public website | Yes | No |
| Read published security advisories | Yes | No |
| Examine publicly available RDAP records | Yes | No |
| Review publicly accessible certificate transparency records | Yes | No |
| Send a ping to a target machine | No | Yes |
| Run Nmap against a target | No | Yes |
| Probe a target's ports | No | Yes |
| Send crafted requests to a web application | No | Yes |

Some activities, such as DNS lookups, involve sending requests to external DNS infrastructure. In this course, we treat ordinary lookups of existing public DNS records as passive information gathering because they do not probe the target's application or scan its ports.

However, not every activity described as passive is completely invisible to all parties. Public services may log queries, and third-party research platforms may record searches.

## 1.3 Passive Reconnaissance vs Enumeration

Students should understand the distinction between these terms.

| Passive reconnaissance | Enumeration |
|---|---|
| Gathers information from existing public sources | Collects specific information about discovered systems, services, or resources |
| Often precedes direct technical testing | Commonly involves more focused investigation |
| May identify published domain records and organizational information | May identify services listening on discovered ports |
| Can help establish an initial picture of the target | Helps describe the technical details of discovered services |
| Does not require probing the target's infrastructure | May involve direct interaction with the target |

The two activities can overlap in a real assessment. The important point is to understand the method used and the type of information obtained.

## 1.4 What Is an Attack Surface?

An attack surface is the collection of points through which a system or organization could potentially be exposed to security risks.

Examples include:

- Public websites.
- Internet-facing applications.
- Publicly accessible services.
- Email infrastructure.
- Remote access services.
- APIs.
- Externally accessible cloud resources.
- Employee-facing authentication portals.

Passive reconnaissance can help an assessor identify possible elements of an organization's external attack surface.

For example, public DNS records may reveal that an organization uses separate hostnames for its website, email, and other services.

These records can help an assessor understand the organization's public-facing structure.

However, discovering a hostname does not prove that the host is reachable, operational, or vulnerable.

## 1.5 What Is a Domain Name?

A domain name is a human-readable name used to identify an internet resource or a namespace within the Domain Name System (DNS).

Examples include:

- `example.com`
- `wikipedia.org`
- `iana.org`

For this lab, students should use an instructor-approved domain or a reserved example domain.

A domain may have several related components.

| Term | Example | Explanation |
|---|---|---|
| Top-level domain (TLD) | `.com` | The highest-level component at the end of the domain |
| Second-level domain | `example` | The component immediately below the TLD |
| Registered domain | `example.com` | The domain registered under the relevant registry |
| Subdomain | `www.example.com` | A name beneath the parent domain |
| Hostname | `www.example.com` | A name that identifies a host or service in a naming system |

The precise relationship between domain names and hostnames depends on the system's configuration.

A subdomain is not necessarily a separate registered domain. It may identify a service, environment, or other resource associated with the parent domain.

## 1.6 What Is OSINT?

OSINT stands for Open-Source Intelligence.

It is the process of collecting, evaluating, and analyzing information from publicly available sources to answer a specific question.

In cybersecurity, OSINT may include:

- Public domain registration information.
- Public DNS records.
- Published documentation.
- Public code repositories.
- Security advisories.
- Public certificate records.
- Publicly available organizational information.
- Publicly disclosed security incidents.

OSINT is not limited to cybersecurity. Journalists, researchers, investigators, and analysts also use it.

### Information collection vs intelligence

Collecting information and producing intelligence are not exactly the same thing.

For example:

**Collected information:** A public DNS record contains a hostname associated with a domain.

**Analysis:** The hostname's name suggests that it may relate to a development environment.

**Limitation:** The name alone does not prove the hostname is a development server or that it is exposed to the internet.

Good reconnaissance requires careful interpretation rather than assumptions.

## 1.7 What Is the Domain Name System (DNS)?

The Domain Name System translates domain names into information used by internet-connected systems.

For example, a browser may use DNS to obtain an IP address associated with a website's hostname.

DNS records can also provide information about other services.

Common DNS record types include:

| Record | Purpose |
|---|---|
| A | Maps a hostname to an IPv4 address |
| AAAA | Maps a hostname to an IPv6 address |
| MX | Identifies mail servers associated with a domain |
| NS | Identifies authoritative name servers |
| TXT | Stores text associated with a DNS name, including some verification and email-security information |
| CNAME | Identifies an alias for another DNS name |
| SOA | Contains administrative information about a DNS zone |

These records may help a researcher understand how a domain is configured.

For example, an MX record may identify the mail servers associated with a domain, while NS records may identify its authoritative name servers.

### Important DNS limitation

A DNS record is not proof that a corresponding server is online or that it is vulnerable.

Records can be outdated, point to shared infrastructure, or describe services that are not accessible to a particular researcher.

Students must report the actual records they observe rather than inventing results.

## 1.8 What Is WHOIS and RDAP?

WHOIS is a traditional protocol and service used to retrieve domain registration information.

RDAP stands for Registration Data Access Protocol. It is a newer standardized approach to retrieving registration data.

Depending on the domain and registry, registration information may include:

- Registrar information.
- Registration and expiration dates.
- Domain status codes.
- Nameservers.
- Registry or registrar contact information.
- Some registrant information, where it is publicly disclosed.

Some personal information is redacted or withheld for privacy reasons.

Students must not assume that the identity of a domain's registrant will be publicly visible.

### WHOIS vs RDAP

WHOIS remains useful, but RDAP provides structured responses and standardized access mechanisms.

The availability and detail of records depend on the registry, registrar, and applicable disclosure policies.

A missing field does not mean the domain is suspicious.

## 1.9 What Is Certificate Transparency?

Certificate Transparency (CT) is a system that makes information about publicly trusted TLS certificates available through public logs.

TLS certificates help establish encrypted connections and authenticate domain names for HTTPS.

Certificate transparency records can reveal information such as:

- Certificate subject names.
- Subject Alternative Names (SANs).
- Certificate issuers.
- Certificate validity periods.
- Certificate-related timestamps.

A certificate may contain several hostnames.

For example, a certificate issued for a parent domain might also contain a hostname associated with a web service.

This can provide useful information about the domain's publicly documented naming structure.

### Important limitation

A hostname appearing in a certificate transparency log does not prove that the hostname currently resolves to an IP address or that a web server is accessible.

Certificates may be expired, revoked, replaced, or associated with systems that no longer exist.

## 1.10 What Is a Subdomain?

A subdomain is a domain name beneath another domain in the DNS hierarchy.

For example, in the illustrative name `portal.example.com`, `portal` is a subdomain of `example.com`.

Organizations may use subdomains for:

- Main websites.
- Customer portals.
- Documentation.
- APIs.
- Mail services.
- Development environments.
- Staging environments.
- Regional services.

Subdomain information may be available from public DNS records, certificate logs, published documentation, or other public sources.

However, the existence of a hostname does not automatically mean it is an active system or an exposed attack surface.

## 1.11 What Is a Security Advisory?

A security advisory is a published notice describing a security issue, affected products or versions, potential impact, and often recommended remediation.

Examples of information sources include:

- National Vulnerability Database (NVD).
- Official vendor security advisories.
- Common Vulnerabilities and Exposures (CVE) records.
- CISA security advisories and Known Exploited Vulnerabilities information.
- Official project security announcements.

A researcher may use these sources to understand the risks associated with a technology identified during reconnaissance.

However, a published vulnerability affecting a particular product does not prove that the target uses that product or is affected by that vulnerability.

The product, version, configuration, and exposure must be verified before making that conclusion.

---

# PART 2: LAB OBJECTIVES, ENVIRONMENT AND SAFETY

## 2.1 Lab Objectives

By the end of this lab, students should be able to:

1. Define passive reconnaissance and explain its importance.
2. Differentiate passive reconnaissance from active reconnaissance.
3. Explain the purpose of OSINT.
4. Identify basic domain name components.
5. Retrieve and interpret public DNS records.
6. Explain the difference between WHOIS and RDAP.
7. Examine publicly available certificate transparency information.
8. Identify publicly documented hostnames and services.
9. Research technologies and security advisories using reliable sources.
10. Record sources, timestamps, findings, and limitations.
11. Distinguish observed facts from assumptions.
12. Produce a structured passive reconnaissance report.

## 2.2 Required Lab Environment

Students need:

- A working Kali Linux VM.
- An internet connection, if permitted by the instructor.
- A web browser.
- A terminal.
- The `dig`, `nslookup`, and `whois` utilities where available.
- Access to public information services.
- A directory for saving notes and screenshots.

The OWASP VM used in Lab 6 is not required for the core exercises in this lesson. Passive reconnaissance does not require students to scan that VM.

The instructor may provide an organization or domain for a public-information case study.

If the classroom has no internet connection, the instructor can provide previously collected public records for students to analyze.

## 2.3 Authorization and Scope

Passive reconnaissance must still follow the course's authorization and ethical requirements.

For this lab:

- Use only an instructor-approved domain or a domain selected for a permitted public-information exercise.
- Do not scan ports.
- Do not probe web applications.
- Do not attempt authentication bypasses.
- Do not attempt exploitation.
- Do not attempt to access private accounts or restricted resources.
- Do not use discovered information to target employees or individuals.
- Do not attempt to access unpublished or private information.
- Do not publish sensitive findings without authorization.
- Record the sources and limitations of collected information.

Public availability does not automatically authorize every possible use of the information.

## 2.4 Pre-Lab Checklist

Before starting, confirm the following:

- [ ] Kali Linux starts successfully.
- [ ] A terminal is available.
- [ ] A web browser is available.
- [ ] Internet access is working, if required.
- [ ] The domain or research target has been approved.
- [ ] The student has created a folder for evidence.
- [ ] The student understands the scope and restrictions.
- [ ] The student has a report template or evidence log.

---

# PART 3: PRACTICAL SESSION

## Practical 1 — Prepare the Lab Workspace

**Objective:** Create a dedicated workspace for storing reconnaissance notes and evidence.

### Step 1: Open the Kali Linux terminal

Start Kali Linux and log in.

Open a terminal.

### Step 2: Create the lab directory

Run:

```bash
mkdir -p ~/ICH-EH102/Lab7/screenshots
```

This creates a directory for Lab 7 and a subdirectory for screenshots.

### Step 3: Create an evidence log

Run:

```bash
cd ~/ICH-EH102/Lab7
touch evidence-log.txt
```

### Step 4: Verify the directory

Run:

```bash
pwd
ls -la
```

The first command displays the current directory. The second displays its contents.

The expected directory structure is:

```text
Lab7/
├── evidence-log.txt
└── screenshots/
```

### Step 5: Record the lab details

Run:

```bash
date -u
```

This displays the current date and time in UTC on a correctly configured system.

Record the time at which the research was performed. If the system clock is incorrect, note that limitation and use the timestamp provided by the relevant source where appropriate.

### Step 6: Capture evidence

Take a screenshot showing the created directory.

Suggested filename:

`lab7_01_workspace_setup.png`

### Expected result

Students should have a dedicated workspace for recording findings, saving screenshots, and preparing the final report.

---

## Practical 2 — Identify the Research Target

**Objective:** Establish the domain being researched and document the scope.

For the examples in this lab, we will use the reserved domain `example.com`. Replace it with the instructor-approved domain when conducting the assigned case study.

### Step 1: Set a domain variable

In the terminal, run:

```bash
TARGET="example.com"
```

### Step 2: Display the target

Run:

```bash
printf '%s\n' "$TARGET"
```

The expected output is:

```text
example.com
```

### Step 3: Record the scope

Complete the following table in your notes.

| Field | Student entry |
|---|---|
| Organization or case study | |
| Approved domain | |
| Research purpose | |
| Authorized activities | |
| Prohibited activities | |
| Date and time of research | |
| Student name or identifier | |

### Step 4: Explain the target selection

Students should be able to explain why the domain was selected and what they are permitted to investigate.

For example, a permitted scope may include reviewing public DNS records and certificate transparency logs for an assigned domain.

It should not automatically include scanning every hostname discovered during the research.

### Step 5: Capture evidence

Suggested filename:

`lab7_02_research_scope.png`

### Expected result

Students should understand that scope must be established before information gathering begins.

---

## Practical 3 — Review the Official Website

**Objective:** Collect basic organizational and technical information from a public website.

### Step 1: Open the website

Open Firefox in Kali Linux.

Enter:

```text
https://example.com
```

Use the instructor-approved domain for the actual assignment.

### Step 2: Examine the homepage

Record publicly visible information such as:

- Organization name.
- Stated purpose or services.
- Public contact channels.
- Links to documentation.
- Links to status pages.
- Links to security policies.
- Links to published technical resources.

Do not assume that every linked service is part of the authorized assessment scope.

### Step 3: Review the website's public documentation

Where available, review official pages such as:

- About.
- Contact.
- Documentation.
- Security.
- Privacy.
- Terms of service.
- Status or service announcements.

Only collect information relevant to the assigned research purpose.

### Step 4: Record the source

Complete this table:

| Field | Observation |
|---|---|
| Page title | |
| URL | |
| Date and time accessed | |
| Information collected | |
| Why the information matters | |
| Limitations | |

### Step 5: Distinguish facts from assumptions

For example:

**Fact:** The official website publishes documentation for an API.

**Possible interpretation:** The organization operates or documents an API service.

**Limitation:** The documentation alone does not establish that the API is currently reachable or that it is vulnerable.

### Step 6: Capture evidence

Take a screenshot of a relevant public page.

Suggested filename:

`lab7_03_public_website.png`

Avoid including personal information or unrelated browser tabs in the screenshot.

### Expected result

Students should be able to gather basic information from official public sources and explain how it may contribute to an assessment.

---

## Practical 4 — Inspect Public DNS Records

**Objective:** Retrieve common DNS records and understand what they reveal.

DNS records are a useful source of information during passive reconnaissance.

For this exercise, the commands query public DNS infrastructure. They do not scan the target's ports.

### Step 1: Check whether `dig` is available

Run:

```bash
command -v dig
```

If the command displays a path, `dig` is available.

If it is missing and the instructor approves installing the required package, the standard Kali package is `dnsutils`:

```bash
sudo apt update
sudo apt install dnsutils
```

Installing a package is not necessary if the utility is already present.

### Step 2: Query the A record

Run:

```bash
dig "$TARGET" A
```

The A record is used to associate a DNS name with an IPv4 address.

Look for the answer section in the output.

Illustrative example:

```text
;; ANSWER SECTION:
example.com.    300    IN    A    192.0.2.10
```

The IP address above belongs to a documentation-only example range. It is not a claim about the current address of the example domain.

### Step 3: Query the AAAA record

Run:

```bash
dig "$TARGET" AAAA
```

The AAAA record is used to associate a DNS name with an IPv6 address.

If no AAAA record appears in the answer section, the lookup may have returned no matching record.

Do not invent an address to fill the report.

### Step 4: Query the MX record

Run:

```bash
dig "$TARGET" MX
```

MX records identify mail servers associated with the domain.

The result may reveal which mail infrastructure is configured for the domain.

However, an MX record does not prove that a mail server is accessible or vulnerable.

### Step 5: Query NS records

Run:

```bash
dig "$TARGET" NS
```

NS records identify authoritative name servers for a DNS zone.

These records may help a researcher understand which name servers are associated with the domain.

### Step 6: Query TXT records

Run:

```bash
dig "$TARGET" TXT
```

TXT records can contain several types of information, including domain verification values and email-security policies.

Some domains publish SPF information in TXT records. SPF is a mechanism used to identify servers authorized to send email for a domain.

Students should not assume that every TXT record is an SPF policy.

### Step 7: Query the SOA record

Run:

```bash
dig "$TARGET" SOA
```

The Start of Authority record contains administrative information about a DNS zone.

Depending on the response, it may include the primary name server, a responsible-party field, serial information, and timing parameters.

Some of these fields are technical configuration details rather than direct evidence of a security weakness.

### Step 8: Record the DNS findings

Complete the following table using the actual results.

| Record type | Observed result | What it may tell us |
|---|---|---|
| A | | IPv4 address association |
| AAAA | | IPv6 address association |
| MX | | Mail-server configuration |
| NS | | Authoritative name servers |
| TXT | | Published text and possible policy information |
| SOA | | DNS zone administrative information |

If a query returns no answer, record that result accurately.

### Step 9: Understand DNS lookup output

The output of `dig` contains several sections.

- `HEADER`: information about the DNS response.
- `QUESTION SECTION`: the name and record type requested.
- `ANSWER SECTION`: records returned in the answer.
- `AUTHORITY SECTION`: information about the relevant authority, when supplied.
- `ADDITIONAL SECTION`: supplementary records, when supplied.

A response may also contain status information such as `NOERROR`, `NXDOMAIN`, or `SERVFAIL`.

These statuses have different meanings.

`NOERROR` does not necessarily mean that a requested record exists. A response may have no answer records even when the DNS name itself exists.

`NXDOMAIN` indicates that the queried name does not exist according to the responding DNS system.

`SERVFAIL` indicates that the server failed to complete the query successfully.

Students should inspect the entire response rather than treating every command as successful simply because it produced output.

### Step 10: Capture evidence

Suggested filenames:

- `lab7_04_dns_a_record.png`
- `lab7_05_dns_mx_record.png`
- `lab7_06_dns_ns_record.png`
- `lab7_07_dns_txt_record.png`

Students may capture additional screenshots if the instructor requires evidence for every record type.

### Expected result

Students should understand how DNS records provide information about domain configuration and how to interpret missing or incomplete results.

---

## Practical 5 — Use `nslookup` for DNS Queries

**Objective:** Use an alternative DNS utility and compare its output with `dig`.

`nslookup` is a command-line utility used to query DNS information.

### Step 1: Confirm that the utility is available

Run:

```bash
command -v nslookup
```

### Step 2: Query the domain

Run:

```bash
nslookup "$TARGET"
```

The result may display the DNS server used for the query and the addresses returned for the domain.

### Step 3: Query the mail servers

Run:

```bash
nslookup -type=MX "$TARGET"
```

This requests MX records associated with the domain.

### Step 4: Query the name servers

Run:

```bash
nslookup -type=NS "$TARGET"
```

This requests NS records associated with the domain.

### Step 5: Compare the results

Complete this table:

| Question | Student response |
|---|---|
| What information did `nslookup` return? | |
| Did the result resemble the corresponding `dig` output? | |
| Which DNS server was shown, if any? | |
| Were any records missing? | |
| What might explain differences between the results? | |

DNS answers can vary because of caching, DNS configuration, record changes, and the resolver used.

### Step 6: Capture evidence

Suggested filename:

`lab7_08_nslookup_results.png`

### Expected result

Students should be familiar with two common DNS utilities and understand that different tools may display the same underlying information differently.

---

## Practical 6 — Examine Domain Registration Information

**Objective:** Understand how public domain registration records can contribute to reconnaissance.

### Step 1: Check whether `whois` is available

Run:

```bash
command -v whois
```

If it is not installed, the instructor may authorize installation using:

```bash
sudo apt update
sudo apt install whois
```

### Step 2: Query the domain

Run:

```bash
whois "$TARGET"
```

The output depends on the domain's registry, registrar, and available registration data.

It may contain:

- Registrar information.
- Domain status.
- Registration dates.
- Expiration dates.
- Name servers.
- Registry-specific details.

Some information may be redacted or omitted.

### Step 3: Review the result

Identify the fields that are present.

Do not assume that every WHOIS server returns the same format.

Do not assume that a missing registrant name indicates suspicious behavior.

### Step 4: Understand RDAP

RDAP is a structured alternative to traditional WHOIS queries.

For many generic top-level domains, public RDAP services can be discovered through the relevant registry or IANA's RDAP bootstrap information.

An example of a public RDAP lookup service for domains is:

`https://rdap.org/domain/example.com`

This is an example lookup URL. For the actual exercise, replace `example.com` with the approved domain and use the relevant registry or service.

Students can also visit the official IANA RDAP bootstrap registry information:

`https://www.iana.org/assignments/rdap-dns/rdap-dns.xhtml`

### Step 5: Compare WHOIS and RDAP

Complete the table:

| Field | WHOIS result | RDAP result |
|---|---|---|
| Registrar | | |
| Domain status | | |
| Registration date, if available | | |
| Expiration date, if available | | |
| Name servers | | |
| Redacted information | | |

Some fields may be unavailable in either result. Record this accurately.

### Step 6: Interpret the findings

Domain registration records may help establish administrative details about a domain.

However, they do not prove:

- That a website is currently accessible.
- That the domain is hosted on a particular physical server.
- That the registrant operates every service associated with the domain.
- That the domain contains a vulnerability.

### Step 7: Capture evidence

Suggested filenames:

- `lab7_09_whois_lookup.png`
- `lab7_10_rdap_lookup.png`

### Expected result

Students should understand how registration information is collected and how privacy controls and registry differences affect the results.

---

## Practical 7 — Examine Certificate Transparency Records

**Objective:** Use publicly available certificate information to identify names associated with a domain.

### Step 1: Open a certificate transparency service

Open Firefox and visit:

`https://crt.sh/`

Certificate transparency search services allow users to examine publicly logged certificates.

### Step 2: Search for the approved domain

Enter the instructor-approved domain into the search field.

For a demonstration only, the domain may be `example.com`.

Review the returned certificate records.

### Step 3: Identify relevant fields

Depending on the record, you may find:

- Certificate identity.
- Common Name (CN).
- Subject Alternative Names (SANs).
- Issuer.
- Validity dates.
- Logged certificate information.

A certificate can contain several DNS names, and some names may be wildcards.

For example, `*.example.com` is a wildcard name that can cover qualifying first-level subdomains under `example.com`, subject to certificate and hostname rules.

It does not prove that every possible subdomain exists or is operational.

### Step 4: Record the results

Complete the table:

| Field | Observation |
|---|---|
| Search domain | |
| Number of relevant records reviewed | |
| Example hostname observed | |
| Certificate issuer, if available | |
| Validity dates, if available | |
| Does the record establish current availability? | |
| What additional verification would be needed? | |

Do not invent a hostname or claim that a certificate is current without examining the record.

### Step 5: Explain the limitations

Certificate transparency logs can reveal historical and current certificate-related information.

However:

- A certificate may be expired.
- A hostname may no longer be in use.
- A certificate may have been replaced or revoked.
- A hostname may not resolve in DNS.
- A logged certificate does not prove that a service is reachable.

Treat discovered names as leads for authorized follow-up, not confirmed live systems.

### Step 6: Capture evidence

Suggested filename:

`lab7_11_certificate_transparency.png`

### Expected result

Students should understand how certificate transparency records contribute to passive reconnaissance and why they cannot be treated as a definitive inventory of live assets.

---

## Practical 8 — Research Publicly Documented Technologies

**Objective:** Identify technologies disclosed through official public documentation and understand how technology information can guide security research.

### Step 1: Review official documentation

Return to the target's official website or documentation pages.

Look for explicit public references to:

- Programming languages.
- Application frameworks.
- Content management systems.
- Public APIs.
- Published software products.
- Cloud services.
- Public developer documentation.

Only record technologies that are explicitly disclosed or supported by a reliable source.

### Step 2: Record the source

Complete the table:

| Technology or service | Public source | Date accessed | Evidence | Confidence |
|---|---|---|---|---|
| | | | | |

Use the following confidence labels:

- **High:** Explicitly stated by a reliable official source.
- **Medium:** Supported by a credible source but not directly confirmed by the organization.
- **Low:** Based on an indirect clue or an uncertain inference.

Students should not identify a framework or server version simply because a website looks similar to another website using that technology.

### Step 3: Review a relevant security advisory

If a technology has been explicitly identified, search an official vendor advisory or the National Vulnerability Database:

`https://nvd.nist.gov/`

Students should record:

- The advisory or CVE identifier.
- The affected product and versions.
- The published impact.
- The vendor's recommended mitigation.
- Whether the evidence establishes that the researched target is affected.

### Step 4: Separate technology identification from vulnerability confirmation

Example:

**Observation:** An official document states that a service uses Product A.

**Research:** A security advisory describes a vulnerability affecting certain versions of Product A.

**Conclusion:** The advisory may be relevant to future assessment planning.

**Limitation:** Unless the deployed version and relevant configuration are verified, the researcher cannot conclude that the target is vulnerable.

This distinction is essential in professional security work.

### Step 5: Capture evidence

Suggested filename:

`lab7_12_public_technology_research.png`

### Expected result

Students should be able to use reliable public sources to research technology-related risks without claiming that a vulnerability has been confirmed.

---

## Practical 9 — Use Search Engines for Public Information

**Objective:** Learn how search engines can help locate public documentation and other relevant information.

Search engines index information from websites and other publicly accessible sources.

They may help researchers locate:

- Official documentation.
- Public security policies.
- Published reports.
- Public developer resources.
- Security advisories.
- Official status pages.
- Public project repositories.

### Step 1: Search for official documentation

Use the search engine to search for the approved organization or domain and its official documentation.

For example, a query might be:

```text
site:example.com documentation
```

This asks the search engine to prioritize results associated with the specified domain.

Search operators vary between search engines, and results may not include every page.

### Step 2: Search for public security information

An example query is:

```text
site:example.com security
```

Review whether the results point to official security documentation, a vulnerability disclosure policy, or another relevant public resource.

### Step 3: Search for published advisories

If the organization publicly documents a product, students can search official vendor advisories or recognized vulnerability databases for relevant security information.

Avoid making conclusions based only on search result snippets. Open the original source and read the relevant information.

### Step 4: Record the sources

| Search purpose | Query used | Relevant source | Main finding |
|---|---|---|---|
| Official documentation | | | |
| Public security policy | | | |
| Technology documentation | | | |
| Relevant security advisory | | | |

### Step 5: Use search results responsibly

Students must not treat the existence of an indexed URL as proof that a resource remains available.

Search results may be outdated, incomplete, or taken out of context.

Do not attempt to access restricted information or use search operators to seek private credentials, personal records, or other sensitive information.

### Step 6: Capture evidence

Suggested filename:

`lab7_13_public_search_results.png`

### Expected result

Students should understand how search engines contribute to OSINT and why findings should be verified against original sources.

---

## Practical 10 — Build a Passive Reconnaissance Findings Table

**Objective:** Consolidate the information collected during the lab into a structured research record.

Students should not submit a collection of screenshots without analysis.

They must explain what each observation means and what remains unknown.

### Step 1: Review the collected evidence

Review the notes and screenshots from Practicals 1–9.

Identify the most relevant observations.

### Step 2: Complete the findings table

| ID | Observation | Source | Security relevance | Confidence | Limitation |
|---|---|---|---|---|---|
| PR-01 | | | | | |
| PR-02 | | | | | |
| PR-03 | | | | | |
| PR-04 | | | | | |
| PR-05 | | | | | |

Students should include only actual observations.

### Step 3: Assign confidence

Use these guidelines:

**High confidence:** The information is explicitly supported by a reliable source and was accurately recorded.

**Medium confidence:** The information is supported by a credible source but requires additional verification.

**Low confidence:** The information is indirect, incomplete, ambiguous, or potentially outdated.

Confidence refers to the reliability of the observation. It does not automatically describe the severity of a security risk.

### Step 4: Identify follow-up questions

For each significant finding, ask:

1. What does the evidence establish?
2. What does it not establish?
3. Could the information be outdated?
4. Is the source authoritative?
5. What further verification would be required?
6. Would that verification require explicit authorization?

### Step 5: Capture evidence

Suggested filename:

`lab7_14_passive_recon_findings.png`

### Expected result

Students should be able to turn collected information into an organized and evidence-based set of findings.

---

# PART 4: ANALYSIS AND REPORTING

## 4.1 What Should Students Conclude?

At the end of the lab, students should be able to explain:

- What passive reconnaissance means.
- Why passive reconnaissance is performed before more intrusive testing.
- What the public sources revealed about the assigned domain.
- Which DNS records were observed.
- What registration information was available.
- Whether certificate records disclosed relevant hostnames.
- What technology information was explicitly documented.
- Which security advisories may be relevant to the identified technologies.
- Which findings are confirmed observations.
- Which findings require further verification.
- What information remains unknown.

Students must not claim that the target is vulnerable simply because a public record exists.

They must distinguish information gathering from vulnerability confirmation.

## 4.2 How to Write a Finding

Each finding should include:

**Finding ID:** A unique reference, such as PR-01.

**Title:** A short description of the observation.

**Observation:** What was actually found.

**Evidence:** The source URL, screenshot, record, or saved output.

**Interpretation:** What the observation reasonably means.

**Security relevance:** Why the information may matter to the assessment.

**Confidence:** How reliably the observation is supported.

**Limitations:** What the observation does not establish.

**Recommended follow-up:** What authorized verification may be appropriate.

### Example finding

**Finding ID:** PR-01

**Title:** Public DNS records are available for the research domain.

**Observation:** A DNS lookup returned an A record for the assigned domain.

**Evidence:** The recorded `dig` output and the date and time of the query.

**Interpretation:** The DNS response associated the queried name with the returned IPv4 address at the time of the lookup.

**Security relevance:** The result provides information about the domain's public DNS configuration.

**Confidence:** High, assuming the query and response were accurately recorded.

**Limitations:** The DNS response does not establish that the associated service is reachable or vulnerable.

**Recommended follow-up:** Include the record in the asset-research notes and verify any relevant asset only if it falls within an explicitly authorized assessment scope.

This example illustrates how to write a finding without exaggerating the significance of the evidence.

## 4.3 Recommended Lab Report Structure

Students must submit a report titled:

**Lab 7: Passive Reconnaissance**

The report should contain:

1. Cover page.
2. Introduction.
3. Lab objectives.
4. Scope and authorization.
5. Lab environment and tools.
6. Research methodology.
7. Public website observations.
8. DNS record findings.
9. WHOIS and RDAP observations.
10. Certificate transparency observations.
11. Public technology and advisory research.
12. Search-engine research.
13. Consolidated findings table.
14. Evidence screenshots.
15. Limitations and uncertainty.
16. Conclusion.
17. References.

## 4.4 Evidence Requirements

Each screenshot must:

- Show the relevant application or terminal.
- Contain enough context to identify the activity.
- Support a finding discussed in the report.
- Be readable.
- Be saved with a meaningful filename.
- Avoid exposing unrelated personal information or secrets.

Students should preserve source URLs and access dates in their evidence log.

A screenshot alone is not always sufficient to support a technical conclusion. Students should also record the command used, the relevant output, or the original source.

## 4.5 Evidence Log Template

| Evidence ID | Filename | Activity | Source or command | Date collected |
|---|---|---|---|---|
| EV-01 | lab7_01_workspace_setup.png | Workspace preparation | `ls -la` | |
| EV-02 | lab7_03_public_website.png | Website review | Official website URL | |
| EV-03 | lab7_04_dns_a_record.png | DNS lookup | `dig "$TARGET" A` | |
| EV-04 | lab7_09_whois_lookup.png | Registration research | `whois "$TARGET"` | |
| EV-05 | lab7_11_certificate_transparency.png | Certificate research | Certificate transparency service | |
| EV-06 | lab7_14_passive_recon_findings.png | Findings consolidation | Research notes | |

Students should adapt this table to match the evidence they actually collected.

## 4.6 Professional Reporting Standards

A professional passive reconnaissance report should be:

- Accurate.
- Clear.
- Reproducible where practical.
- Supported by evidence.
- Honest about uncertainty.
- Limited to the authorized scope.
- Free from unsupported vulnerability claims.
- Written in language appropriate for the intended reader.

Avoid vague statements such as "the website is insecure" when the evidence only establishes that a DNS record exists.

Prefer specific observations and explain what additional evidence would be required to support stronger conclusions.

---

# PART 5: COMMON PROBLEMS AND TROUBLESHOOTING

## 5.1 The `dig` Command Is Not Found

**Possible cause:** The utility is not installed.

**Solution:** Check availability with:

```bash
command -v dig
```

If it is missing, ask the instructor whether package installation is permitted. The utility is commonly provided by the `dnsutils` package in Kali Linux.

## 5.2 The DNS Query Returns No Answer

**Possible causes:**

- The requested record type is not published.
- The domain is configured differently from what the student expected.
- The DNS response contains no matching answer.
- A DNS resolver or connectivity problem affected the query.

**Solution:**

1. Confirm the domain was typed correctly.
2. Review the response status.
3. Retry the query if appropriate.
4. Compare the result with another permitted public DNS source.
5. Record the result accurately.

Do not invent a missing record.

## 5.3 The WHOIS Command Returns Incomplete Information

**Possible causes:**

- The registry does not publish the requested information.
- Personal data is redacted.
- The response requires a referral to another service.
- The registry or server uses a different output format.

**Solution:** Review the relevant registry's RDAP service or official documentation. Record unavailable information as unavailable.

## 5.4 Certificate Transparency Search Returns Many Results

**Possible cause:** The domain has a large or historical certificate record set.

**Solution:**

1. Confirm the correct domain was entered.
2. Review the records relevant to the assigned research scope.
3. Distinguish current-looking records from historical records.
4. Record representative evidence rather than claiming to have verified every result.
5. Do not assume that every hostname is live.

## 5.5 A Search Result Is Outdated

**Possible cause:** Search engines may retain indexed information after the original page has changed or disappeared.

**Solution:** Open the original source, review its publication date if available, and record the date accessed. If the original source cannot be verified, lower the confidence of the finding or exclude it.

## 5.6 The Results Differ Between Tools

**Possible causes:**

- Different DNS resolvers.
- Caching.
- Recent record changes.
- Different query types.
- Different source update schedules.
- Different interpretations or output formats.

**Solution:** Record the sources, timestamps, and differences. Do not silently select whichever result supports a preferred conclusion.

## 5.7 A Hostname Appears in a Public Certificate Record

**Possible cause:** The hostname was included in a certificate issued or logged for the domain.

**Solution:** Record it as a certificate-related observation. Do not claim that it is currently accessible, operational, or vulnerable without appropriate authorized verification.

---

# PART 6: REVIEW QUESTIONS

Answer in your own words.

1. Define passive reconnaissance.
2. Why is passive reconnaissance important in ethical hacking?
3. Explain the difference between reconnaissance and passive reconnaissance.
4. What is the difference between passive and active reconnaissance?
5. Define OSINT.
6. Explain the difference between collecting information and producing intelligence.
7. What is an attack surface?
8. What is a domain name?
9. What is the difference between a domain and a subdomain?
10. What is DNS?
11. What is the purpose of an A record?
12. What is the purpose of an AAAA record?
13. What information can an MX record provide?
14. What is the purpose of an NS record?
15. What types of information may be published in TXT records?
16. What is WHOIS?
17. What is RDAP, and how does it differ from traditional WHOIS?
18. What is certificate transparency?
19. What is the purpose of a TLS certificate?
20. Why does a hostname in a certificate transparency log not prove that the hostname is currently accessible?
21. What is a security advisory?
22. Why must a researcher verify the product and version before concluding that a vulnerability affects a target?
23. Why should researchers record source URLs and access dates?
24. What is the difference between a confirmed observation and an assumption?
25. Why is public information not automatically proof of a security weakness?
26. What should a student do if a DNS query returns no answer?
27. Why is confidence important when documenting reconnaissance findings?
28. Why must passive reconnaissance remain within an authorized scope?
29. What information should a professional reconnaissance finding contain?
30. Explain how passive reconnaissance prepares an ethical hacker for later assessment activities.

---

# PART 7: TAKE-HOME ASSIGNMENT

## Assignment Title: Public-Source Reconnaissance Report

**Objective:** Demonstrate the ability to gather, analyze, and document publicly available information about an approved domain.

### Scenario

You have been assigned to perform an initial public-information review of an organization for a cybersecurity assessment.

Your task is to collect relevant public information without scanning the organization's infrastructure or attempting to exploit its systems.

The instructor will provide an approved domain, or students will use a permitted domain selected under the instructor's guidance.

### Task 1 — Establish the scope

Document:

- The organization or case study.
- The approved domain.
- The purpose of the research.
- The permitted information sources.
- The activities excluded from the exercise.

### Task 2 — Research the public website

Record at least three relevant observations from official public pages.

For each observation, provide the source URL and explain its relevance.

### Task 3 — Examine DNS records

Attempt to collect the following record types:

- A.
- AAAA.
- MX.
- NS.
- TXT.

Record the actual result for each query.

If a record is not returned, document that fact rather than inventing an answer.

### Task 4 — Review registration information

Use WHOIS or RDAP to examine the available registration information.

Record the fields returned and identify any information that is unavailable or redacted.

### Task 5 — Review certificate transparency information

Use a public certificate transparency source to review records associated with the approved domain.

Record at least one relevant observation if records are available.

If no useful records are returned, explain the result and the limitation.

### Task 6 — Research public technology information

Identify at least one technology explicitly documented by an official source, if available.

Find a relevant official security advisory or explain why no relevant advisory could be established from the available evidence.

Do not claim that the target is vulnerable without evidence confirming the affected product, version, and relevant conditions.

### Task 7 — Produce a findings table

Document at least five observations where sufficient evidence is available.

Each finding must contain:

- Finding ID.
- Observation.
- Source.
- Security relevance.
- Confidence.
- Limitations.
- Suggested authorized follow-up.

### Task 8 — Submit evidence

Include screenshots supporting the research activities.

Every screenshot must have a meaningful filename and be referenced in the report.

### Task 9 — Write a conclusion

Explain:

1. What you learned about the target.
2. Which public sources were most useful.
3. Which findings were well supported.
4. What remains unknown.
5. What further investigation might be appropriate if authorization were granted.

### Submission requirements

Submit a report containing:

- Cover page.
- Introduction and objectives.
- Scope and methodology.
- Findings.
- Evidence screenshots.
- References.
- Limitations.
- Conclusion.

Use your own words and actual observations. Do not copy example output and present it as a real finding.

### Suggested marking scheme — 20 marks

| Assessment area | Marks |
|---|---:|
| Understanding of passive reconnaissance | 3 |
| Correct use and interpretation of public sources | 5 |
| Accuracy of findings and supporting evidence | 4 |
| Analysis, confidence, and limitations | 3 |
| Report organization and references | 3 |
| Ethical conduct and scope compliance | 2 |
| **Total** | **20** |

---

# PART 8: CONCLUSION

Passive reconnaissance is an essential early stage of ethical hacking. It enables a security professional to gather and analyze publicly available information before conducting more direct technical assessments.

In this lab, students learn how to investigate a domain using public website information, DNS records, domain registration services, certificate transparency logs, search engines, and official security advisories.

They also learn that collecting information is only the beginning. Professional security work requires accurate interpretation, evidence, clear reporting, and an understanding of uncertainty.

A domain record does not prove that a service is accessible. A certificate record does not prove that a hostname is operational. A published security advisory does not prove that a target is affected.

Students must therefore distinguish facts from assumptions and record the limitations of every important finding.

These skills prepare students for the next lab, where they will learn **active reconnaissance** and examine how direct interaction with an authorized target can reveal additional technical information.

**End of Lab 7 — Passive Reconnaissance**

**Iconic Hub — Secure. Build. Innovate.**
