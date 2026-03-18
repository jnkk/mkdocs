# VLAN Configuration

A VLAN (Virtual Local Area Network) lets you logically segment a network without running separate physical cables. It's like having multiple virtual switches on one physical switch.

## Why Use VLANs?

### Without VLANs
```
Physical Switch

[Router]─┬── [Servers]── [Workstations]── [IoT]
         │       (all same network, can see each other)
```

### With VLANs
```
Physical Switch (with VLANs enabled)

[Router]─┬── VLAN10: Servers (192.168.10.0/24)
         ├── VLAN20: Workstations (192.168.20.0/24)
         └── VLAN30: IoT (192.168.30.0/24)

         (Each VLAN is isolated from others)
```

## VLAN Benefits

1. **Security** - Isolate sensitive systems from general network
2. **Performance** - Reduce broadcast traffic
3. **Flexibility** - Group devices by function, not location
4. **Cost** - One switch, multiple networks
5. **Management** - Easier to apply policies

## VLAN Numbers

| VLAN ID | Common Use | Example |
|---------|------------|---------|
| 1 | Default/Native | Usually not used |
| 10 | Management | Router, switches |
| 20 | Servers/Production | Homelab VMs |
| 30 | Trusted users | Personal devices |
| 40 | Guest network | Visitors |
| 50 | IoT/Untrusted | Smart home devices |
| 99 | Quarantine | Suspicious devices |

## How VLANs Work

### Tagging (802.1Q)

Devices send traffic with a "tag" indicating which VLAN they belong to:

```
┌─────────────────────────────────────────┐
│  Standard Ethernet Frame                │
│  ┌─────┬──────┬───────┬─────────┬─────┐ │
│  │ Dst │ Src  │ Type  │  Data   │ FCS │ │
│  │ MAC │ MAC  │       │         │     │ │
│  └─────┴──────┴───────┴─────────┴─────┘ │
│                                         │
│  VLAN-Tagged Frame (802.1Q)              │
│  ┌────┬────┬──┬──┬──────┬───────┬────┐ │
│  │Dst │Src │TPID│TCI│ Type │ Data │FCS │ │
│  │MAC │MAC │0x│VLAN│      │       │    │ │
│  │    │    │8100│ID │      │       │    │ │
│  └────┴────┴────┴───┴──────┴───────┴────┘ │
│            └────────┬─────────┘              │
│                 4 bytes added               │
└────────────────────────────────────────────┘
```

### Trunk vs Access Ports

| Port Type | Use Case | VLAN Behavior |
|-----------|----------|---------------|
| **Access** | Computers, servers | One VLAN per port |
| **Trunk** | Switch-to-switch, router | Multiple VLANs |

## Setting Up VLANs

### 1. Configure Switch

```
# Example: Managed switch VLAN config
VLAN 10: Servers
VLAN 20: Workstations
VLAN 30: IoT

Port 1-4: VLAN 10 (Servers)
Port 5-8: VLAN 20 (Workstations)
Port 9-12: VLAN 30 (IoT)
Port 24: Trunk to router
```

### 2. Configure Router

Router needs "VLAN interfaces" for each VLAN:

```
Router:
├── eth0.10 (VLAN10) → 192.168.10.1/24
├── eth0.20 (VLAN20) → 192.168.20.1/24
└── eth0.30 (VLAN30) → 192.168.30.1/24
```

### 3. DHCP

Each VLAN gets its own DHCP pool:

```
VLAN10: 192.168.10.10-192.168.10.200
VLAN20: 192.168.20.10-192.168.20.200
VLAN30: 192.168.30.10-192.168.30.200
```

## Inter-VLAN Routing

By default, VLANs are isolated. To allow communication:

### Option 1: Router
```
┌─────────────────────────────────┐
│        Router                   │
│  VLAN10 ↔ VLAN20 ↔ VLAN30        │
│  (Each can route to others)    │
└─────────────────────────────────┘
```

### Option 2: Firewall Rules
```
┌─────────────────────────────────┐
│  Router with Firewall           │
│  VLAN10 ↔ VLAN20: ALLOW          │
│  VLAN10 ↔ VLAN30: DROP          │
│  VLAN20 ↔ VLAN30: DROP          │
└─────────────────────────────────┘
```

## VLAN for Homelab

### Example Setup

```
┌─────────────────────────────────────────────────┐
│  Home Network with VLANs                         │
├─────────────────────────────────────────────────┤
│                                                 │
│  ISP → Modem → Router (with VLAN support)      │
│                     │                            │
│              ┌──────┴──────┐                    │
│              │  Managed    │                    │
│              │  Switch     │                    │
│              └──────┬──────┘                    │
│          ┌─────────┼─────────┐                  │
│      .10 ↓      .20 ↓     .30 ↓               │
│   (Servers) (Trusted) (Untrusted)             │
│                                                 │
│  VLAN10 - 192.168.10.0/24  (homelab)            │
│  VLAN20 - 192.168.20.0/24  (personal)          │
│  VLAN30 - 192.168.30.0/24  (guest/IoT)         │
└─────────────────────────────────────────────────┘
```

### Equipment Needed

| Device | Purpose | Example |
|--------|---------|---------|
| Managed Switch | VLAN support | Netgear GS108, TP-Link TL-SG1008D |
| Router with VLAN | Inter-VLAN routing | OPNsense, Ubiquiti, OpenWrt |
| VLAN-capable AP | WiFi on multiple VLANs | UniFi AP |

## VLAN Limitations

- Requires managed switch (more expensive)
- Adds complexity
- Some devices don't support VLANs
- Network troubleshooting more complex

---

*VLAN concepts based on IEEE 802.1Q standard and network switching documentation.*