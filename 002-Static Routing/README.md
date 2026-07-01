Three-Router Static Routing & Network Segmentation Lab

Project Overview
This project demonstrates a fully functional, segmented network architecture configured within Cisco Packet Tracer. The lab connects three routers in a linear topology to enable end-to-end communication between isolated Local Area Networks (LANs) using manually configured static routing tables.

## Skills Demonstrated
* **Network Segmentation:** Dividing an IP space into distinct LAN and Wide Area Network (WAN) transit segments.
* **Static Routing:** Designing and implementing explicit next-hop routing paths.
* **Cisco IOS CLI:** Interface configuration, subnet assignment, and routing table verification.
* **Network Troubleshooting:** Isolating connection faults using ICMP (ping) and path tracing (`tracert`).

---

## Topology Architecture
The network is split into 4 separate subnets to ensure proper segmentation and traffic isolation:

* **LAN A (Internal):** End-user segment managed by Router A.
* **WAN AB (Transit):** Point-to-point backbone highway linking Router A to Router B.
* **WAN BC (Transit):** Point-to-point backbone highway linking Router B to Router C.
* **LAN C (Internal):** End-user segment managed by Router C.
