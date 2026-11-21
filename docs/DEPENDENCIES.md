# Dependencies

OpenDeath bundles its own engine and interface. This page lists what the app expects from
the operating system and which optional clients unlock each network route.

## Platform

| Platform | Minimum | Notes |
|---|---|---|
| Windows | Windows 10 22H2 or Windows 11, 64-bit | Electron 44 runtime |
| Linux | Modern 64-bit distribution with glibc and GTK 3 | X11 or Wayland; AppImage needs FUSE 2 |

The Linux desktop binary is `opendeath-desktop`. The terminal CLI is `opendeath`.

## Bundled

| Component | Location | Notes |
|---|---|---|
| OpenCode engine | `resources\opencode-cli.exe` (Windows), `resources/opencode-cli` (Linux) | Ships with the app; no separate OpenCode CLI install |
| Interface | `resources\app.asar` | Loaded locally; no remote web app |
| `opendeath` CLI | `bin\opendeath.cmd` on Windows; `/usr/bin/opendeath` on Linux | Wrapper around the bundled engine |

OpenDeath does **not** bundle or use Tor. There is no Tor binary, route, or code path.

## Windows prerequisites

| Requirement | Details |
|---|---|
| **Cloudflare WARP** | Required for the default route. The setup wizard can install it with `winget` or from the official page. |
| **OpenCode Desktop** | Recommended. The apps share the local sign-in store. The wizard checks for it and offers the official download; advanced users can continue without it. |
| **winget** | Used only when you tick optional VPN components. Windows 11 includes it; Windows 10 can install **App Installer** from the Microsoft Store. |

If you skip OpenCode Desktop, sign in through OpenDeath's own provider flow. An existing
OpenCode sign-in store may no longer receive updates from the OpenCode app.

## Linux prerequisites

| Requirement | Details |
|---|---|
| **Cloudflare WARP** | Install `cloudflare-warp` from Cloudflare's package repository, then `sudo systemctl enable --now warp-svc`. The one-command installer adds it with `--warp`. |
| **PolicyKit / systemd** | Used by `warp-svc`. The app only calls `warp-cli`; it does not run a daemon of its own. |
| **FUSE 2** | Needed by the AppImage. Use your distribution's `fuse`/`libfuse2` package, or run the AppImage with `--appimage-extract-and-run`. |

OpenCode Desktop is not required on Linux; the app bundles the engine and its own sign-in
flow.

## Optional VPN clients

Install these from the vendor or tick them in the Windows setup wizard. Each route looks
for the vendor CLI in its standard location and on `PATH`.

| Client | Windows package | Linux package | Used by |
|---|---|---|---|
| **Cloudflare WARP** | `Cloudflare.Warp` | `cloudflare-warp` | WARP and Auto |
| **Mullvad VPN** | `MullvadVPN.MullvadVPN` | `mullvad-vpn` | Mullvad |
| **Windscribe** | `Windscribe.Windscribe` | `windscribe-cli` | Windscribe |
| **WireGuard** | `WireGuard.WireGuard` | `wireguard-tools` | Route64 and WireGuard tunnels |

After installing a client, sign in, start it once, and reopen OpenDeath so the route can
find the CLI. WARP routes use `warp-cli` in proxy mode when the local proxy is available,
and fall back to the tunnel when it is not.

## Network endpoints

| Purpose | Endpoint |
|---|---|
| IP and route checks | Cloudflare trace (`cloudflare.com/cdn-cgi/trace`) and the selected provider |
| WARP control | `warp-cli` and the local WARP daemon |
| Free proxy list | proxyscrape.com and public GitHub lists |
| Updates | `https://open.death.cl/downloads/latest.yml` |
| Model providers | The providers you configure in the app |

OpenDeath adds no telemetry, analytics, or usage reporting of its own. VPN clients and
public proxies are separate services with their own privacy policies.

## Disk and permissions

| Item | Approximate size |
|---|---|
| Windows install | 1.3 GB |
| Linux package or AppImage | 220-450 MB depending on the format |
| User data | A few MB, plus chat history and session archives |

Windows installs per user and needs no administrator account. The optional VPN installers
may ask for elevation. Linux packages install system-wide through the package manager; the
one-command installer falls back to `~/.local` when `sudo` is unavailable.
