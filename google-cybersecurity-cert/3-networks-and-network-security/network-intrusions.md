# Course 3 — Connect and Protect: Networks and Network Security

## Module 3: Secure Against Network Intrusions

---

### Overview
Modules 1 and 2 covered how networks are built and how they run; this module covers how they get **attacked**. It explains why networks are such a target, walks through the two big families of network attacks — **denial of service** (knocking systems offline) and **interception** (packet sniffing and IP spoofing) — and teaches how to read **tcpdump** output to spot them.

---

### The Case for Securing Networks
Networks are always on and carry an organization's most valuable data, which makes them a constant target. Every protocol, open port, and misconfiguration from the last two modules is a potential way in. An analyst has to understand how intrusions happen to recognize and stop them.

**How intrusions compromise a system:**
- **Network interception attacks** — intercepting traffic in transit to read, steal, or alter it.
- **Backdoor attacks** — getting around normal access controls. A backdoor can be a weakness left behind by programmers or admins, or one an attacker installs (often through malware) to keep access after breaking in.

**Impact on an organization:**
- **Financial** — downtime, lost revenue, recovery costs, legal fees, ransom payments.
- **Reputational** — customers and partners lose trust, and it's slow to win back.
- **Public safety** — attacks on government or critical infrastructure (power, water, healthcare) can put people at risk.

---

### Denial of Service (DoS) Attacks
A **DoS attack** targets a network or server and floods it with traffic until legitimate users can't get through. The goal isn't stealing data — it's taking away **availability**, the "A" in the CIA triad.

A **DDoS (distributed denial of service)** attack does the same thing from **many devices in many locations** at once, which makes it much harder to stop: there's no single source to block. DDoS attacks usually run through a **botnet**, a collection of malware-infected computers controlled by a single threat actor called the **bot-herder**. The devices' owners usually have no idea they're involved.

**Network-level DoS attacks.** Three of the four abuse **ICMP**, the protocol devices use to tell each other about data transmission errors (and the one `ping` uses — see Module 2).

| Attack | How it works | What it looks like in traffic |
|--------|--------------|-------------------------------|
| **SYN flood** | Simulates the start of a TCP connection over and over. The server answers each SYN with a SYN-ACK and holds resources open waiting for a final ACK that never comes, until nothing is left for real users. | A flood of SYN packets and SYN-ACK replies with no completing ACKs — piles of half-open connections |
| **ICMP flood** | Repeatedly sends ICMP request packets to a server, which has to process and answer every one until its bandwidth is exhausted. | A huge volume of ICMP echo requests aimed at one target |
| **Ping of death** | Pings a system with an oversized ICMP packet, bigger than 64 KB. A vulnerable system crashes trying to handle it. Mostly patched on modern systems. | Oversized or malformed ICMP packets, often split into fragments |
| **Smurf attack** | DDoS plus IP spoofing. The attacker sends ICMP requests to a network's broadcast address with the victim's IP as the source, so every device on that network replies to the victim at once. | The victim receives a flood of ICMP replies it never asked for |

**Defending against DoS:**
- **SYN flood** — SYN cookies (the server stops holding resources for unfinished handshakes), rate limiting, and firewall/IDS detection.
- **ICMP flood and smurf** — rate-limit or filter ICMP at the firewall, and block directed broadcasts on routers.
- **Ping of death** — keep systems patched; firewalls drop malformed and oversized packets.
- **DDoS** — DDoS protection services and CDNs absorb and filter attack traffic before it reaches the network.

---

### Reading tcpdump Logs
**tcpdump** is a command-line packet analyzer: it captures network traffic and prints each packet as a line of text.

```
13:24:32.192571 IP 198.51.100.15.52444 > 203.0.113.10.443: Flags [S], seq 2873951608, win 64240, length 0
```

| Field | Example | Meaning |
|-------|---------|---------|
| Timestamp | `13:24:32.192571` | When the packet was captured (hours:minutes:seconds.microseconds) |
| Protocol | `IP` | IPv4 traffic |
| Source | `198.51.100.15.52444` | Source IP **and** source port, joined by a dot (port 52444) |
| Direction | `>` | Traffic flows from source to destination |
| Destination | `203.0.113.10.443` | Destination IP and port (443 = HTTPS) |
| TCP flags | `Flags [S]` | Which TCP flags are set — here, SYN |
| Sequence number | `seq 2873951608` | Tracks the order of data in the connection |
| Window | `win 64240` | How much data the receiver can accept at once |
| Length | `length 0` | Bytes of payload — 0 means a control packet with no data |

