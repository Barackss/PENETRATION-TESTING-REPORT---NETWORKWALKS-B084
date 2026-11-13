# PENETRATION-TESTING-REPORT---NETWORKWALKS-B084
Conducting footprinting and network scanning phases on networkwalks as an authorized intern.


W2-PM-FINAL | CYBERSECURITY | NETWORKWALKS
🚧 Project Overview


Field	Detail
Pentester	Barack Kiambati


🎯 1. Liability Disclaimer
The activities documented in this report were conducted strictly on systems and devices for which prior written authorization was obtained, or on personally owned hardware. The contents of this document are intended strictly for educational and research purposes. The instructor, author, and Networkwalks assume no liability for the misuse or misapplication of this knowledge; all subsequent actions taken by the reader are their sole responsibility. Unauthorized system access constitutes a criminal offense in most jurisdictions, regardless of whether data or systems are damaged, and can result in severe legal penalties, significant fines, career impact (employment termination), and a permanent criminal record.

⚠️ Note: Authorization must be Secured from Networkwalks before performing reconnaisance & footprinting

☄️ 2. Introduction
As part of my Networkwalks internship program (week 2), this report provides a comprehensive analysis of the Footprinting/Reconnaissance phases targeting the networkwalks.com domain and subsequent scanning of the local network utilizing Zenmap. By leveraging a combination of Kali Linux tools and Zenmap, this assessment simulates how a malicious actor actively maps live network hosts. The methodology mirrors an attacker's lifecycle, transitioning from passive data gathering (W2-PM1) to active host discovery (W2-PM5).

💡 Footprinting
All footprinting commands were executed within a Kali Linux environment.

🔎 Scanning activities
Were performed via Zenmap on a windows host (pc).

📝 Methodology, analysis framework, and documentation
To ensure reproducibility and rigorous documentation, every technical step outlines the exact commands utilized, corresponding system output, verification screenshots, and a brief analytical note from the attackers point of view regarding the security implications of the exposed data.

⚒️ 3. Tools Used
Each tool used and its purpose can be found in the table below.

Tool/commands	Purpose
Kali Linux	Operating system deployed to conduct initial reconnaissance and information gathering.
Windows OS	Host Operating systems utilized for networking activities including network mapping via Zenmap.
WHOIS	Queries or Finds domain registration details such as registration ownership, critical dates, and authoritative name servers.
whatweb	Identifies and fingerprints web technologies, including servers, CMS - content management system, plugins, and IP.
nslookup	Performs DNS lookup to resolve target domain names to their respective IP address
curl -I	Fetches, analyses, and reads the HTTP response headers of the website.
wafw00f	Probes the web application to detect the presence and type of Web Application Firewall (WAF) present.
dnsrecon	Enumerate or discovers all DNS records (NS, MX, SPF, TXT, SRV).
Zenmap (Nmap GUI)	Scan the local subnet to find live hosts, active IP addresses, and MAC addresses.
Windows CMD	Local IP and MAC address identification via a command line interface.
🛡️Activities Performed

4. Footprinting & Reconnaissance Attacks With Multiple Kali Tools
4.1. Background
Reconnaissance (also called as footprinting) is the first step in any real attack or security test. Before touching a target, an attacker quietly collects as much public information about it as possible. This includes who owns the domain, its real IP address, the hosting provider, the web technologies it runs, its DNS and mail records, and whether a firewall is protecting it. All of this comes from information the target has already made public, so the target never even knows it is being studied. This is why recon is powerful and very hard to detect.

⚠️ In this lab, the live website networkwalks.com was footprinted.

💡4.2. Footprinting & Reconnaissance
I performed reconnaissance against the networkwalks.com domain using six Kali Linux commands:

WHOIS
WhatWeb
Nslookup
Curl 
Wafw00f 
DNSRecon
⚠️ Note: Each tool was used to collect a different type of information about the target as seen in table 3 above.

4.2.1. WHOIS
WHOIS was used to obtain publicly available domain registration information and identify the domain’s name servers. The results provided information about the domain registration and hosting infrastructure.

Command used: whois networkwalks.com
<img width="1365" height="767" alt="whois1results" src="https://github.com/user-attachments/assets/3a0346cd-6da0-4833-af75-cd78cd563fae" />
<img width="1365" height="767" alt="whois2 results" src="https://github.com/user-attachments/assets/4979a2b8-b0af-417f-8b4c-9136e2cb3cc4" />
⚠️ Security relevance: Here the name servers point to HostGator, so an attacker instantly learns the hosting provider. Registration dates, and abuse contacts help with social engineering and planning.

4.2.2. Whatweb
WhatWeb was used to identify and fingerprint technologies used by the website.

Command used: whatweb networkwalks.com
<img width="1365" height="767" alt="whatweb results" src="https://github.com/user-attachments/assets/033e2e45-e6a4-4984-8332-c217123b9a0c" />
⚠️ Security relevance: whatweb exposes the exact software and versions (here Wordpress 7.1.4 and WP Download Manager 3.3.76). An attacker looks these versions up in vulnerability databases to find known exploits. It also leaks the server IP and an email address.

