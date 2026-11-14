ACTIVE DIRECTORY FUNDAMENTALS AND ATTACKS

Course: Ethical Hacking
Module: Enterprise Network Penetration Testing
Topic: Active Directory Fundamentals and Attacks
Level: Beginner to Intermediate
Operating System: Windows Server, Windows Client and Kali Linux
Tools: Active Directory Users and Computers, PowerShell, Nmap, NetExec, Impacket, BloodHound
Prepared by: Iconic PC

---

1. INTRODUCTION TO ACTIVE DIRECTORY

Active Directory (AD) is a directory service developed by Microsoft. It is used to manage users, computers, groups, permissions and other resources within a Windows network.

In an organisation, employees need access to different resources, including:

- Company computers.
- Shared folders.
- Printers.
- Internal applications.
- Databases.
- Network resources.
- Administrative systems.

Managing every computer and user individually would be difficult, especially in organisations with hundreds or thousands of employees.

Active Directory provides a centralised way to manage these resources.

For example, imagine a company with 200 employees.

Instead of creating separate user accounts on every computer, an administrator can create employee accounts in Active Directory and manage their access centrally.

When an employee logs into a domain-joined computer, Active Directory helps verify their identity and determine which resources they are permitted to access.

1.1 Why Active Directory Matters in Ethical Hacking

Active Directory is an important target in enterprise penetration testing because it often controls access to critical organisational resources.

If an attacker compromises an ordinary user account, they may attempt to identify weaknesses that could allow them to access additional systems or privileges.

An improperly configured Active Directory environment can expose an organisation to:

- Unauthorised access.
- Credential theft.
- Privilege escalation.
- Lateral movement.
- Domain compromise.
- Data exposure.
- Ransomware attacks.

Important: Active Directory attacks are not limited to attacking a single computer. They often involve identifying relationships and weaknesses across an entire network.

---

2. UNDERSTANDING ACTIVE DIRECTORY ARCHITECTURE

Before studying attacks, students must understand how Active Directory is organised.

2.1 Domain

A domain is a logical collection of users, computers and other resources managed under a common Active Directory database and security administration.

Example:

"company.local"

An organisation can use a domain to manage its employees and computers centrally.

2.2 Domain Controller (DC)

A Domain Controller is a Windows Server computer that runs Active Directory Domain Services (AD DS).

It is responsible for important tasks such as:

- Authenticating users.
- Managing domain accounts.
- Applying security policies.
- Managing access to domain resources.
- Replicating directory information with other domain controllers.

A Domain Controller is one of the most important systems in an Active Directory environment.

If an attacker gains highly privileged control of a domain controller or the domain, the consequences can be severe.

2.3 Forest

A forest is the highest-level logical structure in Active Directory.

It can contain one or more domains that share a common schema, configuration and global catalogue.

Example:

"company.local"

Possible child domains:

- "sales.company.local"
- "finance.company.local"

2.4 Organizational Units (OUs)

Organizational Units are containers used to organise objects within a domain.

For example:

- Human Resources.
- Finance.
- IT Department.
- Management.
- Marketing.

Administrators can apply Group Policy settings to OUs.

2.5 Users

User accounts represent individuals or services that need to authenticate within the domain.

Examples:

- "john.doe"
- "admin01"
- "finance.user"

Each account has security properties and permissions.

2.6 Groups

Groups allow administrators to assign permissions to multiple users instead of managing each user individually.

Examples include:

- Domain Users.
- Domain Admins.
- Enterprise Admins.
- Account Operators.
- Backup Operators.

Some groups have highly privileged permissions.

A compromised account belonging to a privileged group can expose sensitive resources.

2.7 Group Policy Objects (GPOs)

Group Policy Objects are used to manage security settings and configurations across domain computers and users.

Administrators can use GPOs to enforce:

- Password policies.
- Account lockout policies.
- Firewall settings.
- Software restrictions.
- Windows security configurations.
- Login scripts.

