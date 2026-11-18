# Troubleshooting

## Setup and installation

### “Windows protected your PC” / SmartScreen

The build is not code-signed, so Windows shows a SmartScreen warning the first time.

1. Click **More info**.
2. Check the publisher line and the file name.
3. Click **Run anyway**.

If you want to be certain, compare the SHA-256 with `SHA256SUMS.txt` from the release page
before running the file.

### “OpenCode Desktop was not found”

The wizard checks the default per-user install folder, `Program Files`, and the uninstall
registry.

1. Install OpenCode Desktop from <https://opencode.ai/desktop>.
2. Return to the wizard.
3. Click **Check again** on the Prerequisites page.

If you installed OpenCode somewhere unusual, use the **Continue without OpenCode Desktop
(advanced)** checkbox. OpenDeath is self-contained; the checkbox exists because the shared
sign-in store is normally created by the OpenCode app.

### Optional VPN did not install

The wizard uses `winget`. Check it in PowerShell:

```powershell
winget --version
```

If the command is missing, install **App Installer** from the Microsoft Store, then install
the VPN client from its official site.

Common reasons winget fails:

- no internet access or a blocked Microsoft Store endpoint;
- the package is already installed (non-zero exit code, but nothing is broken);
- the vendor installer needs elevation and the prompt was cancelled;
- a corporate policy blocks `winget`.

You can always install the client manually. See [`DEPENDENCIES.md`](DEPENDENCIES.md).

### The setup stops while replacing files

Close OpenDeath and any OpenCode window, then run the setup again. The installer runs
`taskkill` on `opendeath.exe`, but a file can still be locked by antivirus or a pending
reboot.

### I need a silent install

```powershell
.\OpenDeath-Setup-1.0.0.exe /S
```

Optional components are not selected in silent mode.

---

## First launch

### The window is blank or stuck on the splash

1. Close OpenDeath.
2. Delete `%APPDATA%\dev.opendeath.desktop\Cache`.
3. Relaunch.

If it still fails, check `%APPDATA%\dev.opendeath.desktop\logs` for the last error.

### The app opens OpenCode instead of OpenDeath

That cannot happen from the installed shortcuts: they target
`%LOCALAPPDATA%\Programs\OpenDeath\opendeath.exe`. If you pinned the wrong executable, unpin
it and use the Start menu **OpenDeath** shortcut.

The `opencode://` deep-link scheme is shared intentionally so Console authorisation can
return to the app. Launching a link may open whichever registered handler Windows chooses.
Launch OpenDeath from its own shortcut for normal use.

### OpenDeath is running but the deck says “engine unavailable”

The local OpenCode service did not start.

1. Click **Fix connection**.
2. Close OpenDeath completely, including tray processes.
3. Relaunch.

A stale `opencode-cli.exe` from another build can hold the service port. Open Task Manager,
end any `opencode-cli.exe` running from `%LOCALAPPDATA%\Programs\OpenDeath`, and relaunch.

---

## Network modes

### WARP mode is unavailable

- Install Cloudflare WARP.
- Open WARP once and complete registration.
- Confirm `warp-cli --version` works in a new terminal.
- Restart OpenDeath.

### Windscribe mode is unavailable

- Install Windscribe.
- Sign in and connect once.
- Confirm the Windscribe CLI is installed.
- Restart OpenDeath.

### Mullvad mode is unavailable

- Install Mullvad VPN.
- Sign in with your account.
- Open the app once.
- Restart OpenDeath.

### Tor will not connect

Tor is bundled, so there is nothing to install. Some networks block Tor entry guards.

- Try **Auto** mode.
- Click **Fix connection**.
- Wait up to two minutes for the first circuit.
- If you are behind a captive portal, sign in to the network first.

### The exit IP did not change

Some providers report the same IP through IPv6 or a cached DNS answer.

1. Click **Status**.
2. Try a different mode (WARP → Tor, for example).
3. Click **Fix connection**.
4. Check that the selected VPN client is actually connected.

### “continue” was not sent to a chat

OpenDeath sends `continue` only to chats that are waiting on a free limit. If the chat has
already moved on, there is nothing to resume. The status line reports how many chats were
updated.

### Chats did not resume after a restart

- Check **Settings → Continue after relaunch** is enabled.
- Make sure the app closed normally, not through Task Manager.
- Inspect `%APPDATA%\dev.opendeath.desktop\infinite\pending.json`; entries expire after 12 hours.

---

## Appearance

### Background or text color is unreadable

Open **Settings → Personalization** and reset the background and text color. The defaults
are the dark-red theme with the standard text color.

### Rainbow mode is too intense

Turn off **Rainbow** in Personalization. The setting applies to accents only; the rest of the
theme stays dark.

---

## Reset and logs

### Full settings reset

Close OpenDeath, then rename the data folder:

```powershell
Rename-Item "$env:APPDATA\dev.opendeath.desktop" "dev.opendeath.desktop.bak"
```

Relaunch. A new data folder is created. Rename it back to restore the old settings.

### Reset only the network state

Close OpenDeath and delete:

```text
%APPDATA%\dev.opendeath.desktop\infinite.json
%APPDATA%\dev.opendeath.desktop\infinite\pending.json
```

### Logs

```text
%APPDATA%\dev.opendeath.desktop\logs\
```

Attach the newest log to an issue. Remove anything private before posting.

### Uninstall leaves settings behind

That is intentional. The uninstaller keeps `%APPDATA%\dev.opendeath.desktop` so a reinstall
keeps your chats. Delete the folder manually to remove everything.

---

## Getting help

Open an issue at <https://github.com/saintedKyle/OpenDeath/issues> and include:

- OpenDeath version (Settings → About)
- Windows version
- connection mode
- the exact error text
- the newest log file

Do not post API keys, auth tokens, or private chat content.
