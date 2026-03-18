---
title: Advanced Routing
icon: material/map-marker-radius
tags:
  - networking
  - routing
  - advanced
---

# Advanced Routing

Routing is how packets find their way between networks. While home routers handle this automatically, advanced configurations provide more control and capabilities.

## Basic Routing vs Advanced Routing

### Basic (Default Route)
```
Home Router:
- My network: 192.168.1.0/24
- Default gateway: ISP (sends everything else there)
```

### Advanced
```
Advanced Router:
- Multiple subnets/VLANs
- Custom routing tables
- Policy-based routing
- Multiple WAN connections
```

## Routing Tables

Every device maintains a routing table:

```bash
# Linux routing table
ip route show

# Example output:
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.10
192.168.10.0/24 via 192.168.1.1 dev eth0
default via 192.168.1.1 dev eth0
```

### Table Entries

| Destination | Gateway | Interface | Priority |
|-------------|---------|-----------|----------|
| 192.168.1.0/24 | (direct) | eth0 | 0 |
| 192.168.10.0/24 | 192.168.1.1 | eth0 | 100 |
| default | 192.168.1.1 | eth0 | 1000 |

## Static Routes

Manual route entries for specific networks:

```bash
# Add static route to 192.168.10.0/24 via gateway 192.168.1.1
ip route add 192.168.10.0/24 via 192.168.1.1

# Or in router web interface:
# Static Routes:
# 192.168.10.0/24 → 192.168.1.1
```

## Policy-Based Routing

Route based on criteria beyond just destination:

```yaml
# Example: Route specific traffic through VPN
- rule: "Work traffic via VPN"
  source: 192.168.1.50
  destination: 10.0.0.0/8
  gateway: 172.16.0.1 (VPN endpoint)
```

### Use Cases

1. **Multi-WAN** - Different ISPs for different traffic
2. **VPN routing** - Some devices through VPN, others direct
3. **QoS** - Prioritize gaming/streaming traffic
4. **Failover** - Backup connection if primary fails

## Multi-WAN (Dual ISP)

### Load Balancing
Distributes traffic across connections:

```
                    ┌─ ISP1 (100 Mbps)
Traffic ──→ Router ─┤
                    └─ ISP2 (100 Mbps)
               (Total: 200 Mbps available)
```

### Failover
Automatic switch to backup if primary fails:

```
                    ┌─ ISP1 (Primary)
Traffic ──→ Router ─┤
                    └─ ISP2 (Backup - auto on failure)
```

## OSPF and RIP

### OSPF (Open Shortest Path First)
- Advanced interior routing
- Automatically learns routes
- Fast convergence on changes
- Good for multi-router networks

### RIP (Routing Information Protocol)
- Simpler, older protocol
- Periodic full updates
- Not ideal for larger networks

*Usually not needed in home networks*

## Border Gateway Protocol (BGP)

- Used for internet-wide routing
- Complex, enterprise-level
- Not typically relevant for homelab

## Common Advanced Routing Scenarios

### 1. Route to VPN Network

```bash
# Access VPN network from home
ip route add 10.8.0.0/24 via 192.168.1.1 dev tun0
```

### 2. Route Through Different Interface

```bash
# Send traffic through specific interface
ip route add 192.168.50.0/24 dev eth1
```

### 3. Blackhole Route (Block Traffic)

```bash
# Silently drop traffic to network
ip route add 192.168.99.0/24 dev blackhole
```

## For Homelab

### Simple Multi-Subnet Routing

```
┌─────────────────────────────────────┐
│         Router/Firewall             │
│  192.168.1.1 (WAN/DHCP)             │
│  192.168.10.1 (VLAN10 - Servers)    │
│  192.168.20.1 (VLAN20 - Workstats) │
└─────────────────────────────────────┘

# Router automatically routes between VLANs
# (if firewall allows)
```

### Custom Routing for Services

```yaml
# Example:特定服务通过特定网关
- description: "Web traffic via VPN"
  src: 192.168.10.0/24
  dest: any
  gateway: 10.0.0.1 (VPN)
  protocol: tcp
  ports: 80,443
```

### Load Balancing with Multiple WANs

```
WAN1: Cable modem (primary)
WAN2: LTE/5G backup (secondary)

Config:
- Primary: 192.168.2.1 (WAN side)
- Secondary: 192.168.3.1 (WAN side)
- Load balance: 70% primary, 30% secondary
- Failover: automatic on primary loss
```

## Advanced Routing Platforms

| Platform | Complexity | Best For |
|----------|------------|----------|
| **OPNsense** | Medium | Full-featured firewall/router |
| **pfSense** | Medium | Full-featured firewall/router |
| **OpenWrt** | Medium | Custom firmware routers |
| **Ubiquiti** | Low-Med | Integrated solution |
| **Linux (iproute2)** | High | Maximum control |

---

*Advanced routing concepts based on standard routing protocols and Linux networking documentation.*