Poorly configured Group Policies can create security weaknesses.

---

3. HOW ACTIVE DIRECTORY AUTHENTICATION WORKS

Authentication is the process of verifying a user's identity.

When a domain user logs into a domain-joined computer, Windows typically uses Kerberos authentication in an Active Directory domain.

3.1 Kerberos Authentication

Kerberos is a network authentication protocol that uses tickets to verify identities.

It helps users authenticate to network services without repeatedly transmitting their password.

Important Kerberos components include:

Client: The user or computer requesting authentication.

Key Distribution Center (KDC): The service responsible for issuing and managing Kerberos tickets. In Active Directory, it runs on Domain Controllers.

Ticket Granting Ticket (TGT): A ticket obtained after successful initial authentication. It can be used to request access to other services.

Service Ticket: A ticket used to authenticate to a particular network service.

Simplified Kerberos Authentication Process

1. A user attempts to log in.
2. The client requests authentication from the Domain Controller.
3. The KDC verifies the user's credentials.
4. The KDC issues a Ticket Granting Ticket.
5. The user requests a service ticket for a network resource.
6. The KDC issues the service ticket.
7. The client presents the ticket to the service.
8. The service verifies the ticket and grants access if authorised.

3.2 NTLM Authentication

NTLM is an older Microsoft authentication protocol that may still be encountered in Windows environments.

It uses a challenge-response mechanism.

NTLM is generally less desirable than Kerberos in modern domain environments because it lacks some of Kerberos' security properties and is associated with several credential-relay and hash-based attack risks.

Security teams should identify where NTLM is still required and reduce unnecessary usage.

3.3 Important Active Directory Ports

Students should understand the common ports associated with Active Directory.

Port| Protocol/Service| Purpose
53| DNS| Domain name resolution
88| Kerberos| Authentication
135| MS-RPC| Remote procedure calls
139| NetBIOS| Legacy network services
389| LDAP| Directory queries
445| SMB| File sharing and Windows networking
464| Kerberos password change| Password operations
636| LDAPS| LDAP over TLS
3268| Global Catalog| Directory searches
3269| Global Catalog over TLS| Secure directory searches
3389| RDP| Remote desktop access

Not every domain controller will expose every service to every network.

---

4. ACTIVE DIRECTORY ATTACK SURFACE

An attack surface refers to the different points through which an attacker may attempt to interact with or compromise a system.

In Active Directory, common areas of interest include:

1. User accounts.
2. Password policies.
3. Domain groups.
4. Service accounts.
5. Kerberos authentication.
6. LDAP services.
7. SMB shares.
8. Group Policy configurations.
9. Domain trusts.
10. Delegation configurations.
11. Permissions and access control.
12. Domain Controller security.

An ethical hacker examines these areas to identify weaknesses before malicious attackers exploit them.

---

5. COMMON ACTIVE DIRECTORY ATTACKS

5.1 Password Spraying

Password spraying is an attack where an attacker attempts a small number of commonly used passwords against multiple user accounts.

Unlike traditional brute-force attacks, which may attempt many passwords against one account, password spraying spreads attempts across several accounts.

Example

Suppose an organisation has these accounts:

- john.doe
- mary.smith
- peter.james
- admin01

An attacker may test whether a commonly used password has been reused across multiple accounts.

Why It Can Work

Some organisations have:

- Weak password policies.
- Reused passwords.
- Default credentials.
- Inadequate account monitoring.

Prevention

- Enforce strong password policies.
- Implement MFA.
- Monitor failed authentication attempts.
- Use appropriate account lockout or smart lockout controls.
- Block commonly used and compromised passwords.
- Monitor unusual authentication patterns.

Detection Indicators

- Failed logins involving multiple accounts.
- Similar authentication patterns across many users.
- Login attempts from unusual IP addresses.
- Repeated attempts using common passwords.

---

5.2 Kerberoasting

