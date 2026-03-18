# AdGuard Home

Network-wide ad and tracker blocker. Blocks ads at the DNS level for all devices on your network.

## Why AdGuard Home?

- Blocks ads on all devices (TVs, phones, tablets) without needing apps on each
- Improves browsing speed by blocking tracking domains
- Easy setup with Docker
- Built-in encrypted DNS (DoH/DoT)
- Simple web interface

## Installation

### Quick Start (Docker)

```bash
# Run AdGuard Home (using named volumes)
docker run --name adguardhome \
    -v adguardhome_work:/opt/adguardhome/work \
    -v adguardhome_conf:/opt/adguardhome/conf \
    -p 53:53/tcp \
    -p 53:53/udp \
    -p 3000:3000/tcp \
    -p 853:853/tcp \
    -p 784:784/udp \
    -p 8053:8053/tcp \
    --restart unless-stopped \
    adguard/adguardhome:latest
```

**Or with Docker Compose:**

```yaml
services:
  adguard:
    image: adguard/adguardhome:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "3000:3000/tcp"
      - "853:853/tcp"
      - "784:784/udp"
      - "8053:8053/tcp"
    volumes:
      - adguard_work:/opt/adguardhome/work
      - adguard_conf:/opt/adguardhome/conf
    restart: unless-stopped
```

### Access Web Interface

1. Open browser to `http://your-server-ip:3000`
2. Follow the setup wizard
3. Default credentials: set your own on first run

## Configuration

### Set Up DNS

On your router, change the DNS server to your AdGuard Home server IP:

```
# Example router settings
Primary DNS: 192.168.1.x (your AdGuard server)
Secondary DNS: 1.1.1.1 (fallback)
```

### Recommended Settings

1. **Filters → DNS Blocklist** - Enable "AdGuard DNS filter" and "AdGuard Simplified domain names filter"
2. **Settings → DNS** - Enable "Bind to specific IP" (your server's LAN IP)
3. **Settings → Encryption** - Enable DNS-over-TLS and DNS-over-HTTPS if supported

## Troubleshooting

### DNS Queries Not Resolving

- Check firewall allows port 53 (DNS)
- Verify router DNS points to AdGuard
- Check AdGuard status page shows "Running"

### Slow DNS Response

- Add upstream DNS servers (Cloudflare 1.1.1.1, Google 8.8.8.8)
- Enable caching in Settings → DNS

## Useful Links

- [AdGuard Home GitHub](https://github.com/AdguardTeam/AdGuardHome)
- [Filter Lists](https://adguardteam.github.io/AdGuardSDNSFilter/FiltersRegistry.html)

---

*Researched via: GitHub README (AdGuardTeam/AdGuardHome)*