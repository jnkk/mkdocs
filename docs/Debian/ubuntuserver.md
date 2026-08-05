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

## Ubuntu Server on a Laptop (test setup)

Current ip => http://192.168.122.21

Using portainer.

### TODOs: Trying to make a website using a server inside Virt Manager

Goals is to make a [Server](../Selfhost/homeserver.md)
So far installed docker.

There's [FreeBSD Jails](https://www.freebsd.org/) which is "more stable" based by the people who are using it.

### Current Portainer Stack

[Portainer Link](https://192.168.122.21:9443/#!/home)

#### Gitea

[Gitea Link](http://192.168.122.21:3000/)
