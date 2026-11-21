# Installing OpenDeath

This guide covers the Windows setup wizard, the portable build, the Linux packages, and
the `opendeath` terminal CLI.

The current release is **1.0.5**. Download every file from
[open.death.cl/download](https://open.death.cl/download.html) and verify its SHA-256 before
running it.

## Windows

### Requirements

- Windows 10 22H2 or Windows 11, 64-bit.
- Cloudflare WARP. It is the default route and the setup wizard can install it.
- OpenCode Desktop is recommended because the two apps share the local sign-in store. The
  wizard offers the official download and an advanced "continue without it" option.
- About 1.3 GB of disk space.

### Guided setup

1. Run `OpenDeath-Setup-1.0.5.exe`.
2. On **Prerequisites**, install WARP if it is missing and click **Check again**. Do the
   same for OpenCode Desktop, or tick the advanced override.
3. On **Choose components**:
   - Leave **OpenDeath** selected.
   - Tick **Command line (opendeath)** to add the CLI to your user PATH.
   - Tick any optional VPN client you want. They come from their official winget packages.
4. Choose Desktop and Start menu shortcuts.
5. Install and leave **Launch OpenDeath** checked.

The installer is per user. It writes to `%LOCALAPPDATA%\Programs\OpenDeath`, creates an
uninstaller in **Settings - Apps - Installed apps**, and never modifies your OpenCode
Desktop installation.

### Portable build

`OpenDeath-Portable-1.0.5.exe` needs no installation. Run it from any writable folder. It
extracts to a temporary folder at launch and keeps user data in
`%APPDATA%\dev.opendeath.desktop`. Updates are manual: download a newer build and replace
the file.

### Setup is interactive

Run the setup yourself and follow the wizard. It refuses silent `/S` installs, so OpenDeath
is only installed by a person running it. WARP can still be installed by the wizard when you
choose to.

### Update

OpenDeath does not update itself: there is no launch check, background download, update
feed, or restart prompt. Download the current setup from
[open.death.cl/download](https://open.death.cl/download.html) and run it over the existing
installation. Settings and chats are preserved.

### Uninstall

Use **Settings - Apps - Installed apps - OpenDeath - Uninstall**. The uninstaller removes
the application, shortcuts, and the `opendeath` PATH entry it added. It leaves your settings
and waiting chats in `%APPDATA%\dev.opendeath.desktop`.

## Linux

### Requirements

- A modern 64-bit distribution with glibc, X11 or Wayland, and the usual GTK 3 libraries.
- Cloudflare WARP for the WARP route (`cloudflare-warp` package). The installer can add it
  with `--warp`.
- Optional: Mullvad or Windscribe for their routes.
- The AppImage needs FUSE 2; otherwise run it with `--appimage-extract-and-run`.

### One command

```sh
curl -fsSL https://open.death.cl/downloads/install.sh | sh
```

Add `--warp` to install Cloudflare WARP in the same run:

```sh
curl -fsSL https://open.death.cl/downloads/install.sh | sh -s -- --warp
```

The installer reads `/etc/os-release`, picks the right package, verifies the SHA-256 from
`SHA256SUMS-linux.txt`, and installs the `opendeath` CLI next to the desktop app. It uses
`sudo` when available and falls back to a per-user install under `~/.local` when it is not.

Detected families: Debian, Ubuntu, Mint, Pop!_OS, Kali, Fedora, RHEL, Rocky, AlmaLinux,
Nobara, Arch, CachyOS, Manjaro, EndeavourOS, Garuda, and openSUSE. Anything else gets the
AppImage.

### Packages by hand

```sh
# Debian, Ubuntu, Mint, Pop!_OS, Kali
sudo apt install ./OpenDeath-1.0.5-amd64.deb

# Fedora, RHEL, Rocky, AlmaLinux
sudo dnf install ./OpenDeath-1.0.5-x86_64.rpm

# openSUSE
sudo zypper install ./OpenDeath-1.0.5-x86_64.rpm

# Any distribution, no install
chmod +x OpenDeath-1.0.5-x86_64.AppImage
./OpenDeath-1.0.5-x86_64.AppImage
```

The desktop command is `opendeath-desktop`; the terminal command is `opendeath`. Start the
WARP service once:

```sh
sudo systemctl enable --now warp-svc
warp-cli --accept-tos registration new   # only if no registration exists yet
```

### Arch and CachyOS

Install `opendeath-bin` from the AUR, or build the PKGBUILD from
`packaging/linux/PKGBUILD` in the source repository.

### CLI only

Download `opendeath-cli-1.0.5-linux-x64.tar.gz`, then:

```sh
tar -xzf opendeath-cli-1.0.5-linux-x64.tar.gz
sudo install -m 755 opencode /usr/local/bin/opendeath
opendeath --version
```

The CLI uses the same engine, configuration, and provider sign-in as the desktop app.

### Uninstall

Use your package manager:

```sh
sudo apt remove opendeath
sudo dnf remove opendeath
sudo zypper remove opendeath
```

User data stays in `~/.config/dev.opendeath.desktop`.

## Verify a download

Windows:

```powershell
Get-FileHash .\OpenDeath-Setup-1.0.5.exe -Algorithm SHA256
```

Linux:

```sh
sha256sum -c SHA256SUMS-linux.txt
```

The Linux checksum file covers the AppImage, deb, rpm, CLI tarball, and `install.sh`.
