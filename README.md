# PENESTRATION-TESTING-REPORT-
PENETRATION TESTING REPORT
FOOTPRINTING & NETWORK SCANNING PHASES
W2-PM-FINAL | CYBERSECURITY | NETWORKWALKS
Report detail	Information
Pentester Name	Janet Nnaemeka
Program / Batch	B084 – NetworkWalks
Date	11 November 2026
Modules	W2-PM1 (Multiple Kali Tools); W2-PM5 (Zenmap Scanning)
Targets	networkwalks.com for footprinting; own local LAN for network discovery
Permission	Written permission for the domain exercise is stated as secured by the author.
Phases	Reconnaissance & Footprinting; Scanning & Network Discovery

1. Liability Disclaimer and Scope
This report documents reconnaissance, footprinting and network-discovery exercises completed for the stated training modules. The author states that written permission was secured for the domain footprinting exercise. Zenmap scanning was directed at the author’s own local network range, 172.20.10.2/24. This report is limited to the supplied screenshots. It does not claim exploitation or vulnerability validation. Findings are observations, not confirmed vulnerabilities.
2. Introduction and Objectives
The objective was to practise the early stages of a penetration-testing workflow: reconnaissance and footprinting, followed by scanning and network discovery. The exercises used Kali Linux tools and Zenmap to collect publicly observable domain information and inspect the author’s local network.
Identify domain registration and nameserver information.
Identify web technologies and the website’s resolved IP address.
Detect the presence of a Web Application Firewall.
Review HTTP response headers and DNS records.
Discover hosts visible on the authorised local network range.
3. Tools Used
Tool	Purpose
WHOIS	Domain registration, status, privacy and nameserver lookup.
WhatWeb	Web server, CMS, plugins and technology identification.
Nslookup	DNS query and domain-to-IP resolution.
curl -I	HTTP response-header retrieval.
Wafw00f v2.3.1	Web Application Firewall fingerprinting.
DNSRecon	DNS record enumeration.
Zenmap / Nmap	Local network host discovery and quick scanning.
Kali Linux & windows	Operating environment for the command-line tools.
Windows CMD	Local IP and MAC address identification 

4. Activities Performed and Results
4.1 WHOIS – Domain Registration Footprinting
WHOIS returned domain registration information for networkwalks.com. The registrant details were privacy-protected through Domains By Proxy, LLC. The screenshot showed registrar-related information associated with Tempe, Arizona, USA, domain status restrictions, and four nameservers.
WHOIS item	Observed result
Privacy service	Domains By Proxy, LLC; registrant identity masked.
Location shown	Tempe, Arizona, USA.
Nameservers	NS6135.HOSTGATOR.COM; NS6136.HOSTGATOR.COM; NS29.DOMAINCONTROL.COM; NS30.DOMAINCONTROL.COM.
Domain statuses	clientTransferProhibited; clientUpdateProhibited; clientRenewProhibited; clientDeleteProhibited.
DNSSEC field	WHOIS output displays “DNSSEC: unsigned”.

Observation: WHOIS exposed domain status and nameserver information while masking the registrant identity. The DNSSEC field is recorded as shown; it is not an independent DNSSEC validation.

Figure 1. WHOIS output for networkwalks.com.
4.2 WhatWeb – Web Technology Identification
WhatWeb identified technologies associated with networkwalks.com. The screenshot shows an HTTP redirect and a successful HTTPS response (200 OK).
Item	Observed result
Web server	Apache
CMS	WordPress 7.1.4
WordPress Download Manager	3.3.76
Bootstrap	7.1.4
jQuery	3.7.1
Website title	NetworkWalks Academy
IP address	192.232.216.135
Other technologies	Google Tag Manager; HTML5.

Observation: The tool identified the website platform and components. Technology identification alone does not prove that any component is vulnerable.

Figure 2. WhatWeb technology-identification results.
4.3 nslookup – DNS Resolution
The first two nslookup attempts timed out. A subsequent attempt successfully queried Google Public DNS (8.8.8.8) and returned a non-authoritative answer.
Item	Observed result
Domain	networkwalks.com
DNS server	8.8.8.8
DNS port	53
Resolved IP	192.232.216.135
Response	Non-authoritative answer

Observation: DNS resolution eventually succeeded. The screenshots do not establish the cause of the earlier timeouts.

Figure 3. nslookup output showing initial timeouts followed by successful resolution.
4.4 Wafw00f – WAF Fingerprinting
Wafw00f v2.3.1 checked https://networkwalks.com and identified ModSecurity (SpiderLabs) as the Web Application Firewall. The tool reported two requests.
Item	Observed result
Tool version	Wafw00f v2.3.1
Target	https://networkwalks.com
Detected WAF	ModSecurity (SpiderLabs)
Requests	2

Observation: WAF technology was identifiable. This does not assess the effectiveness of its rules or configuration.

Figure 4. Wafw00f output identifying ModSecurity (SpiderLabs).
4.5 DNSRecon – DNS Enumeration
DNSRecon completed enumeration for networkwalks.com and reported eight records. The visible output includes SOA, NS, A, MX, TXT and SRV-related information.
Record / field	Observed result
Enumeration	8 records found; completed.
SOA nameserver	ns6135.hostgator.com
NS records	ns6135.hostgator.com; ns6136.hostgator.com
A record	networkwalks.com → 192.232.216.135
MX record	mail.networkwalks.com → 192.232.216.135
Nameserver IPs displayed	50.87.144.87; 192.232.216.131
BIND version string	9.16.23-RH
TXT records	Google site-verification value and SPF record beginning v=spf1.
SRV records	cPanel email autodiscovery records.
DNSSEC query	No answer reported for the DNSSEC query.

