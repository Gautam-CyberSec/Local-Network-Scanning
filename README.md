# Local Network Port Discovery & Service Assessment

## Objective
Discover open TCP ports on devices in the local network, identify services running on those ports, capture scan traffic for analysis, and provide a concise risk assessment with recommended mitigations.

## Tools Used
- [Nmap](https://nmap.org/)  
- [Wireshark](https://www.wireshark.org/) 

## Folder Content
- [report.md](https://github.com/Gautam-CyberSec/Local-Network-Scanning/blob/main/Reports/Reports.md)  
- [screenshots](https://github.com/Gautam-CyberSec/Local-Network-Scanning/tree/main/Screenshots)  
- [scan_results.txt](https://github.com/Gautam-CyberSec/Local-Network-Scanning/blob/main/Reports/scan_result.txt) 
- [scan_results.html](https://github.com/Gautam-CyberSec/Local-Network-Scanning/blob/main/Reports/scan_result.html)  

## Conclusion
The assessment identified DNS (TCP 53) on one host and HTTP-related services (TCP 80 and TCP 8000) on another. DNS misconfiguration (open recursion or AXFR) and exposed development/admin interfaces on port 8000 present the highest immediate risk (abuse, information leakage, or unauthorized access). Recommended immediate actions: restrict DNS recursion/AXFR, firewall or restrict access to port 8000 (bind to localhost or VPN-only), patch web/DNS software, and enforce HTTPS for web services (redirect 80→443). After quick mitigations, perform deeper application testing and continuous monitoring.

## Contact

For questions, please contact me at https://discord.gg/zZHyANFq
