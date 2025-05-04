---
Title: "Arch Linux"
---

# Arch

```bash
sudo pacman -S ttf-jetbrains-mono-nerd ttf-liberation ttf-ubuntu-font-family ttf-anonymous-pro ttf-dejavu ttf-bitstream-vera adobe-source-sans-pro-fonts noto-fonts noto-fonts-cjk hunspell-en_US aspell-en gst-plugins-good gst-libav gufw dnscrypt-proxy p7zip tar unzip xdg-user-dirs clang cmake zed rbenv go npm nodejs git github-cli git curl wget eza bat fzf fd zoxide vim micro btop base-devel bash-completion gnome-keyring man less cronie xdg-desktop-portal xdg-desktop-portal-gtk freetype2 fontconfig pkg-config make libxcb libxkbcommon python pkgfile
--needed
```

## Enable YAY

```bash
sudo pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

## Cloudflare

[Source](https://pranavk-official.gitlab.io/posts/post-2/)

```bash
yay -S cloudflare-warp-bin

sudo systemctl start warp-svc

warp-cli register

sudo systemctl enable warp-svc

```
