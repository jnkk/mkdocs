---
title: Cloudflare Tunnel
icon: material/cloud
tags:
  - homelab
  - networking
  - cloudflare
  - tunnel
  - security
date_added: 2026-03-18
last_updated: 2026-03-18
---

# Cloudflare Tunnel

Securely expose your self-hosted services to the internet without opening ports on your router. No port forwarding needed!

## Why Cloudflare Tunnel?

### Traditional Approach (Port Forwarding)
```
Internet → Router → Port Forward (e.g., :443 → 192.168.1.10:443)
```
- Requires opening ports on router
- Your home IP is exposed
- Security risk - direct attacks possible

### Cloudflare Tunnel Approach
```
Internet ← Cloudflare ← Tunnel ← Your Server
```
- **No port forwarding** - outbound connection only
- **Your IP hidden** - traffic goes through Cloudflare
- **DDoS protection** - Cloudflare blocks attacks
- **Free** - no cost for basic usage

## How It Works

```
┌─────────────────────────────────────────────────────────┐
│                    Cloudflare Network                   │
│                         ┌───────┐                        │
│  User → cloudflare.com →│ DNS   │→ Tunnel → Your Server │
│                         └───────┘    (cloudflared)      │
└─────────────────────────────────────────────────────────┘
                           ↑
                    Outbound only
                   (firewall friendly)
```

1. **cloudflared** (daemon) runs on your server
2. Creates outbound connection to Cloudflare global network
3. Traffic flows both ways through the tunnel
4. DNS routes your domain to the tunnel

## Architecture for Homelab

```
┌─────────────────────────────────────────────────────────┐
│                     Your Home Network                  │
│  192.168.1.0/24                                        │
│                                                         │
│  ┌──────────────┐                                      │
│  │ Proxmox/VM   │                                      │
│  │ ┌──────────┐ │                                      │
│  │ │ cloudflared │ ←── Creates tunnel to Cloudflare   │
│  │ └──────────┘ │                                      │
│  │ ┌──────────┐ │                                      │
│  │ │ Services  │ │  ┌─ service1.yourdomain.com          │
│  │ │ (Dockers)│ │  ├─ service2.yourdomain.com          │
│  │ └──────────┘ │  └─ service3.yourdomain.com         │
│  └──────────────┘                                      │
└─────────────────────────────────────────────────────────┘
```

## Setup Steps

### 1. Install cloudflared

```bash
# Linux (amd64)
curl -L https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64 -o /usr/local/bin/cloudflared
chmod +x /usr/local/bin/cloudflared

# Or via apt (Debian/Ubuntu)
sudo apt install cloudflared
```

### 2. Authenticate

```bash
cloudflared tunnel login
# Opens browser to authenticate with Cloudflare
# Select your domain
```

### 3. Create Tunnel

```bash
# Create tunnel
cloudflared tunnel create homelab

# Note the tunnel UUID output
```

### 4. Configure DNS

```bash
# Point your domain to tunnel
cloudflared tunnel route dns homelab service1.yourdomain.com
```

### 5. Run Tunnel

```bash
# Run tunnel
cloudflared tunnel run homelab

# Or as a service (recommended)
sudo cloudflared service install
```

### 6. Configure Services (config.yml)

```yaml
tunnel: <tunnel-uuid>
credentials-file: /root/.cloudflared/credentials.json

ingress:
  - hostname: service1.yourdomain.com
    service: http://192.168.1.10:8080

  - hostname: service2.yourdomain.com
    service: http://192.168.1.11:3000

  - hostname: service3.yourdomain.com
    service: http://192.168.1.12:443

  # Catch-all for unmatched requests
  - service: http_status:404
```

### 7. Start with Config

```bash
cloudflared --config config.yml tunnel run homelab
```

## Multiple Services Example

Running multiple services on one server:

```yaml
# config.yml
tunnel: your-tunnel-uuid
credentials-file: /root/.cloudflared/credentials.json

ingress:
  # Portainer
  - hostname: portainer.yourdomain.com
    service: http://localhost:9443

  # AdGuard
  - hostname: adguard.yourdomain.com
    service: http://localhost:3000

  # Jellyfin
  - hostname: jellyfin.yourdomain.com
    service: http://localhost:8096

  # Home Assistant
  - hostname: homeassistant.yourdomain.com
    service: http://localhost:8123

  # Default fallback
  - service: http_status:404
```

## Docker Deployment (Recommended)

```yaml
# docker-compose.yml
services:
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared
    restart: unless-stopped
    command: tunnel --config /etc/cloudflared/config.yml run
    volumes:
      - ./config.yml:/etc/cloudflared/config.yml:ro
      - ~/.cloudflared:/root/.cloudflared:ro
    network_mode: host
```

## Security Benefits

| Feature | Benefit |
|---------|---------|
| **No open ports** | Attackers can't reach your server directly |
| **Cloudflare WAF** | Block malicious requests before they reach you |
| **DDoS protection** | Cloudflare absorbs attack traffic |
| **Access control** | Limit who can access each service |
| **Zero-trust** | Verify user identity, not just IP |

## Access Policies (Optional)

You can add authentication to protect services:

```bash
# Require Cloudflare Access (zero-trust login)
cloudflared access tunnel configure
```

Options include:
- Email/Email domain restriction
- GitHub/Google/Other OAuth
- Device posture checks
- Bypass for specific IPs

## Troubleshooting

```bash
# Check tunnel status
cloudflared tunnel info homelab

# View logs
cloudflared tunnel logs homelab

# Test DNS resolution
dig service1.yourdomain.com
```

## Useful Links

- [Cloudflare Tunnel Docs](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/)
- [GitHub](https://github.com/cloudflare/cloudflared)
- [Dashboard](https://dash.cloudflare.com/)

---

*Researched via: Cloudflare One official documentation*