---
title: "Sync Claude Code Config Across Multiple Machines with chezmoi: Complete Guide"
date: 2026-04-01T22:00:00+08:00
draft: false
summary: "Change once, sync everywhere. Manage Claude Code configuration and Skills with chezmoi — auto-update, machine-specific templates, one-command setup for new machines."
tags: ["Claude Code", "chezmoi", "dotfiles", "productivity"]
author: "Xiaobei"
cover:
  image: "cover.png"
  alt: "Sync Claude Code config with chezmoi"
  relative: true
---

# Sync Claude Code Config Across Multiple Machines with chezmoi: Complete Guide

> Change once, sync everywhere. One command for new machines.

---

## 1. The Problem: Multi-Machine Config Sync Pain

I have 3 Macs: one MacBook and two Mac Minis. Each has Claude Code installed with a bunch of Skills — gstack, humanizer, marketing skills, etc. Might add cloud servers later.

After using them for a while, problems emerged:

- Skills installed on MacBook aren't on the Mini
- Changed `CLAUDE.md` global config, other machines still have the old version
- Setting up a new machine from scratch takes at least 30 minutes
- Skills updated, have to `git pull` on every machine manually

**What I need**:

```
✅ Change once, all machines sync automatically
✅ Skills auto-update
✅ One command to set up new machines
✅ Different machines can have different configs (e.g., server needs extra skills)
```

---

## 2. Why chezmoi

There are several dotfiles management tools out there. Here's my comparison:

| Approach | Pros | Cons |
|----------|------|------|
| **Manual copy** | Simple | Tedious, error-prone, unsustainable |
| **Bare Git repo** | Pure Git, no deps | Complex setup, merge conflicts |
| **Mackup** | macOS friendly | No custom paths, inflexible |
| **chezmoi** | Templates, encryption, cross-platform, external sources | Learning curve |

I chose **chezmoi** because:

1. **Template support** — Different machines can render different configs
2. **External sources** — Auto-pull skills from GitHub with periodic updates
3. **Encryption support** — API keys can sync securely (not covered here)
4. **One-command init** — `chezmoi init --apply` for new machines

---

## 3. Prerequisites

### 3.1 Install chezmoi

**macOS**:

```bash
brew install chezmoi
```

**Linux**:

```bash
sh -c "$(curl -fsLS get.chezmoi.io)"
```

**Verify installation**:

```bash
chezmoi --version
# chezmoi version 2.70.0
```

### 3.2 Create GitHub Repository

Create a repository on GitHub, e.g., `dotfiles`. Can be public or private.

**Tip**: If your `CLAUDE.md` contains no sensitive info, a public repo is more convenient (no SSH setup needed).

### 3.3 Initialize chezmoi

```bash
# Initialize (creates ~/.local/share/chezmoi directory)
chezmoi init

# Enter chezmoi directory
cd ~/.local/share/chezmoi

# Link remote repository (replace with yours)
git remote add origin https://github.com/yourusername/dotfiles.git
```

---

## 4. Understanding Claude Code Config Structure

Before diving in, understand what's in `~/.claude` and what needs syncing:

![Directory Structure](01-directory-structure.png)

```
~/.claude/
├── CLAUDE.md          ✅ Sync - Global instructions
├── settings.json      ✅ Sync - Plugin and permission config
├── skills/            ✅ Sync - Skill packages
│   ├── gstack/
│   ├── humanizer/
│   └── marketingskills/
├── projects/          ❌ Don't sync - Different per machine
├── sessions/          ❌ Don't sync - Runtime data
├── history.jsonl      ❌ Don't sync - Chat history, too large
├── cache/             ❌ Don't sync - Cache
└── debug/             ❌ Don't sync - Debug logs
```

**Three sources of Skills**:

| Source | Example | Sync Method |
|--------|---------|-------------|
| **Marketplace** | obsidian, frontend-design | Stored in settings.json, auto-installed by Claude Code |
| **Public GitHub** | gstack, humanizer | Pull via chezmoi external |
| **Private/custom** | Your own skills | chezmoi add directly, or private repo |

---

