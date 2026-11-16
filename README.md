# codomax-module-02-reconnaissance
Module 02: Linux, Security Tools & Reconnaissance — Lab Report & README
Intern Name: Akshat Dave
Program: Codomax Cybersecurity Virtual Internship
Module: Module 02 (Day 6 – Day 10)
1. Linux Fundamentals & System Security Basics
Linux forms the backbone of security operations and server infrastructure. Understanding file hierarchy, permission models, and process monitoring is essential for hardening systems and conducting assessments.
File & Directory Navigation: Core system directories include /etc (configuration files), /var/log (system and security logs), /bin & /usr/bin (user binaries), and /home (user directories).
Users, Groups & Permissions: Linux enforces access control using Read (r=4), Write (w=2), and Execute (x=1) permissions across Owner, Group, and Others. Sensitive files like /etc/shadow restrict read access strictly to the root superuser.
Process & Service Management: Active processes and services are monitored and controlled using ps aux, top, and systemctl to detect unauthorized background tasks.
# Permission and user management examples
chmod 640 lab_report.txt          # Owner: rw-, Group: r--, Others: ---
chown akshat:security lab_report.txt
ps aux | grep sshd                # Verify active SSH daemon process
2. Passive vs. Active Reconnaissance
Reconnaissance is the initial phase of security assessment focused on mapping an organization's digital footprint and attack surface.
Attribute
Passive Reconnaissance
Active Reconnaissance
 
Target Interaction
Zero direct packets sent to target systems; relies on public OSINT sources.
Direct network probing and packet exchange with target hosts/services.
Detection Risk
Virtually undetectable by target IDS/IPS or firewall logs.
High visibility in target logs; triggers intrusion detection alerts.
Techniques & Tools
WHOIS lookups, public DNS records, Certificate Transparency logs, Shodan.
Port scanning (Nmap), service banner grabbing, directory brute-forcing.

3. Essential Security Tools Overview
Nmap (Network Mapper): Network discovery and security auditing tool used for host discovery, port scanning, OS fingerprinting, and service version enumeration.
Wireshark: Packet analyzer that captures and inspects live network traffic at the protocol level to diagnose anomalies or clear-text credential leaks.
Burp Suite: Intercepting HTTP/HTTPS proxy used to inspect, modify, and replay web application requests and responses.
OWASP ZAP (Zed Attack Proxy): Open-source web application security scanner used for automated passive/active vulnerability discovery and spidering.
4. Controlled Reconnaissance Lab Findings
Lab Scope: Isolated virtual lab host (192.168.56.102) running a test web application stack inside a host-only virtual network.
# Step 1: Passive DNS & domain record enumeration (simulated lab domain)
whois labtarget.local
dig labtarget.local ANY +noall +answer

# Step 2: Active service & version enumeration with Nmap
nmap -sC -sV -p 21,22,80,8080 192.168.56.102

# Step 3: HTTP traffic capture filter in Wireshark
http.request.method == "POST" || http.response.code == 200
Tool Used
Target Component
Discovered Finding & Remediation
 
Nmap (-sV)
Port 21/TCP (vsftpd 2.3.4)
Legacy FTP service exposed; disable FTP and migrate file transfers to SFTP (Port 22).
Wireshark
Port 80/TCP HTTP Stream
Login form transmitted credentials in plaintext; enforce TLS/HTTPS across all endpoints.
Burp Suite / OWASP ZAP
Port 8080/TCP Web App
Missing security headers (X-Frame-Options, CSP); configure headers on web server.


