---
title: "My macOS Dev Environment Setup Guide"
published: 2026-05-15
description: "From shell to language runtimes, from package management to proxy networking — a complete walkthrough of how my Mac dev environment is built and managed."
image: ""
tags: [macOS, DevEnv, Setup]
category: "Setup"
draft: false
lang: ""
---

After switching to Mac, my dev environment went through several iterations before settling into its current form. Writing this down as a personal reference and a guide for anyone setting up their own environment.

## Shell: Oh My Zsh + Dracula

The terminal is a developer's home base — get the shell comfortable first.

```bash
# Install Oh My Zsh
sh "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Dracula theme
curl -L https://raw.githubusercontent.com/dracula/zsh/master/dracula.zsh-theme -o ~/.oh-my-zsh/themes/dracula.zsh-theme
```

Key `~/.zshrc` config:

```bash
ZSH_THEME="dracula"
plugins=(git zsh-autosuggestions zsh-completions)

# Syntax highlighting (manual install required)
source ~/.zsh/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

Three plugins, each with a job:

- **zsh-autosuggestions** — Auto-complete from history, gray suggestions, press → to accept
- **zsh-completions** — Additional completion rules
- **zsh-syntax-highlighting** — Commands turn green (valid) or red (invalid) as you type

I use iTerm2 as the terminal emulator, with Shell Integration for directory jumping and command history navigation.

## Package Management: Homebrew First

On macOS, Homebrew is virtually the only choice. Most tools in one command:

```bash
brew install ripgrep lsd ffmpeg gh hyperfine himalaya cloudflared
```

I added an alias for easy daily updates:

```bash
alias bup="brew update && brew upgrade && brew cleanup"
```

My own tools are distributed through a Homebrew tap too:

```bash
brew install EasyXdc/tap/ports-rs
```

Plenty of third-party taps as well — `steipete/tap` for gifgrep, imsg, peekaboo, and other small utilities.

## Language Runtimes: Each Its Own Version Manager

No mixing — every language has its own management tool.

### Node.js — Volta

Volta handles version management + global package management. Faster than nvm, instant version switching:

```bash
curl https://get.volta.sh | bash
volta install node@25
volta install pnpm
```

Global packages managed by Volta as well, currently installed:

- `claude-code` — Anthropic AI coding assistant
- `codex` — OpenAI Codex CLI
- `ccline` — Code completion tool
- `openclaw` / `openclaw-qqbot` — QQ bots
- `pnpm` — Frontend package manager
- `mysql-mcp-server` — MCP database service

### Java — SDKMAN

```bash
curl -s "https://get.sdkman.io" | bash
sdk install java 21.0.10-amzn
sdk install java 17.0.18-amzn
sdk default java 21.0.10-amzn
```

Switch Java versions per project. `sdk use java 17...` for temporary, `sdk default` for default.

### Rust — rustup

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Stable toolchain, primary language for CLI tools. Cargo also manages dev tools like rust-analyzer and clippy.

### Python — Homebrew + Mamba

System Python stays untouched. Homebrew installs Python 3.12. Conda/Mamba uses lazy loading to avoid slowing down shell startup:

```bash
# ~/.zshrc
_mamba_lazy_init() {
  unset -f mamba conda _mamba_lazy_init
  eval "$(/opt/homebrew/bin/mamba shell hook --shell zsh 2>/dev/null)"
  eval "$(/opt/homebrew/bin/conda shell.zsh hook 2>/dev/null)"
}

mamba() { _mamba_lazy_init; mamba "$@"; }
conda() { _mamba_lazy_init; conda "$@"; }
```

Initialization only happens on first call — no drag on normal shell startup.

### Go — Homebrew

```bash
brew install go
```

Currently 1.26, mainly used for small tools and backend services.

## Git Config

```gitconfig
[user]
    name = EasyXdc
    email = 1330064686@qq.com
[credential]
    helper = !gh auth git-credential
```

Using `gh auth` for Git credential management — no separate SSH key or token needed for GitHub push/pull over HTTPS. SSH still uses keys, but HTTPS operations go through gh credential for a smoother experience.

SSH config stores common server aliases — `ssh Company` for direct connection, plus RemoteForward to proxy local ports to remote servers.

## Networking: Clash + Cloudflare WARP

Proxy via Clash, config files at `~/.config/clash/`, rules and subscriptions self-managed.

Cloudflare WARP as backup — more convenient in some scenarios (e.g. accessing Cloudflare-protected sites).

The two complement each other: Clash handles everyday proxy needs, WARP handles specific network issues.

## Editor: VS Code + Cursor

Primary editor is VS Code with ~80 extensions, mainly in these categories:

- **AI Assistants**: Claude Code, CodeGeeX
- **Database**: Database Client JDBC
- **Development**: ESLint, Git History, LeetCode Helper
- **Theme/Icons**: One Dark Italic, Great Icons
- **Utilities**: CodeSnap (screenshots), Comment Translate, JSON Tools, Excel Viewer

Cursor as an AI-first editor for occasional use, suited for heavy AI-assisted workflows.

## Docker + K8s

Docker Desktop provides the container runtime, kubectl manages clusters. Docker CLI completion added at the end of `.zshrc`.

## System Utilities

A few tools that make macOS more pleasant:

- **CleanMyMac** — Cleanup and system maintenance
- **iStat Menus** — Real-time system stats in the menu bar (CPU, memory, network, temperature)
- **Parallels Desktop** — Windows/Linux VMs, occasional testing use

## File Structure Habits

```
~/
├── .config/          # Tool configs (clash, gh, neofetch, uv...)
├── .oh-my-zsh/       # Shell config and plugins
├── .volta/           # Node versions and global packages
├── .sdkman/          # Java version management
├── .rustup/          # Rust toolchain
├── .ssh/config       # SSH aliases and port forwarding
├── Desktop/
│   └── PersonalDocument/
│       └── MyCode/   # Code projects live here
└── Downloads/        # Temporary files
```

Config files mostly live under `~/.config/`, following XDG conventions. A few legacy tools (like Oh My Zsh) still use `~/`.

## Core Design Principles

Looking back, the guiding principles of this setup are:

1. **Each language manages its own versions** — Volta, SDKMAN, rustup each do their own thing, no interference
2. **Lazy loading over eager loading** — Mamba lazy init, SDKMAN conditional loading, shell stays fast
3. **Homebrew unifies CLI tooling** — One `brew install` does it, upgrades are easy
4. **Centralized config management** — `~/.zshrc` is the entry point, other configs at their standard paths
5. **Layered networking** — Clash for proxy, WARP for specific networks, no mixing

There's no "best setup" — only "the setup that fits you best." Writing this down mainly so I don't forget when setting up a new machine, and hopefully it helps someone else too.