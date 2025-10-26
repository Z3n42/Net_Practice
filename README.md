<div align="center">

# 🌐 NetPractice

### TCP/IP Addressing and Network Configuration

<p>
  <img src="https://img.shields.io/badge/Score-100%2F100-success?style=for-the-badge&logo=42" alt="Score"/>
  <img src="https://img.shields.io/badge/Type-System_Administration-blue?style=for-the-badge" alt="Type"/>
  <img src="https://img.shields.io/badge/Levels-10-orange?style=for-the-badge" alt="Levels"/>
  <img src="https://img.shields.io/badge/42-Urduliz-000000?style=for-the-badge&logo=42&logoColor=white" alt="42 Urduliz"/>
</p>

*A hands-on introduction to TCP/IP networking, subnetting, and routing through 10 progressive exercises.*

[Overview](#-overview) • [Networking Concepts](#-networking-concepts) • [Levels Guide](#-levels-guide) • [Resources](#-resources)

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Project Structure](#-project-structure)
- [Networking Concepts](#-networking-concepts)
  - [IP Addressing](#ip-addressing)
  - [Subnet Masks](#subnet-masks)
  - [Routing](#routing)
  - [Network Classes](#network-classes)
- [Levels Guide](#-levels-guide)
- [Common Patterns](#-common-patterns)
- [Troubleshooting](#-troubleshooting)
- [Resources](#-resources)

---

## 🎯 Overview

**NetPractice** is a browser-based training tool that teaches fundamental networking concepts through hands-on configuration exercises. You'll learn how devices communicate in a network by configuring IP addresses, subnet masks, and routing tables across 10 progressively challenging levels.

### What You'll Learn

- **TCP/IP addressing** fundamentals
- **Subnetting** and CIDR notation
- **Routing** between networks
- **Network troubleshooting** skills
- **IP address planning** for small networks

### Project Format

- **Training Interface**: Browser-based (index.html)
- **10 Levels**: Each saved as a JSON configuration file
- **Defense**: Solve 3 random levels (6-10) in 15 minutes
- **Goal**: Make networks communicate by configuring IP addresses, masks, and routes

---

## 📁 Project Structure

```
NetPractice/
├── index.html          # Training interface (open in browser)
├── level1.json         # Level 1 solution
├── level2.json         # Level 2 solution
├── level3.json         # Level 3 solution
├── level4.json         # Level 4 solution
├── level5.json         # Level 5 solution
├── level6.json         # Level 6 solution
├── level7.json         # Level 7 solution
├── level8.json         # Level 8 solution
├── level9.json         # Level 9 solution
└── level10.json        # Level 10 solution
```

**Submission**: Export each level's configuration using "Get my config" button and commit all 10 JSON files to your repository.

---

## 🌐 Networking Concepts

### IP Addressing

An **IP address** is a unique identifier for a device on a network. In IPv4, it consists of 4 octets (32 bits total).

**Format**: `XXX.XXX.XXX.XXX` (each octet: 0-255)

**Example**: `192.168.1.1`

- **Binary representation**: `11000000.10101000.00000001.00000001`
- Each octet is 8 bits (1 byte)

**IP Address Components**:
- **Network portion**: Identifies the network
- **Host portion**: Identifies the specific device on that network

The **subnet mask** determines which part is network and which is host.

### Subnet Masks

A **subnet mask** separates the network and host portions of an IP address.

**Common Subnet Masks**:

| CIDR | Subnet Mask | Binary | Usable Hosts | Network Size |
|------|-------------|--------|--------------|--------------|
| /32 | 255.255.255.255 | 11111111.11111111.11111111.11111111 | 1 (point-to-point) | 1 host |
| /30 | 255.255.255.252 | 11111111.11111111.11111111.11111100 | 2 | 4 addresses |
| /29 | 255.255.255.248 | 11111111.11111111.11111111.11111000 | 6 | 8 addresses |
| /28 | 255.255.255.240 | 11111111.11111111.11111111.11110000 | 14 | 16 addresses |
| /27 | 255.255.255.224 | 11111111.11111111.11111111.11100000 | 30 | 32 addresses |
| /26 | 255.255.255.192 | 11111111.11111111.11111111.11000000 | 62 | 64 addresses |
| /25 | 255.255.255.128 | 11111111.11111111.11111111.10000000 | 126 | 128 addresses |
| /24 | 255.255.255.0 | 11111111.11111111.11111111.00000000 | 254 | 256 addresses (Class C) |
| /16 | 255.255.0.0 | 11111111.11111111.00000000.00000000 | 65,534 | 65,536 addresses (Class B) |
| /8 | 255.0.0.0 | 11111111.00000000.00000000.00000000 | 16,777,214 | 16,777,216 addresses (Class A) |

**CIDR Notation**: IP/mask length  
Example: `192.168.1.0/24` = `192.168.1.0` with mask `255.255.255.0`

<details>
<summary><b>How Subnet Masks Work (Binary Example)</b></summary>

**IP Address**: `192.168.1.10`  
**Subnet Mask**: `255.255.255.0` (/24)

```
IP Address:    11000000.10101000.00000001.00001010  (192.168.1.10)
Subnet Mask:   11111111.11111111.11111111.00000000  (255.255.255.0)
               ^^^^^^^^ ^^^^^^^^ ^^^^^^^^ ^^^^^^^^
               Network  Network  Network   Host
```

**Network Address** (IP AND Mask): `192.168.1.0`  
**Broadcast Address** (all host bits = 1): `192.168.1.255`  
**Usable Range**: `192.168.1.1` - `192.168.1.254` (254 hosts)

**Two devices can communicate directly if they're in the same network** (network portion matches).

</details>

### Routing

**Routing** directs packets from source to destination across networks.

**Routing Table Entry**:
- **Destination**: Target network (e.g., `10.0.0.0/8` or `0.0.0.0/0` for default)
- **Gateway**: Next hop router IP address
- **Interface**: Which network interface to use

**Default Route** (`0.0.0.0/0`):
- Matches **any** destination not in routing table
- Gateway: Router IP address to forward unknown destinations

**Example**:
```
Destination       Gateway         Interface
0.0.0.0/0         192.168.1.1     eth0       # Default route
10.0.0.0/8        10.0.0.254      eth1       # Specific network
```

**Routing Decision**:
1. Check if destination is in same network (use subnet mask)
2. If not, look for specific route in routing table
3. If no match, use default route
4. Send packet to gateway/next hop

### Network Classes

**Private IP Ranges** (RFC 1918 - not routable on internet):

| Class | Range | Default Mask | CIDR | Use Case |
|-------|-------|--------------|------|----------|
| **A** | 10.0.0.0 - 10.255.255.255 | 255.0.0.0 | /8 | Large networks |
| **B** | 172.16.0.0 - 172.31.255.255 | 255.255.0.0 | /16 | Medium networks |
| **C** | 192.168.0.0 - 192.168.255.255 | 255.255.255.0 | /24 | Small networks (home/office) |

**Special Addresses**:
- **0.0.0.0**: "This network" or default route
- **127.0.0.1**: Loopback (localhost)
- **255.255.255.255**: Broadcast to all hosts on local network
- **Network address**: First address in subnet (all host bits = 0)
- **Broadcast address**: Last address in subnet (all host bits = 1)

---

## 📊 Levels Guide

### Level 1 - Basic Addressing

**Concepts**: Simple point-to-point connections, IP assignment

**Goal**: Configure IP addresses for interfaces to enable communication between two clients.

**Key Points**:
- Devices must be in the **same network** to communicate directly
- No routing required for direct connections
- Use same network prefix with different host portions

**Example**:
```
Client A (A1): 104.97.23.11/24
Client B (B1): 104.97.23.12/24
```
Both in `104.97.23.0/24` network → can communicate

---

### Level 2 - Introduction to Subnetting

**Concepts**: Subnet masks, network/host separation

**Goal**: Configure IP addresses and subnet masks for multiple pairs of devices.

**Key Points**:
- Subnet mask determines network size
- `/24` = 254 usable hosts
- `/30` = 2 usable hosts (point-to-point links)
- Check that IPs are in valid range for their subnet

**Example**:
```
Client A (A1): 192.168.71.221/24    Mask: 255.255.255.0
Client B (B1): 192.168.71.222/24    Mask: 255.255.255.0
```

---

### Level 3 - Multiple Networks

**Concepts**: Different subnets, switches

**Goal**: Configure networks with switches connecting multiple devices.

**Key Points**:
- Switch connects devices in **same network**
- All devices on same switch must share network prefix
- Use appropriate subnet mask for number of hosts needed

**Example** (3 hosts + switch):
```
Client A: 104.198.23.123/25    Mask: 255.255.255.128
Client B: 104.198.23.124/25    Mask: 255.255.255.128
Client C: 104.198.23.125/25    Mask: 255.255.255.128
```
Network: `104.198.23.0/25` (126 usable hosts)

---

### Level 4 - Introduction to Routers

**Concepts**: Routers, multiple interfaces, inter-network communication

**Goal**: Configure router interfaces to connect separate networks.

**Key Points**:
- Router has **multiple interfaces** (one per network)
- Each router interface must be in the network it connects to
- Router acts as **gateway** between networks
- Router interfaces typically use first or last usable IP in range

**Example**:
```
Network 1: 101.231.0.0/16
  - Client A: 101.231.118.133/16
  - Router R1 interface: 101.231.118.131/16

Network 2: 101.232.0.0/16
  - Router R2 interface: 101.232.0.1/16
  - Client B: 101.232.0.2/16
```

---

### Level 5 - Routing Tables

**Concepts**: Default routes, gateways, routing configuration

**Goal**: Configure routing tables so devices know how to reach networks outside their own.

**Key Points**:
- **Default route**: `0.0.0.0/0` or keyword `default`
- **Gateway**: IP address of router interface in same network
- Client's gateway must be reachable (same subnet)
- Router must have routes for all networks it connects

**Example**:
```
Client A: 92.61.17.125/25
Default route: 0.0.0.0/0 via 92.61.17.126

Router R1 interface (A side): 92.61.17.126/25
Router R1 interface (B side): 159.79.156.254/18

Client B: 159.79.156.253/18
Default route: 0.0.0.0/0 via 159.79.156.254
```

---

### Level 6 - Multiple Routers

**Concepts**: Multi-hop routing, router-to-router communication

**Goal**: Configure routing across multiple routers.

**Key Points**:
- Routers must route to each other
- Each router needs routes for networks it doesn't directly connect to
- **Bidirectional routing**: forward path AND return path must work
- Router interfaces connecting to each other must be in same network

---

### Level 7 - Complex Topologies

**Concepts**: Multiple networks, complex routing scenarios

**Goal**: Configure networks with multiple routers and clients ensuring bidirectional communication.

**Key Points**:
- **Forward path**: A → Router1 → Router2 → B
- **Return path**: B → Router2 → Router1 → A
- Both paths must be configured
- Check logs to see which direction fails

---

### Level 8 - Internet Routing

**Concepts**: Internet connectivity, overlapping subnets

**Goal**: Configure routing to internet and between clients.

**Key Points**:
- Internet gateway acts as default route
- May need specific routes for internal networks
- **Route priority**: More specific routes match before default route
- Check for overlapping subnets (can cause routing conflicts)

---

### Level 9 - Advanced Subnetting

**Concepts**: Efficient IP address allocation, VLSM (Variable Length Subnet Masking)

**Goal**: Use appropriate subnet sizes for different networks.

**Key Points**:
- Use smallest subnet that fits requirements
- `/30` for point-to-point router links (2 hosts)
- `/29` for small networks (6 hosts)
- `/24` or larger for many hosts
- Avoid wasting IP addresses

---

### Level 10 - Final Challenge

**Concepts**: All previous concepts combined

**Goal**: Configure a complex network topology with multiple routers, subnets, and routing requirements.

**Key Points**:
- Plan IP address allocation carefully
- Ensure no overlapping subnets
- Configure all routing tables
- Test both directions of communication
- Check logs systematically

---

## 🔧 Common Patterns

### Pattern 1: Same Network Communication

**Requirements**: Two devices in same network

```
Device A: 192.168.1.10/24
Device B: 192.168.1.20/24
```

✅ **Network**: `192.168.1.0/24`  
✅ **No routing needed** (same network)

### Pattern 2: Router as Gateway

**Requirements**: Client needs to reach other network

```
Client: 192.168.1.10/24
Router interface (client side): 192.168.1.1/24
Client default route: 0.0.0.0/0 via 192.168.1.1
```

✅ **Gateway must be in same network as client**

### Pattern 3: Point-to-Point Router Link

**Requirements**: Two routers connected

```
Router A interface: 10.0.0.1/30
Router B interface: 10.0.0.2/30
```

✅ **/30 mask** provides exactly 2 usable IPs  
✅ Efficient for router-to-router links

### Pattern 4: Subnet Calculation

**Given**: Need 50 hosts in network

- **Minimum hosts**: 50
- **Add**: Network address + Broadcast = 52 total
- **Next power of 2**: 64 (2^6)
- **Subnet**: /26 (255.255.255.192) provides 62 usable hosts

✅ Formula: **Usable hosts = 2^(32-CIDR) - 2**

---

## 🔍 Troubleshooting

### Error: "No route to destination"

**Problem**: Routing table missing entry

**Solution**:
- Add default route (`0.0.0.0/0`)
- Add specific route for destination network
- Verify gateway IP is reachable

### Error: "Packet not for me"

**Problem**: Router receives packet but has no route to forward it

**Solution**:
- Check router's routing table
- Add route for destination network
- Verify all routers in path have correct routes

### Error: "No reverse way"

**Problem**: Forward path works, but return path fails

**Solution**:
- Check routing on destination side
- Ensure return router knows how to reach source
- Verify bidirectional routing configuration

### Error: "Destination unreachable"

**Problem**: IP configuration mismatch

**Solution**:
- Verify IPs are in same network (if direct connection)
- Check subnet masks match
- Ensure no IP conflicts

### Common Mistakes

❌ **Different subnet masks** on same network  
✅ All devices in same network must use same mask

❌ **Gateway not in client's network**  
✅ Gateway must be reachable (same subnet as client)

❌ **Using network or broadcast address** for devices  
✅ Use only usable IP range (not first or last)

❌ **Forgetting return route**  
✅ Configure routing in both directions

---

## 📚 Resources

### Official Documentation

- [RFC 791 - Internet Protocol](https://tools.ietf.org/html/rfc791)
- [RFC 1918 - Private Address Space](https://tools.ietf.org/html/rfc1918)
- [RFC 950 - Internet Standard Subnetting](https://tools.ietf.org/html/rfc950)

### Learning Resources

**TCP/IP Fundamentals**:
- [Cisco Networking Basics](https://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/13788-3.html)
- [Subnet Calculator](https://www.subnet-calculator.com/)
- [Visual Subnet Calculator](https://www.davidc.net/sites/default/subnets/subnets.html)

**Subnetting Guides**:
- [Subnetting Made Easy](https://www.practicalnetworking.net/stand-alone/subnetting-mastery/)
- [IP Addressing and Subnetting](https://www.youtube.com/watch?v=XQ3T14SIlV4) (Professor Messer)
- [Subnetting Practice](https://subnettingpractice.com/)

**Routing Concepts**:
- [How Routing Works](https://www.cloudflare.com/learning/network-layer/what-is-routing/)
- [Understanding IP Routing](https://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/13769-5.html)

### Tools

**Subnet Calculators**:
- [ipcalc](http://jodies.de/ipcalc) - Online subnet calculator
- [Subnet Calc](https://www.calculator.net/ip-subnet-calculator.html) - Detailed subnet info

**Network Simulators**:
- [Packet Tracer](https://www.netacad.com/courses/packet-tracer) - Cisco network simulator
- [GNS3](https://www.gns3.com/) - Network emulation software

### Quick Reference

**Binary to Decimal Conversion**:
```
128  64  32  16  8  4  2  1
 2⁷  2⁶  2⁵  2⁴  2³ 2² 2¹ 2⁰
```

**CIDR to Subnet Mask**:
```
/24 = 255.255.255.0     (Class C default)
/25 = 255.255.255.128   (Half of /24)
/26 = 255.255.255.192   (Quarter of /24)
/27 = 255.255.255.224   
/28 = 255.255.255.240
/29 = 255.255.255.248
/30 = 255.255.255.252   (Point-to-point)
```

**Usable Hosts Formula**:
```
Usable Hosts = 2^(32 - CIDR) - 2
```

**Example**: /26 network  
`2^(32-26) - 2 = 2^6 - 2 = 64 - 2 = 62 usable hosts`

---

## 🎓 Key Takeaways

### Fundamental Rules

1. **Same network = direct communication**  
   Different networks = routing required

2. **Gateway must be reachable**  
   Client and gateway must be in same subnet

3. **Routing is bidirectional**  
   Configure forward AND return paths

4. **Subnet mask must match**  
   All devices in same network use same mask

5. **Default route catches all**  
   Use `0.0.0.0/0` for "everything else"

### Subnetting Quick Tips

- **/30**: Point-to-point links (2 hosts)
- **/24**: Standard small network (254 hosts)
- **/16**: Medium network (65,534 hosts)
- **Smaller CIDR = bigger network** (/16 is bigger than /24)

### Routing Quick Tips

- **Check logs** to see where packets fail
- **Forward ≠ Reverse**: Both directions need routes
- **Most specific route wins**: /24 matches before /0
- **Default route**: Gateway to "outside world"

---

## 🎯 Defense Tips

During evaluation, you'll solve 3 random levels (6-10) in 15 minutes:

1. **Read the goal carefully** - Know what needs to communicate
2. **Check existing configuration** - Some fields are pre-filled
3. **Start with IP addresses** - Ensure devices are in correct networks
4. **Configure routes next** - Default routes for clients, specific routes for routers
5. **Test incrementally** - Use "Check again" to verify progress
6. **Read logs** - They show exactly where communication fails
7. **Work systematically** - One network at a time

---

## 🔗 Related Projects

- **ft_transcendence**: Web application with networking concepts
- **inception**: Docker networking and containers
- **webserv**: HTTP server with socket programming

Skills from NetPractice apply to:
- **System Administration**: Network configuration
- **DevOps**: Understanding network topologies
- **Cloud Computing**: VPC and subnet design
- **Cybersecurity**: Network segmentation and security

---

<div align="center">

**Made with ☕ by [Iñigo Gonzalez](https://github.com/Z3n42)**

*42 Urduliz | System Administration | Circle 3*

[![42 Profile](https://img.shields.io/badge/42_Intra-ingonzal-000000?style=flat&logo=42&logoColor=white)](https://profile.intra.42.fr/users/ingonzal)

</div>
