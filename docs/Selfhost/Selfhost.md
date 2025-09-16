---
title: Selfhost Knowledge Base
tags:
  - selfhost
  - homelab
  - proxmox
  - knowledge-base
---

# Stuff I learn after installing Proxmox

Learning TODOS:
- LXC, or linux containers.
- Docker
- Networking, especially DNS and reverse proxy.

It is not how the youtube tutorials made it look. It is hard.
From "getting a domain, and put it in Cloudflare" to how to set a reverse proxy while connecting to a local DNS, like pihole.

Turns out many mistakes were made.
From the Cloudflare settings, to the LXC network settings. There are many things going on.
> Hint: Tunneling like Cloudflare Zero Trust, Tailscale or Netbird is the way moving forward. Since port forwarding is blocked by my ISP.
Currently don't need to face the internet just yet.
Just needed the domain name for localhost.

So either use [Cloudflare Zero Trust](https://www.cloudflare.com/zero-trust/products/access/), [tailscale](https://tailscale.com/) or [netbird](https://netbird.io/). All are similiar for their use.
Since now is the learning phase, not connecting to the internet part just yet.
Just needed the local domain or local DNS server.

## Navigations

- [Steps I took](./steps.md)
- [Hardware](./hardware.md)


## What is [selfhosting](https://www.reddit.com/r/selfhosted/)

Simply is "cloud computing" in our own house. Using cheap consumer hardware, or used Enterprised hardware. Your wallet is ONLY your limitation.

[Selfhost apps](https://selfh.st/apps/) is a great tool for discovering what to install.

## Goals?

Have a business.

Doing research, [Odoo](https://www.odoo.com/) is a good starting point.
A lot can be done using Odoo.
From websites, inventory to HR.

Also can be selfhosted.

Which means, I can learn on how the internet truly works.

### Where to start?

Old computers or laptops are a great start, since they are cheap or even free, if you have an unused computer hardware.
Or a "specialized" used office hardware that is relatively cheap.
Currently using an old laptop, Intel 4th gen 4010U that released on Q3 2013, which at this time or writing is already 12 years old.

Using [Proxmox](https://www.proxmox.com) only using [LXC](https://en.wikipedia.org/wiki/LXC), I can have more services than my CPU cores allow, with some really minor delay.

### Install LXC or Docker

Right now it is easier to use Proxmox helper scripts to install everything.
I am learning on how docker works.
By seeing a lot of Youtube videos, made it look easy. Until I tried it.

The difficult are storage and databases.


#### Hardware Wishlists

[Hardware](./hardware.md)
