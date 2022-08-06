# nix-index-database
Tri-weekly updated, multi-channel, multi-platform prebuilt [`nix-index`](https://github.com/usertam/nix-index) indices.

## Source
Releases follow _both_ the
[`nixpkgs-unstable`](https://api.github.com/repos/NixOS/nixpkgs/git/refs/heads/nixpkgs-unstable) branch and the nixpkgs
[`master`](https://api.github.com/repos/NixOS/nixpkgs/git/refs/heads/master) branch.

## Oneshot Install
To preform a oneshot install on `master`, do:
```sh
# determine platform with flakes
PLATFORM=$(nix eval --raw nixpkgs#system)

# to explicitly state platform instead, do:
# PLATFORM='aarch64-linux'

mkdir -p $HOME/.cache/nix-index
curl -o $HOME/.cache/nix-index/files \
    https://raw.githubusercontent.com/usertam/nix-index-database-trial/releases/master/indices/index-$PLATFORM
```

## Flakes Install
To install `nixpkgs-unstable` standalone using flakes in home-manager:
```nix
{
  inputs = {
    nix-index-db.url = "github:usertam/nix-index-database-trial/standalone/nixpkgs-unstable";
    nix-index-db.inputs.nixpkgs.follows = "nixpkgs";
  };

  outputs = { nix-index-db, ... }:
    let
      system = "x86_64-linux";
      nix-index-bin = nix-index-db.packages.${system}.default;
    in {
      homeConfigurations."user" = home-manager.lib.homeManagerConfiguration {
        extraSpecialArgs = { inherit nix-index-bin; };
        modules = [
          ({ nix-index-bin, ... }: {
            home.file.".cache/nix-index/files".source = nix-index-bin;
          })
        ];
      };
    };
}
```
