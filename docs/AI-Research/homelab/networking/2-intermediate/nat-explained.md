---
title: NAT Explained
icon: material/swap-horizontal
tags:
  - networking
  - nat
  - intermediate
---

# NAT Explained

NAT (Network Address Translation) is a technique that allows multiple devices to share a single public IP address. It's essential for home networks because IPv4 addresses are limited.

## The Problem NAT Solves

Your ISP gives you ONE public IP address, but you have dozens of devices at home:

```
Before NAT:
┌─────────────────┐      ┌──────────────────┐
│   ISP Network   │ ←──→ │   Your Network   │
│   Public IPs    │      │   Private IPs    │
│                 │      │ (192.168.x.x)    │
└─────────────────┘      └──────────────────┘
                    ✗ No translation
                    Only ONE device can work!
```

## How NAT Works

```
After NAT:
┌─────────────────┐      ┌──────────────────┐
│   ISP Network   │ ←──→ │   Your Router    │ ←─┐
│   Public IP     │      │   (NAT Gateway)  │   │
│   203.0.113.50  │      └──────────────────┘   │
                    │                              │
                    │         ┌──────────────┐     │
                    └────────→│ 192.168.1.10 │     │
                    └────────→│ 192.168.1.11 │     │
                    └────────→│ 192.168.1.12 │─────┘
                     (Outgoing) (Many private IPs)
```

### Translation Process

1. **Device sends data** - Your laptop (192.168.1.10) requests google.com
2. **Router intercepts** - Replaces source IP with public IP
3. **Track connection** - Router remembers which internal device made request
4. **Response comes back** - Router looks up original request, forwards to correct device

## Types of NAT

### 1. Static NAT
One-to-one mapping (rarely used in homes):

```
192.168.1.10 ↔ 203.0.113.50 (always)
192.168.1.11 ↔ 203.0.113.51
```

### 2. Dynamic NAT
Pool-based (also rare for home):

```
192.168.1.10 ↔ 203.0.113.50 (available)
192.168.1.11 ↔ 203.0.113.51 (available)
```
No fixed mapping - gets any available from pool.

### 3. PAT (Port Address Translation)
Most common - Many-to-one:

```
192.168.1.10:54321 → 203.0.113.50:54321
192.168.1.11:54322 → 203.0.113.50:54322
192.168.1.12:54323 → 203.0.113.50:54323
```

## Port Forwarding

Port forwarding is NAT in reverse - allowing external traffic to reach internal devices:

```
Internet          Router              Internal Server
   │                  │                      │
   │ ──→ :80 ──────→  │ ──→ 192.168.1.10:80  │
   │ (Port 80)        │                      │
```

### Example: Hosting a Web Server

```
Router: Forward port 80 → 192.168.1.10:80
External: http://your-public-ip:80 → Your server
```

## NAT in Your Home Router

Typical home router NAT table:

```
┌────────────────────────────────────────────────┐
│   NAT Table (Active Connections)              │
├──────────────┬─────────────┬──────────────────┤
│ Internal IP  │ Internal    │ External   Port  │
│              │ Port        │ Port        (ext)│
├──────────────┼─────────────┼──────────────────┤
│ 192.168.1.10 │ 49156       │ 54321      54321 │
│ 192.168.1.11 │ 49157       │ 54322      54322 │
│ 192.168.1.12 │ 49158       │ 54323      54323 │
└──────────────┴─────────────┴──────────────────┘
```

## NAT Limitations

### Inbound Connections
- Can't initiate connection to internal device from outside
- Requires port forwarding or reverse connections

### NAT Traversal
- Some protocols don't work well through NAT
- P2P, VoIP, gaming can have issues
- Solutions: STUN, TURN, port forwarding

### IPv6 Impact
- With IPv6, every device CAN have a unique public IP
- NAT less necessary (but still used for security)

## For Homelab

### Running Services Behind NAT

1. **Port Forwarding** - Open specific ports to your server
2. **DMZ** - Expose one device (risky)
3. **Reverse Proxy** - One port (80/443) serves multiple services

### Viewing NAT Table

```bash
# Linux
iptables -t nat -L -n

# Router web interface
Look for "NAT", "Port Forwarding", or "Virtual Servers"
```

---

*NAT concepts based on RFC 1631 and standard home router implementation documentation.*