---
title: "在家跑 AI Agent 团队？一条隧道，手机随时管"
date: 2026-03-13T23:00:00+08:00
draft: false
description: "不开端口，不买服务器，不折腾 nginx。Cloudflare Tunnel + OpenClaw，零成本远程访问你的 AI Agent。"
tags: ["AI", "OpenClaw", "Cloudflare", "自托管", "教程"]
categories: ["技术"]
cover:
  image: cover.png
---

> 不开端口，不买服务器，不折腾 nginx。Cloudflare Tunnel + OpenClaw，零成本远程访问你的 AI Agent。

## 起因

我在家里的 Mac Mini 上跑了一支 AI Agent 团队：

- 🤖 **主力助手** — 管日程、写文章、分析数据
- 🩺 **健康管家** — 管体检报告、用药记录
- 📊 **分析师** — 每天分析 AI 热帖
- 💻 **代码工人** — 7×24 写代码提 PR

底层是 OpenClaw，一个开源的 AI Agent 框架。它有个 WebUI，可以在浏览器里看到所有 Agent 的状态、对话、任务进度。

但问题是——**WebUI 跑在 localhost:18789，出了家门就访问不了。**

## 传统方案的痛

远程访问内网服务，传统方案一大堆：

- **公网 IP + 端口映射** — 运营商不一定给，安全风险大
- **内网穿透（frp/ngrok）** — 要买中转服务器，配置繁琐
- **VPN** — 得搭 WireGuard/OpenVPN，手机连 VPN 耗电
- **Tailscale** — 不错，但想要更简单的方案

我想要的很简单：**手机打开一个网址，直接看到家里的 AI Agent 团队在干啥。**

## Cloudflare Tunnel：零成本的答案

Cloudflare Tunnel（以前叫 Argo Tunnel）的逻辑很简单：

你的机器主动跟 Cloudflare 建立一条加密隧道 → Cloudflare 反向代理流量到你的本地服务 → 外部通过域名访问。

**不需要公网 IP，不需要开端口，不需要中转服务器。免费。**

整个配置过程：

```bash
# 1. 安装 cloudflared
brew install cloudflared

# 2. 登录 Cloudflare
cloudflared tunnel login

# 3. 创建隧道
cloudflared tunnel create my-tunnel

# 4. 配置路由
# ~/.cloudflared/config.yml
tunnel: my-tunnel
credentials-file: ~/.cloudflared/<tunnel-id>.json

ingress:
  - hostname: ai.yourdomain.com
    service: http://127.0.0.1:18789
  - service: http_status:404

# 5. DNS 解析
cloudflared tunnel route dns my-tunnel ai.yourdomain.com

# 6. 启动
cloudflared tunnel run my-tunnel
```

六步，十分钟。

然后打开 `ai.yourdomain.com` ——家里 Mac Mini 上的 OpenClaw WebUI 就出来了。

## 安全怎么办？

你可能会担心：这不是把内网服务暴露到公网了吗？

几层防护：

**1. Cloudflare Access（可选）**

Cloudflare 自带零信任访问控制。可以配置邮箱验证、Google/GitHub OAuth 登录，只有认证过的人才能访问。免费版 50 个用户够用。

**2. OpenClaw 自带 Token 认证**

OpenClaw 的 WebUI 有 `auth.mode: "token"` 配置，访问需要输入 token。不知道 token 的人打开页面只会看到登录框。

**3. 只允许特定域名**

OpenClaw 配置里的 `controlUi.allowedOrigins` 可以限制只有你的域名能访问 WebUI，其他来源的请求会被拒绝。

**4. HTTPS 全程加密**

Cloudflare 自动提供 SSL 证书，浏览器到 Cloudflare 到你的机器，全链路加密。

## 实际体验

日常聊天在 Telegram，Agent 有事会推送。WebUI 的价值是——**出门在外也能看后台**。

🚇 **地铁上** — 想看看代码 Agent 昨晚跑得怎么样？手机打开网址，任务队列、PR 状态、日志一目了然。

☕ **咖啡厅** — 客户突然问进度，打开 WebUI 截个图发过去。不用说"我回家看看"。

🏖️ **出差/旅行** — 家里的 Agent 团队照常跑，手机随时能监控。有问题 Telegram 直接指挥。

以前这些场景，要么 VPN 回家，要么等回家再看。现在一个网址搞定。

## 配置清单

| 组件 | 说明 |
|------|------|
| Mac Mini | 家里跑 OpenClaw 的机器 |
| OpenClaw | AI Agent 框架，WebUI 端口 18789 |
| Cloudflare Tunnel | 免费内网穿透 |
| 你的域名 | 指向 Cloudflare DNS |
| 总成本 | **0 元**（域名已有的话） |

## 适合谁？

- 在家跑 OpenClaw / Home Assistant / 任何本地服务的人
- 不想折腾 VPN 和端口映射的人
- 想用手机随时管理 AI Agent 的人
- Mac Mini / 树莓派 / NAS 用户

## 总结

Cloudflare Tunnel 解决了一个很朴素的需求：**我在家里跑了一堆 AI，出门了还想管它们。**

不花钱，不开端口，十分钟搞定。推荐给所有在本地跑 AI Agent 的朋友。
