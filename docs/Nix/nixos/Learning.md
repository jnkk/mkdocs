---
title: Beginning of NixOS Setups
tags: 
  - homelab
  - linux
  - nix
  - nixos
---

# First steps for Nix

I recommend the nix package manager in other linux distributions first, my personal choice is [Debian](https://www.debian.org/), because it is stable.  

Using `nix` package manager, I can play experiment apps like neovim without worrying about breaking the system. The Debian based is not the current version. Because it needs to be stable.

Learning nix is by modifying my apps or programs in my dotfiles directory. Since home-manager and git can manage all of those.  
Adding git is because nix's garbage collection, which can erase generation changes or roll-backs incase something breaks. Keep in mind that the nix/store is really taxing on space on your storage.

TODO, adding dotfiles to Github.

Then make the leap to nixOS.  
You have to configure the ENTIRE operating system while not knowing how to do it.  
Since nixOS have a different package manager from other linux distributions. It is better to understand Nix package manager and the Nix language first.  

## First steps for nixOS

The base of nixOS is in /etc/nixos.
There are 2 files, configuration.nix and hardware-configuration.nix

DO NOT MOVE THESE FILES.  

The command

```bash
sudo nixos-rebuild switch
```

will be defaulted in /etc/nixos
Changes won't happend

Enable ssh, git, curl in configuration.nix

```nix
services.openssh.enable = true;

environment.systemPackages = with pkgs; [
    wget git curl
  ];
```

Add this to configuration.nix

```nix
nix.settings.experimental-features = [ "nix-command" "flakes" ];
```

What these do is "replacing" the channels command, which are the defaults.
To the flakes "channels".

The difference is channel default uses sudo, flakes does not.

### With hardships, zsh is installed and made the default shell

### Next step is emacs or neovim
