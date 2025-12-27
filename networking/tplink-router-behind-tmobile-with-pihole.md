# Working Solution: TP-Link Router Behind T-Mobile Gateway with Pi-hole

## Goal
After determining that the Ubuntu-hosted Wi-Fi access point was not suitable for long-term use, the goal shifted to introducing dedicated networking hardware to:
- Provide reliable Wi-Fi performance
- Control DHCP and DNS behavior
- Enforce Pi-hole DNS filtering for all connected devices

## Environment
- Upstream Internet: T-Mobile 5G Gateway
- Downstream Router: TP-Link AC-1250
- DNS Server: Ubuntu Server running Pi-hole
- Clients: Phones, laptops, smart TVs, and other Wi-Fi devices

## Network Topology
High-level layout:
- T-Mobile Gateway provides upstream internet access
- TP-Link router is connected downstream of the gateway
- TP-Link router manages Wi-Fi and DHCP for connected clients
- Ubuntu Server provides DNS services via Pi-hole

This architecture avoids DNS configuration limitations imposed by the ISP gateway by shifting control to the downstream router.

---

## Approach
Steps taken:
- Connected the TP-Link router behind the T-Mobile Gateway
- Configured the TP-Link router to act as the primary Wi-Fi access point
- Allowed the TP-Link router to manage DHCP for connected devices
- Planned to advertise the Ubuntu Server as the DNS server for all clients

---

## Static IP Issue on Ubuntu Server

### Problem
The Ubuntu Server did not reliably retain the same IP address when connected to the TP-Link router, which caused instability for Pi-hole DNS resolution.

Because Pi-hole functions as network infrastructure, a consistent IP address was required.

### Investigation
- Reviewed DHCP behavior on the TP-Link router
- Observed IP changes after reconnecting or rebooting the Ubuntu Server
- Verified Ubuntu network interface configuration
- Confirmed that relying on dynamic assignment introduced variability

### Root Cause
The Ubuntu Server required a stable, reserved IP address to function reliably as a DNS provider.

Dynamic addressing alone was insufficient for an infrastructure service that other devices depend on.

### Fix
A two-step approach was used to guarantee consistent addressing:

1. Reserved a specific IP address in the TP-Link router by binding the Ubuntu Server’s MAC address to the desired IP.
2. Manually configured the same static IP address on the Ubuntu Server to ensure consistency at the operating system level.

This ensured:
- The router would not assign the IP to another device
- The server would retain the correct address across reboots
- Clients always had a reliable DNS endpoint

### Validation
- Rebooted the Ubuntu Server without IP address changes
- Verified the router continued reserving the IP for the server’s MAC address
- Confirmed Pi-hole remained reachable at a consistent address
- Observed stable DNS behavior across connected client devices

---

## Pi-hole Integration with TP-Link Router

### Configuration
- Configured the TP-Link router to advertise the Ubuntu Server as the primary DNS server
- Ensured no secondary public DNS servers were configured
- Connected all client devices exclusively to the TP-Link Wi-Fi network

### Validation
- Client devices appeared in Pi-hole query logs
- DNS requests were consistently routed through Pi-hole
- Ad blocking behavior was observed across multiple device types
- Network performance was significantly improved compared to the Ubuntu-hosted AP approach

---

## Outcome
This architecture successfully achieved:
- Reliable Wi-Fi performance
- Network-wide DNS enforcement via Pi-hole
- Stable addressing for infrastructure services
- Improved throughput and overall network stability

---

## Lessons Learned
- ISP-provided gateways often restrict advanced network configuration
- Introducing a downstream router restores control over DHCP and DNS
- Infrastructure services require stable IP addressing
- MAC-based IP reservations complement host-level static configuration
- Dedicated networking hardware outperforms general-purpose hosts for Wi-Fi roles

---

## Final Notes
This configuration provided a practical and maintainable solution for network-wide ad blocking using Pi-hole, while operating within the constraints imposed by the ISP gateway.
