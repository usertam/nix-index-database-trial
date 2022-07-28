# nix-index-database
Tri-weekly updated, multi-platform prebuilt [`nix-index`](https://github.com/usertam/nix-index) indices.

## Source
Releases follow the
[`nixpkgs-unstable`](https://api.github.com/repos/NixOS/nixpkgs/git/refs/heads/nixpkgs-unstable) branch, instead of nixpkgs
[`master`](https://api.github.com/repos/NixOS/nixpkgs/git/refs/heads/master).

## Instructions
To install, run:
```sh
# determine platform with flakes
PLATFORM=$(nix eval --raw nixpkgs#stdenv.hostPlatform.system)

# to explicitly state platform instead, do:
# PLATFORM='aarch64-linux'

mkdir -p $HOME/.cache/nix-index
curl -Lo $HOME/.cache/nix-index/files \
    https://github.com/usertam/nix-index-database/releases/latest/download/index-$PLATFORM
```
