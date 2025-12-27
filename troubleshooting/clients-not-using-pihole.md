# Client devices not using Pi-hole for DNS (T-Mobile Gateway limitation)

## Problem
Client devices on the network were not using the Pi-hole server for DNS, even though Pi-hole was installed, running, and reachable from the network.

## Symptoms
- Pi-hole dashboard showed little or no DNS query traffic
- Ads were still appearing on client devices
- Client devices continued resolving DNS through external servers
- Manual DNS testing worked, but network-wide enforcement did not

## Investigation
Steps taken to diagnose the issue:
- Verified Pi-hole service status and local DNS resolution
- Checked Pi-hole query logs for incoming client requests
- Reviewed DNS settings on multiple client devices
- Investigated router and gateway DNS configuration options

Findings:
- The network was using a **T-Mobile 5G Gateway**
- The gateway does **not allow custom DNS configuration**
- There is no supported way to advertise a local DNS server via DHCP
- DNS settings are locked to T-Mobile–managed resolvers

## Root Cause
The T-Mobile Gateway does not support configuring a custom DNS server for the entire network.

Because DNS settings are locked at the gateway level:
- Clients automatically use T-Mobile DNS servers
- Pi-hole cannot be enforced network-wide via DHCP
- Only per-device DNS overrides are possible

This is a hardware and firmware limitation, not a Pi-hole or Linux issue.

## Fix / Workaround
Implemented a per-device workaround:
- Manually configured DNS settings on supported client devices to point to the Pi-hole server
- Verified DNS resolution through Pi-hole on those devices
- Documented gateway limitations to avoid unnecessary reconfiguration attempts

Future options identified:
- Introduce a downstream router/firewall that supports custom DNS
- Place Pi-hole behind a router that controls DHCP
- Replace gateway with hardware that allows DNS customization (if feasible)

## Validation
- Confirmed manually configured devices appeared in Pi-hole query logs
- Verified DNS requests originated from expected client IPs
- Observed ad blocking behavior on affected devices
- Confirmed behavior was consistent with gateway limitations

## Lessons Learned
- ISP-provided gateways can impose hard networking limitations
- Not all DNS issues are software-related
- Understanding hardware and firmware constraints is critical
- Workarounds should be documented clearly for future maintenance
- Root cause analysis should distinguish configuration issues from platform limitations
