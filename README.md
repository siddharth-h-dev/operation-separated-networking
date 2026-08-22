# Operation Separated Networking (OSN)
<div align="center">
  <img width="300" height="300" alt="OSN" src="https://github.com/user-attachments/assets/5cebb9f9-93c5-43d8-a35c-e4f058f67419" />
</div>
OSN is an architectural philosophy prioritizing isolation and delegation of duties without sacrificing management ability, ensuring high security and maximum fault tolerance.


# Industry standards associated

- **NIST SP 800-207 (Zero Trust Architecture)** - Principles 1, 4
- **Purdue Model (ICS/OT Security)** - Principle 1
- **CIS Critical Security Controls (Specifically Control 12)** - Principle 5
- **Disaggregated Networking** - Principle 4

# Why did I write this?

I hate walled-garden ecosystems, all-in-one stuff, SDN, etc.

Some experiences and thoughts that led to this:-
- All-in-one devices and consumer gear are vulnerable. Operations must be isolated so that one service compromise does not cripple the entire network. **Core infrastructure should be isolated**.
- I don't understand the appeal of Unifi Dream Machine. It's a single point of failure. Also have the same appeal with most consumer routers. They are not good for production environments. **Single points of failure should be minimized**.
- OPNSense does not do Traffic shaping as well as OpenWRT. So, gave the traffic shaping duty to that. **The right duty must be given to the right software**.
- Deploying Omada controller was a pain. It was bloated and crashed easily. I did not want a bulky application just to control my omada switch more efficiently. I also felt like i would be locked into omada if i kept it. **Applications should be more lightweight**.
- Walled-garden ecosystems in general are bad. They cause vendor-lock. Management should be easy across products of different companies. **Management should be more simple and unified**.

That's why I developed my own networking philosophy so that software following this philosophy will make networking easier, more secure and fault tolerant. I wrote this with the homelabbing community in mind since I am a homelabber too.

# Core Principles

## Principle 1: Operational Isolation - A network is only as secure as its widest blast radius.
<div align="center">
  <img width="1920" height="1080" alt="OSN P1" src="https://github.com/user-attachments/assets/e5265739-dbd4-4c4e-81a4-435137b61ed6" />
</div>

- **Core infrastructure should be isolated from one another** (through VMs or separate devices) to minimize the blast radius if one service is compromised.
- For example, North-South Traffic (NAT, Edge routing) and East-West Traffic (Local network management) should be separated as **Firewall** and **L3 Managed Switch** instead of an *All-in-one* networking device such as Consumer routers or even the Ubliquiti Unifi Dream Machine series.
- Keep in mind that Linux Containers (LXCs), Docker containers, etc. do not count as Operational Isolation as they are not separated from host hardware and one compromised container can affect others.
- Virtual Machines are a cost-effective way to achieve this and does not break the principle. Just make sure that the VM is isolated to the maximum from the host and other VMs (if present).
- This does not include media servers and stuff, just the backbone stuff such as **Firewall, Routing, Proxying, etc.** Applying Operational Isolation to those is not mandatory but may enhance security by limiting the blast radius.

## Principle 2: Service Segregation - Don't put all your eggs in one basket.
<div align="center">
  <img width="1920" height="1080" alt="OSN P2" src="https://github.com/user-attachments/assets/6593223e-b6e4-4dd9-90d8-29a5b70f116f" />
</div>

- **Services should be segregated** to minimize **Single Points of Failure**.
- Instead of multiple services being hosted on one virtual machine or container, each service should be hosted on its own virtual machine or container.
- Using LXC or Docker containerization is the most efficient way to achieve this but the host will also be a failure point. Virtual machines are also a good option for large services.
- Operation Separation can be done physically by keeping separate hardware for a set of services if possible.

## Principle 3: Efficient Delegation - Give the right role to the right thing.
<div align="center">
  <img width="1920" height="1080" alt="OSN  P3" src="https://github.com/user-attachments/assets/88625c82-664e-4e94-b820-e4f6644a1a7e" />
</div>

