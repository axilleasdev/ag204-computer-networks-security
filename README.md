# AG204 – Computer Networks and Security

Network design for a new 3-floor corporate building, developed for the **AG204 – Computer Networks and Security** module (University of Essex).

The project covers IP subnetting, network design with VLANs & inter-VLAN routing in Cisco Packet Tracer, a network security risk analysis, and packet analysis with Wireshark.

---

## Network Topology

![Network Topology](diagrams/network-topology.png)

Router-on-a-Stick design with 1 router (Cisco 2911) and 3 switches (one per floor), serving 5 departments through 5 VLANs.

---

## Repository Contents

| Folder / File | Description |
|---------------|-------------|
| `AG204_110-12256.pkt` | Cisco Packet Tracer file (Part B) |
| `diagrams/` | Network topology diagram |
| `wireshark/` | Analysis & screenshots (Part D) |

---

## Project Summary

### A — Subnetting
The ISP-provided network `83.112.8.128/25` is divided into 5 subnets using a **/28 (255.255.255.240)** mask, one per department (14 usable hosts each):

| Department | Subnet | Host Range | Gateway |
|------------|--------|------------|---------|
| Sales | 83.112.8.128/28 | .129–.142 | .129 |
| Service | 83.112.8.144/28 | .145–.158 | .145 |
| Management | 83.112.8.160/28 | .161–.174 | .161 |
| E-commerce | 83.112.8.176/28 | .177–.190 | .177 |
| Marketing | 83.112.8.192/28 | .193–.206 | .193 |

### B — Network Design (Packet Tracer)
Star topology with Router-on-a-Stick. Each floor has its own switch; departments are logically separated into VLANs (10, 20, 30, 40, 50), and routing between them is handled via sub-interfaces on the router.

### C — Network Security
Analysis of 8 key security risks (Malware, Phishing, DDoS, MitM, Unauthorized Access, SQL Injection/XSS, Insider Threats, Physical Security) with corresponding mitigation measures.

### D — Wireshark Analysis
Analysis of the `FileTrace1.pcapng` capture file (DNS, FTP, POP3, etc.), answering 10 questions with supporting packet screenshots.

---

## Technologies

`Cisco Packet Tracer` · `Wireshark` · `Cisco IOS` · `VLANs` · `802.1Q` · `Subnetting`

---

> University of Essex — BSc Computing (Artificial Intelligence)
