# Cisco VLAN Switcher (releases)

Windows downloads of **Cisco VLAN Switcher** by VCTech: a desktop app and command-line tool for
changing access-port VLANs, voice VLANs and descriptions on Cisco switches. Each person logs in
with their own username, password and second factor, so every change is tied to a real person.

**[Download the latest version](https://github.com/vernal007/cisco-vlan-switcher-releases/releases/latest)**

## What it does

* **Change a port's VLAN.** Pick a port from a list of every port on the switch (with its VLAN,
  status and description), pick the new VLAN, preview the exact commands, then apply.
* **Voice VLANs.** Add or remove a port's voice VLAN. A voice VLAN is never offered as a data
  VLAN, and changing the data VLAN leaves the phone's VLAN alone.
* **Descriptions.** Change a port's description on its own or together with a VLAN change.
* **Find a device.** Type the last 4 or more characters of a MAC address to find the port it is
  on, then jump straight to that port.
* **Revert.** Each port shows its earlier changes with a **Revert** button. A revert puts back
  only what that change touched.
* **Many ports at once (CLI).** `cisco-vlan bulk changes.csv` applies a CSV of changes with one
  login per switch.

## Logging in (MFA)

The app connects over SSH and shows your MFA server's own prompt, so you type a code or approve a
push. It works with Duo, RSA, Cisco ISE, FortiAuthenticator / FortiToken and other TACACS+ or
RADIUS MFA, whether the server asks for the code in a second prompt or expects it appended to the
password. Passwords and codes are never stored or logged.

## Safety checks

* **Trunk, routed and protected ports can't be changed.** They are listed but greyed out.
* **Only VLANs that exist and are allowed.** A VLAN that is missing, suspended or not in your
  policy's allowed list is refused.
* **Warning for ports that connect network gear.** If CDP/LLDP reports a switch, router or
  access point, or the port has more than 3 MAC addresses, you must tick an extra box.
* **Warning before rebooting a powered device.** Bouncing a port that supplies PoE to a phone,
  access point or camera needs an extra tick.
* **Auto-revert.** With auto-revert on, the port goes back on its own unless you click
  **Keep change** before the countdown ends. Where the switch supports it, the switch's own
  revert timer also rolls back if the app or the PC dies.
* **Checked after every change.** The app re-reads the port and rolls back if the change didn't
  stick.
* **Backups.** Before each change the app saves the port's config in its audit log and asks the
  switch to archive its running config on its own flash.
* **Audit log.** Every change is written to a log on the PC: who, what, when, old and new VLAN,
  ticket and result.
* **Pinned switch keys.** You trust a switch's SSH key the first time. A different key later is
  an error.

## Install

1. Download `cisco-vlan-windows-x64.zip` from
   [Releases](https://github.com/vernal007/cisco-vlan-switcher-releases/releases).
2. Unzip it to a folder you own, such as `C:\Tools\cisco-vlan`.
3. Copy `cisco-vlan.example.toml` to `cisco-vlan.toml` in the same folder and add your switches.
   The file holds switch names, defaults and policy, and never any passwords.
4. Run `cisco-vlan-gui.exe`.

The zip contains:

| File | What it is |
|---|---|
| `cisco-vlan-gui.exe` | The desktop app |
| `cisco-vlan.exe` | The command-line tool (run it from PowerShell or Windows Terminal) |
| `cisco-vlan.example.toml` | Example config |
| `bulk.example.csv` | Example file for `cisco-vlan bulk` |
| `README.md` | The full manual |
| `LICENSE` | MIT licence |

Neither program needs an installer or extra DLLs.

## Requirements

* 64-bit Windows.
* Cisco IOS or IOS-XE switches reachable over SSH. NX-OS output is very similar and mostly works.
* An account that can change access ports on the switch.

The app has been tested against a simulated switch only. Lab-test it on your own switches,
especially the revert timer, before using it in production.

## Updates

The desktop app checks for a new version when it starts and when you click **Check for
updates**. Nothing is installed until you click **Update**. From the command line:

```powershell
cisco-vlan update --check     # is there a newer version?
cisco-vlan update             # download, verify and install it
cisco-vlan update --rollback  # go back to the previous version
```

An update is installed only if the zip matches its published SHA-256 and carries a valid
signature from the key built into the app. Your config, trusted switch keys and audit log are
never touched. Version 0.1.0 has no updater, so install a newer version by hand once.

## Verify a download

Each release has a `.sha256` file. In PowerShell:

```powershell
Get-FileHash cisco-vlan-windows-x64.zip -Algorithm SHA256
```

The hash must match the one in `cisco-vlan-windows-x64.zip.sha256`.

Each release from 0.2.0 on is also signed. To check the signature with
[minisign](https://jedisct1.github.io/minisign/), download `cisco-vlan-windows-x64.zip.minisig`
into the same folder and run:

```powershell
minisign -Vm cisco-vlan-windows-x64.zip -P RWR5eQqltIoX3YFc+G5DSiW4nDDR6RcRJBIp/Ip153SXwLEbKvjgW6vk
```

## Where the app keeps its files

| What | Location |
|---|---|
| Config | `cisco-vlan.toml` next to the `.exe`, or `%APPDATA%\cisco-vlan\config.toml` |
| Trusted switch keys | `%USERPROFILE%\.cisco-vlan\known_hosts` |
| Audit log | `%USERPROFILE%\.cisco-vlan\audit.jsonl` |
| Version replaced by the last update | `previous\` next to the `.exe` files |

## More help

The `README.md` inside the zip is the full manual: every command and option, MFA setup
(including FortiAuthenticator), auto-revert, backups and the recommended switch-side setup.

This repository holds downloads only.
