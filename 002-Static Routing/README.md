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


### IP Addressing Schema
| Device / Segment | Interface | IP Address | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- | :--- |
| **PC-A** | FastEthernet0 | `192.168.1.10` | `255.255.255.0` | `192.168.1.1` |
| **RouterA** | Gig0/0 (LAN A) | `192.168.1.1` | `255.255.255.0` | N/A |
| **RouterA** | Gig0/1 (WAN AB)| `192.168.2.1` | `255.255.255.0` | N/A |
| **RouterB** | Gig0/0 (WAN AB)| `192.168.2.2` | `255.255.255.0` | N/A |
| **RouterB** | Gig0/1 (WAN BC)| `192.168.3.1` | `255.255.255.0` | N/A |
| **RouterC** | Gig0/0 (WAN BC)| `192.168.3.2` | `255.255.255.0` | N/A |
| **RouterC** | Gig0/1 (LAN C) | `192.168.4.1` | `255.255.255.0` | N/A |
| **PC-C** | FastEthernet0 | `192.168.4.10` | `255.255.255.0` | `192.168.4.1` |

---

## Routing Table Configuration

Because routers only recognize directly connected networks by default, static routes were implemented to handle multi-hop forwarding. Traffic logic requires explicit entry and return paths.

### Router A (Edge Router)
Directly connects to `192.168.1.0` and `192.168.2.0`. Forwarding paths point to **Router B** (`192.168.2.2`):
```
ip route 192.168.3.0 255.255.255.0 192.168.2.2
ip route 192.168.4.0 255.255.255.0 192.168.2.2

ip route 192.168.1.0 255.255.255.0 192.168.2.1
ip route 192.168.4.0 255.255.255.0 192.168.3.2
```
## Router B (Core/Transit Router)
Directly connects to transit networks 192.168.2.0 and 192.168.3.0. Forwards left-bound traffic to Router A (192.168.2.1) and right-bound traffic to Router C (192.168.3.2):

```
ip route 192.168.1.0 255.255.255.0 192.168.2.1
ip route 192.168.4.0 255.255.255.0 192.168.3.2
```

## Router C (Edge Router)
Directly connects to 192.168.3.0 and 192.168.4.0. Forwarding paths point back to Router B (192.168.3.1):

```
ip route 192.168.1.0 255.255.255.0 192.168.3.1
ip route 192.168.2.0 255.255.255.0 192.168.3.1
```
