# Dependencies

OpenDeath is a Windows desktop application. This page explains what is required, what is
bundled, and what the setup wizard can install for you.

## Required

| Dependency | Version | Why |
|---|---|---|
| Windows | 10 22H2 or 11, 64-bit | The app uses the Electron 44 runtime and Windows APIs |
| OpenCode Desktop | 2.x | Prerequisite checked by setup; shares the local sign-in store |
| Internet | — | Model providers, sign-in, and optional downloads |

### OpenCode Desktop

Download: <https://opencode.ai/desktop>

OpenDeath keeps its own executable, application data, and bundled engine. It does not launch
OpenCode Desktop and does not modify its installation. The setup wizard checks that OpenCode
Desktop is present before installing.

If you uninstall OpenCode Desktop later, OpenDeath continues to run, but the shared sign-in
store may no longer receive updates from the OpenCode app.

## Bundled

| Component | Version | Notes |
|---|---|---|
| OpenCode engine | OpenDeath build | `resources\opencode-cli.exe` |
| Tor Expert Bundle | 16.0.1, Windows x64 | `resources\tor\tor\tor.exe` |
| Electron | 44.9.1 | Desktop runtime |
| Node.js runtime | Bundled with Electron | No system Node.js required |
| Visual C++ runtime | Bundled where needed | No separate redistributable install |

Tor is always available, even if you do not tick any optional component.

## Optional VPN clients

The setup wizard can install these through `winget`. They are separate products with their
own installers, services, accounts, and terms.

| Client | winget ID | Official download | Used by |
|---|---|---|---|
| Cloudflare WARP | `Cloudflare.Warp` | <https://1.1.1.1/> | WARP mode |
| Windscribe | `Windscribe.Windscribe` | <https://windscribe.com/> | Windscribe mode |
| Mullvad VPN | `MullvadVPN.MullvadVPN` | <https://mullvad.net/> | Mullvad mode |
| WireGuard | `WireGuard.WireGuard` | <https://www.wireguard.com/> | WireGuard tunnels |

### Installing them manually

```powershell
winget install --id Cloudflare.Warp --exact
winget install --id Windscribe.Windscribe --exact
winget install --id MullvadVPN.MullvadVPN --exact
winget install --id WireGuard.WireGuard --exact
```

After installing a client:

1. Open it once and complete its sign-in or setup.
2. Close OpenDeath.
3. Reopen OpenDeath so it can find the client CLI.

The app looks for the vendor CLI in the standard install locations. If the client is not
found, the route shows as unavailable instead of failing silently.

## What OpenDeath does not need

- No system-wide Node.js, Bun, or npm.
- No Python.
- No separate Tor install.
- No OpenCode CLI install — the engine is bundled.
- No account with OpenDeath. There is no OpenDeath server.

## Network endpoints

OpenDeath itself uses:

| Endpoint | Purpose |
|---|---|
| `https://www.cloudflare.com/cdn-cgi/trace` | Read the current exit IP and WARP state |
| `https://opencode.ai` | Connectivity probe through the selected route |
| Provider API endpoints | Your configured model providers |
| `http://127.0.0.1:<port>` | Local OpenCode service |

Public proxy lists are fetched from proxyscrape.com and public GitHub repositories only in
the HTTP Proxy and SOCKS5 modes.

Optional installers are resolved by `winget` to the vendor's official package.

## Disk and memory

| Item | Approximate |
|---|---|
| Installed application | 1.0–1.3 GB |
| Portable extraction at runtime | 1.0–1.3 GB in `%TEMP%` |
| User data | A few MB, plus chat history |
| Idle memory | 400–700 MB, depending on the workspace |
