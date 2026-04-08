# LittleLayouts-Releases

This repository is the public release surface for Little Layouts.

The private source repository builds Little Layouts, stages release assets, and publishes the
public release artifacts here so the application can check for updates without exposing the source
repo.

## What Lives Here

- GitHub Releases that hold downloadable public assets
- machine-readable channel manifests under `releases/`
- the public release contract in `docs/ReleaseContract.md`

## Key Entry Points

- [docs/ReleaseContract.md](docs/ReleaseContract.md)
  - defines the public update contract, manifest shape, checksum policy, and channel semantics
- [releases/stable.json](releases/stable.json)
  - points at the newest supported stable release
- [releases/beta.json](releases/beta.json)
  - points at the newest supported prerelease or beta release

If a channel has not published a release yet, its manifest keeps `"release": null`.
Clients should treat that as "no release available for this channel" rather than as an error.

## Asset Convention

Portable release ZIP assets use this naming convention:

- `LittleLayouts-win-x64-<version>.zip`

GitHub Release tags use semantic versions with a leading `v`:

- `v1.0.0`

Checksums are published in the channel manifest and may also be published as sidecar
`.sha256` files.

## Publishing Notes

This repo is intended to hold:

- release manifests
- release-contract documentation
- downloadable GitHub Release assets

This repo is not intended to hold:

- source code for Little Layouts
- private implementation planning docs
- committed binary release artifacts in git history
