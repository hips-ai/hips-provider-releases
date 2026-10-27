# HIPS provider app

The HIPS provider app lets a GPU owner offer spare GPU time to the HIPS network while the computer is not in use, within a schedule and above a minimum price.

This repository holds the app's release packages only. Download the app at [hips.com/download](https://hips.com/download).

## Platforms

| Platform | File |
|---|---|
| macOS, Apple Silicon | `HIPS-Provider-mac-arm64.pkg` |
| Windows x64 | `HIPS-Provider-windows-x64.msi` |
| Linux x64 (Debian, Ubuntu) | `hips-provider_amd64.deb` |
| Linux x64 (Fedora, RHEL) | `hips-provider.x86_64.rpm` |

Each release lists every file's SHA-256 in `SHA256SUMS`.

## Verify a download

```sh
shasum -a 256 -c SHA256SUMS --ignore-missing
```

On Windows, in PowerShell: `Get-FileHash .\HIPS-Provider-windows-x64.msi -Algorithm SHA256`, and compare it with the line in `SHA256SUMS`.

## Help

Questions and support: [hips.com](https://hips.com), or hello@hips.com.
