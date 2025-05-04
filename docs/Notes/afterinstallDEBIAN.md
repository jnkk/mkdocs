---
title: Steps after installing Debian 12
tags: 
  - index
---

# This is for Debian 12, if another reinstall is needed

## Change sources.list make a backup of the original file and copy and paste in /etc/apt

> [!IMPORTANT]
> Make sure to backup first and then edit the files
> Changing the sources.list in `/etc/apt/` is adding `contrib` and `non-free`

```txt
deb http://deb.debian.org/debian bookworm main non-free-firmware contrib non-free
deb-src http://deb.debian.org/debian bookworm main non-free-firmware contrib non-free

deb http://deb.debian.org/debian-security bookworm-security main  non-free-firmware contrib non-free
deb-src http://deb.debian.org/debian-security bookworm-security main non-free-firmware contrib non-free

deb http://deb.debian.org/debian bookworm-updates main non-free-firmware contrib non-free
deb-src http://deb.debian.org/debian bookworm-updates main non-free-firmware contrib non-free
```

## post debian 12 install to download

```bash
sudo apt install curl wget git micro btop build-essential cmake gcc make clang pandoc zlib1g-dev libffi-dev libffi8 libgmp-dev libgmp10 libncurses-dev libncurses5 libtinfo5 autoconf automake libncurses5-dev libssl-dev libwxgtk3.2-dev libgl1-mesa-dev libglu1-mesa-dev libpng-dev libssh-dev unixodbc-dev xsltproc fop default-jdk inotify-tools libyaml-dev rlwrap ripgrep papirus-icon-theme numix-icon-theme tango-icon-theme numix-icon-theme-circle arc-theme libsdl2-dev libsdl2-image-dev libsdl2-ttf-dev fonts-arphic-ukai fonts-arphic-uming fonts-ipafont-mincho fonts-ipafont-gothic fonts-unfonts-core fonts-unfonts-extra bison flex fonts-tlwg-garuda
```

> [!NOTE]
> current setup is using a dell laptop that has a old nvidia
> use-> nvidia-tesla-470-driver
> nvidia-detect

## Install rust

## Use [mise](https://mise.jdx.dev/)

### Or use asdf instead of Nix for dev deps

[asdf](https://github.com/asdf-vm/asdf) maybe simpler for beginner dev.
[asdf website and documentations](https://asdf-vm.com/)
[asdf plugins](https://github.com/asdf-vm/asdf-plugins), plugins means the "language repo".

Install `Nodejs`, `npm`, `pnpm`, [`elixir language`](https://elixir-lang.org/install.html#gnulinux), [`asdf-elixir`](https://github.com/asdf-vm/asdf-elixir), [`asdf-erlang`](https://github.com/asdf-vm/asdf-erlang)


### Determinate Systems' NIX install

```bash
curl --proto '=https' --tlsv1.2 -sSf -L https://install.determinate.systems/nix | sh -s -- install
```

They really have a good [blog post](https://determinate.systems/posts/determinate-nix-installer/) on how to do it.
Still learning on how to use nix as the main config/dotfile management.

### Configurations

1. Make the `config/home-manager` folder
2. Change the directory to `home-manager` folder, run this command:
```bash
nix run home-manager -- init --switch .
```
3. Remove those files, it is used for init the home-manager
4. Clone your [home-manager repo](https://github.com/jnkk/debian-nix-home-manager)
5. Install Devbox
```bash
curl -fsSL https://get.jetify.com/devbox | bash
```

## Setup XDG BASE DIR TO .BASHRC

```bash
export XDG_CONFIG_HOME="$HOME/.config"
export XDG_DATA_HOME="$HOME/.local/share"
export XDG_CACHE_HOME="$HOME/.cache"
export XDG_STATE_HOME="$HOME/.local/state"

```

### Install [xdg-ninja](https://github.com/b3nj5m1n/xdg-ninja)

> [!NOTE]
> This is after installing the xdg-ninja

```bash
export RUFF_CACHE_DIR="$XDG_CACHE_HOME/ruff"
export RUSTUP_HOME="$XDG_DATA_HOME"/rustup
export CARGO_HOME="$XDG_DATA_HOME"/cargo
export GNUPGHOME="$XDG_DATA_HOME"/gnupg
```

## How to download and make use of personal notes

- Set your git username and email

```bash
git config --global user.name "<yourusername>"
git config --global user.email "<youruseremail>"
```

- Create ssh-key

```bash
ssh-keygen -t ed25519
```

> [!IMPORTANT]
> Do not use the command SUDO; goes to the root and not the $USER.

- Add ssh key to [github](https://github.com/settings/keys)

- Download the GIT version and not the https. so the git push command works.

## other must to install not with apt

### Install [NERDFONTS](https://www.nerdfonts.com/)

### Jetbrainsmono installation

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/JetBrains/JetBrainsMono/master/install_manual.sh)"
```
Restart the terminal, it should be installed.

### Install Cloudflare-warp
Run these command. ONE BY ONE
[Cloudflare Website](https://developers.cloudflare.com/warp-client/get-started/linux/)

```bash
curl -fsSL https://pkg.cloudflareclient.com/pubkey.gpg | sudo gpg --yes --dearmor --output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/cloudflare-client.list

sudo apt update && sudo apt install cloudflare-warp

warp-cli registration new

warp-cli connect

# Run 'curl https://www.cloudflare.com/cdn-cgi/trace/' and verify that 'warp=on'.
```

### Best terminal by far is [FISH](https://fishshell.com/)

have to learn it.
if fish is set up. type this:

```fish
fish_config theme choose Tomorrow
```

### tutorial for installing QEMU/KVM

[youtube](https://www.youtube.com/watch?v=GgAQw08zJzs)
or go to [chatgpt](https://chatgpt.com/)/[perplexity](https://www.perplexity.ai/)

"how to install qemu/kvm on debian 12"

##### install miniconda for shell-gpt

[miniconda](https://docs.anaconda.com/miniconda/miniconda-install/)
Make sure to download sh their file. use bash command to install those.
last time I tried their failed because of the agreements.

[Shell-gpt guide](https://github.com/TheR1D/shell_gpt/wiki/Ollama) is here

Run this if [shell-gpt](https://github.com/TheR1D/shell_gpt) is installed globally

```bash
git diff | sgpt "Generate git commit message, for my changes"
```

takes over 10 seconds with current setup. OLD LAPTOP.
