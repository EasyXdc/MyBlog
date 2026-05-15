---
title: "My macOS CLI Toolbox"
published: 2026-05-15
description: "A roundup of the command-line tools I use daily — from file search and port management to email and iMessage, the terminal covers almost everything I need."
image: ""
tags: [macOS, CLI, DevTools]
category: "Tools"
draft: false
lang: ""
---

After a few years on macOS, I've accumulated a sizable collection of CLI tools. Some are daily essentials, others are occasional lifesavers. This post is both a roundup and a personal cheat sheet.

## File & Search

### ripgrep (rg)

If I could only keep one search tool, it'd be rg. Much faster than grep, respects `.gitignore` by default, supports regex, and ships with colorized output.

```bash
rg "TODO" src/
rg -l "function main"   # Only output filenames
```

### lsd

A modern replacement for `ls` with icons, colors, and tree view.

```bash
lsd -la
lsd --tree --depth 2
```

I added `alias ls='lsd'` in `.zshrc` for a seamless swap.

## Development & Git

### gh

GitHub's official CLI. Create PRs, manage issues, and publish releases — all from the terminal, no browser needed.

```bash
gh pr create --fill
gh issue list --label bug
gh release create v1.0.0
```

### hyperfine

Command-line benchmarking tool. When optimizing, it's far more reliable than manual timing.

```bash
hyperfine 'ports' 'lsof -i -P'
hyperfine --warmup 3 './target/release/ports'
```

### Claude Code

Anthropic's AI coding assistant CLI. Write, edit, and run code through a conversational interface right in the terminal.

```bash
claude "Help me build an HTTP server"
```

It's become indispensable in my daily workflow.

## Ports & Networking

### Port Whisperer (ports-rs)

A tool I wrote myself — check port usage, kill processes, view logs, and verify port availability, all in one place.

```bash
ports              # Scan dev ports
ports 3000         # Port details
ports check 3000   # Pre-launch check
ports kill 3000    # Kill occupant
```

See my [intro post](/posts/port-whisperer) for more.

### cloudflared

Cloudflare Tunnel client. Expose local services to the public internet securely — no open ports, no firewall config.

```bash
cloudflared tunnel --url http://localhost:3000
```

Perfect for sharing local demos.

## System Info

### fastfetch

A faster alternative to `neofetch`. Instant system info in the terminal with ASCII art.

```bash
fastfetch
```

Essential for desktop screenshot flexing.

## Multimedia

### ffmpeg

The swiss-army knife of audio/video processing. Convert formats, trim, merge, extract frames — if you can think of it, ffmpeg can probably do it.

```bash
ffmpeg -i input.mov output.mp4
ffmpeg -i video.mp4 -vn -acodec copy audio.aac
ffmpeg -ss 00:01:30 -i video.mp4 -t 10 -c copy clip.mp4
```

### tesseract

Open-source OCR engine. Extract text from images.

```bash
tesseract screenshot.png output
```

Great for automation pipelines — pair it with ffmpeg frame extraction for video subtitle workflows.

### ncmdump

Convert NetEase Cloud Music `.ncm` files to mp3/flac. One command to unlock.

### gifgrep

Search text inside GIF animations — sounds niche, but genuinely useful when working with product screenshot recordings.

## Daily Utilities

### himalaya

Terminal email client. Supports SMTP/IMAP, can send/receive mail, manage folders, and even PGP.

```bash
himalaya list
himalaya read 42
himalaya send
```

### imsg

Send and receive iMessage and SMS from the terminal. Handy for scripted notifications or when you're too lazy to switch to Messages.

```bash
imsg send +1234567890 "Deploy done"
imsg read
```

### remindctl

Manage Apple Reminders from the terminal. Add, list, and complete tasks without opening Reminders.app.

```bash
remindctl add "Review PR #42"
remindctl list
remindctl complete 1
```

### sag

ElevenLabs' command-line TTS tool. Similar usage to macOS `say`, but with much better voice quality.

```bash
sag "Build passed, ship it"
sag --model eleven_flash_v2_5 "Quick heads up"
```

### autossh

Auto-reconnecting SSH. Keeps remote sessions alive when the network is flaky.

```bash
autossh -M 0 -o "ServerAliveInterval 30" user@host
```

### peekaboo

macOS window management CLI. Script window positions and sizes.

### obsidian-cli

Operate Obsidian notes from the terminal — search, create, move, delete. Fits nicely into automation scripts.

### goplaces

Command-line wrapper for Google Places API. Search and parse location info.

### summarize

Summarize web pages or YouTube videos, output directly in the terminal.

```bash
summarize https://www.youtube.com/watch?v=xxx
```

### eightctl

Terminal control for Eight Sleep smart mattresses. Adjust temperature, check status. Yes, even the mattress can be terminal-controlled.

### clawhub

ClawHub CLI client. Manage AI agent configs and workflows.

## Containers & Deployment

### docker & kubectl

Standard setup. Local containers and K8s cluster management.

## Language Runtimes

- **Go** 1.26 — Primary language for CLI tools
- **Rust** — Port Whisperer is written in it
- **Python** 3.12 — Scripts and data processing
- **Node.js** 25 — Frontend projects and Claude Code dependency

## Final Thoughts

Most of these tools install with a single `brew install`. But what really changes your workflow isn't the tools themselves — it's the habit of using them instead of leaving the terminal. When you realize 90% of what you need can be done in one window, the productivity gain is real.

Got a CLI tool I should know about? Share it in the comments.