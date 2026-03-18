---
title: Basic Firewall Concepts
icon: material/fire
tags:
  - networking
  - firewall
  - security
  - intermediate
---

# Basic Firewall Concepts

A firewall monitors and controls incoming and outgoing network traffic based on predetermined security rules. It's your network's bouncer.

## How Firewalls Work

```
┌─────────────────────────────────────────────────┐
│                    FIREWALL                     │
├─────────────────────────────────────────────────┤
│                                                 │
│  Incoming Traffic    ┌─────────────┐            │
│  ─────────────────→  │  Decision   │──→ ALLOW   │
│  192.168.1.105:80    │   Engine    │            │
│                      │             │            │
│                      │  Rules:     │──→ DROP   │
│                      │  - Allow 80 │            │
│                      │  - Block 23 │            │
│                      └─────────────┘            │
│                                                 │
└─────────────────────────────────────────────────┘
```

## Types of Firewalls

### 1. Network Firewall
Hardware device or software that filters traffic between networks:
- Home router's built-in firewall
- pfSense, OPNsense, UFW

### 2. Host-Based Firewall
Software on individual devices:
- Windows Firewall
- iptables/nftables on Linux
- macOS Firewall

### 3. Application Firewall
Filters traffic for specific applications:
- WAF (Web Application Firewall)
- Next-gen firewalls (NGFW)

## Firewall Rules

### Rule Components

| Component | Description | Example |
|-----------|-------------|---------|
| **Action** | Allow or Block | ALLOW, DROP |
| **Direction** | In or Out | Inbound, Outbound |
| **Protocol** | TCP, UDP, ICMP | TCP, UDP |
| **Port** | Service port | 80, 443, 22 |
| **Source** | Where it comes from | Any, 192.168.1.0/24 |
| **Destination** | Where it's going | Any, 192.168.1.10 |

### Example Rules

```
# Allow HTTP/HTTPS from anywhere
ALLOW TCP 80,443 → ANY

# Allow SSH from internal network only
ALLOW TCP 22 → 192.168.1.0/24

# Block telnet (insecure)
DROP TCP 23 → ANY

# Allow ping (ICMP)
ALLOW ICMP → ANY
```

## Default Policies

### Conservative Approach
- **Default Deny** - Block everything, allow specific
- More secure, requires more configuration

### Permissive Approach
- **Default Allow** - Allow everything, block specific
- Less secure, easier to manage initially

**Recommended:** Default deny for incoming, allow outbound

## Stateful vs Stateless

### Stateful Firewall
Tracks active connections:
- Remembers if a packet is part of an existing connection
- Automatically allows return traffic
- Most modern firewalls are stateful

### Stateless Firewall
Evaluates each packet independently:
- No memory of previous packets
- Must explicitly allow return traffic
- Older, simpler firewalls

## Common Ports to Know

| Port | Service | Notes |
|------|---------|-------|
| 22 | SSH | Secure remote access |
| 80 | HTTP | Plain web traffic |
| 443 | HTTPS | Encrypted web |
| 53 | DNS | Domain resolution |
| 21 | FTP | File transfer (legacy) |
| 25 | SMTP | Email sending |
| 3306 | MySQL | Database |
| 5432 | PostgreSQL | Database |
| 6379 | Redis | Cache/DB |
| 8080 | HTTP Alt | Common alt port |

## Home Router Firewall

Most home routers have basic firewall built in:

### Port Forwarding vs Blocking

```
Port Forwarding:
External :8080 → Internal 192.168.1.10:80
(Expose web server to internet)

Block All Unlisted:
Incoming connections not explicitly forwarded are blocked
(Good default security)
```

### Common Router Firewall Features

- **SPI** (Stateful Packet Inspection) - Track connections
- **DMZ** - Expose one device to internet (risky!)
- **NAT** - Always on by default
- **WAN Blocking** - Block all inbound

## For Homelab

### Basic Server Firewall (UFW - Ubuntu)

```bash
# Allow SSH
ufw allow ssh

# Allow HTTP/HTTPS
ufw allow 80/tcp
ufw allow 443/tcp

# Allow specific IP
ufw allow from 192.168.1.50 to any port 22

# Enable firewall
ufw enable
```

### Best Practices

1. **Default deny incoming** - Block by default, allow needed
2. **Least privilege** - Only open what's necessary
3. **Log blocked traffic** - Monitor for issues
4. **Test rules** - Verify behavior works as expected
5. **Document** - Keep list of what you allow and why

---

*Firewall concepts based on standard networking security practices and iptables documentation.*