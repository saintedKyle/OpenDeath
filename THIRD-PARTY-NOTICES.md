# Third-party notices

OpenDeath is built on open-source software. This page lists the main components and the
licenses that apply to them. The installed application also contains `LICENSE.electron.txt`
and `LICENSES.chromium.html` from the Electron runtime.

OpenDeath is not affiliated with OpenCode, the Tor Project, Cloudflare, Windscribe, Mullvad,
or WireGuard. All names and trademarks belong to their owners.

---

## OpenCode

- Project: <https://github.com/anomalyco/opencode>
- License: MIT
- Copyright (c) 2025 opencode
- Used as: the desktop engine, service protocol, and parts of the user interface

```text
MIT License

Copyright (c) 2025 opencode

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## OpenCode Infinite

- License: GNU General Public License, version 3 (GPL-3.0)
- Copyright (C) 2026 jeko1107
- Used as: the basis of the Infinite network/rotation layer

The OpenDeath executable includes components derived from OpenCode Infinite. GPL-3.0
requires that recipients of the binary can obtain the corresponding source for those
components.

**Written offer.** For at least three years from the release date of each version, the
corresponding source for the GPL-3.0 components is available to anyone who received that
binary. Open an issue at <https://github.com/saintedKyle/OpenDeath/issues> with the release
version and the email address to send the source archive to. This repository does not
publish the source as part of the product download.

GPL-3.0 text: <https://www.gnu.org/licenses/gpl-3.0.html>

## Tor

- Project: <https://www.torproject.org/>
- Component: Tor Expert Bundle 16.0.1, Windows x86_64
- License: BSD 3-Clause
- Used as: the bundled Tor client in `resources/tor`

```text
Copyright (c) 2001-2024, Roger Dingledine, Nick Mathewson, and the Tor Project
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice,
   this list of conditions and the following disclaimer.
2. Redistributions in binary form must reproduce the above copyright notice,
   this list of conditions and the following disclaimer in the documentation
   and/or other materials provided with the distribution.
3. Neither the name of the Tor Project nor the names of its contributors may
   be used to endorse or promote products derived from this software without
   specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
POSSIBILITY OF SUCH DAMAGE.
```

## Electron

- Project: <https://www.electronjs.org/>
- License: MIT
- Copyright (c) Electron contributors; Copyright (c) 2013-2020 GitHub Inc.
- Used as: the desktop runtime

The full text ships with the application in `LICENSE.electron.txt`.

## Chromium

- Project: <https://www.chromium.org/>
- License: BSD-style and other open-source licenses
- Used as: the renderer bundled with Electron

The full text ships with the application in `LICENSES.chromium.html`.

## JavaScript and native packages

OpenDeath bundles open-source npm packages, including components from the OpenCode
workspace, SolidJS, Effect, Vite, esbuild, node-pty, and related libraries. Their licenses
are MIT, ISC, Apache-2.0, BSD, or compatible permissive licenses. License files are retained
inside `resources/app.asar` where the packages include them.

## Optional external software

These are **not** bundled with OpenDeath. The setup wizard can install them from their
official vendor packages, and they remain separate products under their own licenses:

| Product | License / terms |
|---|---|
| Cloudflare WARP | Cloudflare One Client terms |
| Windscribe | Windscribe terms of service |
| Mullvad VPN | Mullvad terms of service (GPL-3.0 client) |
| WireGuard | GPL-2.0 |

## Free proxy lists

The HTTP proxy and SOCKS5 modes can fetch public proxy lists from proxyscrape.com and
public GitHub repositories. Those lists are third-party data, not part of OpenDeath.
