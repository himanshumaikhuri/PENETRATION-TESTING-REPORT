# PENETRATION TESTING REPORT 

### Footprinting & Network Scanning Phases
**W3-PM-FINAL | CYBERSECURITY | NETWORKWALKS**

| Field | Detail |
| :--- | :--- |
| **Pentester Name (Cybersecurity Professional)** | Himanshu Maikhuri |
| **Program/Batch** | B083-Networkwalks |
| **Date** | 15 September 2026 |
| **Modules completed** | W2-PM1 (Multiple Kali Tools)<br>W2-PM5 (Zenmap Scanning) |
| **Client/Target** | 1. Networkwalks (secured written permission already)<br>2. My own local LAN Network |
| **Permission secured from client?** | Yes |
| **Phases covered** | Phase 1: Reconnaissance & Footprinting<br>Phase 2: Scanning & Network Discovery |

---

## 1. Introduction

This report covers footprinting the `networkwalks.com` domain using multiple Kali Linux tools (W2-PM1) and scanning my own local network with Zenmap (W2-PM5). One module covers the footprinting phase and the other covers the scanning phase, so together they show how an attacker moves from gathering public information to mapping live hosts on a network. It is the Week 2 part of my ongoing internship program at Networkwalks.

All commands were run in Kali Linux (footprinting) and on a Windows PC with Zenmap installed (scanning). Every step below includes the exact command used, the result I observed, a screenshot as evidence, and a short note on why the finding matters from an attacker's point of view.

---

## 2. Tools Used

The table below lists each tool used in this report and its purpose:

| Tool | Purpose |
| :--- | :--- |
| **Kali Linux & Windows** | Operating systems used for reconnaissance activities |
| **WHOIS** | Find domain registration details (owner, dates, name servers). |
| **whatweb** | Fingerprint web technologies (server, CMS, plugins, IP). |
| **nslookup** | Resolve the domain name to its IP address using DNS. |
| **curl -I** | Read the HTTP response headers of the website. |
| **wafw00f** | Detect whether a Web Application Firewall protects the site. |
| **dnsrecon** | Enumerate all DNS records (NS, MX, SPF, TXT, SRV). |
| **Zenmap (Nmap GUI)** | Scan the local subnet to find live hosts, IPs and MAC addresses. |
| **Windows CMD** | Local IP and MAC address identification |

---

## 3. Activities Performed

### 3.1 Footprinting & Reconnaissance

I performed reconnaissance against the `networkwalks.com` domain using six Kali Linux tools: **WHOIS**, **WhatWeb**, **Nslookup**, **Curl**, **Wafw00f**, and **DNSRecon**. Each tool was used to collect a different type of information about the target.

1. **WHOIS:** Used to obtain publicly available domain registration information and identify the domain’s name servers. The results provided information about the domain registration and hosting infrastructure.
2. **WhatWeb:** Identified technologies used by the website, including WordPress 7.0.4 and WP Download Manager 3.3.58, along with other exposed information.
3. **Nslookup:** Resolved the domain name to its IP address (`192.232.216.135`).
4. **Curl (`curl -I`):** Inspected HTTP response headers. Exposed the WordPress REST API endpoint `/wp-json/`.
5. **Wafw00f:** Determined whether a Web Application Firewall (WAF) was active. Identified **ModSecurity (SpiderLabs)**.
6. **DNSRecon:** Enumerated DNS records, including name servers, mail servers, SPF/TXT records, service records, and DNS software information.

### 3.2 Network Scanning with Zenmap

For the second activity, I used Zenmap to perform network discovery on my local network. The practical required me to identify my local IP address and subnet, discover live hosts, identify their IP and MAC addresses, and generate a network topology.

1. I first used the Windows `ipconfig` command to identify my local IP address and LAN subnet.
2. I entered the subnet into Zenmap and selected **Ping Scan** (`nmap -sn`) to identify active hosts.
3. The discovered live hosts were:
   * `10.0.0.1`
   * `10.0.0.2`
   * `10.0.0.129`
   * `10.0.0.254`