Kerberoasting is an attack technique that targets Kerberos service accounts.

In Active Directory, service accounts may have Service Principal Names (SPNs) associated with them.

An authenticated domain user can request a service ticket for a service account.

Depending on the encryption type and account configuration, an attacker may obtain ticket material that can be subjected to offline password guessing.

How It Works Conceptually

1. An attacker obtains a valid domain account.
2. They identify service accounts with registered SPNs.
3. They request service tickets.
4. They obtain ticket material.
5. They attempt offline password guessing.
6. If a weak service-account password is recovered, the account may be compromised.

Why It Is Dangerous

Service accounts sometimes have:

- Weak passwords.
- Passwords that never expire.
- Excessive privileges.
- Poor password management.

A compromised service account may provide access to sensitive systems.

Prevention

- Use strong, randomly generated service-account passwords.
- Prefer Group Managed Service Accounts (gMSAs) where appropriate.
- Minimise service-account privileges.
- Monitor unusual Kerberos service-ticket requests.
- Review SPN assignments regularly.
- Avoid using highly privileged accounts for ordinary services.

---

5.3 AS-REP Roasting

AS-REP Roasting targets Active Directory accounts that have Kerberos pre-authentication disabled.

Normally, Kerberos requires pre-authentication before issuing an authentication response.

When pre-authentication is disabled for an account, an unauthenticated party may be able to request authentication material that can be subjected to offline password guessing.

Attack Process

1. Identify accounts with Kerberos pre-authentication disabled.
2. Request the relevant authentication response.
3. Obtain the returned material.
4. Attempt offline password guessing.
5. Determine whether a weak account password can be recovered.

Prevention

- Require Kerberos pre-authentication for ordinary accounts.
- Review accounts with unusual Kerberos settings.
- Use strong, unique passwords.
- Monitor suspicious authentication requests.
- Remove unnecessary legacy configurations.

---

5.4 Pass-the-Hash (PtH)

Pass-the-Hash is an attack technique that abuses NTLM authentication.

Instead of knowing the original password, an attacker uses a captured NTLM password hash to authenticate to another system where that hash is accepted.

How It Works Conceptually

1. An attacker obtains an NTLM hash.
2. They identify systems where the account may have access.
3. They attempt authentication using the hash.
4. If the environment accepts the authentication and the account has permission, access may be granted.

Why It Is Dangerous

A compromised administrator hash may provide access to several systems.

Prevention

- Use unique local administrator passwords.
- Deploy Windows LAPS.
- Restrict privileged account usage.
- Reduce unnecessary NTLM authentication.
- Enable Credential Guard where supported.
- Apply appropriate administrative tiering.
- Monitor suspicious NTLM logons.

---

5.5 Pass-the-Ticket

Pass-the-Ticket is an attack technique that involves stealing or reusing Kerberos tickets.

Instead of obtaining a user's password, an attacker attempts to use an already-issued Kerberos ticket to access a service.

Why It Is Dangerous

If a highly privileged Kerberos ticket is compromised, an attacker may gain access to resources associated with that identity.

Prevention

- Protect privileged accounts.
- Use separate administrative accounts.
- Restrict access to sensitive systems.
- Apply endpoint security controls.
- Monitor unusual Kerberos activity.
- Use appropriate credential protection features.

---

5.6 Golden Ticket Attack

A Golden Ticket attack involves forging Kerberos Ticket Granting Tickets using the compromised KRBTGT account's secret key.

The KRBTGT account is a special Active Directory account used by the Kerberos Key Distribution Center.

If its secret is compromised, an attacker may be able to forge tickets that impersonate domain identities.

This is a highly serious domain-level compromise.

Why It Is Dangerous

A forged ticket may be used to impersonate privileged identities and access domain resources, subject to the environment's configuration and ticket validation.

Prevention

