# Course 3 — Connect and Protect: Networks and Network Security

## Module 2: Network Operations

---

### Overview
Module 1 covered how networks are built; this module covers how they *operate* and how they're *protected while operating*. It starts with the **protocols** devices use to communicate (wired and wireless), then moves into the measures that keep network traffic safe: **firewalls**, **security zones**, **subnetting**, **proxy servers**, and **VPNs**.

---

### Network Protocols
A **network protocol** is a set of rules used by two or more devices on a network to describe the order of delivery of data and the structure of that data. Protocols fall into three categories:

| Category | Purpose | Examples |
|----------|---------|----------|
| **Communication** | Govern how data is exchanged between devices | TCP, UDP *(both covered in Module 1)*, HTTP, DNS |
| **Management** | Monitor and manage activity on a network | SNMP, ICMP |
| **Security** | Make sure data is sent and received securely, usually through encryption | HTTPS, SFTP |

---

### Recognizing Protocols — Port Quick Reference
Analysts identify traffic by its port number constantly, in logs and packet captures (traffic hitting port 22 means SSH, port 53 means DNS, and so on). The ones to know:

| Protocol | Port(s) | What it does |
|----------|---------|--------------|
| **HTTP** | 80 | Communication between clients and web servers — **unencrypted** |
| **HTTPS** | 443 | HTTP secured with SSL/TLS encryption |
| **DNS** | 53 | Translates domain names into IP addresses |
| **SSH** | 22 | Creates an encrypted shell on a remote system |
| **SFTP** | 22 | Secure file transfer between devices (runs over SSH) |
| **Telnet** | 23 | Remote access in **plaintext** — insecure, replaced by SSH |
| **SNMP** | 161 | Monitoring and managing devices on a network |
| **DHCP** | 67 (server), 68 (client) | Automatically assigns each device an IP address, plus its DNS server and default gateway |
| **SMTP** | 25 (unencrypted), 587 (TLS) | Sends and routes email from sender to recipient |
| **POP3** | 110 (unencrypted), 995 (TLS) | Downloads email from the mail server to a local device |
| **IMAP** | 143 (unencrypted), 993 (TLS) | Accesses email while it stays on the server, so it syncs across devices |
| **ARP** | — (no port) | Determines the MAC address of the next router or device on the path |
| **ICMP** | — (no port) | Reports errors and network status between devices — the protocol behind `ping` and `traceroute` |

**Insecure → secure pairs** (a recurring exam pattern — the secure version adds encryption):

| Insecure | Secure replacement |
|----------|--------------------|
| HTTP (80) | HTTPS (443) |
| Telnet (23) | SSH (22) |
| FTP (21) | SFTP (22) |

**Worth knowing about a few of these:**
- **Telnet** sends *everything* in plaintext, including usernames and passwords, so anyone packet sniffing the network can read the credentials. That's the whole reason SSH replaced it.
- **ARP** works on the local network: a device knows the destination's IP address but needs its MAC address to actually deliver the data, so ARP asks and stores the answer in an **ARP cache**. ARP has no authentication, which is what makes **ARP spoofing** attacks possible.
- **NAT (Network Address Translation)** lets devices with *private* IP addresses reach the internet: the router translates their private addresses into its public IP. Private ranges, which aren't routable on the internet: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`.

---

### Wireless Protocols
**IEEE 802.11 (Wi-Fi)** is the set of standards that defines communication for wireless LANs. Because wireless signals travel through the air, anyone in range can capture them, so the security protocol protecting them matters. **WPA (Wi-Fi Protected Access)** is the wireless security protocol devices use to connect securely; it replaced the original WEP and has been upgraded as each version's weaknesses surfaced:

| Protocol | Year | Encryption | Status |
|----------|------|------------|--------|
| **WEP** (Wired Equivalent Privacy) | 1999 | RC4, weak keys | Broken — can be cracked in minutes. Avoid. |
| **WPA** | 2003 | TKIP | Stopgap fix for WEP; vulnerable (KRACK) |
| **WPA2** | 2004 | AES (CCMP) | Long-time standard; KRACK found in 2017 (patched) |
| **WPA3** | 2018 | AES + **SAE** handshake | Current and strongest |

- **WPA3's SAE** (Simultaneous Authentication of Equals) replaces WPA2's pre-shared-key handshake and resists offline password guessing — an attacker can't capture the handshake and brute-force the password later.
- **Personal vs. Enterprise mode** — *Personal* uses one shared passphrase (homes, small offices). *Enterprise* gives each user their own credentials, checked by an authentication server (organizations).

---