Observation: DNSRecon revealed nameserver, mail, address, TXT and service-discovery information. The absence of an answer to the DNSSEC query alone does not prove DNSSEC is disabled. The displayed BIND version should be verified with the service owner before drawing conclusions about patch status.

Figure 5. DNSRecon output showing DNS records and the enumeration summary.
4.6 curl -I – HTTP Response Header Review
curl -I was used to retrieve headers from https://networkwalks.com. The screenshot shows earlier failures (connection reset and a temporary name-resolution failure), followed by successful HTTP/2 200 responses.
Header / response	Observed result
HTTP status	HTTP/2 200
Server	Apache
Content-Type	text/html; charset=UTF-8
Link	WordPress REST API-related links, including /wp-json/ and an API endpoint.
Set-Cookie	__wpdm_client cookie; Secure and HttpOnly attributes visible.
Referrer-Policy	no-referrer-when-downgrade
X-Endurance-Cache-Level	0
X-Nginx-Cache	WordPress
Permissions-Policy	Header present with listed origins and directives.

Observation: Successful responses exposed server and WordPress-related metadata, including REST API links and cookie attributes. The earlier connection and DNS errors are also part of the captured result. Header presence alone does not establish insecure configuration.

Figure 6. curl -I output showing failed attempts followed by successful HTTP/2 200 responses.
4.7 Zenmap – Local Network Scanning
Zenmap was used with the Quick scan profile against 172.20.10.2/24. The command displayed was: nmap -T4 -F 172.20.10.2/24.
Scan detail	Observed result
Target range	172.20.10.2/24
Profile	Quick scan
Command	nmap -T4 -F 172.20.10.2/24
LAN IPs shown	172.20.10.1; 172.20.10.2; 172.20.10.3
Localhost	Also shown as the scanning machine.
Services pane	Lists FTP, domain, IMAP, Microsoft-DS, MS-SQL-S, SMTP and other service names.

Observation: The topology screenshot displays three LAN IP addresses plus localhost. This report therefore records three LAN IPs, not four remote hosts. The Services pane does not clearly map each service to a particular IP address, so no host-to-service mapping is asserted.

Figure 7. Zenmap topology for the 172.20.10.2/24 local network range.
5. Risk Analysis and Potential Impact
The following are preliminary qualitative assessments of information exposure, not confirmed vulnerability severities. Further authorised validation and asset-owner context would be required for formal risk ratings.
Observation	Evidence	Indicative level	Potential impact / limitation
Web technology identifiable	Apache, WordPress 7.1.4 and WP Download Manager 3.3.76.	Low–Medium	May help focus further research; no component vulnerability confirmed.
Domain registration information visible	WHOIS status and nameservers; registrant identity masked.	Low	Provides administrative and infrastructure context.
Public DNS resolution	192.232.216.135 returned by nslookup.	Low	Can assist infrastructure mapping; normal for public websites.
WAF technology identifiable	ModSecurity (SpiderLabs) detected.	Low	Reveals a defensive technology but not its effectiveness.
DNS service information visible	Eight DNSRecon records, including NS, A, MX, TXT and SRV.	Low–Medium	Can assist service mapping; no DNS vulnerability confirmed.
HTTP metadata exposed	Server, cache, policy, cookie and WordPress API link headers.	Low	May disclose implementation details; headers alone do not prove misconfiguration.
LAN hosts discoverable	Three LAN IP addresses visible in Zenmap topology.	Informational	Shows host visibility within the scanned local subnet.

6. Recommendations
Maintain an inventory of the website’s CMS, plugins, server software and dependencies, and apply supported security updates.
Review response headers and remove unnecessary implementation details where practical without disrupting functionality.
Review WordPress REST API exposure and ensure sensitive data and administrative functions have appropriate access controls.
Maintain and periodically review ModSecurity rules, logs and updates; WAF detection alone is not proof of effective application security.
Review DNS records regularly and remove obsolete or unnecessary records.
Verify DNSSEC configuration with the domain administrator and authoritative DNS provider rather than relying only on this enumeration output.
Review SPF and other email-authentication records with the mail administrator.
Maintain an inventory of local network devices and restrict access to services according to operational need.
Investigate intermittent DNS and connection failures if reproducible.
Perform any further testing only with explicit written authorisation and a defined scope.
7. Limitations
This report is based on the supplied screenshots and does not include complete raw scan files.
WHOIS, DNS, HTTP and technology-fingerprinting results can change over time.
The Zenmap topology supports three visible LAN IP addresses plus localhost, not four remote hosts.
The Zenmap screenshot does not clearly associate every service name with a specific IP address.
No exploitation or vulnerability validation is evidenced by the supplied screenshots.
8. Conclusion
The exercises provided practical experience with reconnaissance, domain footprinting, DNS enumeration, web-technology identification, WAF fingerprinting, HTTP-header review and local network discovery. The tools identified publicly observable information about networkwalks.com, including web technologies, its resolved IP address, DNS and mail-related records, nameservers and detectable WAF technology. The Zenmap topology showed three LAN IP addresses in the scanned local network range, in addition to localhost.
The results demonstrate the value of reconnaissance and scanning in understanding a target’s visible attack surface. The observations should guide defensive review and must not be interpreted as confirmed vulnerabilities. Further assessment would require a defined scope, appropriate authorisation and additional validation.


## Report Evidence

![whois ](<img width="1920" height="1191" alt="image" src="https://github.com/user-attachments/assets/0113a27d-b070-4a5e-8b44-9edb0b04f7f5" />

whatweb <img width="1920" height="1191" alt="image" src="https://github.com/user-attachments/assets/1c7a4d4f-e5e0-4574-8737-b11173e3e844" />

wafwoof https://github.com/janetijeomannaemeka/PENESTRATION-TESTING-REPORT-/blob/main/wafwoof%20screenshot%20results.png?raw=true