- Protect Domain Controllers.
- Restrict privileged administrative access.
- Monitor unusual Kerberos ticket activity.
- Investigate suspected KRBTGT compromise.
- Follow Microsoft's recovery guidance, including the appropriate KRBTGT password reset procedure.
- Maintain secure and tested backups.

---

5.7 DCSync Attack

DCSync is an attack technique that abuses Active Directory replication permissions.

Normally, Domain Controllers replicate directory information between one another.

Certain replication permissions can allow an account to request password-related directory data through replication protocols.

If an attacker compromises an account with sufficient replication privileges, they may abuse those permissions to obtain sensitive credential material.

Why It Is Dangerous

This can expose highly sensitive domain credential information.

Prevention

- Restrict directory replication permissions.
- Review accounts with replication rights.
- Protect Domain Controllers.
- Monitor unusual directory replication activity.
- Apply least privilege.

---

5.8 Lateral Movement

Lateral movement is the process of moving from one compromised system to another within a network.

An attacker who compromises an ordinary workstation may attempt to access additional systems using credentials, permissions or trust relationships.

Example

1. An attacker compromises a normal employee workstation.
2. They identify accessible network shares.
3. They discover another system where the compromised account has access.
4. They move to that system.
5. They attempt to identify higher-privileged accounts.

Prevention

- Segment networks.
- Apply least privilege.
- Restrict administrative access.
- Use privileged access workstations.
- Deploy endpoint detection and response.
- Monitor unusual SMB, RDP and remote administration activity.

---

6. ACTIVE DIRECTORY ENUMERATION

Enumeration involves collecting information about an Active Directory environment.

In an authorised assessment, the objective is to understand the domain structure, identify accounts, discover exposed services and examine permission relationships.

Common information gathered includes:

- Domain name.
- Domain Controllers.
- Domain users.
- Domain groups.
- Computer accounts.
- Service accounts.
- SPNs.
- SMB shares.
- Group memberships.
- Domain policies.
- Trust relationships.

6.1 Nmap Enumeration

Nmap can help identify hosts and services exposed by a domain environment.

Example:

nmap -sV 192.168.56.10

This identifies services and attempts service-version detection.

To check selected common Active Directory ports:

nmap -sV -p 53,88,135,389,445,464,636,3268,3269 192.168.56.10

Expected Observations

Depending on firewall and server configuration, students may identify:

- DNS.
- Kerberos.
- LDAP.
- SMB.
- RPC.
- Global Catalog.

Teaching point: An open port indicates an accessible service. It does not automatically mean the service is vulnerable.

6.2 PowerShell Enumeration

On a domain-joined Windows machine, PowerShell can be used to inspect domain information.

Get domain information:

Get-ADDomain

Get domain users:

Get-ADUser -Filter *

Get domain computers:

Get-ADComputer -Filter *

Get domain groups:

Get-ADGroup -Filter *

Get members of a group:

Get-ADGroupMember "Domain Admins"

These commands require the Active Directory PowerShell module and suitable permissions.

6.3 LDAP Enumeration

LDAP is a protocol used to access and manage directory information.

Active Directory uses LDAP to expose directory objects and their attributes.

Examples of directory information include:

- Usernames.
- Group membership.
- Computer objects.
- Organizational Units.
- Service Principal Names.

An assessor should determine which information is accessible to ordinary authenticated users and whether access is appropriately restricted.

6.4 SMB Enumeration

SMB is used for Windows file sharing and other network functions.

During an authorised assessment, security testers may examine:

- Accessible network shares.
- Share permissions.
- Anonymous access.
- Sensitive files exposed to ordinary users.
- Excessive permissions.

Example Nmap script for SMB information:

nmap -p 445 --script smb-os-discovery 192.168.56.10

Another example:

nmap -p 445 --script smb-protocols 192.168.56.10

These scripts help identify SMB information and supported protocols.

---

7. IMPORTANT ACTIVE DIRECTORY SECURITY TOOLS

7.1 Nmap

Used for:

