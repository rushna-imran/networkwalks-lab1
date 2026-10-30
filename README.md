## Project Overview
Setting up a cyber lab isn't about throwing tools at a system and seeing what breaks; it's about building a controlled, isolated testing ground where you actually know what's happening on the wire. 

This project documents the deployment of an isolated virtual lab environment tailored for penetration testing, network analysis, and ethical hacking practice. Configured on a dedicated private virtual network, the setup isolates testing traffic from host infrastructure while leaving immediate room to spin up victim targets (like Metasploitable or custom vulnerable servers) down the line.

> **Important Security & Ethics Note:**  
> This lab environment is strictly built for educational purposes and authorized security testing. All tools, scans, and techniques executed within this infrastructure must strictly target systems you explicitly own or have explicit written permission to test. Unauthorized access to external networks or systems is illegal.

---

## Lab Architecture & Purpose

### Purpose of the Lab
This lab provides a safe sandbox for hands-on cybersecurity learning and testing, specifically designed to support:
* Network reconnaissance and port scanning
* Vulnerability assessments and target enumeration
* Packet analysis and traffic inspection
* Web application security testing
* Controlled exploit execution and security tool testing

### Target Topology Diagram
+--------------------------------------------------------------------+
|                         Windows 11 Host                            |
|                     (Physical Intel i3 Host)                       |
+--------------------------------------------------------------------+
                                 |
                     [ Oracle VirtualBox 7.2 ]
                                 |
                  +------------------------------+
                  |    NatNetwork (10.0.0.0/24)  |
                  +------------------------------+
                                 |
                 +---------------+---------------+
                 |                               |
        +-----------------+             +-----------------+
        |  Kali Linux VM  |             |  Future Target  |
        |  10.0.2.15/24   |             |   VMs (Range)   |
        +-----------------+             +-----------------+

---

## Objectives
* Install and configure Oracle VirtualBox.
* Import and deploy Kali Linux as the primary attack/analysis virtual machine.
* Create and assign a private NatNetwork for multi-VM lab expansion.
* Configure static network connectivity and routing within Kali Linux.
* Assign and verify consistent IP addressing and DNS resolution.
* Capture a clean baseline VM snapshot for instantaneous recovery.
* Fully document the setup process as an operational reference for future cybersecurity projects.

---

## Component Configuration & Hardware Specs

| Component | Detailed Configuration |
| :--- | :--- |
| **Host OS** | Windows 11 |
| **Host RAM** | 20 GB |
| **Host CPU** | Intel Core i3-1125G4 |
| **Hypervisor** | Oracle VM VirtualBox 7.2 |
| **Guest OS** | Kali Linux 2026.2 |
| **Allocated VM RAM** | 4096 MB (4 GB) |
| **Allocated Processors**| 2 vCPUs |
| **Virtual Network Type**| Private NAT Network (NatNetwork) |
| **Subnet Range** | 10.0.0.0/24 |
| **Kali Assigned IP** | 10.0.2.15/24 |
| **Default Gateway** | 10.0.2.1 |
| **DNS Server** | 1.1.1.1 / VirtualBox Internal DNS |
| **Future Target Range** | 10.0.2.16 – 10.0.2.254 |

---

## Lab Setup Procedure

### Step 1: VirtualBox Installation & NAT Network Creation
1. Installed Oracle VirtualBox 7.2 on the Windows 11 host.
2. Navigated to Tools -> Network -> NAT Networks.
3. Created a new NAT Network named NatNetwork configured to support inter-VM communication and outward internet routing.
<img width="1360" height="768" alt="Screenshot (2)" src="https://github.com/user-attachments/assets/3154aaed-3de7-46f4-b0e4-ee0ef1394b68" />

### Step 2: Kali Linux Import & Hardware Allocation
1. Imported the official Kali Linux 2026.2 pre-built virtual appliance.
2. Configured hardware resource limits to balance performance:
   - Base Memory: 4096 MB
   - Processors: 2 Cores
   - Graphics RAM: 128 MB
   - Network Adapter: NAT Network (Name: NatNetwork)
