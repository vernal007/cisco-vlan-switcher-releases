# Cisco VLAN Switcher (releases)

Windows downloads of **Cisco VLAN Switcher** by VCTech: a desktop app for changing access-port
VLANs, voice VLANs and descriptions on Cisco switches, with your own MFA login, safety checks,
backups and revert.

**Download:** see [Releases](../../releases). Get `cisco-vlan-windows-x64.zip`, unzip it to a
folder (e.g. `C:\Tools\cisco-vlan`) and run `cisco-vlan-gui.exe`. Put your `cisco-vlan.toml`
next to the `.exe` (an example is included in the zip).

**Verify the download:** each release has a `.sha256` file. In PowerShell:

```powershell
Get-FileHash cisco-vlan-windows-x64.zip -Algorithm SHA256
```

The hash must match the one in `cisco-vlan-windows-x64.zip.sha256`.
