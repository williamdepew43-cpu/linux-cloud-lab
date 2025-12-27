# First Attempt: Using Ubuntu Server as a Wi-Fi Router / Access Point

## Goal
Before introducing additional networking hardware, the initial goal was to configure the Ubuntu Server to act as a Wi-Fi router/access point so that:
- Wi-Fi clients could connect directly to the Ubuntu host
- The Ubuntu host (running Pi-hole) could enforce DNS filtering for connected devices
- The overall setup could remain simple with fewer devices involved

## Environment
- Upstream Internet: T-Mobile 5G Gateway
- Ubuntu Server host with built-in Wi-Fi capability
- Pi-hole installed on the Ubuntu Server

## Approach
High-level approach:
- Enabled Wi-Fi on the Ubuntu Server
- Configured the Ubuntu Server to broadcast a Wi-Fi SSID (acting as an access point)
- Connected client devices to the Ubuntu-hosted Wi-Fi network
- Intended for all client DNS traffic to be handled locally by Pi-hole

---

## Troubleshooting: Wi-Fi Broadcast Without Internet Access

### Problem
After broadcasting the Wi-Fi network from the Ubuntu Server, client devices were able to connect to the SSID but initially did not have internet access.

### Symptoms
- Devices successfully connected to the Ubuntu-hosted Wi-Fi network
- No outbound internet connectivity from client devices
- DNS resolution and external traffic failed

### Investigation
Steps taken:
- Verified the Ubuntu Server itself had working internet access
- Confirmed the Wi-Fi interface was broadcasting correctly
- Checked routing between the Wi-Fi interface and the upstream internet connection
- Reviewed network interface configuration and forwarding behavior

### Root Cause
The Wi-Fi network was broadcasting successfully at the link layer, but traffic was not being correctly routed or forwarded to the upstream internet connection.

The access point was functioning, but the Ubuntu Server was not initially passing traffic between interfaces as required for routing/NAT behavior.

### Fix
- Adjusted network configuration to correctly route or bridge traffic between interfaces
- Ensured the Wi-Fi network had a valid path to the upstream internet connection
- Retested client connectivity after configuration changes

### Validation
- Connected client devices regained internet access
- Confirmed consistent outbound connectivity
- Verified expected routing behavior after forwarding was corrected

---

## What Worked
- The Ubuntu Server could connect to Wi-Fi as a client
- The Ubuntu Server could broadcast a Wi-Fi SSID for client devices
- Client devices could connect to the Ubuntu-hosted network
- Basic network connectivity was achievable after routing fixes

## Issues Encountered

### 1) 2.4 GHz-only broadcast behavior
Although the hardware could *receive* 5 GHz Wi-Fi, the Ubuntu-hosted access point was effectively limited to broadcasting at 2.4 GHz.

Result:
- Reduced throughput compared to expected 5 GHz performance
- Less stable performance for bandwidth-heavy usage

### 2) Practical throughput limitations
With clients connected to the Ubuntu-hosted Wi-Fi:
- Network speeds were significantly slower than expected
- Performance was insufficient for normal home usage

### 3) Hardware, driver, and regulatory constraints
The limitations appeared to stem from a combination of:
- Wi-Fi chipset AP-mode capabilities
- Driver support for access-point functionality
- Practical regulatory/channel constraints affecting 5 GHz AP broadcasting

## Decision
This approach was not suitable as a long-term solution due to throughput limitations and the inability to broadcast a fast, reliable 5 GHz Wi-Fi network.

Actions taken:
- Disabled Wi-Fi AP/broadcast functionality on the Ubuntu Server
- Shifted strategy to dedicated networking hardware for Wi-Fi and DHCP control

## Lessons Learned
- Wi-Fi client capability does not guarantee effective Wi-Fi access-point capability
- Broadcasting an SSID does not automatically imply correct routing or internet access
- Throughput and stability matter more than theoretical functionality
- Identifying hardware and driver limitations early prevents wasted effort
- Rolling back after validation is a valid and professional decision

## Next Step
Introduce a downstream router behind the T-Mobile Gateway to:
- Control DHCP and DNS for a dedicated Wi-Fi network
- Reliably point client DNS at Pi-hole
- Improve performance and stability compared to the Ubuntu-hosted AP approach
