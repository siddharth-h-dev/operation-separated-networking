# Operation Separated Networking (OSN)

OSN is an architectural philosophy prioritizing isolation and delegation of duties without sacrificing management ability, ensuring high security and maximum fault tolerance.


# Industry standards associated

- **NIST SP 800-207 (Zero Trust Architecture)** - Principles 1, 4
- **Purdue Model (ICS/OT Security)** - Principle 1
- **CIS Critical Security Controls (Specifically Control 12)** - Principle 5
- **Disaggregated Networking** - Principle 4

# Why did I write this?

I hate walled-garden ecosystems, all-in-one stuff, SDN, etc.

Some experiences and thoughts that led to this:-
- All-in-one devices and consumer gear are vulnerable. Operations must be isolated so that one service compromise does not cripple the entire network. **Core infrastructure should be isolated**
- I don't understand the appeal of Unifi Dream Machine. It's a single point of failure. Also have the same appeal with most consumer routers. They are not good for production environments. **Single points of failure should be minimized**.
- OPNSense does not do Traffic shaping as well as OpenWRT. So, gave the traffic shaping duty to that. **The right duty must be given to the right software**.
- Deploying Omada controller was a pain. It was bloated and crashed easily. I did not want a bulky application just to control my omada switch more efficiently. I also felt like i would be locked into omada if i kept it. **Applications should be more lightweight**.
- Walled-garden ecosystems in general are bad. They cause vendor-lock. Management should be easy across products of different companies. **Management should be more simple and unified**.

That's why I developed my own networking philosophy so that software following this philosophy will make networking easier, more secure and fault tolerant. I wrote this with the homelabbing community in mind since I am a homelabber too.

# Core Principles

## Principle 1: Operational Isolation - A network is only as secure as its widest blast radius.
- **Core infrastructure should be isolated from one another** (through VMs or separate devices) to minimize the blast radius if one service is compromised.
- For example, North-South Traffic (NAT, Edge routing) and East-West Traffic (Local network management) should be separated as **Firewall** and **L3 Managed Switch** instead of an *All-in-one* networking device such as Consumer routers or even the Ubliquiti Unifi Dream Machine series.
- Keep in mind that Linux Containers (LXCs), Docker containers, etc. do not count as Operational Isolation as they are not separated from host hardware and one compromised container can affect others.
- Virtual Machines are a cost-effective way to achieve this and does not break the principle. Just make sure that the VM is isolated to the maximum from the host and other VMs (if present).
- This does not include media servers and stuff, just the backbone stuff such as **Firewall, Routing, Proxying, etc.** Applying Operational Isolation to those is not mandatory but may enhance security by limiting the blast radius.

## Principle 2: Service Segregation - Don't put all your eggs in one basket.
- **Services should be segregated** to minimize **Single Points of Failure**
- Instead of multiple services being hosted on one virtual machine or container, each service should be hosted on its own virtual machine or container.
- Using LXC or Docker containerization is the most efficient way to achieve this but the host will also be a failure point. Virtual machines are also a good option for large services.
- Operation Separation can be done physically by keeping separate hardware for a set of services if possible.

## Principle 3: Efficient Delegation - Give the right role to the right thing.
- **Duties should be delegated to specialized hardware (or) software** to achieve maximum efficiency.
- For example, delegating the role of Traffic Shaping to a **Linux-based OS** (OpenWRT, Alpine Linux, etc.) is better than doing it on your **existing firewall OS** (OPNSense pfSense, etc.) as Linux is *better suited* for this.
- Specialized technology can do one or two things very efficiently and other things very poorly. So, delegate those duties to technology specialized for those duties.

## Principle 4: Resource Maximization - Get the most out of what you got.
- **Services and Applications should be lightweight** so that OSN's other principles can be implemented more effectively.
- Use more lightweight operating systems, software, etc. whenever necessary.
- For example, deploying small applications through **Docker** is better than **LXC** as docker containers *use less resources*.

## Principle 5: Easy Management - Your infrastructure needs autopilot at this point.
- **Management of your network should be easy and unified**.
- By following the first three principles, your infrastructure would be separated a lot. So, **Unifiying and Simplifying Management** is necessary
- Use specialized stuff for this (For example, Omada Network Controllers for Omada stuff).

# How to Deploy OSN
- **Deploy Core Infrastructure with Operational Isolation** - Deploy the backbone of your Infrastructure with strict isolation.
- **Segregate your services** - Use Containerization (Docker, LXC) and/or use separate hardware for each set of services if possible.
- **Delegate your duties** - Use specialized hardware or software for your duties.
- **Maximize your resources** - Use lightweight software and make sure your services aren't hogging too much resources.
- **Simplify and Unify Management** - You will thank yourself later.

# Developing Software and Hardware that abides by OSN
Just keep the principles in mind while developing. I am telling you, If you develop your hardware or software with OSN Principles, Network management and efficiency will be way better for enthusiasts and IT professionals alike. We are in desperate need of such software in 2026.

# What I do
I develop software that follows OSN. Check out my other repos if you are interested.

# END
