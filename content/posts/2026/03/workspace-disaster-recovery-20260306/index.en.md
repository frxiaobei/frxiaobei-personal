---
title: "Postmortem: How an Agent Wiped 11,583 Files in 3 Minutes"
date: 2026-03-06T10:00:00+08:00
draft: false
tags:
  - OpenClaw
  - Gemini
  - Postmortem
  - Disaster Recovery
  - Agent Safety
description: A real incident postmortem: Gemini ran rm -rf in the workspace root and 11,583 files disappeared. What failed, how we recovered, and the safeguards we put in place.
cover:
  image: 01-cover.png
---

It was late at night. I was staring at the terminal, and only one word was in my head: **done**.

This wasn’t a single broken file. The entire workspace root got wiped.

**11,583 files. Gone in 3 minutes.**

---

## What I Was Doing

I was running multiple tasks in parallel and delegated frontend work to a sub-agent for speed.

Gemini got a task under `projects/manga-character-platform/frontend`.

Then it executed `rm -rf ./*`.

The problem: it *thought* it had `cd`’d into the target directory. It hadn’t.

---

## Timeline

```
00:48:57  Gemini "cd" to target directory (or so it thought)
00:49:22  Ran rm -rf ./* (actually at workspace root)
00:50:03  Realized something was wrong, started "rebuilding" empty directories
00:51:29  Auto-sync pushed deletions to GitHub
```

From deletion to propagation: 4 minutes.

---

## I Was Also Part of the Problem

To be fair, I can’t blame Gemini alone.

**I cut corners.**

Project repos should never have lived under workspace. I put `projects/` there for convenience, thinking `.gitignore` would protect me.

It didn’t.

`.gitignore` controls Git tracking — not `rm`.

Once Gemini deleted the workspace, `projects/` went with it.

Yes, I restored repos from GitHub. But unpushed local changes and experiment branches were gone.

That one is on me.

---

## Root Causes

**1) `cd` wasn’t persistent**

In OpenClaw exec flows, `cd` in one command doesn’t change cwd for future commands unless explicitly set.

**2) No guardrails before destructive commands**

No forced `pwd`, no path allowlist, no deletion-size check before `rm -rf`.

**3) No circuit breaker in auto-sync**

Deletion got committed and pushed immediately, amplifying damage.

**4) Bad directory boundaries**

I mixed project code with workspace. That architectural shortcut raised blast radius.

---

## Recovery

No magic. Just disciplined execution:

1. **Stop the bleeding** — pause automation
2. **Rollback** — restore core files from pre-incident commit
3. **Rehydrate assets** — pull skills and projects from upstream repos
4. **Validate end-to-end flows** — not just file presence

The most annoying part was `baoyu-skills`: looked restored, but scripts were missing and publishing flow silently broke.

---

## What We Changed

**Implemented safeguards:**

- Force explicit `workdir` for task execution
- Require `pwd + allowlist + threshold` before destructive commands
- Add auto-sync circuit breaker (suspicious mass deletion blocks push)
- Document recovery SOP

**Reorganized directory layout:**

```text
~/.openclaw/workspace/          # Agent workspace (auto-synced)
├── AGENTS.md
├── MEMORY.md
├── SOUL.md
├── memory/
├── skills/
├── data/
├── config/
├── scripts/
├── branding/
├── assets/
└── articles -> Obsidian/

~/projects/                     # Code repos (independent git)
├── manga-platform-nextjs/
├── agent-swarm/
├── elyfinn-website/
└── ...

~/Library/.../Obsidian/         # Knowledge base (iCloud)
├── projects/
├── articles/
└── knowledge/
```

**Simple rule:**

- **workspace** = the agent’s home (auto-sync enabled), no code repos
- **projects** = code home, isolated git repos
- **Obsidian** = human knowledge home, linked in when needed

Separate boundaries reduce blast radius.

---

## Final Takeaway

I used to think backup and safeguards were “nice to have when there’s time.”

Not anymore.

Automation makes you fast. It also makes failure fast.

If you’re running agent automation in production:

- Add guardrails around destructive operations
- Add circuit breakers to sync pipelines
- Keep code repos outside the automation workspace

Don’t wait for 2 a.m. to learn this the hard way.
