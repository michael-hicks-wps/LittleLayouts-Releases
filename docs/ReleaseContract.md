# Little Layouts Release Contract

This repository is the public release surface for Little Layouts.
The private source repository builds release artifacts and publishes them here so the application can check for updates without exposing the source repository.

## Purpose

This repo defines the stable public contract for:

- GitHub Release tags and assets
- machine-readable update manifests
- checksum expectations for downloadable artifacts
- stable versus prerelease channel behavior

## Repository Layout

```text
LittleLayouts-Releases/
  docs/
    ReleaseContract.md
  releases/
    stable.json
    beta.json
```

Published GitHub Releases in this repo should provide the downloadable assets.
The files committed in `releases/` provide the machine-readable entrypoints that the app can query first.

## Channel Entry Points

- `releases/stable.json`
  - points to the newest supported stable release
- `releases/beta.json`
  - points to the newest supported prerelease or beta release

If a channel has no published release yet, its manifest should keep `"release": null`.
Clients must treat that as "no release available for this channel" rather than as an error.

## Manifest Shape

Each channel manifest uses this JSON shape:

```json
{
  "schemaVersion": 1,
  "channel": "stable",
  "generatedAt": "2026-04-01T00:00:00Z",
  "release": {
    "version": "1.0.0",
    "tag": "v1.0.0",
    "publishedAt": "2026-04-01T00:00:00Z",
    "prerelease": false,
    "notesUrl": "https://github.com/michael-hicks-wps/LittleLayouts-Releases/releases/tag/v1.0.0",
    "assets": [
      {
        "name": "LittleLayouts-win-x64-1.0.0.zip",
        "platform": "win-x64",
        "url": "https://github.com/michael-hicks-wps/LittleLayouts-Releases/releases/download/v1.0.0/LittleLayouts-win-x64-1.0.0.zip",
        "sha256": "<hex-encoded sha256>"
      }
    ]
  }
}
```

Allowed initial state before the first public release:

```json
{
  "schemaVersion": 1,
  "channel": "stable",
  "generatedAt": "2026-04-01T00:00:00Z",
  "release": null
}
```

## Asset Naming

Portable release assets should follow this naming convention:

- `LittleLayouts-win-x64-<version>.zip`

Examples:

- `LittleLayouts-win-x64-1.0.0.zip`
- `LittleLayouts-win-x64-1.1.0.zip`

The Git tag should match the semantic version with a leading `v`:

- `v1.0.0`
- `v1.1.0`

## Checksum Policy

Each published asset must have a SHA-256 checksum.
The checksum must appear in the channel manifest entry for that asset.
The release process may also publish a sidecar checksum file such as:

- `LittleLayouts-win-x64-1.0.0.zip.sha256`

The in-app updater should treat the manifest checksum as the primary verification value.
If both a sidecar checksum file and manifest checksum exist, they must match.

## Stable And Beta Semantics

Use the channels this way:

- `stable`
  - general users
  - must only point to non-prerelease GitHub Releases
- `beta`
  - optional early-access channel
  - may point to prerelease GitHub Releases

The app should default to the `stable` channel unless a future settings surface explicitly opts into beta behavior.

## Publishing Expectations

When a new stable release is published:

1. create the GitHub Release in this repo with tag `v<version>`
2. upload the versioned ZIP artifact
3. compute the SHA-256 for that ZIP
4. update `releases/stable.json` to point at the new release and checksum
5. leave `releases/beta.json` unchanged unless the beta channel is also being updated

## Non-Goals For This Repo

This repo is not intended to host:

- source code for Little Layouts
- implementation planning docs from the private repo
- generated release artifacts committed into git history

Git history should hold the manifests and documentation.
Large downloadable binaries should live in GitHub Releases.