## 5. Basic Sync: CLAUDE.md and settings.json

### 5.1 Add Config Files

```bash
# Add global instructions file
chezmoi add ~/.claude/CLAUDE.md

# Add plugin config
chezmoi add ~/.claude/settings.json
```

**chezmoi naming conventions**:

After running the above, you'll see a `private_dot_claude` folder in the chezmoi directory. This is chezmoi's naming convention:

- `dot_` prefix = dot-prefixed files/dirs (`.claude` → `dot_claude`)
- `private_` prefix = file permissions 600 (only owner can read/write)

So `~/.claude` becomes `private_dot_claude`.

### 5.2 View Added Files

```bash
ls ~/.local/share/chezmoi/private_dot_claude/
# CLAUDE.md  settings.json
```

### 5.3 Verify

```bash
# See what chezmoi manages
chezmoi managed
# /Users/yourusername/.claude/CLAUDE.md
# /Users/yourusername/.claude/settings.json
```

---

## 6. Configure .chezmoiignore

To avoid adding unwanted files, create `.chezmoiignore`:

```bash
cat > ~/.local/share/chezmoi/.chezmoiignore << 'EOF'
# Claude Code runtime data, don't sync
.claude/sessions
.claude/cache
.claude/history.jsonl
.claude/debug
.claude/telemetry
.claude/backups
.claude/projects
.claude/file-history
EOF
```

This way, even if you accidentally run `chezmoi add ~/.claude`, these directories are ignored.

---

## 7. Managing GitHub Skills with External

This is the most valuable part — letting chezmoi auto-pull skills from GitHub with periodic updates.

![Workflow](02-workflow.png)

### 7.1 Create External Config

```bash
cat > ~/.local/share/chezmoi/.chezmoiexternal.toml << 'EOF'
# Claude Code Skills - Auto-install and update from GitHub
# chezmoi apply checks for updates (after refreshPeriod)
# Force refresh: chezmoi apply --refresh-externals

# GStack - Garry Tan's engineering team simulation
[".claude/skills/gstack"]
type = "archive"
url = "https://github.com/garrytan/gstack/archive/main.tar.gz"
stripComponents = 1
refreshPeriod = "168h"  # Check weekly

# Humanizer - Remove AI-generated traces
[".claude/skills/humanizer"]
type = "archive"
url = "https://github.com/blader/humanizer/archive/main.tar.gz"
stripComponents = 1
refreshPeriod = "168h"

# Marketing Skills - CRO/SEO/copywriting/growth
[".claude/skills/marketingskills"]
type = "archive"
url = "https://github.com/coreyhaines31/marketingskills/archive/main.tar.gz"
stripComponents = 1
refreshPeriod = "168h"

# Frontend Slides - Create animated HTML presentations
[".claude/skills/frontend-slides"]
type = "archive"
url = "https://github.com/zarazhangrui/frontend-slides/archive/main.tar.gz"
stripComponents = 1
refreshPeriod = "168h"
EOF
```

### 7.2 Config Explanation

| Field | Meaning |
|-------|---------|
| `[".claude/skills/xxx"]` | Target path (relative to home) |
| `type = "archive"` | Download tar.gz and extract |
| `url` | GitHub repo archive download URL |
| `stripComponents = 1` | Strip top-level directory when extracting (GitHub archives include one) |
| `refreshPeriod = "168h"` | 168 hours = 7 days, checks for updates after this period |

### 7.3 Apply Config

```bash
chezmoi apply
```

First run downloads all skills, wait a moment.

### 7.4 Verify

```bash
ls ~/.claude/skills/
# frontend-slides  gstack  humanizer  marketingskills
```

---

## 8. Verify Complete Config

Before committing, verify everything works:

```bash
# 1. See files managed by chezmoi
chezmoi managed

# 2. View diff (local vs chezmoi source)
chezmoi diff

# 3. Verify skills directory
ls ~/.claude/skills/

# 4. Verify CLAUDE.md
head -10 ~/.claude/CLAUDE.md

# 5. Verify settings.json
cat ~/.claude/settings.json
```

---

## 9. Commit and Push

