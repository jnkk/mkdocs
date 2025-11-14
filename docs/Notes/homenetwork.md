---
title: Home Network
---

# Current Home Network Configuration

```mermaid
graph TD

  ISP-->ISPRouter
  ISPRouter-->WifiTop
  WifiTop-->Laptop
  WifiTop-->WifiBottom
  WifiBottom-->TV

```

# Planning Future Home Network Configuration

```mermaid
graph TD
  ISP-->ISPRouter
  ISPRouter-->SwitchTop
  SwitchTop-->Proxmox/Server/NAS
  Proxmox/Server/NAS<-->Main-PC
  SwitchTop-->Main-PC
  SwitchTop-->WifiTop
  SwitchTop-->SwitchBottom
  SwitchBottom-->WifiBottom
  SwitchBottom-->TV
  SwitchBottom-->CCTV
```

# Future-future Network

```mermaid
graph TD
  ISP1-->Proxmox
  ISP2-->Proxmox
  Proxmox-->SwitchTop
  Proxmox-->NAS
  Proxmox-->Main-PC
  SwitchTop-->SwitchBottom
  SwitchBottom-->BottomWifi
  SwitchBottom-->PC/MiniPC
  SwitchBottom-->CCTV
  SwitchBottom-->Dining
  PC/MiniPC-->TV
```
