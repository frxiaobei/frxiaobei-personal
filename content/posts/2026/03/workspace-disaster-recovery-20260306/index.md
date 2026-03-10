---
title: "那晚，我看着 Gemini 把我的 workspace 删空了"
date: 2026-03-06T10:00:00+08:00
draft: false
tags:
  - OpenClaw
  - Gemini
  - 复盘
  - 灾难恢复
  - Agent 安全
description: 一次真实事故复盘：Gemini 误在 workspace 根目录执行 rm -rf 导致 11,583 个文件消失。拆解根因、恢复路径和可落地防线。
translationKey: workspace-disaster-recovery-20260306
cover:
  image: 01-cover.png
---

凌晨，盯着终端，脑子只有一个字：完。

不是某个文件出问题，是整个 workspace 根目录被打空了。

11,583 个文件，3 分钟，没了。

---

## 我在干嘛

当时在并行跑一堆任务，为了快，把活分给了子 Agent。

Gemini 领了前端任务，要在 `projects/manga-character-platform/frontend` 下面干活。

然后它执行了 `rm -rf ./*`。

问题是——它以为自己 cd 进去了，实际没进去。

---

## 时间线

```
00:48:57  Gemini cd 到目标目录（它以为）
00:49:22  执行 rm -rf ./*（在 workspace 根目录）
00:50:03  发现不对，开始"重建"空壳目录
00:51:29  自动同步把删除 push 到 GitHub
```

从删到扩散，4 分钟。

---

## 我自己也有问题

说实话，锅不能全甩给 Gemini。

**我偷懒了。**

项目代码本来不该放 workspace 下面。但我当时图省事，直接把 `projects/` 扔在 workspace 里，想着反正 `.gitignore` 加了，不会传到 GitHub。

结果 gitignore 管的是 git，不是 rm。

Gemini 一删，projects 也跟着没了。

好在项目本身有 GitHub 仓库，后来从上游拉回来了。但那些本地改动、没 push 的实验分支，全没了。

这叫什么？叫自己挖的坑。

---

## 根因

**1. cd 没生效**

OpenClaw exec 的 `cd` 不改变后续命令的 cwd。Gemini 以为自己在子目录，实际还在根目录。

**2. 高危命令没卡**

`rm -rf` 前没有强制 `pwd`，没有路径白名单，没有删除规模检查。

**3. 自动同步没刹车**

删完马上 commit/push，二次伤害。

**4. 目录结构混乱**

projects 不该和 workspace 混在一起。我偷懒了，付出代价。

---

## 怎么救的

没什么花活，就是硬做。

1. **止血** — 停掉所有自动任务
2. **回滚** — 从误删前的 commit 拉回核心文件
3. **补资产** — 从上游仓库补回 skills 和 projects
4. **验链路** — 不看文件在不在，看流程通不通

最烦的是 `baoyu-skills`，恢复后看着正常，实际脚本没了，公众号发布链路直接断。

---

## 改了什么

**已落地：**

- 任务执行强制指定 `workdir`
- 高危命令前强制 `pwd + 白名单 + 阈值`
- 自动同步加熔断（异常删除先停推）
- 恢复流程写成 SOP

**目录也整理了：**

```
~/.openclaw/workspace/          # Agent 工作区（会同步到 GitHub）
├── AGENTS.md                   # Agent 行为规范
├── MEMORY.md                   # 长期记忆
├── SOUL.md                     # 人设
├── memory/                     # 每日记录
├── skills/                     # 技能（纯代码，无 .git）
├── data/                       # 运行时数据（数据库、状态）
├── config/                     # 配置文件
├── scripts/                    # 工具脚本
├── branding/                   # 品牌素材
├── assets/                     # 静态资源
└── articles -> Obsidian/       # 软链接到 Obsidian

~/projects/                     # 代码仓库（独立，各自有 .git）
├── manga-platform-nextjs/      # 当前开发项目
├── agent-swarm/                # Swarm 工具（开发版）
├── elyfinn-website/            # 官网
└── ...

~/Library/.../Obsidian/         # 知识库（iCloud 同步）
├── projects/                   # 项目文档（需求、任务、PRD）
├── articles/                   # 文章草稿
└── knowledge/                  # 方法论、参考资料
```

**原则很简单：**

- **workspace** = Agent 的家，会自动同步，不放代码仓库
- **projects** = 代码的家，各自 git 管理，不混进 workspace
- **Obsidian** = 人的家，笔记和文档，软链接给 Agent 读

分开放，删不干净。

---

## 最后

以前觉得备份是"有空再搞"的事。

那晚之后，这想法没了。

自动化让你快，也让你出事快。

如果你也在跑 Agent 自动化：

- 给删除操作加护栏
- 给自动同步加熔断
- 把代码和 workspace 分开

别等凌晨再补作业。

---

## 附录 A：高危命令安全清单（可直接抄）

每次执行 `rm` / `mv` / `cp --remove-destination` / 批量覆盖操作前，先跑这份清单：

- [ ] 先打印 `pwd`，确认当前目录
- [ ] 目标路径必须在 allowlist 内（如 `~/projects/*`）
- [ ] 禁止在 workspace 根目录执行删除类命令
- [ ] 先 dry-run（或先列出将被影响的文件数）
- [ ] 超过阈值（如 >100 文件）必须二次确认
- [ ] 自动同步任务检测到异常删除时，立即熔断（停止 push）

**一句话策略：**

> 不允许“路径不确定 + 命令破坏性强”同时出现。
