# Spicetify_Auto_Repair_V3
Automatically re-applies Spicetify after Spotify updates — with **zero manual intervention after installation**.

## What is this?
Spotify updates can cause Spicetify customisations to stop working or require the Spicetify backup to be reapplied.
**Spicetify Auto Repair** monitors the installed Spotify version and automatically runs the Spicetify repair process whenever it detects a new version.
It runs silently in the background, so you don't need to manually run PowerShell commands after every Spotify update.

## Features
- 🔄 Detects Spotify version changes automatically
- 🛠️ Runs `spicetify backup apply` after an update
- 🔧 Attempts additional repair commands if the normal apply fails
- 👻 Completely hidden background operation
- ⏱️ Checks every 15 minutes
- 🚀 Survives Windows restarts
- 📝 Keeps a log of repair attempts
- ♻️ Replaces previous versions of the scheduled task automatically
- 🧹 Includes an uninstall script

## Requirements
- Windows 10 or Windows 11
- Spotify installed
- [Spicetify](https://github.com/spicetify/cli) already installed and working
- `spicetify` available as a command in PowerShell/Command Prompt

> **Important:** This tool does not install Spicetify itself. Install Spicetify first.
> 
## Installation
### 1. Download the files
Download the release.
### 2. Extract the file
Right-click the Zip
```text
Spicetify_Auto_Repair_V2_1.zip
```
### Open the Extracted File
```Spicetify_Auto_Repair_V2_1```
### Run the .Bat file
```Install_V2_1.bat```

The installer will:
1. Copy the repair script to your Local AppData folder.
2. Install the hidden launcher.
3. Remove any previous `Spicetify Auto Repair` scheduled task.
4. Create the new hidden scheduled task.
5. Configure it to check every 15 minutes.

After installation, you can close the installer.

## How it works
Every 15 minutes Windows Task Scheduler launches the hidden VBScript.

The script:
1. Finds the installed Spotify executable.
2. Reads its product version.
3. Compares it with the last Spotify version that was processed.
4. If the version hasn't changed, it exits immediately.
5. If Spotify has updated, it waits briefly for the update to finish.
6. Runs:

```powershell
spicetify backup apply
```

If that fails, it attempts:
```powershell
spicetify update
spicetify restore backup apply
```

The processed Spotify version is then saved so the repair isn't repeatedly run for the same version.

## Completely silent
V2.1 uses `wscript.exe` to launch the PowerShell script with a hidden window.
You should **not** see a Command Prompt or PowerShell window every 15 minutes.
The scheduled task itself is:
```text
Spicetify Auto Repair
```

## Logs
Repair activity is recorded in:
```text
%LOCALAPPDATA%\SpicetifyAutoRepair\last-run.log
```

The stored Spotify version is:
```text
%LOCALAPPDATA%\SpicetifyAutoRepair\spotify-version.txt
```

These files can be useful for troubleshooting.
## Uninstall
Run:
```text
Uninstall_V2_1.bat
```

This removes the scheduled task and the files installed by the tool.

## Important limitation
This tool can automatically re-apply Spicetify, but it cannot make an unsupported Spotify version compatible with Spicetify.
If a new Spotify release breaks Spicetify itself, you may need to wait for a Spicetify update.
In that situation, check the official Spicetify project:
https://github.com/spicetify/cli

## Security
This project deliberately **does not** repeatedly download and execute a remote PowerShell command such as:
```powershell
iwr -useb https://raw.githubusercontent.com/spicetify/cli/main/install.ps1 | iex
```

Instead, it uses the locally installed Spicetify CLI.
This reduces unnecessary network execution and means the automation is only responsible for re-applying an already-installed Spicetify setup.

## Files
```text
Spicetify Auto Repair/
│
├── Install_V2_1.bat
├── Spicetify_Auto_Repair.ps1
├── Spicetify_Auto_Repair_Hidden.vbs
├── Uninstall_V2_1.bat
└── README.md
```

