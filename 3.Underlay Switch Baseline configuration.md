
# Underlay Switch Baseline 

## Purpose
Layer 3 underlay configuration for a segmented VMware lab using 802.1Q trunks.
- R640 hosts carry all VLANs 
- R630 management plane host carries only Management + VM Network + DevOps Tools

---

## VLAN Plan
VLAN 10   MANAGEMENT
VLAN 20   VMOTION
VLAN 30   VSAN
VLAN 100  VM_NETWORK
VLAN 110  DEVOPS

## VLAN Creation
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

---

## R640 ESXi Workload Hosts (10Gb SFP+) - Trunk ALL VLANs
Assumptions:
Te3/1/1 = R640 Host 01 (10Gb)
Te3/1/2 = R640 Host 02 (10Gb)

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

---

## R630 Management Plane Host - Trunk LIMITED VLANs (10, 100, 110)
R630 role:
- Management plane / core services host (vCenter,opnsense_firewall, AD,DNS )
- NOT used for workload VM placement, vMotion, or vSAN data traffic


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

---

## Management Workstation / Admin PC - Access VLAN 10
Assumption:
Gi3/0/5 = Management PC

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


## Enable Jumbo Frames (vMotion / vSAN)  to transfer large amounts of data faster with less CPU overhead and lower latency 
conf t
system mtu jumbo 9000
end
write memory
reload
VMware side must also be set to MTU 9000 for:
- vMotion VMkernel (VLAN 20)
- vSAN VMkernel (VLAN 30)

---

## Verification Commands
show vlan brief
show interfaces trunk
show interfaces te3/1/1 switchport
show interfaces te3/1/2 switchport
show interfaces gi3/0/3 switchport
show interfaces gi3/0/24 switchport
show interfaces status
show mac address-table dynamic
