# Network Security

Securing your home network protects your data, privacy, and devices from unauthorized access. Here's a comprehensive approach for homelab environments.

## Security Layers

```
┌─────────────────────────────────────────────────────┐
│                  DEFENSE IN DEPTH                    │
├─────────────────────────────────────────────────────┤
│                                                     │
│  Layer 1: Perimeter (Router/Firewall)               │
│  - Block inbound traffic by default                │
│  - Only forward specific ports if needed            │
│                                                     │
│  Layer 2: Network Segmentation (VLANs)              │
│  - Separate IoT from trusted devices                │
│  - Isolate servers from general network             │
│                                                     │
│  Layer 3: Device Hardening                          │
│  - Change default passwords                         │
│  - Keep firmware/software updated                   │
│  - Disable unnecessary services                     │
│                                                     │
│  Layer 4: Monitoring                                │
│  - Log blocked connections                          │
│  - Alert on unusual activity                        │
│                                                     │
└─────────────────────────────────────────────────────┘
```

## Perimeter Security

### Default Deny (Inbound)

```yaml
# Firewall policy
inbound:
  - default: DROP (block everything not explicitly allowed)

outbound:
  - default: ALLOW (usually fine to allow outgoing)
```

### Close Unnecessary Ports

Run a port scan on your public IP to see what's exposed:

```bash
# Scan your public IP
nmap -sT your-public-ip

# Common services that should NOT be exposed:
# - SSH (22) - not needed from internet
# - Telnet (23) - insecure
# - RDP (3389) - target for attackers
# - SMB (445) - don't expose to internet!
```

### Port Forwarding Best Practices

| Don't Do | Do Instead |
|----------|------------|
| Forward to LAN device directly | Use reverse proxy |
| Open high random ports | Use standard ports (80, 443) |
| No firewall on target | Add firewall to target device |
| Forward multiple services | Limit to essential only |

## Network Segmentation

### VLAN Strategy

```
┌─────────────────────────────────────────────────┐
│              SECURE HOMELAB                     │
├─────────────────────────────────────────────────┤
│                                                 │
│ VLAN10 (Management) - 192.168.10.0/24           │
│   - Router, switches, APs                       │
│   - Admin access only                           │
│                                                 │
│ VLAN20 (Production) - 192.168.20.0/24           │
│   - Homelab servers                             │
│   - Strict firewall rules                       │
│                                                 │
│ VLAN30 (Trusted) - 192.168.30.0/24              │
│   - Personal devices                            │
│   - Full access to servers                      │
│                                                 │
│ VLAN40 (IoT/Guest) - 192.168.40.0/24            │
│   - Smart devices, guests                       │
│   - No access to other VLANs                    │
│   - Internet only                               │
│                                                 │
│ VLAN50 (DMZ) - 192.168.50.0/24                  │
│   - Public-facing services                      │
│   - Most restricted                             │
│   - Internet accessible                          │
└─────────────────────────────────────────────────┘
```

### Inter-VLAN Firewall Rules

```
┌────────────────────────────────────────────┐
│  Example Firewall Rules                    │
├────────────────────────────────────────────┤
│  VLAN20 → VLAN40: DROP (servers can't      │
│                reach IoT devices)          │
│  VLAN40 → VLAN20: DROP                     │
│  VLAN30 → VLAN20: ALLOW (you can reach     │
│                your servers)               │
│  VLAN50 → VLAN20: DROP (exposed services   │
│                can't reach internal)       │
│  VLAN40 → INTERNET: ALLOW                  │
└────────────────────────────────────────────┘
```

## Device Hardening

### Network Equipment

```bash
# Change default credentials!
- Router admin: admin → strong_password
- Switch admin: admin → strong_password
- AP admin: admin → strong_password

# Disable:
- UPnP (unless needed)
- Telnet/SSH from WAN
- Remote management from WAN
```

### Servers and VMs

```bash
# Update regularly
apt update && apt upgrade

# Disable unused services
systemctl disable --now service-name

# Use strong passwords or SSH keys
# (no password auth for SSH)

# Enable and configure firewall
ufw enable
ufw allow ssh
ufw allow 80
ufw allow 443
```

## Monitoring and Logging

### What to Monitor

| Event | Why |
|-------|-----|
| Blocked connections | Detect attack attempts |
| Login failures | Detect brute force |
| New device connections | Detect unauthorized devices |
| Bandwidth spikes | Detect compromised devices |
| Service failures | Detect issues |

### Tools for Homelab

| Tool | Purpose |
|------|---------|
| **Wazuh** | Full SIEM (security monitoring) |
| **Zeek** | Network security monitor |
| **CrowdSec** | Block attacks at firewall level |
| **Uptime Kuma** | Service monitoring |
| **Wireshark** | Packet capture/analysis |

## Common Threats

### 1. Port Scanning
Attackers scan IP ranges looking for open ports:
- **Defense:** Block all inbound except what's needed

### 2. Brute Force
Repeated login attempts:
- **Defense:** Fail2ban, limit login attempts, SSH keys

### 3. IoT Vulnerabilities
Smart devices often have poor security:
- **Defense:** Isolate on separate VLAN, no internet access

### 4. DNS Attacks
DNS hijacking or poisoning:
- **Defense:** Use secure DNS (DoH/DoT), Pi-hole/AdGuard

### 5. Man-in-the-Middle
Intercepting traffic on network:
- **Defense:** Use HTTPS, VPN, don't use HTTP

## VPN for Remote Access

### Why Use VPN?

```
Without VPN:
[External Device] --✗--> [Home Network]

With VPN:
[External Device] --→ [VPN Tunnel] --→ [Home Network]
                      (Encrypted!)
```

### VPN Options for Homelab

| Type | Protocol | Complexity | Use Case |
|------|----------|------------|----------|
| **WireGuard** | WireGuard | Easy | Fast, modern |
| **OpenVPN** | OpenVPN | Medium | Compatible |
| **Tailscale** | WireGuard | Easy | Mesh, zero config |
| **ZeroTier** | WireGuard | Easy | Cross-network |

## Security Checklist

- [ ] Change default passwords on all devices
- [ ] Update router/switch/AP firmware
- [ ] Enable firewall on router
- [ ] Default deny inbound traffic
- [ ] Set up VLANs to isolate devices
- [ ] Use strong WiFi password (WPA3/WPA2)
- [ ] Disable WPS on WiFi
- [ ] Use SSH keys instead of passwords
- [ ] Enable logging for security events
- [ ] Regular backups of configuration
- [ ] Review active connections periodically

---

*Network security best practices based on CIS Benchmarks, NIST guidelines, and home network security research.*