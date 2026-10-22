21. TAKE-HOME ASSIGNMENT

Title: Basic Reconnaissance and Enumeration Report

Objective: Demonstrate an understanding of reconnaissance, enumeration, and the use of basic Linux networking tools.

Part A: Definitions

Explain the following terms in your own words:

Reconnaissance.

Enumeration.

Passive reconnaissance.

Active reconnaissance.

IP address.

Port.

Service.

DNS.

Part B: Practical Evidence

Using your own Kali Linux lab, provide evidence for the following:

Identify your hostname using hostname.

Identify your Linux distribution using cat /etc/os-release.

Display your network interfaces using ip addr.

Display your routing table using ip route.

Check local listening sockets using ss -tuln.

Run the local host discovery command nmap -sn 127.0.0.1.

Scan local web ports using nmap -p 80,443,8080 127.0.0.1.

Perform service detection using nmap -sV -p 80,443,8080 127.0.0.1.

Verify Apache2 status.

Inspect the HTTP response from your existing DVWA installation.

If your DVWA installation uses a different URL, document the URL you actually used.

Part C: Findings

Prepare a table containing at least five observations from your lab.

No.

Observation

What it means

Recommended action

1







2







3







4







5







Do not invent results. Your report must reflect what your own system displayed.

Part D: Short Discussion

Answer the following:

What information did reconnaissance reveal about your system?

What additional information did enumeration provide?

Did you identify any open ports?

What services were identified, if any?

Why is it important to verify findings instead of assuming that every result is a vulnerability?

What would you recommend to improve the security of your lab machine?

Part E: Conclusion

Write one paragraph explaining what you learned and why reconnaissance and enumeration are important in ethical hacking.

Submission Requirements

Use clear headings.

Include screenshots of actual command output.

Explain what each screenshot demonstrates.

Include the commands used.

Do not include passwords, tokens, or other secrets.

Do not scan public systems for this assignment.

Submit the report in Markdown or PDF, as directed by the instructor.

22. ASSESSMENT RUBRIC

Assessment area

Marks

Understanding reconnaissance and enumeration

20

Understanding networking concepts

15

Correct use of Linux commands and Nmap

25

Practical findings and explanations

20

Screenshots and report quality

10

Authorization and ethical conduct

10

Total

100

Students should be assessed on their understanding and the accuracy of their evidence, not merely on whether they can copy commands.

23. LAB REPORT TEMPLATE

Students may use the following template for their submission.

Reconnaissance and Enumeration Report

Student Name:
Course: ICH-EH101
Lab Number: 06
Date: October 23, 2026
Target: Local Kali Linux lab
Authorization: Instructor-approved local environment

1. Objective

Explain the purpose of the exercise.

2. Tools Used

List the tools and commands used during the practical.

3. Methodology

Describe the steps followed to gather and examine information.

4. Findings

Present the results in a table.

Finding

Evidence

Interpretation

Host information





Network configuration





Open ports





Service identification





HTTP response





5. Screenshots

Insert the screenshots and provide a short explanation under each one.

6. Security Recommendations

Explain what should be improved, if anything, based on the actual evidence.

7. Conclusion

Summarize the results and lessons learned.
