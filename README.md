# Operation Separated Networking (OSN)

OSN is an architectural philosophy prioritizing isolation and delegation of duties without sacrificing management ability, ensuring high security and maximum fault tolerance.

# Core Principles

## Principle 1: Operational Isolation - A network is only as secure as its widest blast radius.
- OSN mandates that operational layers be divided across explicit computing boundaries (such as VMs or separate devices) to prevent software dependency, system-wide crashes or single points of failure.
- For example, North-South Traffic (NAT, Edge routing) and East-West Traffic (Local network management) should be separated as **Firewall** and **L3 Managed Switch** instead of an *All-in-one* networking device such as Consumer routers or even the Ubliquiti Unifi Dream Machine series.
- Keep in mind that Linux Containers (LXCs), Docker containers, etc. do not count as Operational Isolation as they are not separated from host hardware and one compromised container can affect others.
- Virtual Machines are a cost-effective way to achieve this and does not break the principle. Just make sure that the VM is as isolated from the host and other VMs (If present) as possible.

# How to Deploy OSN
- **Deploy Core Infrastructure with Operational Isolation**. This does not include media servers and stuff, just the backbone stuff such as Firewall, Routing, Proxying, etc. Applying Operational Isolation to those is not mandatory but may enhance security by limiting the blast radius.
