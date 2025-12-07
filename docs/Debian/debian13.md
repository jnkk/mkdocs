---
title: Debian 13
---

# Debian 13 After Install

!!! note "Important Note"
    Removing Nix from [Debian12 Version](./afterinstallDEBIAN.md).   
    Not smart enough to use it yet.

```bash
sudo apt install curl git git-lfs inotify-tools btop micro make automake wx-common build-essential cmake gcc clang autoconf fonts-jetbrains-mono fonts-firacode
```
# For Fonts:
```bash
apt search fonts-<font name>

sudo apt install fonts-jetbrains-mono
sudo apt install fonts-firacode
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

Use the `elixir@18` for specific version.
