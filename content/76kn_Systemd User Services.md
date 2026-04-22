---
title: Systemd User Services & %t Runtime Variable
date: 2026-04-16 00:59:26
aliases: [systemd --user, user-level services, "%t specifier", XDG_RUNTIME_DIR]
tags: [linux/systemd, services, user-space, devops]
---

# Systemd User Services & the `%t` Runtime Variable

## Context
- Related: [[?]]
- Prerequisites: Basic systemd knowledge
- See also: `systemctl --user` docs, XDG Base Directory spec

## Overview

**User-level systemd services** allow non-root users to manage their own background daemons without sudo. Services are defined in `~/.config/systemd/user/` and controlled via `systemctl --user`.

The `%t` specifier is a **context-aware runtime directory variable**:
- **Root-level service** → `%t` = `/run` (global system runtime dir)
- **User-level service** → `%t` = `$XDG_RUNTIME_DIR` (e.g., `/run/user/1000`)

This enables secure socket-based IPC (e.g., `ssh-agent`) isolated per user.

---

## Quick Reference

### Configuration Locations

| Level | Path | Managed With |
|-------|------|-------------|
| System-wide | `/etc/systemd/system/` | `sudo systemctl` |
| **User-level** | `~/.config/systemd/user/` | `systemctl --user` |

### Essential Commands

```bash
systemctl --user daemon-reload
systemctl --user start <service>
systemctl --user stop <service>
systemctl --user enable --now <service>
systemctl --user status <service>
systemctl --user list-units --type=service
```

### Service File Template

`~/.config/systemd/user/myapp.service`:

```ini
[Unit]
Description=My Background Service
After=network.target

[Service]
Type=simple
ExecStart=/path/to/program
Restart=always
RestartSec=5

[Install]
WantedBy=default.target
```

---

## The `%t` Specifier

### Resolution Table

| Context | `%t` Value | Example |
|---------|-------------|---------|
| System service (root) | `/run` | `%t/my.socket` → `/run/my.socket` |
| **User service** | `$XDG_RUNTIME_DIR` | `%t/ssh-agent.socket` → `/run/user/1000/ssh-agent.socket` |

### Why This Matters

- `/run` is `root:root` — users cannot write there
- `$XDG_RUNTIME_DIR` is owned by the user — **isolated, no sudo**
- Same file works at both levels; `%t` adapts automatically

---

## Key Benefits

1. **No Sudo Required** — users manage their own daemons
2. **Per-User Isolation** — each user gets `/run/user/$UID`
3. **Auto Cleanup** — tmpfs, cleared on logout/reboot
4. **XDG Compliant** — follows base directory spec

---

## Common Use Cases

- `ssh-agent` socket
- GPG agent
- Syncthing (file sync)
- Rootless Docker / Podman
- Local dev servers (HTTP, DB)
- Mail indexing (`mbsync`, `notmuch`)

---

## Troubleshooting

### "Failed to connect to bus"

The user daemon isn't running:

```bash
systemctl --user daemon-reload
sudo loginctl enable-linger $USER
systemctl --user exit && systemctl --user
```

### Check runtime dir

```bash
echo $XDG_RUNTIME_DIR
ls -la $XDG_RUNTIME_DIR
```

---

## Related

- `systemd --user` — per-user systemd instance
- `XDG_RUNTIME_DIR` — user runtime base
- `loginctl enable-linger` — persist after logout
- Rootless containers
