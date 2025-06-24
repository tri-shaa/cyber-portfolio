# cyber-portfolio

Skills Overview
Operating System: Linux (Kali preferred)

Networking Basics: OSI Model, IP, MAC, Ports, TCP/UDP, HTTP/S

Security Tools Used:

nmap – Network scanning

linpeas.sh – Local privilege escalation checks

nikto – Basic web vulnerability scanner

netcat – Simple backdoor + listener tool

burp suite – Web traffic analysis


Nmap Scanning (My System) :

I used the basic nmap command to scan my system at first. 
![Basic Nmap Scan](findings/nmap1.png)
Findings:  No specific http ports were mentioned due to firewall blockings. Only 1 IP address was up. 

 Hence I opted for a more aggressive scan.
![Basic Nmap Scan](findings/nmap2.png)
Findings:  No specific http ports were mentioned due to firewall blockings. Only 1 IP address was up.

I tried to scan my own machine after that:
Nmap scan --> I generated my own IP using: ( ip a | grep inet )
              then scanned it: ( nmap -sS -T4 <your-IP-here> )
![Basic Nmap Scan](findings/nmap3.png)
Findings: due to advanced firewall settings it still didnt show any specific http port.

Nmap scan local host--> I started a basic web server using Python:  ( python3 -m http.server 80 )
                        Then started a basic nmap scan: ( nmap -sV -Pn localhost )
![Basic Nmap Scan](findings/nmap4.png)
Findings : Detected an open HTTP port (80) with service fingerprinting
           Got version info: SimpleHTTPServer 0.6 (Python 3.12.7)

- `-sS`: Stealth/SYN scan
- `-T4`: Faster scan timing
- `-Pn`: Skip host discovery (assume host is up)
- `-sV`: Service version detection

###  What I Learned from Nmap Scanning

Through this process, I learned how to use both basic and advanced Nmap commands to scan localhost and external IPs. I also understood how firewall settings and inactive services affect port visibility. To validate scanning accuracy, I hosted a web server myself and confirmed open ports. This exercise taught me not just tool usage, but also troubleshooting. 


Privilege Escalation Check using linpeas.sh

  What is linpeas.sh?
linpeas.sh is a post-exploitation script used for local privilege escalation enumeration on Linux systems. It detects misconfigurations and insecure setups that attackers can use to gain root access.
-----
--


 Web Vulnerability Scanning (Nikto)

 Nikto is an open-source web server scanner.

It checks a web server for:

- Vulnerabilities
- Misconfigurations
- Insecure files
- Outdated software versions
-Known exploits

Basic syntax:  nikto -h <target-website-or-IP>
I scanned -- testphp.vulnweb.com  --which is a local website.
![Basic Web Scan](findings/websan1.png)
![Basic Web Scan](findings/websan2.png)
Findings : This site is running nginx as a web server
           It's using an old PHP version (5.6.40)
           X-Frame-Options Header Missing -- This allows clickjacking attacks
           X-Content-Type-Options missing	--	MIME type sniffing & XSS possible
           Exposed PHP version	--	Makes fingerprinting & exploitation easier
           
 I the tried to scan my local machine using IP --
 ![Basic Web Scan](findings/websan3.png)
 Findings : It showed 0 hosts scanned as my request was blocked dude to the host not being up or my request getting blocked due to presence of firewall.

 What Nikto Scans For: 
   - Insecure files
   - Default credentials	Login pages with default usernames
   - Misconfigurations	HTTP methods like PUT, TRACE
   -Software versions	Outdated Apache, Nginx, PHP
   - CVE checks	Known vulnerabilities
   -Headers

Limitations of Nikto : 
 - It’s a noisy scanner — easily detected by firewalls.
 - Doesn’t do deep authentication-based scans.
 - Can’t detect modern JavaScript-heavy web app issues.
 - Not stealthy (don't use it for silent pentests).

Nikto is a powerful web vulnerability scanner that helps identify outdated software, insecure headers, and misconfigurations on web servers. It’s easy to use and great for learning the basics of web security testing in ethical hacking.








 