```bash
cd ~/.local/share/chezmoi

# Add all files
git add .

# Commit
git commit -m "feat: add Claude Code config and skills"

# Push
git push -u origin main
```

---

## 10. Advanced: Different Configs for Different Machines

If your server needs extra skills that your laptop doesn't, use templates.

### 10.1 Check Each Machine's Hostname

Run on each machine:

```bash
hostname
```

Note them down, e.g.:
- MacBook: `my-macbook.local`
- Mac Mini 1: `my-mini-1.local`
- Mac Mini 2: `my-mini-2.local`

### 10.2 Convert External Config to Template

```bash
# Rename to template file
mv ~/.local/share/chezmoi/.chezmoiexternal.toml \
   ~/.local/share/chezmoi/.chezmoiexternal.toml.tmpl
```

### 10.3 Edit Template

```bash
cat > ~/.local/share/chezmoi/.chezmoiexternal.toml.tmpl << 'EOF'
# ============================================
# Skills for all machines
# ============================================

[".claude/skills/humanizer"]
type = "archive"
url = "https://github.com/blader/humanizer/archive/main.tar.gz"
stripComponents = 1
refreshPeriod = "168h"

[".claude/skills/gstack"]
type = "archive"
url = "https://github.com/garrytan/gstack/archive/main.tar.gz"
stripComponents = 1
refreshPeriod = "168h"

# ============================================
# Skills only for Mini 1 (main dev machine)
# ============================================
{{ if eq .chezmoi.hostname "my-mini-1.local" }}
[".claude/skills/frontend-slides"]
type = "archive"
url = "https://github.com/zarazhangrui/frontend-slides/archive/main.tar.gz"
stripComponents = 1
refreshPeriod = "168h"
{{ end }}

# ============================================
# Skills only for Mini 2 (server)
# ============================================
{{ if eq .chezmoi.hostname "my-mini-2.local" }}
[".claude/skills/server-monitoring"]
type = "archive"
url = "https://github.com/xxx/server-monitoring/archive/main.tar.gz"
stripComponents = 1
refreshPeriod = "168h"
{{ end }}
EOF
```

**Template syntax**:

- `{{ if eq .chezmoi.hostname "xxx" }}` — If hostname equals xxx
- `{{ end }}` — End condition
- Can also use `{{ if contains .chezmoi.hostname "mini" }}` for fuzzy matching

### 10.4 Verify Template

```bash
# See what renders for current machine
chezmoi execute-template < ~/.local/share/chezmoi/.chezmoiexternal.toml.tmpl
```

---

## 11. One-Command Setup for New Machines

**This is the best part** — new machines need just two steps:

### Public Repository

```bash
# 1. Install chezmoi
brew install chezmoi  # or sh -c "$(curl -fsLS get.chezmoi.io)"

# 2. One-command init (replace with your GitHub username)
chezmoi init yourusername/dotfiles --apply
```

### Private Repository

```bash
# Need SSH URL
chezmoi init git@github.com:yourusername/dotfiles.git --apply
```

**After completion, you automatically get**:

- ✅ `~/.claude/CLAUDE.md` global config
- ✅ `~/.claude/settings.json` plugin settings
- ✅ All configured GitHub skills
- ✅ Marketplace plugins install when Claude Code first launches

---

## 12. Daily Maintenance Commands

```bash
# Check status (local vs chezmoi source)
chezmoi status

# View diff
chezmoi diff

# Apply updates (source → target)
chezmoi apply

# Pull from remote + apply
chezmoi update

# Force refresh all external skills
chezmoi apply --refresh-externals

# Add new file to chezmoi
chezmoi add ~/.claude/newfile

# Edit file in chezmoi (opens editor)
chezmoi edit ~/.claude/CLAUDE.md

# Apply after editing
chezmoi apply
```

---

## 13. FAQ

### Q1: `chezmoi apply` permission error

**Cause**: Target file is locked by Claude Code

**Solution**: Close Claude Code and retry

```bash
# Or force overwrite
chezmoi apply --force
```

### Q2: External skills not updating

**Cause**: Haven't reached `refreshPeriod` (default 7 days)

**Solution**: Force refresh

