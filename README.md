# SysTrayJira

System tray app that displays your Jira issues grouped by custom JQL queries.

![Python](https://img.shields.io/badge/python-3.10+-blue)
![License](https://img.shields.io/badge/license-Apache%202.0-blue)
![Platform](https://img.shields.io/badge/platform-Linux%20|%20Windows%20|%20macOS-lightgrey)

## Features

- 📋 Custom JQL groups with `active` toggle
- 🔴🟠🟡🟢🔵 Priority indicators per issue (auto-mapped to your Jira priorities)
- 🔄 Auto-refresh (configurable interval) + manual refresh
- 🔐 Multiple auth modes (Basic, Bearer, PAT)
- 📁 Bash-style `.env` file support (with `export` prefix)
- 🖱️ Click to open issue in browser
- ♻️ Reload config at runtime without restart
- 🎨 Dynamic tray icon color based on highest priority
- 🖼️ Custom tray icon support (PNG/ICO)

## Compatibility

### Operating Systems

| OS | Version | Status | Notes |
|----|---------|--------|-------|
| **Ubuntu** | 20.04+ | ✅ Tested | Native AppIndicator support |
| **Debian** | 11+ | ✅ | Native AppIndicator support |
| **Fedora** | 36+ | ✅ | Native AppIndicator support |
| **Arch Linux** | Rolling | ✅ | Install `libappindicator-gtk3` |
| **Linux Mint** | 20+ | ✅ | Native tray support |
| **Windows** | 10, 11 | ✅ | Native Win32 system tray |
| **macOS** | 10.14+ (Mojave) | ✅ | Uses NSStatusBar |

### Desktop Environments (Linux)

| DE | Status | Notes |
|----|--------|-------|
| **GNOME 3.x/40+** | ⚠️ | Requires [AppIndicator extension](https://extensions.gnome.org/extension/615/appindicator-support/) |
| **KDE Plasma** | ✅ | Native tray support |
| **XFCE** | ✅ | Native tray support |
| **Cinnamon** | ✅ | Native tray support |
| **MATE** | ✅ | Native tray support |
| **i3/Sway** | ⚠️ | Needs a tray bar (e.g. `i3bar`, `waybar`, `trayer`) |
| **Budgie** | ✅ | Native tray support |

### Display Servers (Linux)

| Server | Status | Notes |
|--------|--------|-------|
| **X11/Xorg** | ✅ | Full support |
| **Wayland** | ⚠️ | Works with StatusNotifier/AppIndicator protocol. GNOME Wayland needs the AppIndicator extension |

### Python

| Version | Status |
|---------|--------|
| 3.10+ | ✅ Required (`str.removeprefix`) |
| 3.9 and below | ❌ Not supported |

### Jira Compatibility

| Type | Status | Auth Mode |
|------|--------|-----------|
| **Atlassian Cloud** | ✅ | `basic` (email + API token) |
| **Jira Server** | ✅ | `bearer` or `pat` |
| **Jira Data Center** | ✅ | `bearer` or `pat` |

## Install

### From GitHub

```bash
pip install git+https://github.com/AxitamSrl/SysTrayJira.git
```

### From source

```bash
git clone https://github.com/AxitamSrl/SysTrayJira.git
cd SysTrayJira
pip install .
```

### Platform-specific dependencies

#### Linux (Debian/Ubuntu)

```bash
sudo apt install python3-dev python3-pip libgirepository1.0-dev gir1.2-appindicator3-0.1
pip install git+https://github.com/AxitamSrl/SysTrayJira.git
```

#### Linux (Fedora)

```bash
sudo dnf install python3-devel python3-pip gobject-introspection-devel libappindicator-gtk3
pip install git+https://github.com/AxitamSrl/SysTrayJira.git
```

#### Linux (Arch)

```bash
sudo pacman -S python-pip libappindicator-gtk3
pip install git+https://github.com/AxitamSrl/SysTrayJira.git
```

#### GNOME (tray icon not visible)

```bash
# Install AppIndicator extension
sudo apt install gnome-shell-extension-appindicator
# Then enable it in GNOME Extensions app or:
gnome-extensions enable appindicatorsupport@rgcjonas.gmail.com
# Log out and back in
```

#### macOS

```bash
brew install python libjpeg
pip install git+https://github.com/AxitamSrl/SysTrayJira.git
```

#### Windows

```bash
pip install git+https://github.com/AxitamSrl/SysTrayJira.git
```

No additional dependencies needed on Windows.

## Quick Start

```bash
# 1. Generate default config
jira-tray --init

# 2. Edit config
nano ~/.config/sysTrayJira/config.yaml   # Linux/macOS
# or
notepad %USERPROFILE%\.config\sysTrayJira\config.yaml  # Windows

# 3. Set your token
export JIRA_API_TOKEN="your-token"       # Linux/macOS
# or
set JIRA_API_TOKEN=your-token            # Windows

# 4. Run
jira-tray
```

## Configuration

Config file location: `~/.config/sysTrayJira/config.yaml`

```yaml
jira_url: "https://your-jira-instance.com"
email: "your-email@example.com"
poll_interval: 300          # auto-refresh interval in seconds
auto_refresh: true          # false = manual refresh only via tray menu

# Optional: custom tray icon (PNG, ICO, or any Pillow-supported format)
# icon: "~/.config/sysTrayJira/jira.png"

# Optional: load env vars from a bash-style .env file (supports 'export' prefix)
# env_file: "~/.env"

# Auth
auth_mode: "bearer"         # basic | bearer | pat
token_env: "JIRA_API_TOKEN" # name of the env var containing the token

# JQL groups
groups:
  - name: "📋 My Issues"
    jql: "assignee = currentUser() AND resolution = Unresolved"
    active: true            # set to false to hide without deleting
  - name: "🔥 Urgent"
    jql: "assignee = currentUser() AND priority in (Highest, High)"
    active: false
```

### Config Reference

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `jira_url` | string | required | Jira instance base URL |
| `email` | string | required for `basic` | Email for basic auth |
| `poll_interval` | int | `300` | Auto-refresh interval (seconds) |
| `auto_refresh` | bool | `true` | Enable/disable auto-refresh |
| `icon` | string | none | Path to custom tray icon (supports `~`) |
| `env_file` | string | none | Path to `.env` file (supports `~` and `export`) |
| `auth_mode` | string | `"bearer"` | `basic`, `bearer`, or `pat` |
| `token_env` | string | `"JIRA_API_TOKEN"` | Env var name for the token |
| `groups[].name` | string | required | Display name (supports emoji) |
| `groups[].jql` | string | required | JQL query |
| `groups[].active` | bool | `true` | Show/hide group |

### Auth Modes

| Mode | Description | Required fields |
|------|-------------|-----------------|
| `basic` | Email + API token (Atlassian Cloud) | `email` + `token_env` |
| `bearer` | Bearer token (Jira Server/DC) | `token_env` only |
| `pat` | Personal Access Token (Jira Server/DC) | `token_env` only |

### Priority Icons

Issues are prefixed with a colored emoji based on their Jira priority:

| Emoji | Priorities |
|-------|-----------|
| 🔴 | Immediate, Blocker, Highest, 1=Must Have, P1 |
| 🟠 | Critical, High, 2=Should Have, P2 |
| 🟡 | Major, Medium, 3=Could Have, P3 |
| 🟢 | Minor, Low |
| 🔵 | Trivial, Lowest, Very Low, P4 |
| ⚪ | Unknown / Undefined |

### Tray Menu

| Action | Description |
|--------|-------------|
| Click on issue | Opens issue in default browser |
| Reload config | Re-reads `config.yaml` (add/remove groups, change settings) |
| Refresh | Force immediate fetch of all active groups |
| Quit | Stop the application |

## Run as a Service

### Linux (systemd)

```bash
mkdir -p ~/.config/systemd/user

cat > ~/.config/systemd/user/jira-tray.service << EOF
[Unit]
Description=Jira System Tray
After=graphical-session.target

[Service]
ExecStart=$(which jira-tray)
Restart=on-failure

[Install]
WantedBy=default.target
EOF

systemctl --user daemon-reload
systemctl --user enable --now jira-tray
```

Useful commands:

```bash
systemctl --user status jira-tray     # Check status
systemctl --user restart jira-tray    # Restart
systemctl --user stop jira-tray       # Stop
journalctl --user -u jira-tray -f     # Follow logs
```

> **Note:** If your token is in a `.env` file, use the `env_file` config option rather than `EnvironmentFile` in the service (systemd doesn't support `export` prefix).

### macOS (launchd)

```bash
cat > ~/Library/LaunchAgents/eu.axitam.jira-tray.plist << EOF
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>eu.axitam.jira-tray</string>
    <key>ProgramArguments</key>
    <array>
        <string>$(which jira-tray)</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <true/>
</dict>
</plist>
EOF

launchctl load ~/Library/LaunchAgents/eu.axitam.jira-tray.plist
```

### Windows (Startup)

Create a shortcut to `jira-tray` in the Startup folder:

```powershell
$WshShell = New-Object -ComObject WScript.Shell
$Shortcut = $WshShell.CreateShortcut("$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup\JiraTray.lnk")
$Shortcut.TargetPath = (Get-Command jira-tray).Source
$Shortcut.Save()
```

Or use Task Scheduler for more control (restart on failure, etc.).

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Tray icon not visible (GNOME) | Install `gnome-shell-extension-appindicator` and enable it |
| Tray icon not visible (i3/Sway) | Add a tray to your bar config (`tray_output` in i3bar) |
| `KeyError: 'JIRA_API_TOKEN'` | Set the env var or use `env_file` in config |
| `401 Unauthorized` | Check `auth_mode` matches your Jira type (Cloud vs Server) |
| `403 Forbidden` | Token lacks permissions or JQL references inaccessible projects |
| Priority icons all ⚪ | Your Jira uses custom priority names — check mapping in source |
| `str.removeprefix` error | Upgrade to Python 3.10+ |

## License

Copyright 1988-2026 Axitam SRL

Lead developer: Regis GILOT — regis.gilot@axitam.eu

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.
