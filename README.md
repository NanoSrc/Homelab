# Homelab

> A personal infrastructure lab for networking, server administration, automation, monitoring, IoT and self-hosted software.

This repository documents and manages my personal homelab, a hands-on environment for learning, experimenting, building and operating real infrastructure.

The lab combines enterprise networking hardware, servers, storage, virtualization, Docker services, custom software, Raspberry Pis, ESP32 devices, sensors and automation.

The goal is simple:

**Build it. Break it. Fix it. Understand it. Document it.**

Beyond learning, I'd also like to gradually build the knowledge and skills needed for a capable, reliable and fun home infrastructure setup in the future of my household.

> [!CAUTION]
> I'm not a certified professional. This homelab is for educational purposes only. Don't blindly follow what I do, I have no idea what I'm doing (well, I sort of do, that's the point of learning from it) and some of it could potentially lead to your demise.

## Setup Pictures

The setup will likely change from time to time (upgrades, downgrades, location changes, combustion).

> **More pictures coming soon**

## Current Topology

The network is currently built around a **Ubiquiti UniFi Dream Machine SE**, a **UniFi Pro Max 16 PoE** and an **Arista DCS-7050SX-64**.

The UDM SE acts as the internet gateway, firewall and router between the lab's VLANs while also providing general network services such as DHCP and hosting the UniFi Network controller.

The Pro Max 16 PoE serves as the main always-on RJ45 and PoE access switch. It handles management interfaces, access points, Raspberry Pis, clients, IoT devices and other lower-bandwidth copper equipment.

The Arista forms the high-speed switching fabric of the lab. Servers, storage and other bandwidth-heavy devices connect through 10G-SR optics and multimode fibre.

<img width="4334" height="3156" alt="New_New_Network_Diagram" src="https://github.com/user-attachments/assets/329045be-455c-44db-8ca7-2849305b0a7d" />

This arrangement allows the Arista and the heavier lab equipment to be powered off without taking down the ordinary RJ45 network, management access, Wi-Fi or other always-on infrastructure.

> **Live UniFi topology screenshot coming at some point.**

Because much of the switching infrastructure consists of third-party equipment, the UniFi topology may not represent every downstream device or link perfectly. The architecture documentation in this repository is the authoritative reference.

### UniFi Port Layout

The physical ports are grouped by purpose rather than assigned arbitrarily. Regular endpoint ports are configured as access ports with a single native VLAN, so connected devices do not need to understand VLAN tagging themselves.

Management devices use VLAN 10, servers use VLAN 20, trusted clients use VLAN 30 and IoT devices use VLAN 40.

The SFP+ uplinks are configured as trunks and carry multiple tagged VLANs between the UDM SE, UniFi Pro Max and Arista. The wireless access point port is also trunk-like: the AP itself is managed through VLAN 10 while client and IoT SSIDs are carried using tagged VLANs such as VLAN 30 and VLAN 40.

Unused ports remain on the default network until they are assigned a specific role.

#### UDM SE Port Layout

> Forgot to commit screenshot while at home

#### UniFi Pro Max 16 PoE Port Layout

<img width="2460" height="1258" alt="Screenshot 2026-10-03 170101" src="https://github.com/user-attachments/assets/47784ba4-21de-4270-843b-075579be635a" />

### Arista Port Layout

The Arista's 48 SFP+ ports are currently divided into logical ranges. This makes the intended network of a physical connection immediately identifiable while still leaving plenty of room between the relatively small number of optics currently installed.

<img width="1419" height="156" alt="Arista_Markings" src="https://github.com/user-attachments/assets/58823f4b-7034-442f-aaa7-b99d6b89e823" />

| Ports | Assignment |
| --- | --- |
| Ethernet 1-8 | 🔴 VLAN 10 Management |
| Ethernet 9-28 | 🔵 VLAN 20 Servers |
| Ethernet 29-33 | 🟠 VLAN 30 Clients |
| Ethernet 34 | 🟢 10 GbE trunk to main access switch |
| Ethernet 35-40 | 🟣 VLAN 40 IoT |
| Ethernet 41-48 | ⚪ Reserved / administratively disabled |
| QSFP+ 49-52 | ⚪ Reserved / administratively disabled |

The large server allocation is partly practical and partly because I have 48 SFP+ ports and therefore absolutely no reason to put every optic directly next to every other optic.

### Management Network

VLAN 10 is reserved for infrastructure management.

This includes devices with dedicated out-of-band management interfaces such as:

* Dell iDRAC
* HPE iLO4
* Proxmox Management
* Arista Management1
* Cisco Management

It can also include in-band management interfaces for devices that do not have a dedicated management controller.

The goal is to keep administrative access separate from ordinary server traffic on VLAN 20 and from client / IoT networks.

## What I'm Building

Alongside the physical infrastructure, I'm building a custom management platform for the homelab.

The goal is to eventually have a single interface for monitoring and interacting with the infrastructure, from network equipment and servers to storage, sensors, virtual machines and self-hosted services.

**Current / planned stack:**

* React + Vite
* React Bits
* Spring Boot + Kotlin
* Gradle
* PostgreSQL
* Proxmox VE
* Docker / Docker Compose
* Harbor
* GitHub Actions

The application itself comes first. Containerization, automated deployment and image management will be introduced once there is actually something worth deploying.

The platform is intended to become a practical project for learning software development alongside networking, infrastructure, virtualization and systems administration.



## Hardware

The current and planned setup includes:

> [!NOTE]
> **NEW ADDITION:**
> Ubiquiti UniFi Pro Max 16 PoE  
> Main always-on RJ45 / PoE access switch  
> 10 GbE uplinks, VLAN-aware access switching and enough PoE to stop abusing the UDM SE for everything  
> Also has Etherlighting, which is technically useful and therefore not RGB slop ;) 

