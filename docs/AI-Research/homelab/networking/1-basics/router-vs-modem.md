# Router vs Modem

These two devices are often combined in modern home equipment, but they serve different purposes.

## The Modem

**Purpose:** Connect your network to your ISP (Internet Service Provider)

The modem translates between:
- Your home network (Ethernet/WiFi)
- Your ISP's network (Fiber/Cable/DSL/5G)

### Types of Modems

| Type | Technology | Typical Speed |
|------|------------|---------------|
| **DSL** | Phone lines | Up to 100 Mbps |
| **Cable** | Coaxial cable | Up to 1 Gbps |
| **Fiber** | Fiber optic | Up to 10+ Gbps |
| **5G/Cellular** | Mobile network | Variable |

### How It Works

```
[ISP Network] ←→ [Modem] ←→ [Your Network]
    (WAN)              (LAN)
```

The modem has:
- **WAN port** - Connects to ISP (coaxial, fiber, or DSL line)
- **LAN port(s)** - Connects to your router or directly to devices

## The Router

**Purpose:** Manage traffic between devices on your network and the internet

The router:
- Assigns IP addresses (DHCP)
- Routes traffic between devices
- Provides NAT (Network Address Translation)
- Often includes firewall functionality

### How It Works

```
[Devices] ←→ [Router] ←→ [Modem] ←→ [Internet]
    LAN            WAN
```

The router has:
- **LAN ports** - Connect wired devices
- **WiFi** - Creates wireless network
- **WAN port** - Connects to modem

## Combined Devices (Most Common)

Most consumer "routers" are actually **router-modem combos**:

```
┌─────────────────────────────────────┐
│         Router + Modem Combo        │
│  (Your ISP-provided or bought)      │
│                                      │
│  ┌─────────┐    ┌─────────────────┐ │
│  │  Modem │ ←→ │     Router      │ │
│  └─────────┘    └─────────────────┘ │
│       ↑               ↑             │
│   WAN input       LAN/WiFi         │
└─────────────────────────────────────┘
```

## Why Separate Them?

For advanced homelabs, separating can be beneficial:

### Benefits of Separate Devices

1. **More control** - Choose your own router with better features
2. **Better performance** - Dedicated hardware for each function
3. **Flexibility** - Upgrade router without changing ISP connection
4. **Advanced features** - VLANs, VPN, traffic shaping, better firewall

### Example Setup

```
ISP → Modem → [WAN] Router [LAN] → Switch → Devices
                        │
                        └─ WiFi Access Point
```

## For Your Homelab

### Recommended Setup

1. **Use ISP modem in bridge mode** - Let your own router handle everything
2. **Get a better router** - Better features, more control
3. **Add a switch** - More Ethernet ports for servers/VMs
4. **Add WiFi APs** - Better WiFi coverage than router antennas

### Common Router Options for Homelab

| Type | Examples | Best For |
|------|----------|----------|
| **Consumer** | Netgear, TP-Link, Asus | Basic needs |
| **Prosumer** | Ubiquiti EdgeRouter, Flame | Advanced features |
| **Enterprise** | Cisco, Juniper | Full control |
| **Open Source** | OpenWrt, PfSense, OPNsense | Maximum control |

---

*Router and modem fundamentals based on standard networking equipment documentation.*