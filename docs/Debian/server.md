---
title: Steps after installing Debian / Ubuntu Server
tags:
  - index
  - server
  - debian
---

> Use [CheckMK](https://checkmk.com/) for monitoring more than 1 server.

# Steps after installing Debian / Ubuntu Server

## Using from a laptop

- Go to `/etc/systemd/`
- Edit `logind.conf`
- `HandleLidSwitch=ignore`

## Setup ssh

`ssh-keygen -t ed25519`

- Go to `.ssh`
- Put your device that you are using to `dot pub key`, inside the `authorized_keys` file

## Change apt to nala

```bash
sudo apt update && sudo apt install curl vim nala build-essential cmake gcc make clang
```
> Faster than APT.
```bash
sudo nala fetch
```

Grab the fastest. Preferable under 20ms.

## Install Determinate System Nix

```bash
curl --proto '=https' --tlsv1.2 -sSf -L https://install.determinate.systems/nix | \
  sh -s -- install
```

> Source: [Github](https://github.com/DeterminateSystems/nix-installer)


## Erlang deps for Phoenix Framework
This is for Ubuntu. For others see source below.
```bash
sudo nala install build-essential autoconf m4 libwxgtk3.2-dev libwxgtk-webview3.2-dev libgl1-mesa-dev libglu1-mesa-dev libpng-dev libssh-dev unixodbc-dev xsltproc fop libxml2-utils libncurses-dev openjdk-11-jdk inotify-tools
```

> Source [Github](https://github.com/asdf-vm/asdf-erlang)
