# Changelog

All notable changes to OpenDeath are documented here.

## 1.0.0 — 2026-11-18

First public release.

### Added

- Standalone Windows desktop app with its own `opendeath.exe`, app id, and user data folder.
- Infinite network deck: Auto, WARP, Tor, HTTP Proxy, SOCKS5, Route64, Mullvad, Windscribe,
  and None.
- Reset limit, Auto-rotate (1–50 attempts), Fix connection, Status, and Settings.
- Automatic `continue` delivery when a provider free limit is detected.
- Relaunch resume: waiting chats are saved to disk and restored after a restart.
- Bundled OpenCode engine and bundled Tor Expert Bundle 16.0.1.
- New red pixel-art skull identity, splash screen, app icon, favicons, and installer artwork.
- Redesigned Settings → Control room with Basic, Advanced, and Personalization tabs.
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
- Self-update is disabled; updates are installed from official releases.

### Security

- No telemetry or analytics added.
- The original upstream update feed is removed.
- Tor is verified against the Tor Project’s signed build list before packaging.

### Known limitations

- Windows only.
- The build is not code-signed, so SmartScreen may warn on first run.
- OpenCode Desktop must be installed for the setup wizard’s prerequisite check.
- Optional VPN installs depend on `winget` and vendor availability.