4. The results also included corresponding MAC addresses for the VMware virtual adapters and gateways.
5. After completing the scan, I opened the **Topology** section in Zenmap, enabled the legend, and exported/saved the network topology.

---

## 4. Risk Analysis / Impact

Based on the information collected during footprinting and network scanning activities, the following potential risks were identified:

| # | Risk / Finding | Evidence / Observation | Potential Impact | Risk Level |
| :-: | :--- | :--- | :--- | :-: |
| **1** | Web technology information exposed | WhatWeb identified WordPress and WP Download Manager | Attackers may use exposed technology/version information to identify software requiring further security review | 🟡 Medium |
| **2** | Server IP address identifiable | Nslookup resolved the domain to `192.232.216.135` | Provides information about the network location of the web service | 🟢 Low |
| **3** | HTTP technical information exposed | Curl returned HTTP response headers and exposed `/wp-json/` | May assist technology fingerprinting and further enumeration | 🟢 Low |
| **4** | WAF technology identifiable | Wafw00f identified ModSecurity (SpiderLabs) | Reveals information about the web application’s security architecture | 🟢 Low |
| **5** | DNS infrastructure information exposed | DNSRecon identified DNS, mail and service-related records | DNS information can help build a broader infrastructure profile | 🟡 Medium |
| **6** | Multiple live hosts visible on local network | Zenmap identified four live hosts in the network | Unknown or unauthorized devices may potentially be present on a network | 🟡 Medium |

> **Risk Level Key:** 🔴 Critical | 🟡 Medium | 🟢 Low

*Note: The risks above are observations from footprinting and scanning exercises, not confirmed vulnerabilities. The practical exercises primarily involved information gathering and host discovery. No exploitation or vulnerability validation was performed.*

---

## 5. Recommendations

Based on the observations from these activities, I recommend the following security improvements:

* **Review publicly exposed technology information:** Organizations should regularly review what information about their web technologies, CMS, and plugins is publicly visible.
* **Keep software updated:** CMS platforms, plugins, and web components should be regularly updated and audited against known vulnerabilities.
* **Review HTTP headers:** Disable unnecessary information headers (e.g., `X-Powered-By`, server banners) to prevent reconnaissance.
* **Review DNS records regularly:** Periodically check DNS configurations to ensure only active, intended services are publicly exposed.
* **Properly configure and monitor the WAF:** Keep ModSecurity rules tuned and updated to block automated scanning and injection attempts.
* **Perform regular internal network discovery:** Scan internal networks periodically to audit active devices and unauthorized endpoints.
* **Investigate unknown devices:** Any unexpected device discovered during network scans should be quarantined and verified.
* **Maintain network documentation:** Keep topology diagrams and device inventories updated.
* **Perform security testing with authorization:** Ensure all reconnaissance and scanning activities strictly adhere to agreed scopes.

---

## 6. Conclusion

During Week 2 of my Cybersecurity & Ethical Hacking internship, I completed practical activities covering footprinting, reconnaissance, and network scanning.

In the footprinting activity, I used six Kali Linux tools to collect public intelligence on the target domain. I learned how WHOIS provides domain ownership details, WhatWeb fingerprints web frameworks, Nslookup resolves DNS names, Curl checks HTTP headers, Wafw00f detects firewall solutions, and DNSRecon enumerates DNS infrastructure.

In the network scanning activity, I used Zenmap to discover active hosts, MAC addresses, and generate a subnet map. The exercises demonstrated that information gathering is a crucial initial phase in security testing, enabling professionals to evaluate exposures before executing active assessments.

---

## 7. Evidences Collected

Screenshots collected as evidence during the activities are stored in the `/screenshots` directory:

* `screenshots01-whois-whatweb.png`
* `screenshots02-nslookup-curl.png`
* `screenshots03-wafw00f-dnsrecon.png`
* `screenshots04-zenmap-scan-results.png`
* `screenshots05-zenmap-topology.png`

---
*- End of Report -*
