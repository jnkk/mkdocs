# DHCP Explained

DHCP (Dynamic Host Configuration Protocol) automatically assigns IP addresses and other network settings to devices. It's what makes "plug and play" networking work.

## Without DHCP (Manual Configuration)

Before DHCP, network admins had to manually configure each device:

```
Admin: "Okay, Server1 gets 192.168.1.10, Printer is .11,
       Laptop Bob is .12... wait, which one was .12?"
```

## How DHCP Works

### The DORA Process

```
┌─────────┐     ┌─────────┐
│  Client │     │  Server │
│  (Your  │     │ (Router)│
│  Laptop)│     │         │
└────┬────┘     └────┬────┘
     │               │
     │  1. DISCOVER  │ (Broadcast: "Any DHCP server?")
     │──────────────→│
     │               │
     │  2. OFFER     │ (Broadcast: "Here's 192.168.1.100")
     │←──────────────│
     │               │
     │  3. REQUEST   │ (Broadcast: "I'll take 192.168.1.100")
     │──────────────→│
     │               │
     │  4. ACK       │ (Broadcast: "Confirmed!")
     │←──────────────│
     │               │
     ▼               ▼
  Connected!
```

### Lease Time

DHCP assignments aren't permanent - they're "leases":
- Typical lease: 24 hours to 1 week
- Device renews before expiration
- If device leaves, IP returns to pool

## DHCP in Your Home Network

### Typical Configuration

```
Router (DHCP Server):
├─ Gateway: 192.168.1.1
├─ Subnet: 192.168.1.0/24
├─ Pool: 192.168.1.100 - 192.168.1.200
├─ DNS: 192.168.1.1 (router)
└─ Lease Time: 24 hours
```

### What's Assigned

When your laptop connects, it receives:

| Setting | Example | Purpose |
|---------|---------|---------|
| **IP Address** | 192.168.1.105 | Your device's address |
| **Subnet Mask** | 255.255.255.0 | Network size |
| **Gateway** | 192.168.1.1 | Where to send external traffic |
| **DNS** | 192.168.1.1 | Domain name resolution |
| **Lease Time** | 86400 seconds | 24 hours |

## Static vs Dynamic IPs

### Dynamic (DHCP)
- Automatic assignment
- Changes after lease renewal
- Good for temporary devices (phones, guests)

### Static (Manual)
- Fixed address you set on device
- Doesn't change
- Required for: servers, routers, some services

## Reserving IPs for Homelab

Instead of static IPs on servers, use DHCP reservations:

```
┌────────────────────────────────────────────┐
│            DHCP Reservation                │
├────────────────────────────────────────────┤
│ MAC Address:   AA:BB:CC:DD:EE:FF           │
│ Reserved IP:    192.168.1.10                │
│ Hostname:       homelab-server              │
│ Description:    Proxmox VM host            │
└────────────────────────────────────────────┘
```

Router still assigns it via DHCP, but always the same IP!

### Finding MAC Address

```bash
# Linux
ip link show | grep link/ether

# macOS
ifconfig en0 | grep ether

# Windows
ipconfig /all | findstr "Physical"
```

## DHCP for Multiple Subnets

If you have VLANs or multiple subnets:

```
┌─────────────────────────────────────────────┐
│             DHCP Relay Agent                │
│ (Usually built into router/switch)         │
├─────────────────────────────────────────────┤
│ VLAN10 (192.168.10.0/24) → DHCP Server      │
│ VLAN20 (192.168.20.0/24) → DHCP Server      │
│ VLAN30 (192.168.30.0/24) → DHCP Server      │
└─────────────────────────────────────────────┘
```

## DHCP Ports

- **Server port:** 67
- **Client port:** 68
- Uses broadcast initially, then unicast

## For Homelab Best Practices

1. **Use DHCP reservations** - Easy management, static-like behavior
2. **Keep server IPs in different range** - e.g., .10-.50 for servers, .100+ for devices
3. **Document your DHCP scope** - Know what IPs are assigned
4. **Set appropriate lease times** - Longer (24h+) for stable networks

---

*DHCP concepts based on RFC 2131-2132 and standard router implementation.*