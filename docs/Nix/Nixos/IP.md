# connect to local vm
ssh jnkk@192.168.122.148

## useful commands
  
```bash
sudo nixos-rebuild switch --flake .
```

## update nix flake lock file

```bash
nix flake update
```

## update home-manager

```bash
home-manager switch --flake .
```

## mynixos search packages website

nixpkgs options are in configuration.nix
home-manager / option are in home-manager
