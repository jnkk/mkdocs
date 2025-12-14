---
title: Debian 13
---

# Debian 13 After Install

!!! note "Important Note"
    Removing Nix from [Debian12 Version](./afterinstallDEBIAN.md).   
    Not smart enough to use it yet.

```bash
sudo apt install curl git git-lfs inotify-tools btop micro make automake wx-common build-essential cmake gcc clang autoconf fonts-jetbrains-mono fonts-firacode golang 
```
# For Fonts:
```bash
apt search fonts-<font name>

sudo apt install fonts-jetbrains-mono
sudo apt install fonts-firacode
```

## Install [Rust](https://rust-lang.org/tools/install/)
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## Install [UV](https://docs.astral.sh/uv/getting-started/installation/) for Python
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

## Install [Mise](https://mise.jdx.dev/)

```bash
mise install erlang@latest
mise install elixir@latest
mise install zig@latest
mise install just@latest
```

Use the `elixir@1.18.4` for specific version.   
Use the `mise ls-remote elixir` to see versions of Elixir

## [Docker](https://github.com/docker/docker-install)

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

### Docker After Install

```bash
sudo groupadd docker
sudo usermod -aG docker $USER
exit

newgrp docker
docker run hello-world
```

## [NodeJS](https://nodejs.org/en/download)
### [Fast-CLI](https://github.com/sindresorhus/fast-cli)
