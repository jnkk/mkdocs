---
title: Steps after installing Debian Server
tags:
  - index
  - server
  - debian
---

> Use [CheckMK](https://checkmk.com/) for monitoring more than 1 server.

# Steps after installing Debian Server

##

```bash
sudo apt update && sudo apt install curl vim nala build-essential cmake gcc make clang
```

### Set Nala

Faster than APT.

```bash
sudo nala fetch
```

### Install Determinate System Nix

```bash
curl --proto '=https' --tlsv1.2 -sSf -L https://install.determinate.systems/nix | \
  sh -s -- install
```

[Github](https://github.com/DeterminateSystems/nix-installer)
