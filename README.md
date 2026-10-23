<p align="center">
  <img src="assets/app_icon.png" width="120" alt="SSTP Shield">
</p>

<h1 align="center">SSTP Shield</h1>

<p align="center">
  A fast, cross-platform VPN client for <b>SSTP</b> and <b>SoftEther</b> servers.<br>
  Android · Windows · Linux
</p>

<p align="center">
  <a href="https://github.com/sstp-pinger/sstp_shield_releases/releases/latest"><b>⬇️ Download the latest release</b></a>
  ·
  <a href="https://sstp-pinger.github.io/sstp_shield_releases/"><b>🌐 Website</b></a>
</p>

---

SSTP Shield connects to SSTP and SoftEther VPN servers (including free
[VPN Gate](https://www.vpngate.net/) servers). On desktop it runs its own
pure-Dart SSTP stack; the SoftEther protocol drives the official SoftEther
client. Activation is by code — paste it or load it from a file.

This repository hosts **downloads and documentation only**. Each
[release](https://github.com/sstp-pinger/sstp_shield_releases/releases) carries
prebuilt binaries for all platforms.

## Download

Grab the [**latest release**](https://github.com/sstp-pinger/sstp_shield_releases/releases/latest) and pick the file for your platform:

| Platform | File | Notes |
|----------|------|-------|
| **Android** | `sstp-shield.apk` | Universal APK — works on any phone (~50 MB). |
| **Android** (smaller) | `sstp-shield-arm64-v8a.apk` | Per-ABI split (~20 MB). Most modern phones are **arm64-v8a**; older ones **armeabi-v7a**; emulators **x86_64**. |
| **Windows** | `sstp-shield-setup-x64.exe` | Installer — choose a folder, adds a desktop shortcut, updates itself. |
| **Windows** (portable) | `sstp-shield-windows-x64.zip` | Unzip and run **as Administrator**. No in-app updates. |
| **Linux** | `sstp-shield-linux-x64.tar.gz` | Extract, grant a capability once, then launch. |

Check the [**system requirements**](REQUIREMENTS.md) first: Android 7.0+,
Windows 10/11 x64, or 64-bit Linux with glibc 2.39+ (Ubuntu 24.04 or newer).

## Install & first run

### Android

1. Download an APK (universal if unsure).
2. Open it. Android will ask to allow installing from this source — allow it.
3. Launch **SSTP Shield**, paste your **activation code** (or load it from a
   file), and connect. On the first connect Android asks to add a VPN
   configuration — accept it.

### Windows

1. Download and run `sstp-shield-setup-x64.exe`. It is not code-signed yet, so
   if SmartScreen warns, choose **More info → Run anyway**.
2. Pick an install folder and whether to add a desktop shortcut, then finish.
3. Launch **SSTP Shield**. It asks for Administrator rights at every launch;
   they are required to create the virtual network adapter.
4. Paste your activation code and connect.

New versions are offered inside the app: it downloads the installer, checks
its SHA-256 checksum, installs it and restarts. Your settings and account are
kept, including when you uninstall.

Prefer no installer? Unzip `sstp-shield-windows-x64.zip` and **right-click
`sstp_shield.exe` → Run as administrator**. The portable copy cannot update
itself; download each new version by hand.

> **SoftEther protocol on Windows:** install the official
> [SoftEther VPN Client](https://www.softether-download.com/) first. It registers
> a signed network-adapter driver that only its own installer can provide; the
> app finds and drives it automatically. The default **SSTP** protocol needs no
> extra install.

### Linux

The default **SSTP** protocol creates a TUN device, which needs the
`CAP_NET_ADMIN` capability. Do **not** run the app with `sudo` — grant the
capability once instead:

```bash
tar xzf sstp-shield-linux-x64.tar.gz
cd sstp-shield-linux-x64

# One-time: grant the capability (needs gcc, libcap, patchelf).
./tool/setup_privilege.sh ./sstp_shield

# Launch via the generated wrapper.
./sstp-vpn
```

Re-run `setup_privilege.sh` after replacing the binary (an update clears the
capability).

> **SoftEther protocol on Linux:** no setup needed — just run `./sstp_shield`.
> Choosing SoftEther elevates a small helper through `pkexec`, so a password
> prompt appears on connect (needs `polkit`).

## Using the app

- **Activate** — paste your activation code, or tap the file button to load it
  from the code file the app saved earlier.
- **Pick a server** — browse the **Servers**, **Bookmarks**, and **Recents**
  tabs. The header shows how many servers are available and how many are
  currently reachable. A view-mode toggle switches between grouped and a flat,
  sorted list.
- **Refresh servers** — the download-style button (next to ping) fetches a fresh
  server list.
- **Ping** — measures latency so you can pick a fast server.
- **Protocol** *(desktop only)* — choose **SSTP** or **SoftEther** in a
  profile's settings. Hidden on mobile, which is SSTP-only.
- **Reconnection** — set a retry count and interval in Settings to auto-recover
  dropped tunnels.

## Command-line tool (Windows, Linux)

The Windows and Linux downloads also include `sstp-shield-cli`, which does
the same without the window: connect, measure servers, manage your access and
change settings. It uses the app's account and settings, so there is nothing
to activate twice. **Close the app first**: only one of them runs at a time.

- **Windows:** it is in the install folder. Tick **Add the command-line tool to
  PATH** in the installer to run it from any folder. `connect` needs a terminal
  opened with **Run as administrator**.
- **Linux:** after the app's setup above, grant it the same capability once,
  and again after every update:
  `sudo setcap cap_net_admin+ep ./sstp-shield-cli`

```bash
sstp-shield-cli ping                    # measure every server
sstp-shield-cli connect                 # fastest server; q or Ctrl+C disconnects
sstp-shield-cli connect --auto-switch   # keep moving until one works; n = next
sstp-shield-cli account status
sstp-shield-cli config get
```

Every command and option: [CLI.md](CLI.md).

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| Windows: connects but "connection refused" fetching servers | Make sure you launched **as Administrator**. |
| Windows: the app does not start | Install the latest [Visual C++ Redistributable (x64)](https://learn.microsoft.com/cpp/windows/latest-supported-vc-redist). |
| Linux: the app does not start (GLIBC error) | Your distribution is too old; see the [requirements](REQUIREMENTS.md). |
| Windows: SoftEther fails to connect | Install the official SoftEther VPN Client, then retry. |
| Linux: "needs CAP_NET_ADMIN" | Run `./tool/setup_privilege.sh ./sstp_shield` (no sudo), then launch `./sstp-vpn`. |
| `sstp-shield-cli`: "already running" | Close the SSTP Shield app (or the other `sstp-shield-cli`) first. |
| A server hangs or won't connect | Pick another — free servers come and go. Ping first to find a live one. |

## Privacy

SSTP Shield is a VPN client. It talks only to the VPN servers you connect to and
the activation/server-list endpoint. No source is hosted here — this repo is
downloads and docs only.

See the full [Privacy Policy](https://sstp-shield-server.vercel.app/privacy) and
[Terms of Service](https://sstp-shield-server.vercel.app/terms).
</content>
</invoke>