4.2.3 Nslookup
Nslookup was used to resolve the targets domain name to its IP address. The provided result identified 192.232.216.135.

Command used: nslookup networkwalks.com;
<img width="1365" height="767" alt="nslookup results" src="https://github.com/user-attachments/assets/7b7b0731-653e-46b0-82df-4873e956e5cf" />
⚠️ Security relevance: nslookup turns a domain name into its real IP address (192.232.216.135). Knowing the IP lets an attacker scan the server directly, look up other sites on the same IP, and map the target's infrastructure.

4.2.4 curl
Curl was used with the -I option to inspect the HTTP response headers. This provided additional information about the web application and exposed the WordPress REST API endpoint /wp-json/.

Command used: curl -I https://networkwalks.com
<img width="1365" height="767" alt="HTTP curl analysis" src="https://github.com/user-attachments/assets/92c35a04-2e38-470e-9d04-09c816c73a58" />
⚠️ Security relevance: HTTP headers leak the web server, caching stack and hidden endpoints (here the WordPress REST API at /wp-json/). Attackers read headers to fingerprint the stack and find entry points without even loading the full page.

4.2.5 Wafwoof
wafwoof was used to determine whether a Web Application Firewall was protecting the website. The result identified ModSecurity (SpiderLabs) Web Application Firewall.

Command used: wafwoof networkwalks.com
<img width="1365" height="767" alt="waf results" src="https://github.com/user-attachments/assets/73f857de-3d47-4052-aef7-85db747c43ae" />
⚠️ Security relevance: wafwoof tells an attacker if a firewall is watching. Here the site sits behind ModSecurity (SpiderLabs). Knowing a WAF is present shapes the whole attack: naive attempts will be blocked or logged, so the attacker must adapt or try to bypass it.

4.2.6 dnsrecon
dnsrecon was used to enumerate DNS records. The results provided information relating to name servers, mail servers, SPF/TXT records, service records and DNS software information.

Command used: dnsrecon -d networkwalks.com
<img width="1365" height="767" alt="dnsrecon results" src="https://github.com/user-attachments/assets/7da5748c-6386-4619-a2b2-112c318bf552" />
⚠️ Security relevance: dnsrecon maps the target's entire DNS footprint: mail servers, DNS software version (Bind 9.16.23), SPF policy and cPanel service records. Each record is a potential foothold and helps an attacker understand the email and hosting setup.

5. Network Scanning with Zenmap
5.1. Background
Zenmap is the official GUI version of Nmap which can be used on Windows PC. It is a security scanner software tool which is used by Cybersecurity professionals & Hackers. It is a multi-platform (Linux, Windows, Mac OS X, BSD, etc.) free and open source application which aims to make Nmap easy for beginners to use while providing advanced features for experienced Nmap users. Frequently used scans can be saved as profiles to make them easy to run repeatedly.

5.2. Introduction and Objectives
For this second activity, I used Zenmap to perform network discovery on my local network. This practical required me to:

a). Identify my local IP address and subnet
b). Discover live hosts
c). Identify their IP addresses
d). Identify their MAC addresses
e). Generate a network topology
5.2.1. Procedure
The Windows ipconfig command was entered in my cmd prompt to identify my local IP address and LAN subnet.
I then entered the subnet into Zenmap and selected Ping Scan to identify active hosts.

<img width="1366" height="768" alt="Screenshot (667)" src="https://github.com/user-attachments/assets/9baed7b9-6c86-4449-9699-7333507b3a9c" />

<img width="1366" height="768" alt="Screenshot (668)" src="https://github.com/user-attachments/assets/d7f89945-c2f7-4331-9438-a5237b3b2753" />

6. Networkwalks Zenmap Quiz Results
<img width="1366" height="768" alt="Screenshot (669)" src="https://github.com/user-attachments/assets/662b8942-5cd2-4965-8fd1-e5b393f1c34f" />

7. Risk Analysis & Threat Impact
Passive Reconnaissance Information Exposure: Publicly accessible DNS, MX, and WHOIS metadata allow external attackers to map corporate web infrastructure without triggering intrusion alarms.
Internal Subnet Exposure: Open ports and unsegmented broadcast domains allow potential lateral movement once an initial network foothold is gained.

9. Security Recommendations
Implement WHOIS privacy protection on corporate domains to obscure administrative contact vectors.
Enforce internal network micro-segmentation (VLANs) and ARP inspection controls to limit lateral discovery.
Harden exposed web servers and update Web Application Firewall (WAF) filtering rulesets.

11. Conclusion
All Phase 1 and Phase 2 tasks specified in the Week 2 curriculum were successfully executed and validated. Technical evidence including CLI output logs, host IP/MAC tables, screenshot captures, and exported topology graphics have been compiled for portfolio verification.

Author
Barack  Kiambati
Cybersecurity Intern — NetworkWalks
Batch: B084 LinkedIn: www.linkedin.com/in/barack-kiambati-b29208300



