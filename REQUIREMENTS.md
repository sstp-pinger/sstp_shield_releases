# System requirements

What SSTP Shield needs on each platform. Sizes are for v3.3.0.

## Android

| | Requirement |
|---|---|
| **Android version** | Android 7.0 (API 24) or newer |
| **Processor** | 64-bit ARM (`arm64-v8a`, most phones), 32-bit ARM (`armeabi-v7a`, older phones) or `x86_64` (emulators). 32-bit x86 is not supported. |
| **Storage** | About 20 MB download for a per-ABI APK, or about 56 MB for the universal `sstp-shield.apk`. Leave room for an in-app update to download beside the installed app. |
| **Permissions** | Accept the VPN connection request on the first connect. On Android 13+, allow notifications so the connection status can be shown. Allow **Install unknown apps** for SSTP Shield if you want in-app updates. |
| **Other** | One VPN app at a time: Android stops any other VPN when SSTP Shield connects. |

## Windows

| | Requirement |
|---|---|
| **Windows version** | Windows 10 or Windows 11, 64-bit |
| **Processor** | x64 (Intel or AMD). Windows on ARM is not supported: the network adapter driver is x64 only. |
| **Storage** | About 40 MB installed. An in-app update downloads the 16.5 MB installer into the temp folder first. |
| **Account** | Administrator rights. Installing asks for them once, and the app asks again at every launch (a UAC prompt): creating the network adapter and changing routes both require them. |
| **Runtime** | The [Microsoft Visual C++ Redistributable (x64)](https://learn.microsoft.com/cpp/windows/latest-supported-vc-redist), latest version. Most Windows 10/11 PCs already have it; install it if the app does not start. |
| **SoftEther protocol (optional)** | The official [SoftEther VPN Client](https://www.softether-download.com/). The default SSTP protocol needs nothing extra. |

The installer (`sstp-shield-setup-x64.exe`) is not code-signed yet, so Windows SmartScreen may warn on the first run: choose **More info → Run anyway**. In-app updates work only in an installed copy; the portable zip opens the download page instead.

## Linux

| | Requirement |
|---|---|
| **Distribution** | 64-bit x86 with **glibc 2.39 or newer**, for example Ubuntu 24.04+, Debian 13+ or Fedora 40+. The build is made on Ubuntu 24.04, so older distributions may fail to start it. |
| **Processor** | x64 only. ARM Linux is not built. |
| **Storage** | About 27 MB download, more once extracted. |
| **Desktop** | GTK 3 (installed with GNOME, KDE, Xfce and most desktops); X11 or Wayland |
| **Kernel** | TUN support (`/dev/net/tun`), present in all mainstream kernels |
| **Tools** | `ip` from iproute2. For the one-time setup: `gcc`, `libcap` (`setcap`), `patchelf` and `sudo`. |
| **SoftEther protocol (optional)** | `polkit` (`pkexec`), which asks for your password when you connect. The SoftEther client itself is bundled. |

## Command-line tool

`sstp-shield-cli` has the same requirements as the desktop app it ships with.
On Windows, `connect` needs a terminal opened with **Run as administrator**. On
Linux, it needs the capability once, after the app's setup:
`sudo setcap cap_net_admin+ep ./sstp-shield-cli`. See [CLI.md](CLI.md).

## Network (all platforms)

- Outbound TCP to the VPN server's port, usually 443.
- HTTPS access to the server-list and activation service.
- In-app updates download from GitHub (`github.com` and its release-download host). If those are blocked on your network, download the new version manually.
- Proxy sharing (optional): the other devices must be on the same local network.
