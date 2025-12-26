# Network-Wide Ad Blocking with Pi-hole (Linux Home Lab)

This repository documents the setup, configuration, and troubleshooting of a Linux-based network-wide ad blocking solution using Pi-hole.

**Background:** I am a career switcher currently working in a hands-on trade role. This home lab demonstrates my approach to Linux systems administration, networking fundamentals, and support-style troubleshooting through a real, always-on service used on my home network.

## Project Overview
The goal of this lab is to provide DNS-based ad blocking for all devices on a local network by routing DNS queries through a dedicated Linux server running Pi-hole.

This setup mirrors common small-office and home network environments and emphasizes reliability, observability, and clean configuration.

## Architecture
- **Server OS:** Ubuntu Server
- **Service:** Pi-hole (DNS sinkhole)
- **Role:** Primary DNS server for the local network
- **Clients:** Phones, laptops, smart TVs, and other Wi-Fi devices

## Key Concepts Practiced
- Linux system administration
- Network configuration and DNS fundamentals
- Service management and uptime
- Log inspection and troubleshooting
- Permissions and filesystem layout
- Safe configuration changes and rollback awareness

## Setup Summary
High-level steps used in this project:
1. Installed Ubuntu Server on dedicated hardware
2. Assigned a static IP address to ensure DNS stability
3. Installed Pi-hole and verified DNS resolution
4. Configured router/DHCP settings to use Pi-hole as DNS
5. Validated ad blocking across multiple client devices
6. Monitored query logs and performance

## Troubleshooting & Maintenance
This repository includes documented troubleshooting scenarios such as:
- DNS resolution failures
- Client devices bypassing Pi-hole
- Permission or service startup issues
- Network misconfiguration and verification

Each issue is documented with:
- Symptoms
- Investigation steps
- Root cause
- Fix
- Lessons learned

## Operational Considerations
- Pi-hole is treated as critical network infrastructure
- Changes are tested incrementally
- Logs are reviewed regularly
- Configuration is documented to support reproducibility

## Disclaimer
This repository documents a personal learning environment. No credentials, private keys, or sensitive network details are committed.
