# Arkivix Downloads

[![Latest release](https://img.shields.io/github/v/release/stoxello/Arkivix-Releases?display_name=tag&sort=semver)](https://github.com/stoxello/Arkivix-Releases/releases/latest)
[![Platforms](https://img.shields.io/badge/platform-Windows%20%7C%20Linux-4f46e5)](#available-downloads)

Arkivix is a self-hosted file backup and recovery system for protecting Windows and Linux machines. This repository is the official public download channel for compiled Arkivix agent releases.

> [!IMPORTANT]
> Arkivix is proprietary, closed-source software. This repository contains release binaries and public documentation only. The source code is private and is not distributed here.

## Available downloads

Download the newest version from the [latest release](https://github.com/stoxello/Arkivix-Releases/releases/latest).

| Platform | Package | Service integration |
| --- | --- | --- |
| Windows x64 | [`arkivix-agent-win-x64.zip`](https://github.com/stoxello/Arkivix-Releases/releases/latest/download/arkivix-agent-win-x64.zip) | Windows Task Scheduler, running as `SYSTEM` |
| Linux x64 | [`arkivix-agent-linux-x64.tar.gz`](https://github.com/stoxello/Arkivix-Releases/releases/latest/download/arkivix-agent-linux-x64.tar.gz) | `systemd` service |

Each release also includes [`SHA256SUMS.txt`](https://github.com/stoxello/Arkivix-Releases/releases/latest/download/SHA256SUMS.txt) so you can verify the packages before installation. These downloads are self-contained; the .NET SDK is not required on agent machines.

## Before installing

You need:

- A running Arkivix server
- The server URL and a one-time enrollment code created with **Add device** in the Arkivix dashboard
- A local folder to protect
- Administrator access on Windows or `sudo` access on Linux

Use HTTPS when the agent connects to a server on another machine. Plain HTTP enrollment is accepted only for a server running on the same machine through a loopback address.

## Install on Windows

1. Download and extract `arkivix-agent-win-x64.zip`.
2. Open PowerShell as Administrator in the extracted folder.
3. Run:

```powershell
.\install-agent.ps1 `
  -ServerUrl "https://backup.example.com:5199" `
  -EnrollmentToken "YOUR_ONE_TIME_CODE" `
  -SourcePath "C:\Users\you\Documents"
```

The installer enrolls the device, copies the agent to `C:\Program Files\Arkivix Agent`, and creates an auto-restarting startup task named **Arkivix Agent**.

## Install on Linux

Extract the archive, then run the installer with `sudo`:

```bash
tar -xzf arkivix-agent-linux-x64.tar.gz

sudo ./install-agent.sh \
  --server "https://backup.example.com:5199" \
  --enrollment-token "YOUR_ONE_TIME_CODE" \
  --path "/home/you/Documents"
```

The installer enrolls the device, installs the executable under `/opt/arkivix-agent`, and enables an auto-restarting `arkivix-agent.service` systemd unit.

## Snapshot consistency

Both installers accept an optional snapshot mode:

- `Auto` (default) uses the native snapshot provider when available and otherwise performs a live-filesystem backup.
- `Required` stops the backup if a consistent snapshot cannot be created.
- `Off` always performs a live-filesystem backup.

On Windows, add `-SnapshotMode Required` to the PowerShell command. On Linux, add `--snapshot-mode Required` to the installer command. Native snapshot creation requires the elevated privileges used by the installers.

## Verify a download

Download `SHA256SUMS.txt` from the same release as the agent package, then compare its listed hash with the package you downloaded.

Windows PowerShell:

```powershell
Get-FileHash .\arkivix-agent-win-x64.zip -Algorithm SHA256
```

Linux:

```bash
sha256sum arkivix-agent-linux-x64.tar.gz
```

Do not install a package if its hash does not exactly match the corresponding value in `SHA256SUMS.txt`.

## Support

- For release-package or download problems, [open an issue](https://github.com/stoxello/Arkivix-Releases/issues).
- For account, licensing, or private support requests, use [Stoxello Support](https://stoxello.com/support).
- Do not post enrollment codes, device credentials, server secrets, or sensitive file paths in a public issue.

## Source code and license

Arkivix is commercial, proprietary software. No open-source license is granted by this repository, and publishing compiled artifacts here does not grant access to the private source code or permission to copy, modify, or redistribute the software except under a separate written license or as required by applicable law.

Copyright &copy; 2026 Stoxello. All rights reserved. Use of Arkivix is subject to the [Stoxello Terms](https://stoxello.com/terms).
