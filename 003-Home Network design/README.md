# Enterprise-Grade SOHO Network Design & Deployment

## Project Overview
This project demonstrates the design, physical implementation, and logical configuration of a high-performance Small Office / Home Office (SOHO) network using Cisco enterprise-grade hardware in Cisco Packet Tracer. 

Rather than utilizing generic consumer-grade equipment, this topology models industry-standard SOHO design principles—using a dedicated Cisco ISR router for centralized gateway services and DHCP allocation, a managed switch for wired segmentation, and an autonomous Access Point (AP) for secure wireless distribution.

---

## Key Skills & Concepts Demonstrated
* **SOHO Network Architecture:** Designing and deploying physically separated media zones (wired vs. wireless).
* **DHCP Scope & Pool Management:** Configuring a Cisco IOS DHCP server with strict exclusion ranges to prevent IP conflicts.
* **Wired vs. Wireless Optimization:** Aligning hardware choices with device-specific bandwidth and mobility requirements.
* **Cisco IOS Command Line:** Accessing global configuration mode, setting static IPs on physical interfaces, and launching DHCP processes.
* **Secure Wireless Implementation:** Setting up WPA2-Personal (AES) on autonomous access points.

---

## Network Topology & Architecture



The physical design separates devices based on mobility, security, and throughput needs:
1. **The Backbone (Router & Switch):** A Cisco 1941 Router connects to a Cisco 2960 Switch over a 1 Gbps GigabitEthernet link, forming the high-speed trunk of the local network.
2. **High-Speed Wired Segment:** one of the laptops is hardwired directly to the switch using copper Ethernet to guarantee interference-free, maximum throughput for streaming.
3. The other laptop, mobile phone, and tablet are connected via a wireless Access point
4. **The Static Office Segment:** The **Office Printer** is wired to the switch and assigned a fixed, static IP address. This ensures its network address never shifts, preventing client connection errors.
5. **Secure Wireless Segment:** The autonomous **Access Point (AP)** broadcasts a secure Wi-Fi signal (`SOHO_WiFi`) encrypted with WPA2-PSK (AES). All the **wireless devices** authenticate wirelessly to maintain total physical mobility.

---

## Logical IP Address & Schema Design

The network is built on the Class C private address block **`192.168.1.0/24`** (Subnet Mask: `255.255.255.0`). 

To maintain clean management, the IP address space is structured logically:
* **Infrastructure Gateway:** `192.168.1.1`
* **Static Device Range:** `192.168.1.2` – `192.168.1.20` *(Reserved for printers, servers, and switches)*
* **Dynamic Client Range (DHCP):** `192.168.1.21` – `192.168.1.254` *(Dynamically leased to TVs, Laptops, and mobile clients)*

### Address Allocation Table
| Host Name | Connection | IP Assignment | IP Address | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **SOHO-Router** | Physical Link | **Static** | `192.168.1.1` | `255.255.255.0` | N/A |
| **Office-Printer** | Wired (Cat6) | **Static** | `192.168.1.10` | `255.255.255.0` | `192.168.1.1` |
| **SOHO-Laptop Wireless** | Wireless (802.11n) | **Dynamic (DHCP)** | `192.168.1.21-254` | `255.255.255.0` | `192.168.1.1` |
| **SOHO-Laptop Wired** | Wired (Cat6) | **Dynamic (DHCP)** | `192.168.1.22` | `255.255.255.0` | `192.168.1.1` |
| **SOHO-Mobile Phone** | Wireless (802.11n) | **Dynamic (DHCP)** | `192.168.1.21-254` | `255.255.255.0` | `192.168.1.1` |
| **SOHO-Tablet** | Wireless (802.11n) | **Dynamic (DHCP)** | `192.168.1.21-254` | `255.255.255.0` | `192.168.1.1` |

---

## Cisco IOS Configuration CLI Script

Below are the exact Cisco IOS terminal configurations applied to the **SOHO-Router** to initialize the gateway interface and configure the DHCP scope.

```text
Router> enable
Router# configure terminal
Router(config)# hostname SOHO-Router

! --- 1. INTERFACE CONFIGURATION ---
SOHO-Router(config)# interface GigabitEthernet0/0
SOHO-Router(config-if)# description LAN Gateway Interface
SOHO-Router(config-if)# ip address 192.168.1.1 255.255.255.0
SOHO-Router(config-if)# no shutdown
SOHO-Router(config-if)# exit

! --- 2. DHCP SERVER SCOPE CONFIGURATION ---
! Exclude the static range (.1 to .20) from being handed out to dynamic clients
SOHO-Router(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.20

! Define the DHCP Pool properties
SOHO-Router(config)# ip dhcp pool HOME_DHCP_POOL
SOHO-Router(dhcp-config)# network 192.168.1.0 255.255.255.0
SOHO-Router(dhcp-config)# default-router 192.168.1.1
SOHO-Router(dhcp-config)# dns-server 8.8.8.8
SOHO-Router(dhcp-config)# exit
