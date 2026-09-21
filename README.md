# 🚀 2.5 GbE Home Lab Deployment & Sandbox Isolation Log

## 📝 Executive Summary
Over a multi-stage deployment window, a major home lab infrastructure overhaul was successfully completed. An unstable, corrupted graphical desktop operating system on a **Geekom A5 mini PC** was completely decommissioned, wiped, and redeployed as a dedicated, headless **Fedora Server 44** node. This node joins a high-speed **2.5 GbE network topology** controlled via an **M1 Mac (OrbStack)** and a **Geekom A6 (Fedora KDE)** workstation.

---

## 🌐 Current System Architecture & Hardware Topology
The home lab environment has been established with full **2.5 GbE line-rate throughput** across the internal switching fabric:

| Device Node | Hardware Platform | Operating System / Environment | Primary Function |
| :--- | :--- | :--- | :--- |
| **Core Router** | TP-Link Omada 2.5 GbE | Omada SDN Framework | Core Routing & Hardware Firewall |
| **Core Switch** | Dedicated 2.5 GbE Switch | Layer 2 Switching Fabric | Local High-Speed Data Backplane |
| **Workstation 1**| Apple Mac mini / Studio | **macOS (M1)** + **OrbStack** | Primary Workstation & Docker Engine |
| **Workstation 2**| Geekom A6 (AMD Ryzen) | **Fedora KDE Plasma** | Local Graphical Dev Node |
| **Server Node** | Geekom A5 | **Fedora Server 44 (Headless)** | Isolated Sandbox & KVM Hypervisor |

---

## 🛠️ Phase 1: Bare-Metal Server Deployment
### 1. Troubleshooting Corrupted Installation Media
* **Issue:** The initial installer loop encountered severe `erofs: read error -5` and black screen crashes. This was diagnosed as corrupted block data and leftover partition signatures from a broken KDE configuration on the USB flash drive.
* **Resolution:** Performed a hard electrical reboot to clear volatile system cache and utilized **KDE ISO Image Writer** alongside low-level terminal diagnostics (`wipefs`) to strip conflicting partition identifiers. A pristine **Fedora Server 44 Netinst/DVD** image was cleanly written back to the SanDisk installation media.

### 2. Drive Decommissioning & OS Initialization
* Bypassed "Troubleshooting/Rescue" modes to execute a pure bare-metal deployment.
* Targeted the **Internal NVMe SSD** storage drive on the Geekom A5, explicitly unchecking the temporary boot media.
* Applied **Automatic Storage Configuration** with explicitly **disabled encryption** to ensure seamless, headless autonomous boot sequences during power recovery cycles.
* Utilized the Anaconda Installer filesystem wizard to issue a master **Delete All / Reclaim Space** instruction, permanently vaporizing legacy remnants.
* Provisioned the system as an exclusive **Root Administrator** instance (`root@localhost`), confirming successful local network interface synchronization (`enp5s0`).

---

## 🛡️ Phase 2: Next Steps & Unidirectional Sandbox Isolation
The immediate next phase focuses on locking down the newly initialized Geekom A5 node before spinning up legacy, experimental virtual sandboxes (e.g., Hannah Montana Linux).

### ⚙️ Planned Security Implementations:
1. **Stateful Unidirectional Gateway ACLs:** Configuring the TP-Link Omada SDN controller to allow management packets out of the M1 Mac into the Geekom A5, while strictly dropping any traffic initiated *by* the A5 seeking to move back into the private workstation pool.
2. **Local Software Hardening:** Executing `firewalld` configuration profiles on the server node to block all peripheral incoming communication vectors, restricting entry exclusively to **Port 22 (SSH)** and **Port 9090 (Fedora Cockpit)**.
3. **Headless Disconnect:** Safely severing the HDMI monitor and external keyboard layouts to allow the Geekom A5 cluster node to function natively as an independent, silent background server.
