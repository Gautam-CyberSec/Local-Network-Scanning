# Task: Local Network Port Discovery & Service Assessment

## Objective
Discover open TCP ports on devices in the local network, identify the services running on those ports, and assess potential security risks and recommended mitigations.

## Summary of Steps Performed

### Step1. CHecking IP:

`ip a`

<img width="719" height="372" alt="Image" src="https://github.com/Gautam-CyberSec/Local-Network-Scanning/blob/main/Screenshots/Screenshot%202025-09-22%20174141.png" />

### Step2. nmap scanning:

`nmap -sS 192.168.X.X -oA scan_result`

<img width="719" height="372" alt="Image" src="https://github.com/Gautam-CyberSec/Local-Network-Scanning/blob/main/Screenshots/Screenshot%202025-09-22%20181243.png" />


### Step3. Checked in wireshark with filer:

`(ip.src == 192.168.X.X && ip.dst == 192.168.X.X) || (ip.src == 192.168.X.X && ip.dst == 192.168.X.X)`

<img width="719" height="372" alt="Image" src="https://github.com/Gautam-CyberSec/Local-Network-Scanning/blob/main/Screenshots/Screenshot%202025-09-22%20175728.png" />

## Findings (Observed Hosts & Open Ports)
> Host A  
> - IP: 192.168.X.X  
> - Open port(s): 53/tcp  
> - Reported service: DNS (domain)

> Host B  
> - IP: 192.168.X.X  
> - Open port(s): 80/tcp, 8000/tcp  
> - Reported service: HTTP / web application (port 80). App/dev/admin interface or alternate HTTP server (port 8000).

## Research on Open Ports & Typical Service Context

### TCP 53 — DNS (domain)
- Typical role: Resolves hostnames to IPs (recursive resolver for clients or authoritative server for zones).  
- Common software: BIND, dnsmasq, Unbound, PowerDNS, Microsoft DNS.  
- Common uses in LANs: Local name resolution, DHCP-integrated DNS, caching for clients.

### TCP 80 — HTTP
- Typical role: Public or internal web server serving web pages, redirects, or API endpoints.  
- Common software: Apache, Nginx, IIS, lighttpd, reverse proxies.

### TCP 8000 — Alternate HTTP / App / Management Interface
- Typical role: Development servers, web application backends, admin consoles, internal APIs, or staging instances.  
- Common software: Django dev server, Gunicorn, Node.js/Express, Tomcat, web-based administration tools.

---

## Potential Security Risks (per service) — Priority & Impact

### DNS (port 53)
- High priority risks
  - Open recursive resolver: Can be abused for DNS amplification DDoS.  
  - AXFR (zone transfer) enabled: Attackers could retrieve full zone information revealing internal hostnames and IPs.  
  - Unpatched DNS software: Known remote vulnerabilities can allow takeover or remote code execution.
- Impact: Service abuse (DDoS amplification), information disclosure, potential server compromise.
- Indicators to check: Responses to recursive queries from arbitrary clients; successful AXFR requests; server version and patch status.

### HTTP (port 80)
- Medium–High priority risks
  - Outdated web server or frameworks: May contain CVEs leading to RCE, path traversal, or other exploits.  
  - Directory listing / default pages: May leak sensitive files (backups, config, credentials).  
  - No HTTPS: Credentials and session tokens may be transmitted in cleartext.
- Impact: Data leakage, account compromise, website defacement, remote code execution.
- Indicators to check: Server headers, default content, known vulnerable software versions, exposed admin pages.

### Alternate HTTP / App (port 8000)
- High priority risks
  - Exposed development/debug interfaces: Debug consoles or dev servers often leak secrets or allow code execution.  
  - Admin/UIs with default credentials: Easily abused if not protected.  
  - Internal APIs exposed to the network: May expose privileged endpoints not intended for public access.
- Impact: High chance of information leak, unauthorized access, remote exploitation.
- Indicators to check: Presence of debug banners, default pages, accessible admin consoles, unauthenticated endpoints.

## Conclusion
The scan indicates a DNS service (port 53) on one host and HTTP-style services on another host (ports 80 and 8000). DNS misconfiguration (open recursion or AXFR) and exposed development/admin interfaces on port 8000 are the highest immediate concerns because they enable service abuse, information leakage, and potential compromise. HTTP on port 80 is a moderate concern if it is unpatched or serving sensitive content without HTTPS.

Immediate recommended actions: restrict DNS recursion and AXFR, block or restrict access to port 8000 to trusted hosts/VPN only, patch exposed services, and enforce HTTPS for web traffic. After these quick mitigations, perform deeper application testing and continuous monitoring.
