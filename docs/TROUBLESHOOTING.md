# Troubleshooting

Start with **Help** in the network deck. It shows the last network error, the usual cause,
the fix, and copyable commands for Windows and Linux. The **Copy diagnostics** button puts
the app version, route, IP check, and recent errors on the clipboard.

## Windows

| Problem | Fix |
|---|---|
| SmartScreen says the publisher is unknown | The build is not code-signed. Choose **More info - Run anyway**, or verify the SHA-256 against `SHA256SUMS.txt` first. |
| "OpenCode Desktop was not found" | Install it from [opencode.ai/desktop](https://opencode.ai/desktop), then click **Check again**. Advanced users can tick **Continue without OpenCode Desktop**. |
| WARP is missing | Use **Install WARP now** in the wizard, or install it from [cloudflare.com/warp](https://www.cloudflare.com/warp/), start it once, then click **Check again**. |
| Optional VPN did not install | Confirm `winget --version` works. Install the client from its official site if `winget` is blocked. |
| App is blank after an update | Close OpenDeath, delete `%APPDATA%\dev.opendeath.desktop\Cache`, and relaunch. |
| Reset the whole app | Close it and rename `%APPDATA%\dev.opendeath.desktop` to `dev.opendeath.desktop.bak`. |
| Uninstall left the CLI on PATH | The uninstaller removes the entry it added. If PATH was edited manually, remove `%LOCALAPPDATA%\Programs\OpenDeath\bin` in **Environment Variables**. |

## Linux

| Problem | Fix |
|---|---|
| `warp-cli: command not found` | Install `cloudflare-warp` from Cloudflare's repository, or rerun the one-command installer with `--warp`. The app checks `PATH` and `/usr/bin`. |
| WARP says the daemon is not running | `sudo systemctl enable --now warp-svc`, then click **Fix connection**. |
| AppImage will not start | Install FUSE 2 (`libfuse2` on Debian/Ubuntu, `fuse` on Fedora/Arch), or run `./OpenDeath-1.0.4-x86_64.AppImage --appimage-extract-and-run`. |
| No tray icon on GNOME | Install the AppIndicator extension, or use the package build instead of the AppImage. |
| `opendeath` is not found after install | Open a new terminal, or run `command -v opendeath`. The one-command installer puts it in `/usr/local/bin`; the per-user fallback uses `~/.local/bin`, which must be on PATH. |
| Package manager refuses the deb/rpm | Install the matching dependencies with `sudo apt -f install` or `sudo dnf install`, or use the AppImage. |

## WARP and rotation

### WARP will not register

Cloudflare limits how many new device registrations one address may create. Since 1.0.4
this does not block **New IP**: the app rotates the local tunnel key with
`warp-cli tunnel rotate-keys`, which Cloudflare does not rate-limit, and the existing
registration keeps connecting.

If the daemon really has no registration:

```sh
warp-cli --accept-tos registration new
```

Wait 10-30 minutes after a 429, run the command, then click **Fix connection**.

### Cloudflare is throttling registrations

Only **new registrations** are paused, for 30 minutes. The existing WARP connection and key
rotation continue to work. **New IP** still changes the exit. Use **Proxy** if you also need
a different country.

### New IP returns the same country

A key rotation changes the exit address, not the country Cloudflare assigns. WARP picks the
country from your network. Use **Proxy** and choose a country from your list, or rotate again
later. **Auto** does this for you.

### WARP does not answer on 127.0.0.1:40000

That port is WARP's local proxy, and older clients do not have one. 1.0.4 detects this and
uses WARP's tunnel instead. If the route still fails, update Cloudflare WARP from the
official source and restart it.

### Reset limit signed me out

Fixed in 1.0.4. The session is archived, the IP changes, and the session is restored before
the app reconnects. If a rotation fails, the session is still put back. Make sure WARP or
Proxy works before using Reset.

## Updates

Installed Windows and Linux builds check for updates at launch and every ten minutes. If an
update does not appear:

1. Confirm the app was installed with the setup wizard, not run as the portable build.
2. Click **Help - Copy diagnostics** and check the version.
3. Download the current setup from [open.death.cl/download](https://open.death.cl/download.html)
   and install it over the old version. Settings and chats are preserved.

Portable builds do not self-update. Replace the executable.

## Verify a release file

Windows:

```powershell
Get-FileHash .\OpenDeath-Setup-1.0.4.exe -Algorithm SHA256
```

Linux:

```sh
sha256sum -c SHA256SUMS-linux.txt
```

If a hash does not match, delete the file and download it again. Do not run a file whose
hash does not match the published checksum.

## Collect diagnostics

**Help - Copy diagnostics** includes:

- OpenDeath version and route
- WARP client state and whether the local proxy port answers
- The current exit IP check
- The last network error and the fixes shown in Help

Paste that into a bug report. It contains no chat contents and no provider keys.