```bash
chezmoi apply --refresh-externals
```

### Q3: Can't pull private repository

**Cause**: Using HTTPS without authentication

**Solution**: Use SSH

```bash
chezmoi init git@github.com:yourusername/dotfiles.git --apply
```

### Q4: Hostname not matching

**Cause**: Some systems have `.local` suffix, some don't

**Solution**: Use `contains` instead of `eq`

```
{{ if contains .chezmoi.hostname "mini" }}
...
{{ end }}
```

### Q5: Changed config locally, how to sync back to chezmoi?

**Method 1**: Re-add

```bash
chezmoi add ~/.claude/CLAUDE.md
```

**Method 2**: Edit chezmoi source directly

```bash
chezmoi edit ~/.claude/CLAUDE.md
# After editing
chezmoi apply
```

### Q6: Marketplace plugins not auto-installing

**This is expected**. Marketplace plugins (like obsidian, frontend-design) are stored in `settings.json`, but actual plugins download when Claude Code launches.

chezmoi only syncs `settings.json`; plugin download is Claude Code's job.

---

## 14. Complete Config Reference

### Directory Structure

```
~/.local/share/chezmoi/
├── .chezmoiignore              # Ignore rules
├── .chezmoiexternal.toml.tmpl  # External skills config (template)
├── private_dot_claude/
│   ├── CLAUDE.md               # Global instructions
│   └── settings.json           # Plugin config
└── README.md                   # Optional: documentation
```

### settings.json Example

```json
{
  "enabledPlugins": {
    "obsidian@obsidian-skills": true,
    "frontend-design@claude-plugins-official": true
  },
  "extraKnownMarketplaces": {
    "claude-plugins-official": {
      "source": {
        "source": "github",
        "repo": "anthropics/claude-plugins-official"
      }
    }
  }
}
```

### CLAUDE.md Example

```markdown
# Python Web Backend Rules

## Environment
- Use uv for dependency management
- Use .venv/ virtual environment
- Run commands with uv run

## Project Structure
app/
  __init__.py
  routes.py
  models.py
main.py
tests/

## Common Commands
- Start server: uv run uvicorn app.main:app --reload
- Run tests: uv run pytest
- Format: uv run ruff format .
```

---

## 15. Summary

| Pain Point | Solution |
|------------|----------|
| Multi-machine config inconsistency | chezmoi unified management, Git sync |
| Manual skill installation tedious | External auto-pulls from GitHub |
| Different machines need different configs | Template syntax with hostname conditions |
| New machine setup time-consuming | `chezmoi init --apply` one command |
| Manual skill updates | refreshPeriod auto-checks |

**One-line summary**:

> Dotfiles manager + GitHub = Ultimate sync solution for Claude Code config

---

## Appendix: Recommended Claude Code Skills

| Skill | Purpose | GitHub |
|-------|---------|--------|
| gstack | Simulate full engineering team (CEO/Designer/QA) | garrytan/gstack |
| humanizer | Remove AI-generated traces | blader/humanizer |
| marketingskills | CRO/SEO/copywriting/growth (34 skills) | coreyhaines31/marketingskills |
| frontend-slides | Create animated HTML presentations | zarazhangrui/frontend-slides |
| baoyu-skills | Baoyu's skill pack (translation, WeChat publishing, etc.) | JimLiu/baoyu-skills |

---

## Beyond Claude Code

chezmoi can manage far more than Claude Code config. The same approach works for:

- **zshrc / bashrc** — Your shell config
- **git config** — Global Git settings
- **SSH config** — Server connection config
- **Vim / Neovim** — Editor config
- **tmux** — Terminal multiplexer config
- **Any other dotfiles**

Once you start using chezmoi, you'll never go back to manually copying configs.

---

## The Lazy Way

If you don't want to configure step by step, just throw this article to your Claude Code or OpenClaw and let AI handle it:

> "Help me configure chezmoi following this tutorial to sync my Claude Code config to GitHub."

AI will help you create the repo, add configs, set up externals — you just need to confirm at the end.

---

*If this article helped you, feel free to share it with friends who also use Claude Code.*
