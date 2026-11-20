# Security policy

## Reporting a vulnerability

Open an issue at <https://github.com/saintedKyle/OpenDeath/issues> with a short description
and the affected version. If the report includes secrets or personal data, say so first and
we will arrange a private channel.

Please do not publish a working exploit before the issue has been reviewed.

## Supported versions

| Version | Supported |
|---|---|
| 1.0.x | Yes |
| Older | No |

## What OpenDeath does on your machine

- Runs `opendeath.exe` and a bundled `opencode-cli.exe` service on a loopback port.
- Stores settings in `%APPDATA%\dev.opendeath.desktop`.
- Writes waiting-chat state to `%APPDATA%\dev.opendeath.desktop\infinite\pending.json`.
- Optionally calls the installed WARP, Windscribe, Mullvad, or WireGuard client.
- Fetches public proxy lists in HTTP Proxy and SOCKS5 modes.
- Calls the providers you configure. It does not proxy them through an OpenDeath server.

OpenDeath adds no telemetry, analytics, crash reporting, or remote command endpoint.

## Download integrity

Only use assets from <https://github.com/saintedKyle/OpenDeath/releases>. Every release
includes `SHA256SUMS.txt`. The binaries are not code-signed.

## Scope

The OpenCode engine and the optional VPN clients are separate products. Report engine issues
to the OpenCode project and client issues to the relevant vendor. Reports about the OpenDeath
installer, branding, deck, settings, and resume layer belong here.
