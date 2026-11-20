# Installation guide

This guide covers the Windows setup wizard and the portable build.

## Before you begin

1. Use **Windows 10 22H2 or Windows 11, 64-bit**.
2. Install **OpenCode Desktop** from <https://opencode.ai/desktop>.
3. Make sure you have an internet connection.
4. Optional: confirm `winget --version` works in PowerShell. You only need it if you tick
   optional VPN clients in the wizard.

OpenDeath installs per user under `%LOCALAPPDATA%\Programs\OpenDeath`. It does not need an
administrator account of its own. Some optional VPN installers may ask for elevation.

---

## Setup wizard

### 1. Welcome

The first page introduces OpenDeath and explains that it installs alongside OpenCode
Desktop. Click **Next**.

### 2. Prerequisites

The wizard checks for OpenCode Desktop in these places:

- `%LOCALAPPDATA%\Programs\ai.opencode.desktop\OpenCode.exe`
- `%PROGRAMFILES%\OpenCode\OpenCode.exe`
- the per-user uninstall registry

If the app is found, the page turns green and you can continue.

If it is not found:

1. Click **Open download page**.
2. Install OpenCode Desktop.
3. Return to the wizard and click **Check again**.

There is an advanced checkbox to continue without OpenCode. This is not recommended, because
OpenDeath shares the local sign-in store and expects the OpenCode desktop prerequisite to be
present.

### 3. Components

`OpenDeath (required)` is always selected. It contains the desktop app, the bundled engine,
and the uninstaller.

Choose your shortcuts:

- **Desktop shortcut** — creates `OpenDeath.lnk` that launches `opendeath.exe`.
- **Start menu shortcut** — creates an OpenDeath folder with the app and uninstaller.

Choose optional VPN clients:

- **Cloudflare WARP**
- **Windscribe**
- **Mullvad VPN**
- **WireGuard**

These checkboxes call `winget install` with the official package ID. Nothing is downloaded
from a third-party mirror. The clients are not bundled and are not required unless you want
to use that connection mode. You can leave all of them unchecked and install them later.

### 4. Install location

The default is `%LOCALAPPDATA%\Programs\OpenDeath`. You may choose another folder. Avoid
installing into `Program Files` unless you understand the per-user permission model.

### 5. Install

The wizard extracts the application, writes the uninstall entry, and creates shortcuts.
Optional VPN installs may take several minutes and may show a Windows elevation prompt.

If a VPN package is already installed, winget may return a non-zero code. The wizard records
it and continues; the existing installation is fine.

### 6. Finish

Leave **Launch OpenDeath** checked and click **Finish**. This launches
`%INSTDIR%\opendeath.exe` — not OpenCode.

---

## Portable build

1. Download `OpenDeath-Portable-1.0.3.exe`.
2. Move it to any writable folder.
3. Run it.

The portable build extracts itself to a temporary folder at launch. User data is written to
`%APPDATA%\dev.opendeath.desktop`, so it is shared with an installed copy. Do not run the
portable and installed copies at the same time.

To remove it, close OpenDeath and delete the portable executable. Remove
`%APPDATA%\dev.opendeath.desktop` if you also want to delete settings and chats.

---

## Silent install

```powershell
.\OpenDeath-Setup-1.0.3.exe /S
```

Silent mode installs the required files and creates the default shortcuts. Optional
dependency sections are not selected. To choose a directory:

```powershell
.\OpenDeath-Setup-1.0.3.exe /S /D=C:\Apps\OpenDeath
```

`/D` must be the last argument and must not be quoted.

---

## First launch

1. If Windows SmartScreen appears, choose **More info → Run anyway**.
2. Open **Settings → Control room**.
3. Pick a connection mode. Start with **Auto** or **None**.
4. Click **Fix connection**, then **Status**, to confirm the route.
5. Start a chat.

WARP is required and is installed by the wizard. Windscribe, Mullvad, and WireGuard need their client installed,
running, and signed in.

---

## Updating

Run the newer setup over the old installation. The wizard replaces application files and
keeps `%APPDATA%\dev.opendeath.desktop` untouched.

For the portable build, close the app and replace the executable.

---

## Verifying a download

```powershell
Get-FileHash .\OpenDeath-Setup-1.0.3.exe -Algorithm SHA256
Get-Content .\SHA256SUMS.txt
```

The hashes must match exactly. If they do not, delete the file and download it again from
the official release page.

---

## Uninstalling

Use **Settings → Apps → Installed apps → OpenDeath → Uninstall**, or run
`Uninstall OpenDeath.exe` from the install folder.

The uninstaller removes:

- application files under the install folder;
- Desktop and Start menu shortcuts;
- the uninstall registry entry;
- the `opendeath.exe` App Paths entry.

It does **not** remove `%APPDATA%\dev.opendeath.desktop`. Delete that folder manually for a
full reset.
