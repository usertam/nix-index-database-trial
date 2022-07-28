# nix-index-database
Tri-weekly updated, multi-platform prebuilt [`nix-index`](https://github.com/usertam/nix-index) indices.

## Source
Releases follow the
[`nixpkgs-unstable`](https://api.github.com/repos/NixOS/nixpkgs/git/refs/heads/nixpkgs-unstable) branch, instead of nixpkgs
[`master`](https://api.github.com/repos/NixOS/nixpkgs/git/refs/heads/master).

## Oneshot Install
To preform a oneshot install, do:
```sh
# determine platform with flakes
PLATFORM=$(nix eval --raw nixpkgs#system)

# to explicitly state platform instead, do:
# PLATFORM='aarch64-linux'

mkdir -p $HOME/.cache/nix-index
curl -Lo $HOME/.cache/nix-index/files \
    https://github.com/usertam/nix-index-database/releases/latest/download/index-$PLATFORM
```

## Flakes Install
To install using flakes in home-manager:
```nix
{
  inputs = {
    # nixpkgs, home-manager...
    nix-index-db.url = "github:usertam/nix-index-database-trial/standalone/nixpkgs-unstable";
    nix-index-db.inputs.nixpkgs.follows = "nixpkgs";
  };

  outputs = { nixpkgs, home-manager, nix-index-db, ... }: {
    homeConfigurations."user" = home-manager.lib.homeManagerConfiguration {
      pkgs = nixpkgs.legacyPackages."aarch64-linux";
      extraSpecialArgs = { inherit nix-index-db; };
      modules = [
        ({ pkgs, nix-index-db, ... }: { 
           home.file.".cache/nix-index/files" = {
             source = nix-index-db.packages.${pkgs.system}.default;
           };
        })
        # { home = ... };
      ];
    };
  };
}
```
