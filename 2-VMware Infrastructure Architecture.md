
# VMware Infrastructure Design
## Goal

Design a segmented and highly available VMware infrastructure platform that supports reliable operations, automation workflows, and realistic lab testing aligned with enterprise architecture practices.
## Purpose

Provide a stable foundation for testing infrastructure automation, high availability features, and workload deployment scenarios in a controlled lab environment.

## 1. Physical Hardware Inventory (The "Underlay")

| Device | Model | Role | Primary Network |
| :--- | :--- | :--- | :--- |
| **Host 1** | Dell PowerEdge R640 | Production Node A | **10Gb SFP+** |
| **Host 2** | Dell PowerEdge R640 | Production Node B | **10Gb SFP+** |
| **Host 3** | Dell PowerEdge R630 | Management & Witness Node | **1Gb Ethernet** |
| **Switch** | Layer 3 Switch | catalyst 3750-x series| 10Gb SFP+ / 1Gb Ports |

---

## 2. Logical Network Design (Class A Schema)

| VLAN ID | Name | Subnet (CIDR) | Gateway | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **VLAN 1** | WAN Uplink | DHCP / ISP | ISP Gateway | Internet Edge |
| **VLAN 10** | Management | 10.10.10.0/24 | 10.10.10.1 | ESXi, vCenter, AD, DNS, firewall  |
| **VLAN 20** | vMotion | 10.10.20.0/24 | 10.10.20.1 | 10Gb Live VM Migration |
| **VLAN 30** | vSAN/Storage | 10.10.30.0/24 | 10.10.30.1 | 10Gb SSD Data Traffic |
| **VLAN 100** | VM Network | 10.10.100.0/24 | 10.10.100.1 | Ubuntu VMs / App Factory |
 **VLAN 110** | DevOps Tools |10.10.110.0/24 | 10.10.110.1 |Automation tools: Git, Ansible, Terraform runner, Jenkins, AWX, monitoring |

---

## 3. Core Services (VLAN 10)

| Service | IP Address | Hostname | Role |
| :--- | :--- | :--- | :--- |
| **OPNsense** | 10.10.10.1 | firewall.lab.local | NAT, Routing, Edge Security |
| **Active Directory** | 10.10.10.10 | ad-01.lab.local | Identity & DNS  |
| **vCenter Server** | 10.10.10.20 | vcenter.lab.local | Infrastructure Management |

## 5. Storage Architecture

The workload cluster uses VMware vSAN to provide centralized shared storage across the hosts.

Each workload host is equipped with four 1TB drives configured in JBOD mode with no hardware RAID, allowing direct disk access for vSAN disk groups and accurate health monitoring.

This design enables distributed software defined storage providing data redundancy, high availability, and improved performance for virtual machine workloads.

Storage traffic is isolated on VLAN 30 using dedicated 10Gb networking to ensure low latency and high throughput.


---

## 4. Reliability Strategy
- **High Availability (HA):** 3-node cluster with the R630 acting as the **Witness/Quorum** node.
- **Enhanced vMotion (EVC):** Enabled to allow live-migration between different CPU generations (R630/R640).
- **Traffic Isolation:** High-speed storage and vMotion traffic isolated to dedicated 10Gb SFP+ ports.
