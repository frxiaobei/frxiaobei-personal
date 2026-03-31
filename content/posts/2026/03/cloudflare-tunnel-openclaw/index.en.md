---
title: "Running AI Agents at Home? One Tunnel, Mobile Access Anywhere"
date: 2026-03-13T23:00:00+08:00
draft: false
description: "No port forwarding, no servers, no nginx. Cloudflare Tunnel + OpenClaw for zero-cost remote access to your AI Agents."
tags: ["AI", "OpenClaw", "Cloudflare", "Self-hosted", "Tutorial"]
categories: ["Tech"]
cover:
  image: cover.png
---

> No port forwarding, no servers, no nginx. Cloudflare Tunnel + OpenClaw for zero-cost remote access to your AI Agents.

## The Problem

I run a team of AI Agents on my Mac Mini at home:

- 🤖 **Main Assistant** — Manages calendar, writes articles, analyzes data
- 🩺 **Health Manager** — Tracks medical reports and medications
- 📊 **Analyst** — Analyzes AI trending posts daily
- 💻 **Code Worker** — Writes code and opens PRs 24/7

The foundation is OpenClaw, an open-source AI Agent framework. It has a WebUI where you can see all Agent statuses, conversations, and task progress.

But here's the problem — **WebUI runs on localhost:18789, inaccessible once you leave home.**

## Traditional Solutions Suck

Remote access to local services? Plenty of traditional options:

- **Public IP + Port Forwarding** — ISP might not provide it, security risks
- **Tunneling (frp/ngrok)** — Need a relay server, complex config
- **VPN** — Set up WireGuard/OpenVPN, drains phone battery
- **Tailscale** — Good, but I wanted something simpler

What I wanted was simple: **Open a URL on my phone, see what my AI Agents at home are doing.**

## Cloudflare Tunnel: The Zero-Cost Answer

Cloudflare Tunnel (formerly Argo Tunnel) works like this:

Your machine initiates an encrypted tunnel to Cloudflare → Cloudflare reverse-proxies traffic to your local service → External access via domain.

**No public IP needed. No ports to open. No relay server. Free.**

The entire setup:

```bash
# 1. Install cloudflared
brew install cloudflared

# 2. Login to Cloudflare
cloudflared tunnel login

# 3. Create tunnel
cloudflared tunnel create my-tunnel

# 4. Configure routing
# ~/.cloudflared/config.yml
tunnel: my-tunnel
credentials-file: ~/.cloudflared/<tunnel-id>.json

ingress:
  - hostname: ai.yourdomain.com
    service: http://127.0.0.1:18789
  - service: http_status:404

# 5. DNS routing
cloudflared tunnel route dns my-tunnel ai.yourdomain.com

# 6. Start
cloudflared tunnel run my-tunnel
```

Six steps. Ten minutes.

Then open `ai.yourdomain.com` — your Mac Mini's OpenClaw WebUI appears.

## What About Security?

You might worry: Doesn't this expose your internal service to the internet?

Multiple layers of protection:

**1. Cloudflare Access (Optional)**

Cloudflare has built-in zero-trust access control. Configure email verification, Google/GitHub OAuth login — only authenticated users can access. Free tier supports 50 users.

**2. OpenClaw's Built-in Token Auth**

OpenClaw's WebUI has `auth.mode: "token"` config. Access requires entering a token. Without the token, visitors just see a login screen.

**3. Allowed Origins Only**

OpenClaw's `controlUi.allowedOrigins` config restricts access to only your domain. Requests from other origins are rejected.

**4. End-to-End HTTPS**

Cloudflare automatically provides SSL certificates. Browser to Cloudflare to your machine — fully encrypted.

## Real-World Experience

Daily chat happens on Telegram — Agents push notifications there. WebUI's value is **checking the backend when you're out and about**.

🚇 **On the subway** — Want to see how the code Agent did overnight? Open the URL, see task queues, PR status, logs at a glance.

☕ **At a café** — Client suddenly asks about progress? Open WebUI, screenshot, send. No need to say "I'll check when I get home."

🏖️ **On a trip** — Agent team keeps running at home, monitor anytime on your phone. Issues? Direct orders via Telegram.

Before, these scenarios meant either VPN back home or waiting until you got back. Now one URL handles it all.

## Setup Checklist

| Component | Description |
|-----------|-------------|
| Mac Mini | Machine running OpenClaw at home |
| OpenClaw | AI Agent framework, WebUI on port 18789 |
| Cloudflare Tunnel | Free tunneling |
| Your domain | Pointed to Cloudflare DNS |
| Total cost | **$0** (if you already have a domain) |

## Who Is This For?

- People running OpenClaw / Home Assistant / any local service at home
- People who don't want to mess with VPN and port forwarding
- People who want to manage AI Agents from their phone anytime
- Mac Mini / Raspberry Pi / NAS users

## Summary

Cloudflare Tunnel solves a simple need: **I run a bunch of AI at home, and I want to manage them when I'm out.**

No cost, no port opening, ten minutes to set up. Recommended for anyone running local AI Agents.
