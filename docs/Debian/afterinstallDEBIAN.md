---
title: Steps after installing Debian 12
tags:
  - index
---

# This is for Debian 12, if another reinstall is needed

## Change sources.list make a backup of the original file and copy and paste in /etc/apt

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
sudo apt install curl wget git micro btop build-essential cmake gcc make clang pandoc zlib1g-dev libffi-dev libffi8 libgmp-dev libgmp10 libncurses-dev libncurses5 libtinfo5 autoconf automake libncurses5-dev libssl-dev libwxgtk3.2-dev libgl1-mesa-dev libglu1-mesa-dev libpng-dev libssh-dev unixodbc-dev xsltproc fop default-jdk inotify-tools libyaml-dev rlwrap ripgrep papirus-icon-theme numix-icon-theme tango-icon-theme numix-icon-theme-circle arc-theme libsdl2-dev libsdl2-image-dev libsdl2-ttf-dev fonts-arphic-ukai fonts-arphic-uming fonts-ipafont-mincho fonts-ipafont-gothic fonts-unfonts-core fonts-unfonts-extra bison flex fonts-tlwg-garuda m4 libwxgtk-webview3.2-dev libxml2-utils openjdk-17-jdk

```

> current setup is using a dell laptop that has a old nvidia
> use-> nvidia-tesla-470-driver
> nvidia-detect

### Determinate Systems' NIX install

```bash
curl --proto '=https' --tlsv1.2 -sSf -L https://install.determinate.systems/nix | sh -s -- install
```

Source:
```bash
https://github.com/DeterminateSystems/nix-installer
```

They really have a good [blog post](https://determinate.systems/posts/determinate-nix-installer/) on how to do it.
Still learning on how to use nix as the main config/dotfile management.

### Configurations

1. Make the `.config/home-manager` folder. It is hidden, so add the `dot`
2. Change the directory to `home-manager` folder, run this command:
```bash
nix run home-manager -- init --switch .
```
3. Remove those files, it is used for init the home-manager
4. Clone your [home-manager repo](https://github.com/jnkk/debian-nix-home-manager)

## How to download and make use of personal notes

- Create ssh-key

```bash
ssh-keygen -t ed25519
```

Need to change this to [nix-home-manager]

- Set your git username and email

```bash
git config --global user.name "<yourusername>"
git config --global user.email "<youruseremail>"
git config --global init.defaultBranch main
git branch -m main
```

> [!IMPORTANT]
> Do not use the command SUDO; goes to the root and not the $USER.

- Add ssh key to [github](https://github.com/settings/keys)

- Download the GIT version and not the https. so the git push command works.


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