**Gotcha:** the number after the *last* dot is the **port**, not part of the IP address. tcpdump may also show service names instead of port numbers (`.https`, `.http`, `.domain` for DNS) unless it's run with `-nn`.

| Flag | Meaning |
|------|---------|
| `[S]` | SYN — start a connection |
| `[S.]` | SYN-ACK (the `.` means ACK) |
| `[.]` | ACK |
| `[P.]` | PSH-ACK — carrying data |
| `[F.]` | FIN-ACK — closing the connection |
| `[R]` | RST — connection reset or refused |

A normal connection reads `[S]` → `[S.]` → `[.]`, then data. A **SYN flood** reads `[S]`, `[S]`, `[S]`… with the handshake never finishing.

**Common options:** `-i` (interface to capture on), `-nn` (don't resolve names or ports), `-c` (stop after a set number of packets), `-w` (save the capture to a file), `-r` (read a saved capture), `-v` (more detail).

---

### Real-Life DDoS: The 2016 Dyn Attack
In October 2016, **Dyn**, a major DNS provider, was hit with a massive DDoS attack powered by the **Mirai botnet** — a huge army of hijacked **IoT devices** (security cameras, DVRs, home routers) still running their factory-default passwords. Because so many companies relied on Dyn for DNS, sites like Twitter, Netflix, and Reddit became unreachable for users across the US and parts of Europe, even though none of them were attacked directly.

**Lessons:**
- **Default credentials are a vulnerability** — cheap, unsecured devices became the weapon.
- **DNS is a single point of failure** — take down the DNS provider, and every site depending on it goes dark too.
- **Anyone's device can be part of an attack** — botnet devices usually belong to unaware owners.

---

### Network Attack Tactics: Interception

#### Malicious Packet Sniffing
**Packet sniffing** (defined in Module 1) is a legitimate analyst tool, but attackers use the same technique to steal or tamper with data in transit:

| Type | What the attacker does | Example |
|------|------------------------|---------|
| **Passive** | Connects to a network hub and **reads** all the traffic without changing anything | Reading credentials sent in plaintext |
| **Active** | **Manipulates** packets in transit | Redirecting packets to an unintended port, or changing the data they carry |

Passive sniffing is the hub problem from Module 1 in action: a hub broadcasts every packet to every device, so anyone connected sees everything.

**Defenses:** make sniffed traffic useless by encrypting it — use a trusted **VPN**, stick to **HTTPS** sites, and avoid unprotected public Wi-Fi.

#### IP Spoofing
**IP spoofing** is when an attacker changes a packet's **source IP** to impersonate an authorized system and gain access to a network. Three attacks build on it:

| Attack | How it works | Signs and defenses |
|--------|--------------|--------------------|
| **On-path attack** (formerly "man-in-the-middle") | The attacker sits in the middle of an authorized connection and intercepts or alters the data in transit, while both sides think they're talking directly | Certificate warnings or ARP anomalies (one MAC address claiming several IPs); defend with encryption (HTTPS/TLS, VPNs) |
| **Replay attack** | The attacker intercepts a packet and **delays or repeats it** later — e.g., resending a captured login or transaction | The same request showing up again later; defend with timestamps, one-time session tokens, and encryption |
| **Smurf attack** | Spoofs the victim's IP to bury it in ICMP replies | See the DoS table above |

**Defending against IP spoofing:** configure firewalls to **reject incoming traffic claiming an internal IP address**. A packet arriving from the internet should never carry your own network's addresses — if it does, it's spoofed. Encryption adds protection, since a spoofer can't read or convincingly forge properly encrypted traffic.

---

### Identifying & Analyzing Network Attacks
Analyzing suspicious traffic follows the same pattern every time:
1. **Start from the symptom** — connection timeouts, a slow site, an unreachable service.
2. **Pull the logs** — tcpdump or Wireshark captures from around the time it started.
3. **Find the pattern** — one source sending far more than normal, handshakes that never complete, floods of ICMP, repeated identical requests, or ICMP error messages (e.g., `udp port 53 unreachable` means the DNS service on that server isn't responding).
4. **Name the attack** — match the pattern to the attack types above.
5. **Assess the impact and respond** — what was affected, how to contain it, and which controls (firewall rules, rate limiting, patching) stop it from happening again.
