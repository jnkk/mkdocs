---
title: Routers and Subnets
icon: material/router-wireless
tags:
  - homelab
  - networking
  - router
  - subnet
  - vlan
date_added: 2026-03-18
last_updated: 2026-03-18
---

# Routers and Subnets

How to create separate networks in your home for better security and organization.

## Why Separate Networks?

- **Security** - Isolate IoT devices from your main devices
- **Performance** - Reduce broadcast traffic
- **Control** - Different rules for different device types
- **Organization** - Keep homelab separate from personal

## Understanding Your Router

### Basic Consumer Router (ISP-provided)

Typical features:
- 1 main LAN network (192.168.1.0/24)
- Maybe a "guest network" (separate subnet)
- Usually NO VLAN support
- Limited firewall rules

**Typical home router:**
```
┌─────────────────────┐
│  ISP Router         │
│  192.168.1.1        │
│                     │
│  LAN: 192.168.1.0/24│
│  (all devices)      │
└─────────────────────┘
```

### What You Can Change

| Feature | Consumer Router | Better Router/OPNsense |
|---------|-----------------|------------------------|
| Multiple LANs | Guest network (maybe) | Yes - full control |
| VLANs | Rarely | Yes |
| Firewall rules | Basic | Advanced |
| DHCP reservations | Yes | Yes |
| QoS | Rarely | Yes |

## Options for Creating Subnets

### Option 1: Guest Network (Easiest)

If your router supports it:

```
Main LAN: 192.168.1.0/24  (your devices)
Guest:    192.168.2.0/24  (visitors, isolated)
```

**Pros:** Easy, no hardware needed
**Cons:** Limited control, may not truly isolate

### Option 2: Second Router

Connect second router to main router's LAN port:

```
ISP → Main Router (192.168.1.1)
        ↓
        [LAN port] → Second Router WAN
                          ↓
                    192.168.2.0/24 (new network)
```

**Setup:**
1. Connect second router's WAN to main router's LAN
2. Set second router to get IP via DHCP (or static)
3. Devices on second router are in separate subnet

**Pros:** Simple, separate network
**Cons:** Two DHCP servers, more to manage

### Option 3: Custom Firmware (OpenWrt)

Flash your router with OpenWrt for more features:

**Compatible routers:** https://openwrt.org/toh

**What you get:**
- Full VLAN support
- Advanced firewall
- Custom routing
- More control

**Pros:** Free, powerful
**Cons:** Void warranty, technical setup

### Option 4: Better Router (Recommended)

Buy a router with built-in features:

| Router | Price | Features |
|--------|-------|----------|
| Ubiquiti EdgeRouter X | ~£50 | VLANs, firewall, routing |
| TP-Link Omada ER7206 | ~£60 | VLANs, VPN, controller |
| Netgate SG-1100 | ~£100 | pfSense, enterprise features |

**Pros:** Works out of the box, reliable
**Cons:** Costs money

### Option 5: OPNsense/pfSense (Full Control)

Run dedicated firewall software:

```
Hardware: Old PC or Protectli Vault (~£150-200)
↓
Install OPNsense/pfSense
↓
Full VLAN support, firewall, routing, VPN, and more
```

**Pros:** Maximum control, enterprise features
**Cons:** Requires dedicated hardware, learning curve

## Recommended Homelab Setup

### For Beginners

Start with what you have:
- Use guest network for IoT if available
- Second router for homelab if needed

### For Serious Homelab

```
┌────────────────────────────────────────────────────┐
│                   Recommended Setup                │
├────────────────────────────────────────────────────┤
│                                                    │
│  ISP → Modem → [WAN] OPNsense/pfSense [LAN]       │
│                    │                               │
│            ┌────────┴────────┐                    │
│            ↓                 ↓                    │
│      192.168.10.0/24   192.168.20.0/24           │
│      (Main devices)    (Homelab servers)          │
│                                                    │
│      VLAN10: Trusted   VLAN20: Homelab             │
│      VLAN30: Guest     VLAN40: IoT                 │
│                                                    │
└────────────────────────────────────────────────────┘
```

### Subnet Allocation Example

| Subnet | Purpose | Example Range | Devices |
|--------|---------|---------------|---------|
| /24 | Main network | 192.168.1.0/24 | Laptops, phones |
| /26 | Homelab | 192.168.10.0/26 | Servers, VMs |
| /26 | IoT | 192.168.20.0/26 | Smart devices |
| /27 | DMZ | 192.168.30.0/27 | Exposed services |
| /28 | Management | 192.168.40.0/28 | Network gear |

## Setting Up VLANs (If Your Router Supports)

### In Router Interface

1. Create VLANs (e.g., VLAN 10, VLAN 20)
2. Assign ports to each VLAN
3. Set up DHCP for each VLAN
4. Configure inter-VLAN routing/firewall

### Example: TP-Link Omada

```
Settings → Wireless Networks → VLAN
→ Add VLAN: ID 20, Subnet 192.168.20.0/24
→ Assign SSID to VLAN
```

### Example: OPNsense

```
Interfaces → VLANs → Add
→ Parent: LAN, VLAN Tag: 20
→ Create interface, enable DHCP
```

## Firewall Between Subnets

Default: VLANs can't communicate (isolated)

To allow specific traffic:

```
# Example: Allow homelab to access main network
Pass: Source VLAN20 → Destination VLAN10
Block: Source VLAN30 → Destination VLAN10, VLAN20
```

## Summary

| Approach | Cost | Difficulty | Features |
|----------|------|-------------|----------|
| Guest network | Free | Easy | Limited |
| Second router | Low | Easy | Basic |
| OpenWrt | Free | Medium | Advanced |
| Better router | £50-100 | Easy | Good |
| OPNsense/pfSense | £150+ | Hard | Maximum |

**Start simple, upgrade as needed!**

---

*Networking best practices based on standard networking literature and home lab configurations.*