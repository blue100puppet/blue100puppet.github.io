---
title: Systemd [Service] Section Options
date: 2026-04-16 01:10:27
aliases: [systemd service options, systemd service directives]
tags: [linux/systemd, services, configuration]
---

# Systemd `[Service]` Section — Complete Option Reference

## Context
- Related: [[76kn_Systemd User Services]]
- Prerequisites: Understand basic systemd unit structure
- See also: `man systemd.service`

## Overview

The `[Service]` section defines **how a service runs**: execution type, commands, environment, resource limits, security hardening, and lifecycle behavior.

---

## Quick Reference Tables

### Type & Execution

| Directive | Values | Notes |
|-----------|--------|-------|
| `Type=` | `simple` (default) | Process runs directly; started when ExecStart returns |
| | `exec` | Run and exit immediately |
| | `forking` | Daemon forks; systemd waits for fork |
| | `oneshot` | Run-to-completion; exit = done |
| | `notify` | Program calls sd_notify READY=1 |
| | `idle` | Delays until job queue empty |

### Command Directives

| Directive | Purpose |
|-----------|---------|
| `ExecStart=` | Primary start command (usually required) |
| `ExecStartPre=` | Pre-start setup (run sequentially, multiple allowed) |
| `ExecStartPost=` | Post-start actions |
| `ExecStop=` | Clean shutdown command |
| `ExecStopPost=` | Post-shutdown cleanup |
| `ExecReload=` | Config reload (called by `systemctl reload`) |

**Specifiers:** `$MAINPID` (main process PID), `%i` (instance name), `%t` (runtime dir), etc.

### Restart Policy

| Directive | Values | Default |
|-----------|--------|---------|
| `Restart=` | `no` / `on-success` / `on-failure` / `on-abnormal` / `on-abort` / `always` | `no` |
| `RestartSec=` | Seconds | `100ms` |
| `StartLimitBurst=` | Integer | `5` |
| `StartLimitIntervalSec=` | Seconds | `10s` |

**Common:** `Restart=on-failure` + `RestartSec=5`

### User / Environment

| Directive | Example | Notes |
|-----------|---------|-------|
| `User=` | `User=appuser` | Drop privileges |
| `Group=` | `Group=appuser` | Primary group |
| `SupplementaryGroups=` | `SupplementaryGroups=disk` | Extra groups |
| `Environment=` | `Environment="PORT=8080"` | Set variable (repeatable) |
| `EnvironmentFile=` | `EnvironmentFile=/etc/default/app` | Load KEY=val file |
| `WorkingDirectory=` | `WorkingDirectory=/var/lib/app` | Process pwd |

### Resource Limits

| Directive | Unit | Example |
|-----------|------|---------|
| `LimitNOFILE=` | N | `65535` |
| `MemoryLimit=` | B/K/M/G | `512M` |
| `CPUQuota=` | % | `80%` |
| `LimitNPROC=` | N | `4096` |

### Security Hardening

| Directive | Values | Effect |
|-----------|--------|--------|
| `PrivateDevices=` | `yes` | Hide /dev except minimal |
| `PrivateUsers=` | `yes` | Hide /etc/passwd |
| `ProtectHome=` | `read-only` | /home, /root read-only |
| `ProtectSystem=` | `strict` | Mount /usr, /boot read-only (or `full` immutable) |
| `NoNewPrivileges=` | `yes` | Block `execve` gaining privileges |
| `CapabilityBoundingSet=` | `CAP_NET_BIND_SERVICE` | Drop all but listed |

**Typical secure baseline:**
```ini
PrivateDevices=yes
PrivateUsers=yes
ProtectHome=read-only
ProtectSystem=strict
NoNewPrivileges=yes
```

### Logging

| Directive | Values |
|-----------|--------|
| `StandardOutput=` | `journal` (default), `syslog`, `kmsg`, `tty` |
| `StandardError=` | Same |
| `SyslogIdentifier=` | `"mydaemon"` |
| `LogLevelMax=` | `emerg`…`debug` |

**View logs:** `journalctl -u <service> -f`

