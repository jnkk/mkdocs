# IP Addresses

An IP address (Internet Protocol address) is a unique identifier assigned to each device on a network. Think of it like a postal address for your computer - it tells other devices where to send data.

## IPv4 vs IPv6

### IPv4 (Most Common)
- 32-bit address (4 numbers, 0-255 each)
- Example: `192.168.1.100`
- Total: ~4.3 billion addresses (running out!)

### IPv6
- 128-bit address (8 groups of 4 hex digits)
- Example: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- Total: ~340 unillion addresses (virtually unlimited)

## IP Address Structure

### Network Portion
- Identifies the network your device belongs to
- All devices on the same network share this portion

### Host Portion
- Identifies the specific device on that network
- Must be unique within the network

```
IP: 192.168.1.100
      └────┬────┘ └──────┘
      Network   Host
      (same for  Network
       all in    portion
       network)  (unique)
```

## Public vs Private IP Addresses

### Public IPs
- Assigned by your ISP
- Unique across the entire internet
- Used for communication on the internet

**Examples:**
- `8.8.8.8` (Google DNS)
- `1.1.1.1` (Cloudflare DNS)

### Private IPs (Non-Routable)
- Used only within local networks
- Not accessible from the internet
- Three reserved ranges:

| Range | Example |
|-------|---------|
| **10.0.0.0/8** | 10.x.x.x |
| **172.16.0.0/12** | 172.16-31.x.x |
| **192.168.0.0/16** | 192.168.x.x |

Most home routers use: `192.168.1.x` or `10.0.0.x`

## Classes (Legacy IPv4)

| Class | Range | Default Mask | Purpose |
|-------|-------|--------------|---------|
| A | 1-126 | /8 | Large networks |
| B | 128-191 | /16 | Medium networks |
| C | 192-223 | /24 | Small networks |
| D | 224-239 | - | Multicast |
| E | 240-255 | - | Reserved |

*Note: CIDR (below) replaced this system.*

## CIDR Notation

CIDR (Classless Inter-Domain Routing) lets you define network size precisely:

```
192.168.1.0/24
        └─┬─┘
   Subnet mask in bits
   (24 = 255.255.255.0)
```

### Common Subnet Sizes

| CIDR | Hosts | Description |
|------|-------|-------------|
| /24 | 254 | Typical home network |
| /25 | 126 | Half /24 |
| /26 | 62 | Quarter /24 |
| /27 | 30 | Small subnet |
| /28 | 14 | Very small subnet |

## Finding Your IP Address

### Linux/macOS
```bash
ip addr show
# or
ifconfig
```

### Windows
```cmd
ipconfig
```

### Common Output
```
192.168.1.100/24
↑ private IP     ↑ subnet mask
                 (24 bits = 255.255.255.0)
```

## Key Points for Homelab

1. **Static vs DHCP** - Servers usually need static IPs
2. **Same subnet** - Devices must be on the same subnet to communicate directly
3. **Gateway** - Your router's IP (usually .1 on your subnet)
4. **DNS** - Usually same as gateway, or specific servers like 1.1.1.1

---

*IPv4/IPv6 fundamentals based on standard networking RFCs and documentation.*