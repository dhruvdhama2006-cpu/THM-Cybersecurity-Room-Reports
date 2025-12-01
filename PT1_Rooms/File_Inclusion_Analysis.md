🔒 TryHackMe PT1 Report: File Inclusion Analysis
Difficulty: Moderate
Category: Web Exploitation / Vulnerability Chaining
Room URL: TryHackMe | File Inclusion
Time Taken: 1.5 Hours
Personal Rating: ⭐⭐⭐/5
1. Technical Summary and Attack Chain
Feature
Description
Vulnerability Type
Local File Inclusion (LFI) and Remote File Inclusion (RFI).
Key Techniques/Tools
Directory Traversal (../), HTTP/Cookie Manipulation, Log Poisoning, Netcat.
Initial Access Vector
Exploiting LFI via path traversal to read critical system files for enumeration.
Escalation Method
Achieved through Log Poisoning, leading directly to Remote Code Execution (RCE).

2. Path Traversal and System Manipulation
The foundation of the practical exploitation was Path Traversal, specifically utilizing the ../ sequence (or variations like ..// for specific bypasses) to manipulate the URL and escape the web root. This was crucial for system enumeration, primarily targeting the following files for information gathering:
/etc/issue and /proc/version: Used to identify the target machine's operating system and kernel version.
/etc/passwd and /etc/shadow: Targeted for retrieving user list enumeration and password hashes, respectively.
This initial access demonstrated that the application's file inclusion function lacked proper input validation, allowing direct manipulation of the server's file system path.
3. Escalation and Challenging Flag Acquisition
Flag acquisition in this room was the most challenging and practical aspect, requiring a chain of vulnerabilities and browser manipulation techniques beyond simple LFI:
Flag 1 Challenge (HTTP Method Bypass): The first flag required identifying an access control mechanism based on the HTTP request method. Successful retrieval involved using browser developer tools (Inspect Element) to manually change the form method from GET to POST before submitting the request, bypassing the front-end restriction.
Flag 2 Challenge (Cookie and Domain Manipulation): A later flag required a more advanced bypass:
Cookie Modification: Using browser developer tools (specifically Storage/Application -> Cookies), the session cookie value was manually changed from a low-privilege status (e.g., guest) to a high-privilege status (e.g., admin) to satisfy an authorization check.
Domain Traversal: This cookie bypass was followed by the application of path traversal against a specific domain parameter to finally traverse the necessary file path to locate the flag.
Remote Code Execution (RCE) via Log Poisoning: The final stage involved achieving RCE by chaining LFI with Log Poisoning. A PHP payload (<?php system($_GET['cmd']); ?>) was injected into an accessible server log file and executed by instructing the vulnerable page to "include" the poisoned log, confirming the ability to perform Command Injection on the host.
4. Personal Assessment & Progression
This room marked a successful and encouraging entry into Moderate-level challenges. The eight distinct tasks provided clear structure, ensuring systematic mastery of both LFI theory and RCE practicalities. The primary achievement was gaining confidence in both technical chaining (LFI to RCE) and resourceful problem-solving using browser tooling (cookie and method manipulation). This learning experience serves as a strong foundation for future topics, especially dedicated Command Injection modules.
