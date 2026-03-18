# What is a Network?

A computer network is a collection of devices (computers, servers, phones, tablets, IoT devices) that are connected together to share resources and communicate.

## Key Components

### Nodes (Devices)
Any device that sends or receives data on the network:
- Computers and laptops
- Smartphones and tablets
- Servers
- Smart TVs
- IoT devices (cameras, thermostats)
- Network devices (routers, switches, access points)

### Links (Connections)
The physical or wireless medium that connects devices:
- **Ethernet cables** (copper/fiber)
- **WiFi** (wireless radio waves)
- **Fiber optic cables**

### Protocols
The rules that govern how data moves through the network:
- **TCP/IP** - the fundamental protocol suite of the internet
- **HTTP/HTTPS** - web traffic
- **DNS** - domain name resolution
- **SSH** - secure remote access

## Types of Networks

| Type | Scale | Example |
|------|-------|---------|
| **PAN** (Personal) | Few meters | Bluetooth connecting phone to earbuds |
| **LAN** (Local) | Home/Office | Your home WiFi network |
| **MAN** (Metropolitan) | City | City-wide fiber network |
| **WAN** (Wide) | Global | The Internet |

## How Data Moves

Data travels in **packets** - small chunks of information that include:
1. **Header** - source/destination addresses, protocol info
2. **Payload** - the actual data (part of a file, message, etc.)
3. **Trailer** - error-checking information

```
[Header] [Payload] [Trailer]
   ↓         ↓         ↓
Address   Data    Checksum
```

## Home Network Example

```
ISP → Router → Switch → Devices
                  ↓
            [WiFi Access Point]
                  ↓
            [Wireless Devices]
```

In a typical home:
- **ISP** provides internet connection
- **Router** manages traffic between your network and the internet
- **Switch** connects wired devices
- **WiFi Access Point** provides wireless connectivity

## Why This Matters for Homelabbing

Understanding networks helps you:
- Set up servers and services correctly
- Troubleshoot connectivity issues
- Configure firewalls and security
- Design VLANs for isolation
- Set up reverse proxies and VPNs

---

*This content covers foundational networking concepts as understood in standard computer networking literature.*