# Deep In Net

A comprehensive networking project using Cisco Packet Tracer to understand fundamental networking concepts, protocols, and configurations.

## Table of Contents

- [Exercise 1: Direct PC Connections](#exercise-1-direct-pc-connections)
- [Exercise 2: Switch vs Hub](#exercise-2-switch-vs-hub)
- [Exercise 3: Network Services](#exercise-3-network-services)
- [Exercise 4: Router Basics](#exercise-4-router-basics)
- [Exercise 5: Multi-Subnet Networking](#exercise-5-multi-subnet-networking)
- [Exercise 6: Static Routing](#exercise-6-static-routing)
- [Exercise 7: Extended Multi-Subnet](#exercise-7-extended-multi-subnet)
- [Exercise 8: Complex Three-Subnet Routing](#exercise-8-complex-three-subnet-routing)
- [Key Concepts](#key-concepts)

---

## Exercise 1: Direct PC Connections

### Network Architecture

Three pairs of PCs connected directly using crossover cables:

```
PC0 (192.168.1.3/24) ←→ PC1 (192.168.1.4/24)
PC2 (192.168.13.82/29) ←→ PC3 (192.168.13.81/29)
PC4 (192.168.13.249/29) ←→ PC5 (192.168.13.254/29)
```

### Devices Used

- 6x PC-PT (Personal Computers)

### Cabling

- **Copper Cross-Over cables** (dashed lines) for PC-to-PC connections

### IP Addressing

| PC | IP Address | Subnet Mask | Network |
|---|---|---|---|
| PC0 | 192.168.1.3 | 255.255.255.0 | 192.168.1.0/24 |
| PC1 | 192.168.1.4 | 255.255.255.0 | 192.168.1.0/24 |
| PC2 | 192.168.13.82 | 255.255.255.248 | 192.168.13.80/29 |
| PC3 | 192.168.13.81 | 255.255.255.248 | 192.168.13.80/29 |
| PC4 | 192.168.13.249 | 255.255.255.248 | 192.168.13.248/29 |
| PC5 | 192.168.13.254 | 255.255.255.248 | 192.168.13.248/29 |

### Knowledge

**What is an RJ-45 cable?**
RJ-45 (Registered Jack 45) is a standard connector used for Ethernet networking. It's an 8-pin connector that looks like a larger telephone jack, used to connect devices to local area networks (LANs).

**Straight-Through vs Crossover Cables:**
- **Straight-Through**: Used for connecting different device types (PC to Switch, Switch to Router). Both ends use the same wiring standard (T568B-T568B or T568A-T568A).
- **Crossover**: Used for connecting same device types (PC to PC, Switch to Switch). One end uses T568A, the other uses T568B, which "crosses" the transmit and receive wires.

**How are IP addresses calculated?**
- The subnet mask determines which portion of the IP is the network address and which is the host address.
- For /29 (255.255.255.248): Block size = 256 - 248 = 8. Subnets increment by 8: .0, .8, .16, .24, ..., .80, .88, ..., .248.
- Valid hosts = Network + 1 through Broadcast - 1.

---

## Exercise 2: Switch vs Hub

### Network Architecture

Two separate groups: one connected via Switch, one via Hub.

```
Switch Group:                    Hub Group:
S-PC1 ←→ Switch0 ←→ S-PC2      H-PC1 ←→ Hub0 ←→ H-PC2
S-PC3 ←→ Switch0 ←→ S-PC4      H-PC3 ←→ Hub0 ←→ H-PC4
S-PC5 ←→ Switch0                H-PC5 ←→ Hub0
```

### Devices Used

- 1x 2960-24TT Switch
- 1x Hub-PT
- 10x PC-PT

### IP Addressing

**Switch Group (192.168.1.0/29):**

| PC | IP Address | Subnet Mask |
|---|---|---|
| S-PC1 | 192.168.1.1 | 255.255.255.248 |
| S-PC2 | 192.168.1.2 | 255.255.255.248 |
| S-PC3 | 192.168.1.3 | 255.255.255.248 |
| S-PC4 | 192.168.1.4 | 255.255.255.248 |
| S-PC5 | 192.168.1.5 | 255.255.255.248 |

**Hub Group (192.168.1.192/27):**

| PC | IP Address | Subnet Mask |
|---|---|---|
| H-PC1 | 192.168.1.193 | 255.255.255.224 |
| H-PC2 | 192.168.1.194 | 255.255.255.224 |
| H-PC3 | 192.168.1.195 | 255.255.255.224 |
| H-PC4 | 192.168.1.196 | 255.255.255.224 |
| H-PC5 | 192.168.1.197 | 255.255.255.224 |

### Knowledge

**Switch:**
- Operates at **Layer 2 (Data Link Layer)** of the OSI model
- Uses **MAC addresses** to forward frames to the correct port
- Learns which MAC address is on which port by building a **MAC address table**
- Only sends data to the intended recipient port (efficient, secure)

**Hub:**
- Operates at **Layer 1 (Physical Layer)** of the OSI model
- Simply **broadcasts** incoming data to ALL ports
- No intelligence - doesn't know where devices are located
- Creates unnecessary network traffic (broadcast storms)

**Key Differences:**
| Feature | Switch | Hub |
|---|---|---|
| OSI Layer | Layer 2 | Layer 1 |
| Intelligence | MAC address learning | None |
| Data forwarding | Unicast to specific port | Broadcast to all ports |
| Security | Better (isolated ports) | Worse (all see all traffic) |
| Performance | Full bandwidth per port | Shared bandwidth |

---

## Exercise 3: Network Services

### Network Architecture

A server rack with dedicated servers providing specific services to client PCs.

```
┌─────────────────────────────────────┐
│           Server Rack               │
│  ┌─────────┐  ┌─────────┐          │
│  │  DHCP   │  │   DNS   │          │
│  │ SERVER  │  │ SERVER  │          │
│  └────┬────┘  └────┬────┘          │
│       │            │                │
│  ┌────┴────┐  ┌────┴────┐          │
│  │  FTP    │  │  HTTPS  │          │
│  │ SERVER  │  │ SERVER  │          │
│  └────┬────┘  └────┬────┘          │
│       │            │                │
└───────┼────────────┼────────────────┘
        │            │
      ┌─┴────────────┴─┐
      │    Switch0      │
      └─┬──┬──┬──┬──┬──┘
        │  │  │  │  │
       PC0 PC1 PC2 PC3 PC4
```

### Devices Used

- 4x Server-PT (DHCP, DNS, FTP, HTTPS)
- 1x 2960-24TT Switch
- 5x PC-PT

### Server Configuration

**DHCP Server (192.168.1.10):**
- Service: ON
- Pool: 192.168.1.20 - 192.168.1.254
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.1.1
- DNS Server: 192.168.1.11

**DNS Server (192.168.1.11):**
- Records:
  - `deep-in-net.local` → 192.168.1.13 (A Record)
  - `deep-in-net.com` → `deep-in-net.local` (CNAME Record)

**FTP Server (192.168.1.12):**
- User: `deepinnet`
- Permissions: RWDNL (Read, Write, Delete, Rename, List)

**HTTPS Server (192.168.1.13):**
- HTTPS: ON
- HTTP: OFF
- Displays "Hello" message on index page

### Knowledge

**Server:** A computer that provides services to other computers (clients) on the network. Servers are always-on and respond to client requests.

**DHCP (Dynamic Host Configuration Protocol):**
- Automatically assigns IP addresses to devices on a network
- Uses a 4-step process: Discover → Offer → Request → Acknowledge (DORA)
- Operates at **Layer 7 (Application Layer)**
- Uses **UDP ports 67 (server) and 68 (client)**

**DNS (Domain Name System):**
- Translates domain names (deep-in-net.com) to IP addresses (192.168.1.13)
- Like a phone book for the internet
- Operates at **Layer 7 (Application Layer)**
- Uses **UDP port 53** (TCP for zone transfers)
- Record types: A (address), CNAME (alias), MX (mail), NS (nameserver)

**HTTP vs HTTPS:**
- **HTTP (HyperText Transfer Protocol)**: Unencrypted, uses port 80
- **HTTPS (HTTP Secure)**: Encrypted with SSL/TLS, uses port 443
- HTTPS ensures data integrity and confidentiality

**FTP (File Transfer Protocol):**
- Transfers files between client and server
- Operates at **Layer 7 (Application Layer)**
- Uses **TCP ports 20 (data) and 21 (control)**
- Two modes: Active (server connects to client) and Passive (client connects to server)

**TCP vs UDP:**
| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| Reliability | Guaranteed delivery | Best-effort delivery |
| Order | Maintains packet order | No order guarantee |
| Speed | Slower (overhead) | Faster (no overhead) |
| Use cases | Web, email, file transfer | DNS, streaming, gaming |

**Ports:**
A port is a logical endpoint for communication. Port numbers range from 0-65535.
- Well-known ports: 0-1023 (HTTP=80, HTTPS=443, FTP=21, DNS=53)
- Registered ports: 1024-49151
- Dynamic ports: 49152-65535

---

## Exercise 4: Router Basics

### Network Architecture

Two PCs in different subnets connected through a router.

```
PC0 (192.168.1.2/30) ←→ Router0 ←→ PC1 (192.168.2.2/30)
                        Fa0/0  Fa0/1
                       192.168.1.1  192.168.2.1
```

### Devices Used

- 2x PC-PT
- 1x 1841 Router

### Router Configuration

```
Router0:
  FastEthernet0/0: 192.168.1.1 255.255.255.252
  FastEthernet0/1: 192.168.2.1 255.255.255.252
```

### IP Addressing

| Device | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC0 | 192.168.1.2 | 255.255.255.252 | 192.168.1.1 |
| PC1 | 192.168.2.2 | 255.255.255.252 | 192.168.2.1 |

### Knowledge

**Router:**
- Operates at **Layer 3 (Network Layer)** of the OSI model
- Forwards packets between different networks based on IP addresses
- Uses **routing tables** to determine the best path for data
- Connects different subnets together

**Switch vs Router:**
| Feature | Switch | Router |
|---|---|---|
| OSI Layer | Layer 2 | Layer 3 |
| Addressing | MAC addresses | IP addresses |
| Function | Connects devices in same network | Connects different networks |
| Intelligence | MAC address table | Routing table |

**Default Gateway:**
- The IP address of the router interface on the local subnet
- When a device wants to communicate with a different subnet, it sends traffic to the default gateway
- The router then forwards the traffic to the correct destination network
- Without a default gateway, devices can only communicate within their local subnet

---

## Exercise 5: Multi-Subnet Networking

### Network Architecture

Two subnets connected through a router, each with its own switch.

```
Subnet 1 (192.168.1.0/29)          Subnet 2 (192.168.1.192/27)
    PC0-PC4                              PC5-PC9
       │                                    │
    Switch0                             Switch1
       │                                    │
    Router1 ←──────────────────────────→ Router1
      Fa0/0                              Fa0/1
   192.168.1.1                       192.168.1.193
```

### Devices Used

- 10x PC-PT
- 2x 2960-24TT Switch
- 1x 2911 Router

### Router Configuration

```
Router1:
  FastEthernet0/0: 192.168.1.1 255.255.255.248
  FastEthernet0/1: 192.168.1.193 255.255.255.224
```

### IP Addressing

**Subnet 1 (192.168.1.0/29):**

| Device | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC0 | 192.168.1.2 | 255.255.255.248 | 192.168.1.1 |
| PC1 | 192.168.1.3 | 255.255.255.248 | 192.168.1.1 |
| PC2 | 192.168.1.4 | 255.255.255.248 | 192.168.1.1 |
| PC3 | 192.168.1.5 | 255.255.255.248 | 192.168.1.1 |
| PC4 | 192.168.1.6 | 255.255.255.248 | 192.168.1.1 |

**Subnet 2 (192.168.1.192/27):**

| Device | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC5 | 192.168.1.194 | 255.255.255.224 | 192.168.1.193 |
| PC6 | 192.168.1.195 | 255.255.255.224 | 192.168.1.193 |
| PC7 | 192.168.1.196 | 255.255.255.224 | 192.168.1.193 |
| PC8 | 192.168.1.197 | 255.255.255.224 | 192.168.1.193 |
| PC9 | 192.168.1.198 | 255.255.255.224 | 192.168.1.193 |

### Communication

- Devices in the same subnet communicate directly through the switch
- Devices in different subnets communicate through the router (default gateway)

---

## Exercise 6: Static Routing

### Network Architecture

Two routers connecting two different networks with a router-to-router link.

```
PC0 (192.168.1.2/24) ←→ Router0 ←→ 10.10.0.0/30 ←→ Router1 ←→ PC1 (192.168.2.2/24)
                       Fa0/0  Fa0/1              Fa0/0  Fa0/1
                      192.168.1.1 10.10.0.1      10.10.0.2 192.168.2.1
```

### Devices Used

- 2x PC-PT
- 2x Router-PT

### Router Configurations

**Router0:**
```
FastEthernet0/0: 192.168.1.1 255.255.255.0
FastEthernet0/1: 10.10.0.1 255.255.255.252
Static Route: ip route 192.168.2.0 255.255.255.0 10.10.0.2
```

**Router1:**
```
FastEthernet0/0: 10.10.0.2 255.255.255.252
FastEthernet0/1: 192.168.2.1 255.255.255.0
Static Route: ip route 192.168.1.0 255.255.255.0 10.10.0.1
```

### IP Addressing

| Device | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC0 | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 |
| PC1 | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 |
| Router0 Fa0/0 | 192.168.1.1 | 255.255.255.0 | - |
| Router0 Fa0/1 | 10.10.0.1 | 255.255.255.252 | - |
| Router1 Fa0/0 | 10.10.0.2 | 255.255.255.252 | - |
| Router1 Fa0/1 | 192.168.2.1 | 255.255.255.0 | - |

### Knowledge

**Routing Table:**
A routing table is a data table stored in a router that lists the routes to particular network destinations. It contains:
- **Destination Network**: The target network address
- **Subnet Mask**: Defines the network portion of the address
- **Next Hop**: The IP address of the next router to forward packets to
- **Interface**: The outgoing interface to use
- **Metric**: Cost value used to select the best path

**Static Routing:**
- Manually configured by network administrators
- Best for small, simple networks
- Uses the command: `ip route [destination] [mask] [next-hop]`
- Doesn't adapt to network changes automatically

---

## Exercise 7: Extended Multi-Subnet

### Network Architecture

Two subnets connected through two routers with a serial link.

```
Subnet 1 (192.168.1.0/24)                    Subnet 2 (192.168.2.0/24)
PC1, PC2, PC3, PC4, PC5                      Laptop0, PC6, PC7, PC8
       │                                              │
    Switch0                                        Switch1
       │                                              │
    Router1 ─────────10.10.0.0/30───────────── Router2
   Fa0/0 (192.168.1.1)    Serial0/0/0    Serial0/0/0    Fa0/0 (192.168.2.1)
                       10.10.0.1    10.10.0.2
```

### Devices Used

- 8x PC-PT (PC1-PC8)
- 1x Laptop-PT (Laptop0)
- 2x 2960-24TT Switch (Switch0, Switch1)
- 2x Router-PT (Router1, Router2)

### Router Configurations

**Router1:**
```
enable
configure terminal
interface FastEthernet0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
interface Serial0/0/0
ip address 10.10.0.1 255.255.255.252
clock rate 64000
no shutdown
exit
ip route 192.168.2.0 255.255.255.0 10.10.0.2
exit
```

**Router2:**
```
enable
configure terminal
interface FastEthernet0/0
ip address 192.168.2.1 255.255.255.0
no shutdown
exit
interface Serial0/0/0
ip address 10.10.0.2 255.255.255.252
no shutdown
exit
ip route 192.168.1.0 255.255.255.0 10.10.0.1
exit
```

### IP Addressing

**Subnet 1 (192.168.1.0/24):**

| Device | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| Router1 Fa0/0 | 192.168.1.1 | 255.255.255.0 | - |
| PC1 | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 |
| PC2 | 192.168.1.3 | 255.255.255.0 | 192.168.1.1 |
| PC3 | 192.168.1.4 | 255.255.255.0 | 192.168.1.1 |
| PC4 | 192.168.1.5 | 255.255.255.0 | 192.168.1.1 |
| PC5 | 192.168.1.6 | 255.255.255.0 | 192.168.1.1 |

**Subnet 2 (192.168.2.0/24):**

| Device | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| Router2 Fa0/0 | 192.168.2.1 | 255.255.255.0 | - |
| Laptop0 | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 |
| PC6 | 192.168.2.3 | 255.255.255.0 | 192.168.2.1 |
| PC7 | 192.168.2.4 | 255.255.255.0 | 192.168.2.1 |
| PC8 | 192.168.2.5 | 255.255.255.0 | 192.168.2.1 |

**Serial Link (10.10.0.0/30):**

| Device | IP Address | Subnet Mask |
|---|---|---|
| Router1 Serial0/0/0 | 10.10.0.1 | 255.255.255.252 |
| Router2 Serial0/0/0 | 10.10.0.2 | 255.255.255.252 |

### Communication

- Same subnet: devices communicate through their switch
- Cross-subnet: traffic goes through both routers via the serial link

---

## Exercise 8: Complex Three-Subnet Routing

### Network Architecture

Three subnets connected through three routers forming a triangular topology.

```
                    Subnet 2 (192.168.2.0/24)
                         Switch2
                      Laptop1, PC6-PC8
                           │
                      Router2
                       │    │
        10.10.0.0/30  │    │  10.10.1.0/30
                       │    │
              Router1 ─┘    └─ Router3
                 │                │
            Switch0           Switch3
         PC1-PC5             PC9-PC11
              │                │
    Subnet 1 (192.168.1.0/26)  Subnet 3 (192.168.3.0/28)
```

### Devices Used

- 12x PC-PT + 1x Laptop-PT
- 3x 2960-24TT Switch
- 3x Router-PT

### Router Configurations

**Router1:**
```
FastEthernet0/0: 192.168.1.1 255.255.255.192 (Subnet 1)
FastEthernet0/1: 10.10.0.1 255.255.255.252 (Link to Router2)
Static Routes:
  ip route 192.168.2.0 255.255.255.0 10.10.0.2
  ip route 192.168.3.0 255.255.255.240 10.10.0.2
```

**Router2:**
```
FastEthernet0/0: 10.10.0.2 255.255.255.252 (Link to Router1)
FastEthernet0/1: 10.10.1.1 255.255.255.252 (Link to Router3)
FastEthernet0/2: 192.168.2.1 255.255.255.0 (Subnet 2)
Static Routes:
  ip route 192.168.1.0 255.255.255.192 10.10.0.1
  ip route 192.168.3.0 255.255.255.240 10.10.1.2
```

**Router3:**
```
FastEthernet0/0: 10.10.1.2 255.255.255.252 (Link to Router2)
FastEthernet0/1: 192.168.3.1 255.255.255.240 (Subnet 3)
Static Routes:
  ip route 192.168.1.0 255.255.255.192 10.10.1.1
  ip route 192.168.2.0 255.255.255.0 10.10.1.1
```

### IP Addressing

**Subnet 1 (192.168.1.0/26):**

| Device | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC1 | 192.168.1.2 | 255.255.255.192 | 192.168.1.1 |
| PC2 | 192.168.1.3 | 255.255.255.192 | 192.168.1.1 |
| PC3 | 192.168.1.4 | 255.255.255.192 | 192.168.1.1 |
| PC4 | 192.168.1.5 | 255.255.255.192 | 192.168.1.1 |
| PC5 | 192.168.1.6 | 255.255.255.192 | 192.168.1.1 |

**Subnet 2 (192.168.2.0/24):**

| Device | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| Laptop1 | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 |
| PC6 | 192.168.2.3 | 255.255.255.0 | 192.168.2.1 |
| PC7 | 192.168.2.4 | 255.255.255.0 | 192.168.2.1 |
| PC8 | 192.168.2.5 | 255.255.255.0 | 192.168.2.1 |

**Subnet 3 (192.168.3.0/28):**

| Device | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC9 | 192.168.3.2 | 255.255.255.240 | 192.168.3.1 |
| PC10 | 192.168.3.3 | 255.255.255.240 | 192.168.3.1 |
| PC11 | 192.168.3.4 | 255.255.255.240 | 192.168.3.1 |

### Routing Logic

Each router needs routes to the two remote subnets it's not directly connected to:
- **Router1**: Knows Subnet 1 (direct), needs routes to Subnet 2 and 3 via Router2
- **Router2**: Knows Subnet 2 (direct), needs routes to Subnet 1 via Router1 and Subnet 3 via Router3
- **Router3**: Knows Subnet 3 (direct), needs routes to Subnet 1 and 2 via Router2

---

## Key Concepts

### OSI Model Layers

| Layer | Name | Devices | Protocols |
|---|---|---|---|
| 7 | Application | - | HTTP, HTTPS, FTP, DNS, DHCP |
| 6 | Presentation | - | SSL/TLS, JPEG, ASCII |
| 5 | Session | - | NetBIOS, RPC |
| 4 | Transport | - | TCP, UDP |
| 3 | Network | Router | IP, ICMP |
| 2 | Data Link | Switch | Ethernet, MAC |
| 1 | Physical | Hub | Cables, Signals |

### Network Commands (Linux)

- `ifconfig` / `ip addr`: Show IP configuration
- `ping`: Test connectivity
- `traceroute`: Show path to destination
- `nslookup` / `dig`: DNS queries
- `netstat` / `ss`: Show network connections
- `tcpdump`: Capture network packets

### Subnetting Reference

| CIDR | Subnet Mask | Usable Hosts | Block Size |
|---|---|---|---|
| /24 | 255.255.255.0 | 254 | 1 |
| /27 | 255.255.255.224 | 30 | 8 |
| /28 | 255.255.255.240 | 14 | 16 |
| /29 | 255.255.255.248 | 6 | 8 |
| /30 | 255.255.255.252 | 2 | 4 |

---

## Submission

Files submitted:
- ex01.pkt
- ex02.pkt
- ex03.pkt
- ex04.pkt
- ex05.pkt
- ex06.pkt
- ex07.pkt
- ex08.pkt
- README.md