<img width="1280" height="655" alt="VirtualBox_kali-linux-2026 3-virtualbox-amd64_30_10_2026_20_02_41" src="https://github.com/user-attachments/assets/5e05f555-a11e-4482-ba96-35bd970ab110" />


### Step 3: Network Adapter Binding & Internal Settings
1. Open VM Settings -> Network.
2. Set Adapter 1 to Attached to: NAT Network.
3. Selected NatNetwork from the name dropdown and enabled Promiscuous Mode: Allow VMs for future packet sniffing capabilities across target nodes.
<img width="1360" height="768" alt="Screenshot (9)" src="https://github.com/user-attachments/assets/79a8d8f6-58ec-4c0d-bf86-d6d722917f30" />

---

## Verification & Testing

The environment was validated using standard network diagnostic commands inside the Kali Linux terminal:

| Verification Test | Terminal Command | Expected / Verified Result | Status |
| :--- | :--- | :--- | :--- |
| **IP Assignment** | ip a | eth0 displays valid IP 10.0.2.15/24 | **PASS** |
| **Routing Table** | ip route | Default route configured via 10.0.2.1 | **PASS** |
| **Gateway Reachability** | ping -c 4 10.0.2.1 | 4 packets sent, 0% packet loss, instant response | **PASS** |
| **Internet Routing** | ping -c 4 1.1.1.1 | ICMP replies returned with 0% packet loss | **PASS** |
| **State Preservation** | VirtualBox UI Snapshot | Snapshot taken & named cyber Lab1 | **PASS** |
<img width="1280" height="655" alt="VirtualBox_kali-linux-2026 3-virtualbox-amd64_30_10_2026_20_08_46" src="https://github.com/user-attachments/assets/10fe8953-53fe-4007-8b55-7fc41ff891cc" />
<img width="1280" height="655" alt="VirtualBox_kali-linux-2026 3-virtualbox-amd64_30_10_2026_20_31_40" src="https://github.com/user-attachments/assets/4f0d6b93-e97c-4055-a89a-0a994382a755" />

---

## Problems Encountered & Solutions

* **Issue: Host Resource Contention & VM Boot Stalls**
  * *Symptom:* During initial startup, the guest VM experienced boot delays due to background Windows update tasks and temporary saved state lock files following a system reboot.
  * *Solution:* Cleared residual hypervisor state logs (Discard Saved State), verified host RAM availability, and allocated a clean 4 GB RAM/2 CPU allocation to keep system responsiveness smooth.

---

## Key Learnings & Takeaways

* **Architectural Network Isolation:** Understood the functional difference between Standard NAT (isolated host translation) and NatNetwork (creating an isolated virtual switch where multiple attack and target VMs can interact directly while sharing single-point internet access).
* **Network Troubleshooting:** Re-enforced command-line network verification practices using ip, ping, and routing inspection tools to diagnose interface bindings systematically rather than relying on GUI indicators.
* **Hypervisor Recovery Protocols:** Learned the necessity of establishing baseline VM snapshots immediately after initial configuration to allow quick rollbacks during future exploit experiments.

---

## Tools & Resources
* **Hypervisor:** Oracle VM VirtualBox 7.2
* **Operating System:** Kali Linux 2026.2
* **Core Utilities:** iproute2, ping, nmap, wireshark
* **Curriculum:** NetworkWalks Cybersecurity Internship

---

## Author & Project Info

* **Author:** Junior SOC / Cybersecurity Trainee
* **Program:** NetworkWalks Cybersecurity Internship
* **Module:** Week 01 - Cybersecurity Lab Environment Setup
* **Repository:** Official Internship GitHub Workspace
* **LINKEDIN:** [https://www.linkedin.com/in/rushna-imran-3338303b6/]
