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
| **Android** | `sstp-shield-local.apk` | Universal APK — works on any phone (~50 MB). |
| **Android** (smaller) | `sstp-shield-local-arm64-v8a.apk` | Per-ABI split (~20 MB). Most modern phones are **arm64-v8a**; older ones **armeabi-v7a**; emulators **x86_64**. |
| **Windows** | `sstp-shield-windows-x64.zip` | Unzip and run. Must run **as Administrator**. |
| **Linux** | `sstp-shield-linux-x64.tar.gz` | Extract, grant a capability once, then launch. |

> The `-foreign` APKs are an alternate build variant. Unless you were told to use
> them, choose the plain (`local`) ones.

## Install & first run

### Android

1. Download an APK (universal if unsure).
2. Open it. Android will ask to allow installing from this source — allow it.
3. Launch **SSTP Shield**, paste your **activation code** (or load it from a
   file), and connect. On the first connect Android asks to add a VPN
   configuration — accept it.

### Windows

1. Download and unzip `sstp-shield-windows-x64.zip`.
2. **Right-click `sstp_shield.exe` → Run as administrator.** Administrator rights
   are required to create the virtual network adapter; the app will not connect
   without them.
3. Paste your activation code and connect.

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

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| Windows: connects but "connection refused" fetching servers | Make sure you launched **as Administrator**. |
| Windows: SoftEther fails to connect | Install the official SoftEther VPN Client, then retry. |
| Linux: "needs CAP_NET_ADMIN" | Run `./tool/setup_privilege.sh ./sstp_shield` (no sudo), then launch `./sstp-vpn`. |
| A server hangs or won't connect | Pick another — free servers come and go. Ping first to find a live one. |

## Privacy

SSTP Shield is a VPN client. It talks only to the VPN servers you connect to and
the activation/server-list endpoint. No source is hosted here — this repo is
downloads and docs only.
</content>
</invoke>
