# home-lab-
Tracking my completion of my cybersecurity course and security + certification home lab network.
## 👤 Professional Profile & Dashboard
* **Name:** Gean Charles  
* **Contact:** geancharles2310@yahoo.com  
* **Status:** Actively Upskilling / Lab Engineer

---

## 📊 Completed Academic Modules & Verification

This physical lab infrastructure directly reinforces the competencies validated through my academic progress in the **Google Cybersecurity Professional Certificate** program.

### ✅ Course 1: Foundations of Cybersecurity
* **Provider:** Google 
* **Final Grade:** 🌟 91%
* **Core Competencies:** Understanding the cybersecurity landscape, identifying common security threats, vulnerabilities, and the role of an analyst in a modern organization.

### ✅ Course 2: Play It Safe: Manage Security Risks
* **Provider:** Google
* **Final Grade:** 🌟 100% (Perfect Score)
* **Core Competencies:** Risk management frameworks (NIST, CIA Triad), analyzing asset impacts, business continuity planning, and implementing security controls.

### ✅ Course 3: Connect and Protect: Networks and Network Security
* **Provider:** Google
* **Final Grade:** 🌟 96%
* **Core Competencies:** Network architecture mapping, TCP/IP and OSI models, firewall configuration, DNS management, and auditing physical/virtual infrastructure networks.

---

## 🛠️ Next Milestone (In Progress)
* **Current Objective:** Course 4: Tools of the Trade (Linux and SQL)
* **Lab Action Item:** Provisioning bare-metal **Fedora Server** and **Fedora KDE Plasma Workstations** to test active command-line network configurations and database inquiries natively over a 2.5 Gbps hardware switch.
troubleshooting case study
## 🔍 Post-Mortem Incident Report: Hardware vs. Software Compatibility

### 🚨 Incident Overview
During the initial deployment phase of the home lab array, an attempt was made to install bare-metal **Debian 13 (Bookworm)** onto the **GEEKOM A6** hardware node to serve as a local workstation. While the base operating system successfully written to disk, the deployment suffered an immediate **Network Layer critical failure**. The system was entirely unable to establish an internet or local network connection via the physical RJ45 port.

### 🔄 The Troubleshooting Command Loop
To diagnose the failure, a multi-hour terminal isolation process was executed natively on the device command line. The investigation sequence included:
* **Interface Verification:** Running `ip a` and `ip link show` to audit the state of the network interfaces. The operating system failed to register a valid logical device mapping for the hardware.
* **Network Stack Restarts:** Executing `sudo systemctl restart networking` and manually tweaking network configuration files in an attempt to force a local DHCP lease request.
* **Driver & Kernel Auditing:** Utilizing tools like `lspci -v` to pull hardware identifiers and parsing kernel ring logs via `dmesg | grep -i eth` to isolate why the network link was reporting a down state.

### 🎯 Root Cause Analysis (RCA)
The root cause was determined to be a **Hardware Compatibility List (HCL) mismatch** between Debian's legacy framework and the GEEKOM architecture:
1. **The Component:** The GEEKOM A6 utilizes a modern **Realtek 2.5GbE Multi-Gigabit Ethernet controller**.
2. **The Software Barrier:** Debian 13 prioritizes rock-solid stability by utilizing older, heavily vetted Linux kernel trees and strictly stripping out non-free or proprietary firmware by default. 
3. **The Mismatch:** The native kernel space on the installer did not contain the modern source code drivers required to drive the Realtek 2.5G chip, leaving the physical hardware entirely unrecognized by the operating system.

### 🛡️ Core Engineering Takeaway & Resolution
This deployment blocker highlighted the absolute necessity of auditing **hardware-to-software compatibility matrices** prior to production deployment. 

Instead of introducing system instability by manually side-loading external, unverified driver blobs into a fractured kernel, a strategic architectural pivot was made to the **Fedora Project ecosystem**. Because Fedora ships with cutting-edge, modern upstream Linux kernels (`6.x+`), it contains native, secure support for modern multi-gigabit chipsets out of the box—instantly resolving the loop and bringing the node fully online.