- Host discovery.
- Port scanning.
- Service identification.
- Service-version detection.
- Basic network enumeration.

7.2 NetExec

NetExec is a network security assessment framework used to interact with services such as SMB, LDAP, WinRM and other network protocols.

It can assist authorised assessors with:

- Domain environment enumeration.
- SMB information gathering.
- Authentication security checks.
- Share enumeration.
- Identifying exposed services.

It should only be used with authorised lab accounts and systems.

7.3 Impacket

Impacket is a collection of Python classes and tools for working with network protocols.

It includes utilities used in Active Directory security assessments.

Examples of its capabilities include:

- Kerberos interaction.
- SMB interaction.
- LDAP-related operations.
- Windows authentication protocol testing.
- Security assessment of domain environments.

7.4 BloodHound

BloodHound is a tool used to visualise relationships and permissions within Active Directory environments.

It helps security professionals understand how users, groups, computers and permissions are connected.

For example, BloodHound may reveal that an ordinary user has a chain of permissions that could lead to control of a privileged account.

This is called attack-path analysis.

7.5 SharpHound

SharpHound is a data collector commonly used with BloodHound.

It collects information from an authorised Active Directory environment so that relationships can be analysed in BloodHound.

7.6 PowerView

PowerView is a PowerShell-based Active Directory enumeration tool.

It can help security testers understand:

- Domain users.
- Domain groups.
- Computer objects.
- Domain policies.
- Trust relationships.
- Permissions.

---

8. PRACTICAL LAB: BUILDING AN ACTIVE DIRECTORY ENVIRONMENT

Students should not practise these techniques against a real company's network.

Build an isolated virtual lab first.

8.1 Required Lab Components

Component| Purpose
Windows Server| Domain Controller
Windows 10/11 VM| Domain-joined workstation
Kali Linux VM| Security testing machine
VirtualBox| Virtualisation
Internal Network| Isolated lab communication

Suggested lab network:

Network: 192.168.56.0/24

Windows Server:
192.168.56.10

Windows Client:
192.168.56.20

Kali Linux:
192.168.56.30

Domain:
lab.local

These are example lab addresses. Assign them to your own isolated virtual network.

8.2 Install Windows Server

1. Download a suitable Windows Server evaluation ISO from Microsoft's official website.
2. Create a new virtual machine.
3. Allocate sufficient RAM, CPU and storage.
4. Attach the Windows Server ISO.
5. Install Windows Server.
6. Configure a static IP address.
7. Rename the server to:

DC01

8. Restart the server.

8.3 Install Active Directory Domain Services

On Windows Server:

1. Open Server Manager.
2. Select Manage.
3. Select Add Roles and Features.
4. Choose Role-based or feature-based installation.
5. Select the local server.
6. Select Active Directory Domain Services.
7. Add the required features.
8. Complete the installation.

After installation, select the notification flag in Server Manager.

Choose:

Promote this server to a domain controller.

Select:

Add a new forest.

Enter the root domain:

lab.local

Complete the configuration and install.

The server will restart.

It is now configured as a Domain Controller for the new lab domain.

8.4 Configure DNS

Active Directory depends heavily on DNS.

The Windows client must be able to resolve the domain and locate the Domain Controller.

Set the Windows client DNS server to:

192.168.56.10

The IP address above represents the example Domain Controller.

Check name resolution:

nslookup lab.local

Check domain-controller discovery:

nltest /dsgetdc:lab.local

Expected result: The client should be able to locate the lab Domain Controller.

8.5 Create Domain Users

On the Domain Controller:

1. Open Server Manager.
2. Select Tools.
3. Open Active Directory Users and Computers.
4. Expand "lab.local".
5. Right-click Users.
6. Select New.
7. Select User.
8. Create example accounts:

student01
student02
helpdesk01
itadmin01

Use unique, lab-only passwords.

8.6 Create an Organisational Unit

1. Open Active Directory Users and Computers.
2. Right-click the domain.
3. Select New.
4. Select Organizational Unit.
5. Name it:

