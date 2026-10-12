# Course 3 — Connect and Protect: Networks and Network Security

## Module 1: Network Architecture

---

### Overview
This module builds the mental model of *how networks are physically and logically put together* — the devices that make them up, how they're arranged (on-prem and in the cloud), how data is measured and inspected, and the addressing and protocols that let devices actually talk. It closes with the two layering models (**TCP/IP** and **OSI**) that describe how data moves across a network.

---

### What Is a Network?
A **network** is simply a group of connected devices. Networks exist so devices can communicate and share resources and data. The individual devices on a network (computers, servers, phones, printers) are often called nodes.

---

### Network Devices
The core hardware that moves data around. The difference between them comes down to *what they connect* and *how smartly they forward data*:

| Device | What it does | Forwards based on |
|--------|--------------|-------------------|
| **Modem** | Connects your router to the ISP, bringing internet access to the LAN | — |
| **Router** | Connects multiple **networks** together and directs traffic between them | IP addresses |
| **Switch** | Connects specific **devices within** a network, sending data only to the intended recipient | MAC addresses |
| **Hub** | Broadcasts data to **every** device on the network (older, inefficient) | — (sends to all) |

**Security note on hubs:** because a hub blindly broadcasts to every port, every device on it sees all the traffic — which makes eavesdropping trivial. A switch is both more efficient and more secure because it sends data only to the device it's meant for. This is why switches replaced hubs.

---

### Network Types & Design
Networks are categorized by the geographic area they cover:

- **LAN (Local Area Network)** — spans a small area like an office building, a school, or a home.
- **WAN (Wide Area Network)** — spans a large geographic area like a city, state, or country. (The internet is the largest WAN of all.)

A **network diagram** is a visual map of a network's devices and how they connect — used to design and document networks.

**Designing a LAN** typically chains the devices like this:

> **Devices → Switch → Router → Modem → ISP / Internet**

The switch links local devices together, the router connects that local network outward, and the modem bridges to the internet provider.

---

### Cloud Networks
Instead of owning all the hardware, organizations increasingly rent infrastructure that lives in someone else's data center:

- **Cloud computing** — using remote servers, applications, and network services hosted on the internet instead of local physical devices.
- **Cloud network** — a collection of servers or computers that store resources and data in remote data centers, accessed via the internet.
- **Software-defined networking (SDN)** — managing and configuring network infrastructure through **software** rather than manual hardware setup. It makes networks programmable, centrally managed, and flexible, which is a natural fit for cloud environments.

The pull of the cloud is scalability, lower maintenance, and pay-for-what-you-use cost. Many organizations run a **hybrid** of on-prem and cloud.

---

### Measuring & Inspecting a Network
Key terms for how much data a network moves and how it's examined:

- **Bandwidth** — the *maximum* data transmission capacity of a network, measured in **bits per second (bps)**. Think of it as the width of the pipe.
- **Speed** — the *actual* rate at which a device sends and receives data, also measured in bps. (Bandwidth is the ceiling; speed is what you're getting right now.)
- **Data packet** — the basic unit of information that travels from one device to another within a network.
- **Port** — a software-based location that organizes the sending and receiving of data between devices. Ports map traffic to specific services — e.g., **80** (HTTP), **443** (HTTPS), **22** (SSH), **53** (DNS).
- **Packet sniffing** — capturing and inspecting data packets as they cross a network. It's a legitimate analyst tool for troubleshooting and monitoring, and also a technique attackers use to steal data.

---

### Network Communication — Addressing
Every device needs addresses so data knows where to go. There are two kinds, and the distinction matters:

| | **IP address** | **MAC address** |
|---|---|---|
| Identifies | A device's **location** on a network/internet | A specific **physical device** (its network card) |
| Type | **Logical** — can change | **Physical** — permanent, assigned to the hardware |
| Used by | **Routers**, to route *between* networks | **Switches**, to deliver *within* a local network |
| Example | `192.168.1.10` | `00:1A:2B:3C:4D:5E` |

- **Internet Protocol (IP)** — the set of standards used for routing and addressing data packets as they travel between devices.
- **IP address** — a unique string of characters identifying a device's location on the internet.
- **MAC address** — a unique alphanumeric identifier assigned to each physical device on a network.

---

### Protocols: IP, TCP, and UDP
Two devices need an agreed set of rules — a protocol — to communicate. The two transport protocols differ in whether they guarantee delivery:

- **TCP (Transmission Control Protocol)** — lets two devices form a connection and stream data. It's **connection-oriented** and reliable: it opens with a **three-way handshake** (`SYN → SYN-ACK → ACK`), then delivers data in order and re-sends anything lost.
- **UDP (User Datagram Protocol)** — **connectionless**: it does *not* establish a connection before transmitting. That makes it faster but with no guarantee of delivery or ordering.

| | **TCP** | **UDP** |
|---|---|---|
| Connection | Connection-oriented (handshake first) | Connectionless (no handshake) |
| Reliability | Reliable — ordered, acknowledged, retransmits | Best-effort — no delivery/order guarantee |
| Speed | Slower (more overhead) | Faster (less overhead) |
| Typical uses | Web, email, file transfer | Streaming, VoIP, DNS, gaming |

---

### The TCP/IP Model
The **TCP/IP model** is a framework used to visualize how data is organized and transmitted across a network — it's the practical model the internet actually runs on. Google presents it as **4 layers** (shown top to bottom):

| Layer | What it handles |
|-------|-----------------|
| **4. Application** | The protocols apps use to exchange data (HTTP, DNS, etc.) |
| **3. Transport** | End-to-end communication between devices (TCP / UDP) |
| **2. Internet** | Addressing and routing packets *between* networks (IP) |
| **1. Network Access** | Physical hardware and local delivery (MAC addresses, cabling) |

---

### The OSI Model
The **OSI (Open Systems Interconnection) model** is a standardized *conceptual* reference describing the **7 layers** computers use to communicate. It's more granular than TCP/IP and used mainly for understanding and troubleshooting. Here it is, mapped to the TCP/IP layers above:

| OSI layer | Maps to TCP/IP layer |
|-----------|----------------------|
| 7. Application | Application |
| 6. Presentation | Application |
| 5. Session | Application |
| 4. Transport | Transport |
| 3. Network | Internet |
| 2. Data Link | Network Access |
| 1. Physical | Network Access |

**Mnemonic (layers 1→7):** *Please Do Not Throw Sausage Pizza Away* — **P**hysical, **D**ata Link, **N**etwork, **T**ransport, **S**ession, **P**resentation, **A**pplication.

The two models describe the same journey — OSI just splits the top into three layers and the bottom into two, where TCP/IP keeps each as one.
