# Swift Salamander, on Nix

A dual-pane file manager for macOS. Local folders, network shares and 38
storage providers in one native window, written in Rust.

Apple silicon only, so the flake builds for `aarch64-darwin` and nothing else.

## Try it

```sh
nix run github:c0desurfer/swift-salamander-nix
```

Downloads the signed and notarized disk image, unpacks the app into the Nix
store and launches it. Nothing is installed.

## Install

```sh
nix profile install github:c0desurfer/swift-salamander-nix
```

The app lands in your profile at `~/.nix-profile/Applications/Swift
Salamander.app`, and `swift-salamander` goes on your `PATH`.

## In a nix-darwin configuration

```nix
{
  inputs.swift-salamander.url = "github:c0desurfer/swift-salamander-nix";

  # in your darwin configuration
  environment.systemPackages = [
    inputs.swift-salamander.packages.aarch64-darwin.default
  ];
}
```

nix-darwin does not put packaged apps in `/Applications`. It links them into
`/Applications/Nix Apps`, which Spotlight and the Dock both read, so the app is
where you would expect to find it under a folder of its own.

There is an overlay too, if you would rather have the package in your own
`pkgs`:

```nix
nixpkgs.overlays = [ inputs.swift-salamander.overlays.default ];
# then: pkgs.swift-salamander
```

## The licence

Swift Salamander is proprietary: a free tier, and a one-time purchase for Pro.
The flake declares `license = lib.licenses.unfree` because that is what it is,
and it imports nixpkgs with `allowUnfree` set so the commands above work
without you having to set anything first.

## Updates

The app has its own updater, and it will offer you a new version the moment one
is released. Nix does not know about that update, and if you let the app update
itself, the copy in the Nix store stays at the version Nix built.

The honest answer: this flake follows every release within minutes, because the
release script regenerates and pushes it. So take the update from Nix instead.

```sh
nix profile upgrade swift-salamander
```

## Where this comes from

This repository is a mirror. Nobody edits it by hand: it is generated from the
Swift Salamander source tree and pushed by the release script, so an edit made
here would be overwritten by the next release.

Something wrong with the package, or with the app? The feedback board is at
<https://feedback.codesurfer.ch/>.

Homepage: <https://salamander.codesurfer.ch/>
