# HIPS provider app

The HIPS provider app lets a GPU owner offer spare GPU time to the HIPS network while the computer is not in use, within a schedule and above a minimum price.

This repository holds the app's release packages only. Download the app at [hips.com/download](https://hips.com/download).

## Install

On Linux (x86_64 or arm64, with apt or dnf) and on a Mac with Apple silicon:

```sh
curl -fsSL https://hips.com/install.sh | bash
```

What it does, step by step:

1. Checks the computer: Linux on x86_64 or arm64 with apt (Debian, Ubuntu) or dnf (Fedora, RHEL, Rocky Linux, AlmaLinux, Amazon Linux 2023) and glibc 2.34 or newer, or macOS 13 or later on Apple silicon. On any other computer it stops and installs nothing.
2. Downloads the newest release's `SHA256SUMS` and the package for this computer from this repository's releases, into a temporary folder that is removed when it ends.
3. Checks the package's SHA-256 against `SHA256SUMS`, and installs nothing if they differ.
4. Installs the package with apt-get or dnf on Linux, so its dependencies resolve, or with the macOS installer on a Mac. Only this step and the next two run as root, through sudo, which may ask for your password.
5. Makes sure the HIPS Provider service is running.
6. Sets how this computer works for HIPS. On Linux it asks: Always, whenever the computer is on (the default, for a server or a dedicated machine), or Auto, only while nobody is using it; with no terminal to ask in, Always. On a Mac, Auto. A computer linked already keeps its mode. Change it later in My GPUs in the HIPS Console, in the app's settings, or with `sudo hips-provider config set availability_mode always` (or `scheduled` for Auto).
7. Hands over to `hips-provider setup`, which shows the GPUs it found, links this computer to your HIPS account (a code and a QR code to approve at [console.hips.com/device](https://console.hips.com/device), waiting until you have approved it) and says what runs next. On a Mac it also starts the menu bar app.

The script is POSIX sh, so `| sh` works as well. Settings, as environment variables for the shell after the pipe:

```sh
curl -fsSL https://hips.com/install.sh | HIPS_VERSION=0.1.5 bash   # install that release instead of the newest
curl -fsSL https://hips.com/install.sh | HIPS_MODE=auto bash       # always or auto, without the question
curl -fsSL https://hips.com/install.sh | HIPS_NO_LINK=1 bash       # install without linking; link later with: sudo hips-provider setup
```

It ends with exit code 0 when the app is installed, running and linked (or not linked, as `HIPS_NO_LINK=1` asks), and otherwise says what to do: 1 the computer is not supported, 2 a setting is not valid, 3 a download failed, 4 a download does not match `SHA256SUMS`, 5 the package could not be installed, 6 the service is not running, 7 the computer is not linked yet, 8 the computer has no GPU HIPS supports, so it cannot work for HIPS.

### Read the script first

```sh
curl -fsSL https://hips.com/install.sh -o install.sh
less install.sh
bash install.sh
```

### Check the script

`hips.com/install.sh` serves the newest release's `install.sh`, which that release lists in its `SHA256SUMS`. In the folder where you saved `install.sh`:

```sh
curl -fsSL https://github.com/hips-ai/hips-provider-releases/releases/latest/download/SHA256SUMS -o SHA256SUMS
shasum -a 256 -c SHA256SUMS --ignore-missing     # on Linux: sha256sum -c SHA256SUMS --ignore-missing
```

The check prints `install.sh: OK`.

### Install a package yourself

Download the package for your computer from the [newest release](https://github.com/hips-ai/hips-provider-releases/releases/latest), check it against `SHA256SUMS` (see "Verify a download" below), and install it:

| Computer | Command |
|---|---|
| Debian, Ubuntu, x86_64 | `sudo apt install ./hips-provider_amd64.deb` |
| Debian, Ubuntu, arm64 | `sudo apt install ./hips-provider_arm64.deb` |
| Fedora, RHEL, x86_64 | `sudo dnf install ./hips-provider.x86_64.rpm` |
| Fedora, RHEL, Amazon Linux, arm64 | `sudo dnf install ./hips-provider.aarch64.rpm` |
| Mac with Apple silicon | `sudo installer -pkg HIPS-Provider-mac-arm64.pkg -target /`, or open the package |
| Windows x64 | Open `HIPS-Provider-windows-x64.msi` |

A test build that is not signed yet asks you to confirm before its package opens on macOS or Windows; the release's notes say how. Then link the computer to your HIPS account: choose Connect this device in the app on macOS and Windows, or run `sudo hips-provider setup` in a terminal.

### Uninstall

On Linux and macOS:

```sh
sudo hips-provider remove
```

On macOS and Windows, the app's Settings window has Remove too; on Windows, Add or Remove Programs removes it as well.

## Platforms

| Platform | File |
|---|---|
| macOS, Apple silicon | `HIPS-Provider-mac-arm64.pkg` |
| Windows x64 | `HIPS-Provider-windows-x64.msi` |
| Linux x86_64 (Debian, Ubuntu) | `hips-provider_amd64.deb` |
| Linux x86_64 (Fedora, RHEL) | `hips-provider.x86_64.rpm` |
| Linux arm64 (Debian, Ubuntu) | `hips-provider_arm64.deb` |
| Linux arm64 (Fedora, RHEL, Amazon Linux) | `hips-provider.aarch64.rpm` |
| Linux and macOS, the one-line installer | `install.sh` |

Each release lists every file's SHA-256 in `SHA256SUMS`, and carries each package under a name with its version too, such as `hips-provider_0.1.5_amd64.deb`.

## Verify a download

In the folder that holds the download and the release's `SHA256SUMS`:

```sh
shasum -a 256 -c SHA256SUMS --ignore-missing
```

On Windows, in PowerShell: `Get-FileHash .\HIPS-Provider-windows-x64.msi -Algorithm SHA256`, and compare it with the line in `SHA256SUMS`.

## Help

Questions and support: [hips.com](https://hips.com), or hello@hips.com.
