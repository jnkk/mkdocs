# Subnets & CIDR

A subnet (subnetwork) is a logical subdivision of an IP network. CIDR (Classless Inter-Domain Routing) is the notation system used to define subnets.

## Why Subnet?

Subnets allow you to:
- **Divide a large network** into smaller, manageable pieces
- **Isolate traffic** - Devices in different subnets don't directly communicate
- **Improve security** - Control which subnets can access each other
- **Reduce broadcast** - Smaller broadcast domains

## CIDR Notation Explained

```
192.168.1.0/24
       └──┬──┘
   Number of bits used for network portion
```

### Breaking It Down

| CIDR | Subnet Mask | Total IPs | Usable IPs |
|------|-------------|-----------|------------|
| /32 | 255.255.255.255 | 1 | 1 (single host) |
| /30 | 255.255.255.252 | 4 | 2 |
| /29 | 255.255.255.248 | 8 | 6 |
| /28 | 255.255.255.240 | 16 | 14 |
| /27 | 255.255.255.224 | 32 | 30 |
| /26 | 255.255.255.192 | 64 | 62 |
| /25 | 255.255.255.128 | 128 | 126 |
| /24 | 255.255.255.0 | 256 | 254 |
| /23 | 255.255.254.0 | 512 | 510 |
| /22 | 255.255.252.0 | 1024 | 1022 |

## Calculating Subnets

### Quick Rule: 32 - CIDR = host bits

```
/24 → 32-24 = 8 host bits → 2^8 = 256 IPs
/26 → 32-26 = 6 host bits → 2^6 = 64 IPs
```

### Example: 192.168.1.0/24

```
Network: 192.168.1.0
First IP: 192.168.1.1 (usually gateway)
Last IP: 192.168.1.254
Broadcast: 192.168.1.255
Netmask: 255.255.255.0
```

### Example: 192.168.1.64/26

```
Network: 192.168.1.64
First IP: 192.168.1.65
Last IP: 192.168.1.126
Broadcast: 192.168.1.127
Netmask: 255.255.255.192
```

## Subnetting Example

Starting with: `192.168.1.0/24` (254 hosts)

Divide into four /26 subnets:

```
┌────────────────────────────────────────────┐
│         192.168.1.0/24 (254 hosts)          │
├────────────┬────────────┬──────────────────┤
│ Subnet 1   │ Subnet 2   │ Subnet 3  Subnet 4
│ .0/26      │ .64/26     │ .128/26  .192/26
│ .1-.62     │ .65-.126   │ .129-.190 .193-.254
└────────────┴────────────┴──────────────────┘
```

## Common Home Network Subnets

### Basic Setup (Single Subnet)

```
Network: 192.168.1.0/24
- Gateway: 192.168.1.1
- Devices: 192.168.1.2 - 192.168.1.254
- All devices can communicate directly
```

### Advanced Setup (Multiple Subnets)

```
┌─────────────────────────────────────┐
│ Home Network                        │
├─────────────────────────────────────┤
│ 192.168.1.0/24 - Main devices       │
│ 192.168.2.0/24 - IoT devices        │
│ 192.168.3.0/24 - Guest network      │
│ 192.168.10.0/24 - Homelab servers   │
└─────────────────────────────────────┘
```

### VLAN-Based (More Secure)

```
VLAN 10 - Management (192.168.10.0/24)
VLAN 20 - Production (192.168.20.0/24)
VLAN 30 - Guests (192.168.30.0/24)
```

## Subnet Masks in Binary

Understanding binary helps visualize subnets:

```
/24: 11111111.11111111.11111111.00000000
     (Network)  (Network)  (Network)  (Host)

/26: 11111111.11111111.11111111.11000000
     (Network)  (Network)  (Network)  (Host bits)
```

## For Homelab: Planning Your Subnets

### Recommended Allocation

| Subnet | Purpose | Example |
|--------|---------|---------|
| /24 (254 hosts) | Main network | 192.168.1.0/24 |
| /26 (62 hosts) | Homelab | 192.168.10.0/26 |
| /26 (62 hosts) | IoT/Smart Home | 192.168.20.0/26 |
| /27 (30 hosts) | Docker/Containers | 192.168.30.0/27 |
| /28 (14 hosts) | DMZ/Exposed services | 192.168.40.0/28 |

### Tools for Planning

- **Subnets.com** - Web-based subnet calculator
- **ipcalc** - Command line tool
- **Visual Subnet Calculator** - Various online tools

---

*Subnet and CIDR concepts based on RFC 1517-1519 and standard networking practice.*