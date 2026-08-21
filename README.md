# Operation Separated Networking (OSN)

OSN is an architectural philosophy prioritizing isolation and delegation of duties without sacrificing management ability, ensuring high security and maximum fault tolerance.

# Core Principles

## Principle 1: Operational Isolation - A network is only as secure as its widest blast radius.
- OSN mandates that operational layers be divided across explicit computing boundaries (such as VMs or separate devices) to prevent software dependency, system-wide crashes or single points of failure.
- For example, North-South Traffic (NAT, Edge routing) and East-West Traffic (Local network management) should be separated as **Firewall** and **L3 Managed Switch** instead of an *All-in-one* networking device such as Consumer routers or even the Ubliquiti Unifi Dream Machine series.
- Keep in mind that LXCs, Docker containers, etc. do not count as Operational Isolation as if the host is compromised, they are too.

# How to Deploy OSN
- **Deploy Core Infrastructure with Operational Isolation**. This does not include media servers and stuff, just the backbone stuff such as Firewall, Routing, Proxying, etc. Applying Operational
