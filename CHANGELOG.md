# Changelog

All notable changes to OpenDeath are documented here.

## 1.0.4 - 2026-11-21

### Added

- **Linux support**: AppImage, deb and rpm packages, a one-command installer that detects
  Debian, Ubuntu, Mint, Pop!_OS, Kali, Fedora, RHEL, Rocky, AlmaLinux, Nobara, Arch,
  CachyOS, Manjaro, EndeavourOS, Garuda and openSUSE, and an AUR `opendeath-bin` package.
- **Terminal CLI**: the bundled engine is exposed as `opendeath` on Windows through an
  optional setup component, and on Linux through the installer or the standalone tarball.
  The Linux desktop binary is `opendeath-desktop` so the two commands never collide.
- **Help** in the network deck: the last network error with its cause, the matching fix,
  copyable commands for Windows and Linux, and a diagnostics copy button.
- Linux WARP detection on PATH and in `/usr/bin`, `systemctl enable --now warp-svc`
  guidance, and a service-start grace period.
- Linux path lookup for Mullvad and Windscribe clients.

### Changed

- **New IP and Auto-rotate now use `warp-cli tunnel rotate-keys` first.** A new local key
  makes Cloudflare assign a new exit without a new device registration, so the registration
  throttle no longer blocks rotation. Disconnect/connect is the second step; registration
  is attempted only once per ten minutes after both fail.
- A Cloudflare 429 pauses **new registrations only** for 30 minutes. It no longer disables
  a working WARP connection or removes an existing registration.
- New registrations are tried in place first. The old registration is deleted only when
  the daemon rejects the in-place create, so a throttle cannot destroy a working setup.
- Rotation waits 4 seconds for the action lock, uses cancelable probes, detects a WARP
  daemon that is down even when `warp-cli status` exits 0, and has a 150-second hunt limit.
- The desktop app checks for updates at launch and every ten minutes, downloads in the
  background, and asks once to restart.

### Fixed

- **Reset limit** no longer signs users out when rotation fails: the session is archived,
  the IP changes, the session is restored, and only then does the service restart.
- Reset no longer leaves the service stopped when WARP or the proxy list is unavailable.
- **New IP** from Auto mode no longer no-ops when WARP is missing; it escalates to the
  free proxy list instead of reporting nothing to rotate.
- WARP tunnel-only clients (no `127.0.0.1:40000` proxy port) are detected and used through
  the tunnel instead of failing the route.
- The website download counter starts at **612** and is monotonic in both the browser and
  the server script. A rotated, missing or unreachable log no longer sends it backwards.

### Security

- Tor remains fully removed: no bundled binary, no route, no code path.
- No telemetry or analytics added.

## 1.0.3 - 2026-11-20

### Changed

- Removed Tor at every layer: no bundled Tor Expert Bundle, no Tor route, no Tor install
  path, and no Tor code. The remaining routes are Auto, WARP, HTTP Proxy, SOCKS5, Route64,
  Mullvad, Windscribe, and None.
- WARP became the default route and the setup wizard installs it first.
- Improved WARP connection checks and clearer route errors.

## 1.0.0 - 2026-11-18

First public release.

### Added

- Standalone Windows desktop app with its own `opendeath.exe`, app id, and user data folder.
- Infinite network deck: Auto, WARP, Tor, HTTP Proxy, SOCKS5, Route64, Mullvad, Windscribe,
  and None.
- Reset limit, Auto-rotate (1-50 attempts), Fix connection, Status, and Settings.
- Automatic `continue` delivery when a provider free limit is detected.
- Relaunch resume: waiting chats are saved to disk and restored after a restart.
- Bundled OpenCode engine and bundled Tor Expert Bundle 16.0.1.
- New red pixel-art skull identity, splash screen, app icon, favicons, and installer artwork.
- Redesigned **Settings - Control room** with Basic, Advanced, and Personalization tabs.
- Custom background, rainbow accents, text color, light/dark/system themes, and dictation.
- Guided NSIS setup wizard with OpenCode Desktop prerequisite check.
- Optional WARP, Windscribe, Mullvad, and WireGuard installation through official winget
  packages.
- Portable single-executable build.
- Desktop and Start menu shortcuts that launch OpenDeath only.
- Per-user uninstaller that preserves chats and settings.

### Changed

- Dark red theme is the first-run default.
- Infinite controls moved from the bottom edge to a command deck under the title bar.
- The workspace is an inset rounded card with a slim status band.
- The upstream animated wordmark is replaced by the OpenDeath wordmark.

### Security

- No telemetry or analytics added.
- The original upstream update feed is removed.
- Tor is verified against the Tor Project's signed build list before packaging.

### Known limitations

- Windows only.
- The build is not code-signed, so SmartScreen may warn on first run.
- OpenCode Desktop must be installed for the setup wizard's prerequisite check.
- Optional VPN installs depend on `winget` and vendor availability.
