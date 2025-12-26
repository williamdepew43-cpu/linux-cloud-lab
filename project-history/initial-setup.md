# Initial Setup: Ubuntu Server to Network-Wide Pi-hole

## Project Goal
The goal of this project was to set up a dedicated Linux server to provide reliable, network-wide DNS-based ad blocking using Pi-hole for all devices on a home Wi-Fi network.

This server is intended to behave like small-office infrastructure: always on, stable, and easy to troubleshoot when issues arise.

---

## Hardware & Environment
- Dedicated machine repurposed as a home server
- Connected via Ethernet to the local network
- Intended to run continuously (24/7)

---

## Operating System Installation
- Installed **Ubuntu Server** (headless, no desktop environment)
- Chose Ubuntu Server for:
  - Stability
  - Long-term support
  - Strong documentation and community support
- Verified system access via local terminal and SSH

---

## Initial System Configuration
After installation, the following baseline steps were completed:

- Updated system packages:
  ```bash
  sudo apt update && sudo apt upgrade

- Confirmed hostname and network connectivity
- Verified storage layout and mounted filesystems
- Ensured SSH access was working for remote administration

These steps established a clean, known-good baseline before installing any services.

---

## Networking Considerations
Because the server would act as a DNS provider for the network, network stability was critical.

Steps taken:
- Assigned a **static IP address** to the server
- Verified the server maintained the same IP after reboot
- Confirmed the server could reach external DNS endpoints

This ensured client devices could consistently reach the DNS service.

---

## Pi-hole Installation
Pi-hole was selected as the DNS ad-blocking solution because it:
- Operates at the DNS level
- Is widely used in home and small-business environments
- Provides clear logging and observability

Installation was performed using the official Pi-hole installation method.

Post-install checks included:
- Verifying the Pi-hole service was running
- Confirming the web interface was accessible
- Testing DNS resolution from the server itself

---

## Router & Client Integration
To enable network-wide blocking:

- Router DHCP/DNS settings were updated to point clients to the Pi-hole server
- Multiple client devices were tested (phones, laptops, smart TVs)
- DNS queries were observed in Pi-hole logs to confirm traffic flow

This validated that Pi-hole was actively handling DNS requests for the network.

---

## Validation & Testing
Validation steps included:
- Loading known ad-heavy websites
- Confirming reduced ad requests
- Verifying normal DNS resolution still worked
- Checking Pi-hole query logs for expected activity

At this point, the server was successfully operating as the primary DNS resolver.

---

## Lessons Learned
- DNS is critical infrastructure — small misconfigurations have large impact
- Static IP assignment is essential for network services
- Logs are the first place to look when diagnosing network issues
- Making one change at a time simplifies troubleshooting

---

## Next Steps
After initial setup, focus shifted to:
- Monitoring reliability
- Troubleshooting client behavior
- Documenting issues and fixes
- Treating the server as production infrastructure
