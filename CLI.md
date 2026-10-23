# sstp-shield-cli

SSTP Shield without the window, for Windows and Linux. It ships next to the app
(`sstp-shield-cli.exe` in the install folder on Windows, `sstp-shield-cli` in
the Linux bundle) and uses the app's account, settings, server list and
bookmarks. An account activated in one works in the other.

The app and the CLI never run at the same time: whichever starts second says
so and exits. Close the app before using the CLI, and the other way round.

## Before the first connect

**Windows:** `connect` needs administrator rights. Open the terminal with
**Run as administrator**. To run the tool by name from any folder, tick
**Add the command-line tool to PATH** in the installer. Otherwise run it from
the install folder.

**Linux:** creating the tunnel needs the `CAP_NET_ADMIN` capability, the same
as for the app. Do the app's one-time setup, then give the CLI the capability:

```sh
./tool/setup_privilege.sh ./sstp_shield
sudo setcap cap_net_admin+ep ./sstp-shield-cli
```

Repeat the `setcap` after installing a new version. Don't run the CLI with
`sudo`. The SoftEther protocol (`--protocol softether`) needs neither step:
it asks for your password through `pkexec` when it connects.

## Commands

```
sstp-shield-cli connect   [host[:port]] [--country CC | --bookmark N]
                          [--auto-switch] [--protocol sstp|softether]
                          [--no-share] [--username U --password P]
sstp-shield-cli servers   [--refresh] [--country CC] [--limit N | --all] [--json]
sstp-shield-cli ping      [--refresh] [--top N]
sstp-shield-cli account   status | activate <code> | activate --file F | trial
                          | subscribe [--network bep20|trc20 --tx HASH]
                          | sessions | link [--out F]
sstp-shield-cli bookmarks list | add <host[:port]> | remove <host[:port]>
sstp-shield-cli config    get [key] | set <key> <value>
sstp-shield-cli logs      [--follow] [--app] [--path]
sstp-shield-cli version   [--check]
```

`sstp-shield-cli help <command>` shows every option.

### connect

With no address, it connects to the fastest measured server (or the fastest
in `--country`). If nothing has been measured yet, it measures first. An
address that is in the server list uses that server; any other address is
treated as a custom node, with `vpn`/`vpn` as the default credentials.

It stays in the foreground, with a status line showing the connection time and
traffic. Press **q** or **Ctrl+C** to disconnect. A second Ctrl+C stops
waiting for a clean teardown.

`--auto-switch` works like holding Connect in the app. It keeps checking that
pages load through the tunnel, and moves through every measured server until
one keeps working. Press **n** to move to the next server yourself.

Reconnecting after a drop, the traffic check and proxy sharing all follow your
settings (see `config`). `--no-share` turns proxy sharing off for one
connection.

### Settings

`config get` lists every setting with its allowed values. They are the same
settings as in the app:

| Key | Values |
|---|---|
| `protocol` | `sstp`, `softether` |
| `ping-mode` | `accurate` (TLS handshake), `fast` (TCP connect) |
| `ping-timeout` | 500–8000 ms |
| `ping-batch` | 1–1000 servers at once |
| `fetch-count` | 50–5000 servers per refresh |
| `region` | `all`, `curated` |
| `reconnect-count` | 0–20 (0 turns reconnecting off) |
| `reconnect-interval` | 1–60 s |
| `proxy-sharing` | `on`, `off` |
| `verify-deadline` | 2–15 s |
| `verify-nodes` | 1–20 |
| `guard-interval`, `guard-timeout`, `guard-failures` | auto-switch tuning |
| `softether-natt`, `softether-natt-wait` | SoftEther transport |

## Exit codes

| Code | Meaning |
|---|---|
| 0 | Done (for `connect`: connected, then disconnected on request) |
| 1 | Failed: the message says why |
| 64 | Wrong arguments |
| 75 | The app, or another `sstp-shield-cli`, is running |
| 77 | Needs administrator rights (Windows) or `CAP_NET_ADMIN` (Linux) |

## Files

| | Windows | Linux |
|---|---|---|
| Settings and account (shared with the app) | `%APPDATA%\com.atasanbratan\SSTP Shield\shared_preferences.json` | `~/.local/share/com.atasanbratan.sstp_shield/shared_preferences.json` |
| CLI log (cleared at every start) | `%LOCALAPPDATA%\SSTPShield\sstp-shield-cli.log` | `~/.local/share/sstp-shield/sstp-shield-cli.log` |

`sstp-shield-cli logs` prints the log, and `--verbose` also prints it as it
is written.

## Not in the CLI

- Android and iOS.
- Running in the background. `connect` stays in the foreground, and `status`
  and `disconnect` from another terminal are not available yet.
- Installing updates. `version --check` says whether a newer version is out.
  The CLI ships with the app, so updating the app updates the CLI too.
- The Telegram QR code for proxy sharing. `connect` prints the address
  instead.