- **Duties should be delegated to specialized hardware (or) software** to achieve maximum efficiency.
- For example, delegating the role of Traffic Shaping to a **Linux-based OS** (OpenWRT, Alpine Linux, etc.) is better than doing it on your **existing firewall OS** (OPNSense pfSense, etc.) as Linux is *better suited* for this.
- Specialized technology can do one or two things very efficiently and other things very poorly. So, delegate those duties to technology specialized for those duties.

## Principle 4: Resource Maximization - Get the most out of what you got.
- **Services and Applications should be lightweight** so that OSN's other principles can be implemented more effectively.
- Use more lightweight operating systems, software, etc. whenever necessary.
- For example, deploying small applications through **Docker** is better than **LXC** as docker containers *use less resources*.

## Principle 5: Easy Management - Your infrastructure needs autopilot at this point.
- **Management of your network should be easy and unified**.
- By following the first three principles, your infrastructure would be separated a lot. So, **Unifiying and Simplifying Management** is necessary.
- Use specialized stuff for this (For example, Omada Network Controllers for Omada stuff).

# FAQs

## 1. "How do I Deploy OSN in my network?"
- **Deploy Core Infrastructure with Operational Isolation** - Deploy the backbone of your Infrastructure with strict isolation.
- **Segregate your services** - Use Containerization (Docker, LXC) and/or use separate hardware for each set of services if possible.
- **Delegate your duties** - Use specialized hardware or software for your duties.
- **Maximize your resources** - Use lightweight software and make sure your services aren't hogging too much resources.
- **Simplify and Unify Management** - You will thank yourself later.

## 2. "I do not have the necessary resources to deploy a network like this. What do I do?"
I can understand that not everyone will have the resources to deploy OSN right away. But it's OK. OSN is not meant to be deployed right away when someone starts homelabbing. It is meant for people who want higher security, easy management and fault tolerance.
You can deploy OSN once you have the resources needed.

## 3. "Why do you not like all-in-one network gateways like the Unifi Dream Machine?"
Two reasons:-
1. If it is hacked, the hacker can easily take control of your network (Violating Principle 1)
2. It is a Single Point of Failure where all networking functions are in one box. (Violating Principle 2)

## 4. "Management of a network like this will be very hard. There is no such software. What do you suggest?"
There are some software that will make management a tad bit easier, like Homepage, Proxmox Datacenter Manager, Grafana, etc. but they do not allow full unified network management. If there is such software, let me know.
I want to develop such a software.Stay tuned for that.

## 5. "OSN does not appeal to everyone. You know that right?"
I completely understand. OSN is for people with specific goals such as high security, maximum fault tolerance, etc. I don't care if it appeals to everyone. Maybe all-in-one boxes or SDN may be good for them.
**Who OSN may not appeal to:-**
- General Consumers with standard all-in-one wifi routers.
- Companies with already established networks.
- Companies like Omada and Ubliquiti who create walled-garden ecosystems.
- People who are fine with being in their existing Unifi or Omada ecosystems.
**Who I want OSN to appeal to:-**
- The homelabbing community.
- People who built open-source networking and network management projects.
- People who want maximum security and fault tolerance in their network.
- People who want a change in the current SDN trend.

## 6. "What's wrong with the Omada controller?"
I just had a bad experience deploying it. I also had to cannibalize one of my services. I think that it is way too bloated just to manage the only Ethernet switch that I have. Also, it is a Single Point of Failure and if it is compromised, your Omada network is compromised i guess. (Violates Principles 1 and 2)

## 7. "How do I develop Software and Hardware that abides by OSN?"
Just keep the principles in mind while developing. I am telling you, If you develop your hardware or software with OSN Principles, Network management and efficiency will be way better for enthusiasts and IT professionals alike. We are in desperate need of such software and hardware in 2026.

# What I do
I develop software that follows OSN. Check out my other repos if you are interested.

# A final note: Support OSN
OSN is built for the community. If you're tired of vendor lock-in and all-in-one stuff (I know I am), star this repo to support the movement, or open an issue to discuss how OSN can be improved. We need a revolution!

# END
