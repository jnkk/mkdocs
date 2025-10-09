---
title: Proxmox After Install
tags:
  - proxmox
  - learning
---

# Steps to take after installing Proxmox

Setting up the server(s):
- Apt-Cacher-NG
- Pihole
- Reverse Proxy
- Homepage


## [Proxmox Post Install Script](https://community-scripts.github.io/ProxmoxVE/scripts?id=post-pve-install)

```
bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/post-pve-install.sh)"
```

## [apt-cacher-ng](https://community-scripts.github.io/ProxmoxVE/scripts?id=apt-cacher-ng)

```
bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/ct/apt-cacher-ng.sh)"
```

After installing, make sure to go to `/etc/apt-cacher-ng` and "resave" the `acng.conf` file (open the file, save it. Then restart the apt-cacher-ng systemctl).

Do not include the InReleases:
```
DontCacheResolved: .*InRelease
```
## [Pihole](https://community-scripts.github.io/ProxmoxVE/scripts?id=pihole)

Mainly for routing the ip addresses to the proxy manager. The proxy manager's job is to reroute the port(s).
Based on the international standardization, it is better and wise to use `*.home.arpa` domain name.


## [Nginx Proxy Manager] or [Caddy]

Nginx Proxy Manager is for rerouting the ip to the ports with GUI, while caddy is just a config file.
Each IP address can use multiple ports.

### Caddy

Using Proxmox Helper Scripts to install `caddy`.
The settings are in `/etc/caddy/` the file in `Caddyfile`
Caddy example:
```
git.home.arpa {
        reverse_proxy 192.168.1.14:3000
}
```

`git.home.arpa` is the address you put in your browser.
`reverse_proxy <ip addr>:<port>` is the machine you want to connect.

`caddy reload` is to reload caddy to set your config.
`systemctl restart caddy`.

Set up the domain name and registering it in Pihole's DNS.

## [HomePage]

There are a lot of custom "homepage".

List:
- [Homarr](https://homarr.dev/)
