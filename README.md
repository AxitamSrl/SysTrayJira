# Jira System Tray

System tray app that displays your Jira issues grouped by custom JQL queries.

![Python](https://img.shields.io/badge/python-3.10+-blue)
![License](https://img.shields.io/badge/license-Apache%202.0-blue)

## Features

- 📋 Custom JQL groups with `active` toggle
- 🔴🟠🟡🟢🔵 Priority indicators per issue
- 🔄 Auto-refresh + manual refresh
- 🔐 Multiple auth modes (Basic, Bearer, PAT)
- 📁 Bash-style `.env` file support (with `export`)
- 🖱️ Click to open issue in browser
- ♻️ Reload config without restart

## Install

```bash
pip install git+https://github.com/YOUR_USER/jira-tray.git
```

Or clone and install locally:

```bash
git clone https://github.com/YOUR_USER/jira-tray.git
cd jira-tray
pip install .
```

## Quick Start

```bash
# Generate default config
jira-tray --init

# Edit config
nano ~/.config/sysTrayJira/config.yaml

# Set your token
export JIRA_API_TOKEN="your-token"

# Run
jira-tray
```

## Configuration

Config file: `~/.config/sysTrayJira/config.yaml`

```yaml
jira_url: "https://your-jira-instance.com"
email: "your-email@example.com"
poll_interval: 300          # seconds
auto_refresh: true          # false = manual only

# icon: "~/.config/sysTrayJira/jira.png"  # optional custom icon
# env_file: "~/.env"                       # optional bash-style .env

auth_mode: "bearer"         # basic | bearer | pat
token_env: "JIRA_API_TOKEN" # env var name for the token

groups:
  - name: "📋 My Issues"
    jql: "assignee = currentUser() AND resolution = Unresolved"
    active: true
  - name: "🔥 Urgent"
    jql: "assignee = currentUser() AND priority in (Highest, High)"
    active: false
```

### Auth Modes

| Mode | Description | Uses |
|------|-------------|------|
| `basic` | Email + token (Atlassian Cloud) | `email` + `token_env` |
| `bearer` | Bearer token (Jira Server) | `token_env` only |
| `pat` | Personal Access Token (Jira DC) | `token_env` only |

## Run as systemd service

```bash
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

## License

Copyright 1988-2026 Axitam SRL

Lead developer: Regis GILOT — regis.gilot@axitam.eu

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.
