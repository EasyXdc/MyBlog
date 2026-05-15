---
title: "Port Whisperer: A Rust-Built Port Management CLI"
published: 2026-05-15
description: "When your local dev environment gets crowded — port conflicts, zombie processes, can't find who's holding which port... Port Whisperer was built to solve exactly that."
image: ""
tags: [Rust, CLI, DevTools]
category: "Projects"
draft: false
lang: ""
---

When your local dev environment gets "crowded" — port conflicts, zombie processes, can't find who's holding which port — it's a headache. So I built [Port Whisperer](https://github.com/EasyXdc/PortsWhisper-Rust) in Rust, a CLI tool focused on local port management.

## What It Does

Port Whisperer's core idea is simple: **give you a clear view of local port status at a glance, and let you quickly locate and resolve issues**.

Typical daily usage looks like this:

- `ports` — The dev environment feels a bit "crowded", want a quick scan
- `ports 3000` — Frontend won't start, need to find the process occupying the port immediately
- `ports check 3000 5173 8080` — Before launching multiple services, confirm ports are free
- `ports logs 3000 --grep error` — Fish out useful signals from noisy logs
- `ports kill --signal SIGINT 3000` — Graceful stop instead of a force kill

It provides two commands: `ports` and `whoisonport`. The latter is just an alias for `ports <port>`, a bit more intuitive.

## Installation

The easiest way is via npm:

```bash
npm i -g ports-rs
```

macOS users can also use Homebrew:

```bash
brew install EasyXdc/tap/ports-rs
```

If you prefer Cargo, installing from source is straightforward:

```bash
git clone https://github.com/EasyXdc/PortsWhisper-Rust.git
cd port-whisperer-rust
cargo install --path .
```

Pre-compiled binaries are also available on the GitHub Releases page.

## Feature Overview

### View Dev Ports

```bash
ports
```

By default, only shows dev-related processes (Node.js, Python, Docker, frontend dev servers, etc.), so you won't be overwhelmed by system-level port info. Want everything? Add `--all`.

### View Specific Port Details

```bash
ports 3000
```

Displays process name, PID, health status, framework, memory usage, uptime, working directory, project name, Git branch, process tree, and more. If there's a listener on that port, you can confirm whether to terminate it directly.

### Pre-launch Port Check

```bash
ports check 3000 5173 8080
```

Exit code is 0 when all ports are free, 1 if any is occupied — great for scripting.

### Open Local Service in Browser

```bash
ports open 3000
```

### View Logs

```bash
ports logs 3000 --grep error --since 10m
```

Port Whisperer auto-discovers redirected stdout/stderr files, `.log` directories, `nohup.out`, and system logs (macOS unified log, Linux journalctl). `--grep` for filtering, `--since` for time range, `--err` to prioritize stderr.

### Clean Zombie Processes

```bash
ports clean
```

One command to clean up orphaned ports and zombie processes.

### Watch Port Changes in Real-time

```bash
ports watch
```

## Performance

Local release mode testing (`hyperfine --warmup 3`):

| Command | Time |
| --- | ---:|
| `ports` | 99.8 ms |
| `ports ps` | 96.2 ms |
| `ports 3000` | 43.9 ms |

Compared to the first-phase baseline, `ports` dropped from 116ms to 100ms, `ports 3000` from 54ms to 44ms. Actual numbers vary by machine and load.

## Tech Stack

| Layer | Technology |
| --- | --- |
| Core CLI | Rust |
| Distribution | npm + Node.js install script |
| macOS Process Discovery | `lsof` + `ps` + `log` |
| Linux Process Discovery | `/proc` + `ss`/`netstat` + `ps` + `journalctl` |
| Windows Process Discovery | PowerShell + `Get-NetTCPConnection`/`Get-Process` + `taskkill` |

## Acknowledgments

This project is a Rust rewrite of [LarsenCundric/port-whisperer](https://github.com/LarsenCundric/port-whisperer), preserving the core CLI workflow and user behavior while re-implementing the underlying layer in Rust.