> [!NOTE]
> **NEW ADDITION:**
> Modular 1U utility insert  
> 6 interchangeable module slots  
> Currently / Will be housing a Raspberry Pi 4, Raspberry Pi 5 and Zigbee dongle  
> 3 slots still waiting to acquire a purpose  
> One of them will probably be sacrificed to decoration because of course infrastructure also needs a bit of life

* Dell PowerEdge R630
  * Primary server
  * 1U enterprise server
  * 8 × 2.5" SFF hot-swap bays
  * 2 × Intel Xeon E5-2620 v4
  * 16 cores / 32 threads total
  * 64 GB DDR4 ECC memory
  * 10K SAS storage
  * Proxmox VE
  * iDRAC8 remote management
  * `10.10.20.10` on the server network
  * 10 GbE SFP+ connectivity planned / being added
  * Main host for VMs, containers, game servers, websites and other self-hosted services

* HPE ProLiant DL180 Gen9
  * ~12 years old
  * Enterprise 2U server
  * Secondary lab / testing / backup server
  * Proxmox VE
  * iLO4 remote management
  * 10 GbE connectivity
  * Intended for testing, backup workloads and experiments that I would rather not perform on the primary R630

* Arista DCS-7050SX-64
  * ~12 years old
  * Main high-speed switch (Main noisemaker)
  * 48 × 1/10GbE SFP+ ports
  * 4 × 40GbE QSFP+ ports
  * 1.28 Tbps switching capacity
  * 10G-SR optics for fibre connectivity
  * The 40GbE ports currently exist mostly as a flex

* Ubiquiti UniFi Dream Machine SE
  * Main internet gateway and firewall
  * Inter-VLAN routing
  * DHCP and general network services
  * UniFi Network controller
  * 10 GbE trunk to the UniFi Pro Max 16 PoE
  * Built-in RJ45 ports reserved for direct, emergency or deliberately assigned access
  * Acts as the friendly modern translator between me and the pile of retired enterprise hardware

* Ubiquiti UniFi Pro Max 16 PoE
  * Main always-on RJ45 / PoE access switch
  * 16 × copper Ethernet ports
  * 2 × 10 GbE SFP+ uplinks
  * PoE for access points, Raspberry Pis and other infrastructure
  * **Etherlighting** for useful physical port identification (not RGB slop because it is useful)
  * 10 GbE trunk to the UDM SE
  * 10 GbE trunk to the Arista
  * Keeps the ordinary network alive while the Arista and lab servers are powered down

* Custom TrueNAS / Storage Server
  * Planned 2U or 3U custom NAS
  * Reuses storage hardware from the old Synology RS810+
  * Bulk storage, backups and network shares
  * 10 GbE connectivity
  * TrueNAS SCALE planned
  * Because apparently using an old rack NAS (required rails which I did NOT want to invest in) was only the first step toward building another NAS

* Raspberry Pi
  * Planned
  * Intended for management, monitoring, console access and various small services

* ESP32 microcontrollers
  * Used for sensors and environmental monitoring
  * Cheap enough to deploy basically anywhere

* 1 GbE / 10 GbE networking
  * 1 GbE for peripherals, IoT and devices that simply don't need more
  * 10 GbE SFP+ for servers and my PC (it's local anyway)
  * Multimode OM3 fibre and 10G-SR optics / Arista optics
  * 40 GbE available for future questionable decisions

* UPS / power protection
  * Maybe™

* Various sensors and IoT hardware
  * Temperature and other telemetry

* Rack power distribution
  * 2 × metered / protected rack PDUs
  * The rack is divided into an always-on infrastructure side and a lab / high-power side
  * Core networking and low-power infrastructure can remain online while the Arista, servers and storage lab are powered down
  * Integrated power monitoring because I would probably rather not know what this setup costs to run

> [!NOTE]
> Old hardware doesn't mean useless hardware

## Documentation

* [Networking](docs/networking.md)
* [Infrastructure](docs/infrastructure.md)

## Repository Structure

```text
homelab/
├── frontend/          # React + Vite management interface
├── backend/           # Spring Boot + Kotlin API
├── docker/            # Docker and Compose configuration
├── infrastructure/    # Infrastructure automation
├── configs/           # Sanitized device configurations
├── scripts/            # Utility and management scripts
├── services/           # Homelab services
├── docs/               # Project documentation
└── diagrams/           # Network and infrastructure diagrams
```

## Philosophy

This is a learning environment rather than a production datacenter.

Things will be changed, tested, broken, rebuilt, optimized and occasionally set on fire metaphorically.

> I will try not to make the setup combust into flames non-metaphorically. :)

The important part is understanding **why** something works, **why** it breaks and **how** to fix it.

> [!NOTE]  
> I want to avoid daily journals for a home project, I'm not sisyphus incarnated.

## Security

This repository is public.

No passwords, API keys, private keys, tokens, credentials or sensitive device backups should be committed (unless I mess up real bad).

Configuration examples should be sanitized before being added to the repository.

## Ideas, Questions & Contributions

Have an idea for something I could add, improve or experiment with?

Have a question about the homelab or how something works?

Feel free to open a **GitHub Issue**. I'm always happy to answer questions, discuss ideas and see what could be added to the project.

Whether it's a networking idea, a new service, an automation, a monitoring feature or something completely ridiculous, I'd love to hear it. :)

## Status

**Active and continuously evolving**

I will make some custom icons and logos for the fun of it.

Also hi cally ♡ our future house will have a SICK network and SICK rack (electricity bills and noise issues will be addressed later).

<img width="444" height="153" alt="image" src="https://github.com/user-attachments/assets/22ecadfc-c07c-4d02-9e34-2439abac41fc" />

This project and the ideas of it will grow alongside the homelab.
