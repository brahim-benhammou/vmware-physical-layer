# Underlay Switch Baseline

## Purpose

Layer 3 underlay configuration for a segmented VMware lab environment using 802.1Q trunking.

* R640 workload hosts carry all VLANs
* R630 management plane host carries only Management, VM Network, and DevOps VLANs

---

## VLAN Plan

* VLAN 10 — Management
* VLAN 20 — vMotion
* VLAN 30 — vSAN
* VLAN 100 — VM Network
* VLAN 110 — DevOps

---

## VLAN Creation

```bash
conf t
 vlan 10
  name MANAGEMENT
 vlan 20
  name VMOTION
 vlan 30
  name VSAN
 vlan 100
  name VM_NETWORK
 vlan 110
  name DEVOPS
end
write memory
```

---

## R640 ESXi Workload Hosts (10Gb SFP+) — Trunk All VLANs

**Assumptions**

* Te3/1/1 = R640 Host 01 (10Gb)
* Te3/1/2 = R640 Host 02 (10Gb)

```bash
conf t
 interface TenGigabitEthernet3/1/1
  description R640-HOST-01-10GB
  switchport
  switchport trunk encapsulation dot1q
  switchport mode trunk
  switchport trunk allowed vlan 10,20,30,100,110
  spanning-tree portfast trunk
  no shutdown

 interface TenGigabitEthernet3/1/2
  description R640-HOST-02-10GB
  switchport
  switchport trunk encapsulation dot1q
  switchport mode trunk
  switchport trunk allowed vlan 10,20,30,100,110
  spanning-tree portfast trunk
  no shutdown
end
write memory
```

---

## R630 Management Plane Host — Limited VLAN Trunk

**Role**

* Management plane and core services host
* Not used for workload placement, vMotion, or vSAN traffic

```bash
conf t
 interface GigabitEthernet3/0/3
  description R630-MGMT-PLANE
  switchport
  switchport trunk encapsulation dot1q
  switchport mode trunk
  switchport trunk allowed vlan 10,100,110
  spanning-tree portfast trunk
  no shutdown
end
write memory
```

---

## Management Workstation — Access VLAN 10

```bash
conf t
 interface GigabitEthernet3/0/5
  description MGMT-PC
  switchport
  switchport mode access
  switchport access vlan 10
  spanning-tree portfast
  no shutdown
end
write memory
```

---

## Jumbo Frames

MTU set to 9000 to support high throughput storage and migration traffic, reducing CPU overhead and improving performance.

VMware side must also be configured with MTU 9000 for:

* vMotion VMkernel (VLAN 20)
* vSAN VMkernel (VLAN 30)

```bash
conf t
 system mtu jumbo 9000
end
write memory
re
```
