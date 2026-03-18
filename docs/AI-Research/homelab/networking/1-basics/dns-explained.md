# DNS Explained

DNS (Domain Name System) is like the phonebook of the internet. It translates human-readable domain names (like google.com) into IP addresses that computers use to identify each other.

## The Problem DNS Solves

Without DNS, you would need to remember:
- `142.250.190.46` instead of `google.com`
- `151.101.1.140` instead of `github.com`

## How DNS Works

### Step-by-Step Resolution

```
User types: google.com
        ↓
[1] Check local cache (browser/OS)
        ↓
[2] Query ISP's DNS server
        ↓
[3] DNS server checks its cache
        ↓
[4] If not found, queries root DNS
        ↓
[5] Root DNS points to .com TLD server
        ↓
[6] TLD server points to authoritative DNS
        ↓
[7] Authoritative DNS returns IP address
        ↓
[8] Cached at each level for next time
        ↓
Result: 142.250.190.46
```

### DNS Record Types

| Type | Purpose | Example |
|------|---------|---------|
| **A** | Domain → IPv4 | `example.com → 93.184.216.34` |
| **AAAA** | Domain → IPv6 | `example.com → 2606:2800...` |
| **CNAME** | Alias to another domain | `www → example.com` |
| **MX** | Mail server | `example.com → mail.example.com` |
| **TXT** | Text records | Verification, SPF |
| **NS** | Name servers | `example.com → ns1.dnshost.com` |

## DNS in Your Home Network

### Typical Configuration

```
┌─────────────────────────────────────┐
│          Your Home Network          │
│                                      │
│  Router (also acts as DNS server)   │
│  - Primary DNS: 192.168.1.1         │
│  - Secondary: (optional)             │
│                                      │
│  Devices use router as DNS           │
│  Router forwards unknown queries     │
│  to ISP DNS or configured DNS        │
└─────────────────────────────────────┘
```

### Custom DNS Servers

For better control, you can change DNS on your devices:

| Provider | Primary | Secondary |
|----------|---------|-----------|
| Google | 8.8.8.8 | 8.8.4.4 |
| Cloudflare | 1.1.1.1 | 1.0.0.1 |
| Quad9 | 9.9.9.9 | 149.112.112.112 |

## DNS for Homelab

### Why Run Your Own DNS?

1. **Local resolution** - Access `homelab.local` instead of remembering IPs
2. **Ad blocking** - Block ads at DNS level (AdGuard, Pi-hole)
3. **Custom domains** - Use `.home` or `.local` domains
4. **Faster lookups** - Cache results locally

### Setting Up Local DNS

With AdGuard Home or Pi-hole:

```yaml
# Example: Add custom DNS entry
# In AdGuard Dashboard → DNS Settings → Custom DNS
# Point your domain to server IP
homelab.local → 192.168.1.100
```

## DNS and Ports

DNS uses specific ports:
- **Port 53** - Standard DNS (both TCP and UDP)
- **Port 853** - DNS-over-TLS (encrypted)
- **Port 443** - DNS-over-HTTPS

---

*DNS concepts based on standard RFC 1035 and ISP/resolver documentation.*