### Firewalls
A **firewall** is a network security device that monitors traffic to or from your network, allowing or blocking it based on rules. One basic function is **port filtering** — blocking or allowing specific port numbers to limit unwanted communication (e.g., blocking Telnet's port 23 entirely).

**By form:**
- **Hardware firewall** — a physical appliance that inspects each packet before it's allowed onto the network.
- **Software firewall** — a program installed on a computer or server that does the same job without dedicated hardware.
- **Cloud-based firewall** — a software firewall hosted by the cloud service provider, protecting cloud assets.

**By how they inspect traffic:**

| Type | How it works | Security level |
|------|--------------|----------------|
| **Stateless** | Applies predefined rules to each packet on its own; keeps no record of earlier packets | Basic — can't recognize suspicious patterns across packets |
| **Stateful** | Tracks information about the connections passing through it and proactively filters out threats | Stronger — knows whether a packet belongs to a legitimate, established connection |
| **Next-generation (NGFW)** | Stateful inspection *plus* deep packet inspection, intrusion prevention, and threat-intelligence feeds | Strongest |

---

### Security Zones & Network Segmentation
**Network segmentation** is a security technique that divides a network into segments. A **security zone** is a segment that protects the internal network from the internet. There are two zone types:

- **Uncontrolled zone** — the portion of the network outside the organization (the internet).
- **Controlled zone** — a subnet that protects the internal network from the uncontrolled zone. Inside it:
  - **DMZ (demilitarized zone)** — public-facing services that need internet access, such as web servers, proxy servers, and DNS servers. It's the buffer between the internet and the internal network.
  - **Internal network** — private servers and data the organization needs to protect.
  - **Restricted zone** — highly confidential information, accessible only to employees with specific privileges; usually behind its own firewall.

> **Internet (uncontrolled) → Firewall → DMZ → Firewall → Internal network → Firewall → Restricted zone**

**Why it matters:** segmentation limits how far an attacker can move once they're in. Compromising the DMZ still leaves more barriers between the attacker and anything sensitive. Missing segmentation is how the 2013 Target breach spread — attackers who entered through a vendor's access reached the payment systems.

---

### Subnetting & CIDR
**Subnetting** is the subdivision of a network into logical groups called **subnets**. Each subnet behaves like its own small network, which:
- **Improves performance** — traffic stays inside its subnet when it can, cutting unnecessary traffic.
- **Improves security** — each subnet can get its own controls, and a problem in one is easier to contain.

**CIDR (Classless Inter-Domain Routing)** is the method used to define subnets. A CIDR address is a normal IPv4 address followed by a slash and a number, the **network prefix**:

> `198.51.100.0/24` → covers every address from `198.51.100.0` to `198.51.100.255`

The prefix says how many of the address's 32 bits identify the **network**; the remaining bits identify **hosts** inside it. So a **bigger prefix means a smaller subnet**.

| CIDR | Subnet mask | Total addresses | Usable hosts |
|------|-------------|-----------------|--------------|
| /8 | 255.0.0.0 | 16,777,216 | 16,777,214 |
| /16 | 255.255.0.0 | 65,536 | 65,534 |
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /28 | 255.255.255.240 | 16 | 14 |

**The math:** total addresses = 2<sup>(32 − prefix)</sup>. Usable hosts = total − 2, because the first address (network ID) and the last (broadcast) are reserved.

---

### Proxy Servers
A **proxy server** fulfills the requests of its clients by forwarding them to other servers. It's a middleman, so the client and server never talk directly, which lets it hide IP addresses, filter traffic, and cache content. Proxies are commonly placed in the DMZ.

| | **Forward proxy** | **Reverse proxy** |
|---|---|---|
| Sits in front of | Internal **users** | Internal **servers** |
| Regulates | A person's access *to* the internet | The internet's access *to* an internal server |
| Hides | The user's IP address | The internal server's identity |
| Typical use | Blocking sites, restricting browsing, caching | Shielding web servers, load balancing |

**Email proxy** — a variation that filters spam by checking whether a sender's address was forged, cutting down on phishing that impersonates people the organization knows.

---

### VPNs
A **VPN (virtual private network)** is a network security service that changes your public IP address and masks your virtual location, keeping your data private when you're on a public network like the internet.

It works through **encapsulation**: your data is encrypted and wrapped inside another data packet. Anyone intercepting the traffic sees only the outer packet traveling between you and the VPN server, not what's inside or where it's really headed. The result is an encrypted **tunnel**.

**Two main types:**
- **Remote access VPN** — an individual connects their own device to a VPN server (e.g., working from home or on public Wi-Fi).
- **Site-to-site VPN** — connects whole networks to each other, like a branch office to headquarters. Mainly used by enterprises.

**Limits worth knowing:** a VPN protects data *in transit* and hides your IP from the network you're on, but you're shifting trust to the VPN provider, who can see where your traffic goes, and it does nothing against phishing or malware.

#### VPN Protocols: WireGuard vs. IPSec

| | **WireGuard** | **IPSec** |
|---|---|---|
| Age | Newer | Older, long-established |
| Design | Simple, with a very small open-source codebase | More complex to configure |
| Performance | Fast | Heavier, typically slower |
| Strength | Speed and simplicity; less code is easier to audit | Maturity — extensive security testing and built-in support in most operating systems |
| Used for | Site-to-site and client-server connections | Site-to-site and remote access |

IPSec works at the network layer, encrypting and authenticating each data packet. Neither is "the right answer"; it's a trade-off between WireGuard's performance and simplicity and IPSec's track record and compatibility.

---

### System Identification
Everything in this module adds up to one practical skill: **identifying what's on a network and what it's doing.** Addresses (IP and MAC) tell you *which* devices are present, ports and protocols tell you *what services* they're running, and security zones tell you *where* they sit. You can't protect a system you don't know exists, so building that picture is the first step in defending a network.