IT Department

Create another OU:

Finance Department

Move the relevant lab accounts into their appropriate OUs.

8.7 Join the Windows Client to the Domain

On the Windows client:

1. Set its DNS server to the Domain Controller.
2. Open System Properties.
3. Select the option to change the computer name or domain.
4. Select Domain.
5. Enter:

lab.local

6. Provide authorised domain credentials.
7. Restart the client.

After restarting, the client should be able to authenticate using an account created in Active Directory.

Lab checkpoint: Students should be able to log into the domain-joined workstation using their lab domain accounts.

---

9. PRACTICAL: ACTIVE DIRECTORY RECONNAISSANCE

Once the lab is working, students can begin examining it.

Exercise 1: Identify the Domain Controller

From Kali Linux:

nmap -sV 192.168.56.10

Students should record the discovered services and ports.

Exercise 2: Identify the Domain

From the Windows client:

Get-ADDomain

Students should identify:

- Domain name.
- Domain SID.
- Domain functional level.
- Domain Controller information.

Exercise 3: Enumerate Domain Users

Run:

Get-ADUser -Filter *

Students should record the usernames of the lab accounts.

Exercise 4: Examine Privileged Groups

Run:

Get-ADGroupMember "Domain Admins"

Discuss why membership of this group is sensitive.

Then inspect:

Get-ADGroupMember "Domain Users"

Explain the difference between ordinary domain users and highly privileged administrative groups.

Exercise 5: Inspect Domain Computers

Run:

Get-ADComputer -Filter *

Students should identify the domain-joined computer accounts.

---

10. UNDERSTANDING ATTACK PATHS

An attack path is a sequence of relationships, permissions or security weaknesses that could allow an attacker to move from an initial point of access to a more privileged target.

Consider this simplified example:

Ordinary User
      |
      v
Accessible Shared Folder
      |
      v
Exposed Service Credentials
      |
      v
Service Account
      |
      v
Excessive Permissions
      |
      v
Privileged System

This is a hypothetical attack path, not a guaranteed sequence.

Why Attack-Path Analysis Matters

An organisation may have many individual security weaknesses.

However, some weaknesses become much more dangerous when combined.

For example:

- An ordinary user has access to a shared folder.
- The folder contains sensitive configuration information.
- The information exposes credentials.
- Those credentials belong to an account with excessive privileges.

The combined weaknesses can create a route to sensitive systems.

This is why penetration testers examine relationships rather than only individual vulnerabilities.

---

11. DETECTION AND DEFENSIVE SECURITY

Ethical hackers should understand how security teams detect Active Directory attacks.

Attack technique| Useful detection focus
Password spraying| Repeated failed authentication across accounts
Kerberoasting| Unusual Kerberos service-ticket request patterns
AS-REP Roasting| Requests involving accounts without pre-authentication
Pass-the-Hash| Unusual NTLM authentication patterns
Pass-the-Ticket| Abnormal Kerberos ticket usage
DCSync| Unexpected directory replication activity
Lateral movement| Unusual remote logons and SMB/RDP access
Privilege escalation| Unexpected privileged group changes

Important Windows Security Event IDs

Students should become familiar with these Windows Security Event IDs.

Event ID| Description
4624| Successful logon
4625| Failed logon
4648| Logon using explicit credentials
4672| Special privileges assigned to a new logon
4720| User account created
4728| Member added to a security-enabled global group
4732| Member added to a security-enabled local group
4740| User account locked out
4768| Kerberos authentication ticket requested
4769| Kerberos service ticket requested
4771| Kerberos pre-authentication failed
4776| NTLM credential validation

Event IDs must be interpreted in context. One event alone does not prove an attack occurred.

---

12. ACTIVE DIRECTORY SECURITY BEST PRACTICES

Organisations should implement several controls to reduce the risk of Active Directory compromise.

Identity Security

