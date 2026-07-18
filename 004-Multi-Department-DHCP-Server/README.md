#  Enterprise Multi-Scope DHCP Server Deployment

##  Project Architecture
This repository implements a centralized Multi-Scope DHCP Server solution utilizing a Cisco ISR platform. The design services multiple organizational business units (Accounting and Engineering) across isolated network layers, optimizing address space utilization while enforcing department-level segmentation.

##  Engineering Objectives Achieved
* **Multi-Pool Allocation:** Programmed individual Class C scopes matching distinct physical interface broadcast paths.
* **DHCP Verification :** Analyzed live address lease tracking matrices using internal IOS monitoring hooks (`show ip dhcp binding`).

---

## Applied Cisco IOS Shell Commands

```text
! Global exclusion criteria
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10

! Scope declaration for Accounting Segment
ip dhcp pool ACCOUNTING_POOL
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8

! Scope declaration for Engineering Segment
ip dhcp pool ENGINEERING_POOL
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 4.2.2.2
