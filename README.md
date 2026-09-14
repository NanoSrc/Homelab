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

The network is being built around a **Ubiquiti UniFi Dream Machine SE**, an **Arista DCS-7050SX-64** and a **Cisco Catalyst 3750-X**, each serving a different purpose.

The UDM SE handles the internet gateway, firewalling and general network services such as DHCP, while the Arista provides the primary high-speed 10 GbE switching fabric.

The Cisco serves as a secondary / access switch for 1 GbE devices, backup connectivity and experimenting with Layer 2 / Layer 3 networking without having to disturb the main network.

The network will eventually connect servers, storage, clients, wireless access points and IoT devices using a combination of 1 GbE, 10 GbE and VLANs.

<img width="4444" height="3126" alt="network_topology_diagram" src="https://github.com/user-attachments/assets/dc992f82-14bb-4344-82a2-03d47769d60d" />

> **Live UniFi topology screenshot coming soon.**

Because much of the switching infrastructure consists of third-party equipment, the UniFi topology may not represent every downstream device or link perfectly. The architecture documentation in this repository is the authoritative reference.


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
> Arista DCS-7050SX-64  
>  1.28 Terabits per second of switching capacity  
> I actually did need an SFP+ switch.  
> (Maybe not one this old and ridiculous)  

* Arista DCS-7050SX-64
  * ~12 years old
  * Main high-speed switch
  * 48 × 1/10GbE SFP+ ports
  * 4 × 40GbE QSFP+ ports
  * 1.28 Tbps switching capacity
  * 10G-SR optics for fibre connectivity
  * The 40GbE ports currently exist mostly as a flex

* Ubiquiti UniFi Dream Machine SE
  * Modern compared to most of the rest
  * Main internet gateway and firewall
  * UniFi controller
  * Intended to handle general network services such as DHCP
  * Built-in PoE connectivity
  * Likely to be the more user-friendly side of the network

* Cisco Catalyst 3750-X
  * ~16 years old (vintage networking hardware, but it suffices)
  * Secondary / backup / access switch
  * Useful for 1 GbE devices that don't need to consume Arista ports
  * Layer 2 / Layer 3 switching and routing
  * 24 × 1GbE RJ45 ports
  * Also serves as a platform for Cisco networking experiments

* Cisco C3KX-NM-10G
  * Network module for the Catalyst 3750-X
  * 2 × 1/10GbE SFP+ ports
  * 2 × 1GbE SFP ports
  * Bought before the 48-port Arista appeared and made the purchase slightly questionable
  * Still useful for a 10GbE uplink between the Cisco and the rest of the network

* HPE ProLiant DL180 Gen9
  * ~12 years old
  * Enterprise 2U server
  * Running Proxmox VE
  * iLO remote management
  * Used for virtualization, development, infrastructure experiments and self-hosted services
  * Has 10GbE connectivity (thanks to cheap NICs)

* Synology RS810+
  * Old 1U rackmount NAS
  * 4-bay storage system
  * Currently provides 16 TB active storage with another spare 16 TB
  * 1 GbE sadly

* Raspberry Pi
  * Planned
  * Intended for management, monitoring, console access and various small services

* ESP32 microcontrollers
  * Used for sensors and environmental monitoring
  * Cheap enough to deploy basically anywhere

* 1 GbE / 10 GbE networking
  * 1 GbE for peripherals, IoT and devices that simply don't need more
  * 10 GbE SFP+ for servers and my PC (it's local anyway)
  * Multimode OM3 fibre and 10G-SR optics
  * 40 GbE available for future questionable decisions

* UPS / power protection
  * Maybe

* Various sensors and IoT hardware
  * Temperature and other telemetry

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

This project and the ideas of it will grow alongside the homelab.