- Enforce strong and unique passwords.
- Enable multifactor authentication where supported.
- Remove unused accounts.
- Disable accounts promptly when employees leave.
- Review privileged group membership.

Privilege Management

- Apply the principle of least privilege.
- Separate ordinary and administrative accounts.
- Avoid using Domain Admin accounts for daily activities.
- Restrict privileged logon locations.
- Review delegated permissions.

Network Security

- Segment the network.
- Restrict access to Domain Controllers.
- Reduce unnecessary exposed services.
- Monitor SMB and remote administration.
- Use secure protocols and configurations.

Monitoring

- Enable appropriate Windows auditing.
- Monitor authentication events.
- Monitor privileged group changes.
- Investigate unusual Kerberos activity.
- Monitor directory replication.
- Deploy endpoint detection and response.

Domain Controller Protection

- Keep Windows Server updated.
- Restrict interactive access.
- Protect system backups.
- Monitor administrative activities.
- Secure the KRBTGT account.
- Apply Microsoft's security recommendations.

---

13. STUDENT PRACTICAL ASSIGNMENT

Assignment: Active Directory Security Assessment

Objective: Examine a deliberately created Active Directory lab and identify potential security weaknesses.

Part A: Lab Setup

1. Install Windows Server.
2. Configure Active Directory Domain Services.
3. Create a domain named "lab.local".
4. Create at least four domain users.
5. Create two Organizational Units.
6. Join a Windows client to the domain.

Part B: Enumeration

1. Identify the Domain Controller.
2. Identify the domain name.
3. Enumerate domain users.
4. Enumerate domain groups.
5. Identify privileged group members.
6. Identify domain-joined computers.
7. Examine important exposed services.

Part C: Security Analysis

Students should investigate:

1. Why are privileged groups sensitive?
2. What makes a service account risky?
3. How does Kerberos authentication work?
4. What is Kerberoasting?
5. What is AS-REP Roasting?
6. How can an attacker move laterally?
7. What security controls reduce these risks?

Part D: Reporting

Submit a report containing:

- Introduction.
- Lab architecture.
- Tools used.
- Enumeration results.
- Security observations.
- Potential attack paths.
- Risk assessment.
- Recommended security controls.
- Screenshots and evidence.
- Conclusion.

Do not report hypothetical attacks as successfully executed findings.

---

14. ASSESSMENT QUESTIONS

1. What is Active Directory?
2. What is the role of a Domain Controller?
3. Explain the difference between a domain and a forest.
4. What is an Organizational Unit?
5. What is the purpose of Group Policy?
6. Explain how Kerberos authentication works.
7. What is the difference between Kerberos and NTLM?
8. What is an SPN?
9. Explain Kerberoasting.
10. What is AS-REP Roasting?
11. Explain Pass-the-Hash.
12. What is Pass-the-Ticket?
13. What is a Golden Ticket attack?
14. What is DCSync?
15. Explain lateral movement.
16. Why is Domain Admin membership sensitive?
17. What is BloodHound used for?
18. What is the principle of least privilege?
19. Name five ways to protect Active Directory.
20. Why is attack-path analysis important in enterprise penetration testing?

---

15. KEY TAKEAWAYS

Active Directory is a central component of many enterprise Windows environments.

Understanding its architecture, authentication mechanisms, users, groups and permissions is essential for anyone interested in enterprise penetration testing.

An ethical hacker should be able to:

- Understand domain architecture.
- Identify Domain Controllers.
- Enumerate users, groups and computers.
- Understand Kerberos and NTLM.
- Recognise common Active Directory attack techniques.
- Analyse permissions and attack paths.
- Identify security weaknesses.
- Recommend appropriate remediation.

Remember: The objective of Active Directory penetration testing is not simply to compromise a Domain Controller. It is to understand how an attacker could abuse identity, authentication, permissions and trust relationships to compromise an organisation.

---

Prepared by: Iconic PC

Building. Securing. Learning.
