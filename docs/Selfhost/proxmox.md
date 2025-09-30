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

## [Nginx Proxy Manager]