### Dependencies

| Directive | Meaning |
|-----------|---------|
| `After=` | Start after these units |
| `Before=` | Start before these units |
| `Wants=` | Weak dependency; start if available, don't fail if missing |
| `Requires=` | Hard dependency; fail if not active |
| `BindsTo=` | Like Requires, but stop together |

---

## Complete Minimal Example

```ini
[Unit]
Description=My Background Process
After=network.target

[Service]
Type=simple
User=appuser
ExecStart=/usr/local/bin/myapp --config /etc/myapp.conf
Restart=on-failure
RestartSec=5
Environment="PORT=8080"
WorkingDirectory=/var/lib/myapp
SyslogIdentifier=myapp
StandardOutput=journal

[Install]
WantedBy=multi-user.target
```

---

## Gotchas & Tips

1. **`ExecStart` is required** for non-`oneshot` services
2. **Multiple `ExecStartPre=`** allowed; they run sequentially
3. **`%t` resolves per-context:** `/run` (system) vs `$XDG_RUNTIME_DIR` (user)
4. **Quote env with spaces:** `Environment="KEY=value with spaces"`
5. **Check effective config:** `systemctl show <service> -p <directive>`
6. **Verify syntax:** `systemd-analyze verify /path/to/file.service`
7. **User services:** place in `~/.config/systemd/user/`, manage with `systemctl --user`

---

## Full Directive List (Alphabetical)

`AmbientCapabilities=`, `CapabilityBoundingSet=`, `ConfigurationDirectoryMode=`, `DefaultDependencies=`, `Delegate=`, `DevicePolicy=`, `DynamicUser=`, `Environment=`, `EnvironmentFile=`, `ExecStart=`, `ExecStartPre=`, `ExecStartPost=`, `ExecStop=`, `ExecStopPost=`, `ExecReload=`, `Group=`, `IOSchedulingClass=`, `IOSchedulingPriority=`, `IPAddressDeny=`, `IPAddressAllow=`, `KeyringMode=`, `LockPersonality=`, `LogExtraFields=`, `LogLevelMax=`, `LogRateLimitBurst=`, `LogRateLimitIntervalSec=`, `MainPID=`, `MemoryDenyWriteExecute=`, `MemoryLimit=`, `MountAPIVFS=`, `NoNewPrivileges=`, `OOMScoreAdjust=`, `PAMName=`, `PermissionsStartOnly=`, `PrivateDevices=`, `PrivateMounts=`, `PrivateNetwork=`, `PrivateTmp=`, `PrivateUsers=`, `ProcSubset=`, `ProtectClock=`, `ProtectControlGroups=`, `ProtectHome=`, `ProtectHostname=`, `ProtectKernelLogs=`, `ProtectKernelModules=`, `ProtectKernelTunables=`, `ProtectSystem=`, `ReadOnlyPaths=`, `ReadWritePaths=`, `RemoveIPC=`, `Restart=`, `RestartForceExitStatus=`, `RestartSec=`, `RootDirectory=`, `RootImage=`, `RuntimeDirectory=`, `RuntimeDirectoryMode=`, `RuntimeDirectoryPreserve=`, `SecureBits=`, `SELinuxContext=`, `Slice=`, `SmackProcessLabel=`, `StandardError=`, `StandardInput=`, `StandardOutput=`, `StaticPUID=`, `SupplementaryGroups=`, `SystemCallArchitectures=`, `SystemCallErrorNumber=`, `SystemCallFilter=`, `SystemCallLog=`, `TimerSlackNSec=`, `TTYPath=`, `TTYReset=`, `TTYVHangup=`, `TTYVTDisallocate=`, `UMask=`, `User=`, `UtmpIdentifier=`, `WorkingDirectory=`

(See `man systemd.service` for details on each.)

---

## See Also

- `man systemd.service` — Service unit configuration
- `man systemd.exec` — Execution environment settings (many [Service] options live here)
- `man systemd.unit` — Common unit directives (After, Before, Wants, etc.)
- `man systemd.directive` — Alphabetical directive list
- `man systemctl` — Control systemd
