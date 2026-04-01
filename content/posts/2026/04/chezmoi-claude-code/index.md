---
title: "用 chezmoi 同步多台机器的 Claude Code 配置：完整教程"
date: 2026-04-01T22:00:00+08:00
draft: false
summary: "一处修改，处处同步。用 chezmoi 管理 Claude Code 配置和 Skills，支持自动更新、模板区分机器、新机器一键配置。"
tags: ["Claude Code", "chezmoi", "dotfiles", "效率工具"]
image: cover.png
---

> 一处修改，处处同步。新机器一条命令搞定。

---

## 一、背景：多机器配置同步的痛点

我有 3 台 Mac：一台 MacBook、两台 Mac Mini。每台都装了 Claude Code，也装了一堆 Skills——gstack、humanizer、marketing skills 等等。以后可能还会加云服务器。

用了一段时间后，问题来了：

- 在 MacBook 上装的 skill，Mini 上没有
- 改了 `CLAUDE.md` 全局配置，另一台还是旧版
- 换新机器要从头配一遍，每次至少半小时
- Skills 更新了，要手动去每台机器 `git pull`

**我需要的是**：

```
✅ 一处修改，所有机器自动同步
✅ Skills 能自动更新
✅ 新机器一条命令搞定
✅ 不同机器可以有不同配置（比如服务器需要额外的 skills）
```

---

## 二、方案选型：为什么选 chezmoi

市面上管理 dotfiles（配置文件）的工具不少，我对比了几种：

| 方案 | 优点 | 缺点 |
|-----|------|------|
| **手动复制** | 简单 | 累死，容易漏，不可持续 |
| **Git 裸仓库** | 纯 Git，无依赖 | 配置复杂，冲突处理麻烦 |
| **Mackup** | macOS 友好 | 不支持自定义路径，不够灵活 |
| **chezmoi** | 模板、加密、跨平台、支持外部源 | 有学习曲线 |

最终选了 **chezmoi**，原因：

1. **模板支持**——不同机器可以渲染出不同配置
2. **external 外部源**——可以自动从 GitHub 拉取 skills，还能定期更新
3. **加密支持**——API keys 也能安全同步（本文不涉及）
4. **一条命令初始化**——新机器 `chezmoi init --apply` 搞定

---

## 三、前置准备

### 3.1 安装 chezmoi

**macOS**：

```bash
brew install chezmoi
```

**Linux**：

```bash
sh -c "$(curl -fsLS get.chezmoi.io)"
```

**验证安装**：

```bash
chezmoi --version
# chezmoi version 2.70.0
```

### 3.2 创建 GitHub 仓库

去 GitHub 创建一个仓库，比如 `dotfiles`。可以是公开的，也可以是私有的。

**建议**：如果你的 `CLAUDE.md` 里没有敏感信息，公开仓库更方便（不用配 SSH）。

### 3.3 初始化 chezmoi

```bash
# 初始化（会创建 ~/.local/share/chezmoi 目录）
chezmoi init

# 进入 chezmoi 目录
cd ~/.local/share/chezmoi

# 关联远程仓库（替换成你的）
git remote add origin https://github.com/你的用户名/dotfiles.git
```

---

## 四、理解 Claude Code 的配置结构

在动手之前，先搞清楚 `~/.claude` 目录里有什么，哪些需要同步：

![目录结构](01-directory-structure.png)

```
~/.claude/
├── CLAUDE.md          ✅ 要同步 - 全局指令
├── settings.json      ✅ 要同步 - 插件和权限配置
├── skills/            ✅ 要同步 - 技能包
│   ├── gstack/
│   ├── humanizer/
│   └── marketingskills/
├── projects/          ❌ 不同步 - 每台机器的项目不同
├── sessions/          ❌ 不同步 - 运行时数据
├── history.jsonl      ❌ 不同步 - 对话历史，太大了
├── cache/             ❌ 不同步 - 缓存
└── debug/             ❌ 不同步 - 调试日志
```

**Skills 的三种来源**：

| 来源 | 例子 | 如何同步 |
|-----|------|---------|
| **Marketplace** | obsidian, frontend-design | 记录在 settings.json，Claude Code 启动时自动安装 |
| **GitHub 公开仓库** | gstack, humanizer | 用 chezmoi external 自动拉取 |
| **私有/自己写的** | 公司内部 skill | chezmoi add 直接管理，或放私有 repo |

---

## 五、基础同步：CLAUDE.md 和 settings.json

### 5.1 添加配置文件

```bash
# 添加全局指令文件
chezmoi add ~/.claude/CLAUDE.md

# 添加插件配置
chezmoi add ~/.claude/settings.json
```

**chezmoi 命名规则**：

运行上面的命令后，你会发现 chezmoi 目录里多了个 `private_dot_claude` 文件夹。这是 chezmoi 的命名规则：

- `dot_` 前缀 = 点开头的文件/目录（`.claude` → `dot_claude`）
- `private_` 前缀 = 文件权限 600（只有自己能读写）

所以 `~/.claude` 对应 `private_dot_claude`。

### 5.2 查看添加的文件

```bash
ls ~/.local/share/chezmoi/private_dot_claude/
# CLAUDE.md  settings.json
```

### 5.3 验证

```bash
# 查看 chezmoi 管理了哪些文件
chezmoi managed
# /Users/你的用户名/.claude/CLAUDE.md
# /Users/你的用户名/.claude/settings.json
```

---

## 六、配置 .chezmoiignore

为了避免把不需要同步的文件也加进去，创建 `.chezmoiignore`：

```bash
cat > ~/.local/share/chezmoi/.chezmoiignore << 'EOF'
# Claude Code 运行时数据，不同步
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

这样即使你不小心运行 `chezmoi add ~/.claude`，这些目录也会被忽略。

---

## 七、用 external 管理 GitHub Skills

这是最有价值的部分——让 chezmoi 自动从 GitHub 拉取 skills，还能定期更新。

![工作流程](02-workflow.png)

### 7.1 创建 external 配置

```bash
cat > ~/.local/share/chezmoi/.chezmoiexternal.toml << 'EOF'
# Claude Code Skills - 从 GitHub 自动安装和更新
# 运行 chezmoi apply 会检查更新（超过 refreshPeriod 自动拉取）
# 强制刷新：chezmoi apply --refresh-externals

# GStack - Garry Tan 的工程团队模拟框架
[".claude/skills/gstack"]
type = "archive"
url = "https://github.com/garrytan/gstack/archive/main.tar.gz"
stripComponents = 1
refreshPeriod = "168h"  # 每周检查一次

# Humanizer - 去除 AI 生成痕迹
[".claude/skills/humanizer"]
type = "archive"
url = "https://github.com/blader/humanizer/archive/main.tar.gz"
stripComponents = 1
refreshPeriod = "168h"

# Marketing Skills - CRO/SEO/文案/增长
[".claude/skills/marketingskills"]
type = "archive"
url = "https://github.com/coreyhaines31/marketingskills/archive/main.tar.gz"
stripComponents = 1
refreshPeriod = "168h"

# Frontend Slides - 创建动画 HTML 演示文稿
[".claude/skills/frontend-slides"]
type = "archive"
url = "https://github.com/zarazhangrui/frontend-slides/archive/main.tar.gz"
stripComponents = 1
refreshPeriod = "168h"
EOF
```

### 7.2 配置详解

| 字段 | 含义 |
|-----|------|
| `[".claude/skills/xxx"]` | 目标路径（相对于 home 目录） |
| `type = "archive"` | 下载 tar.gz 压缩包并解压 |
| `url` | GitHub 仓库的 archive 下载地址 |
| `stripComponents = 1` | 解压时去掉顶层目录（GitHub archive 会包一层） |
| `refreshPeriod = "168h"` | 168 小时 = 7 天，超过这个时间会检查更新 |

### 7.3 应用配置

```bash
chezmoi apply
```

第一次运行会下载所有 skills，稍等片刻。

### 7.4 验证

```bash
ls ~/.claude/skills/
# frontend-slides  gstack  humanizer  marketingskills
```

---

## 八、验证完整配置

在提交之前，验证一切正常：

```bash
# 1. 查看 chezmoi 管理的文件
chezmoi managed

# 2. 查看 diff（本地 vs chezmoi 源）
chezmoi diff

# 3. 验证 skills 目录
ls ~/.claude/skills/

# 4. 验证 CLAUDE.md
head -10 ~/.claude/CLAUDE.md

# 5. 验证 settings.json
cat ~/.claude/settings.json
```

---

## 九、提交并推送

```bash
cd ~/.local/share/chezmoi

# 添加所有文件
git add .

# 提交
git commit -m "feat: add Claude Code config and skills"

# 推送
git push -u origin main
```

---

## 十、进阶：不同机器不同配置

如果某台机器需要额外的 skills，而其他机器不需要，可以用模板。

### 10.1 查看每台机器的 hostname

先在每台机器上运行：

```bash
hostname
```

记下来，比如：
- MacBook: `xiaobei-mbp.local`
- Mac Mini 1: `xiaobei-mini.local`
- Mac Mini 2: `xiaobei-mini2.local`

### 10.2 把 external 配置改成模板

```bash
# 重命名为模板文件
mv ~/.local/share/chezmoi/.chezmoiexternal.toml \
   ~/.local/share/chezmoi/.chezmoiexternal.toml.tmpl
```

### 10.3 编辑模板

```bash
cat > ~/.local/share/chezmoi/.chezmoiexternal.toml.tmpl << 'EOF'
# ============================================
# 所有机器都安装的 Skills
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
# 仅 Mini 1（主力开发机）安装的 Skills
# ============================================
{{ if eq .chezmoi.hostname "xiaobei-mini.local" }}
[".claude/skills/frontend-slides"]
type = "archive"
url = "https://github.com/zarazhangrui/frontend-slides/archive/main.tar.gz"
stripComponents = 1
refreshPeriod = "168h"
{{ end }}

# ============================================
# 仅 Mini 2（服务器）安装的 Skills
# ============================================
{{ if eq .chezmoi.hostname "xiaobei-mini2.local" }}
[".claude/skills/server-monitoring"]
type = "archive"
url = "https://github.com/xxx/server-monitoring/archive/main.tar.gz"
stripComponents = 1
refreshPeriod = "168h"
{{ end }}
EOF
```

**模板语法**：

- `{{ if eq .chezmoi.hostname "xxx" }}` — 如果 hostname 等于 xxx
- `{{ end }}` — 条件结束
- 也可以用 `{{ if contains .chezmoi.hostname "mini" }}` 做模糊匹配

### 10.4 验证模板

```bash
# 查看当前机器会渲染出什么
chezmoi execute-template < ~/.local/share/chezmoi/.chezmoiexternal.toml.tmpl
```

---

## 十一、新机器一键配置

**这是最爽的部分**——新机器只需要两步：

### 公开仓库

```bash
# 1. 安装 chezmoi
brew install chezmoi  # 或 sh -c "$(curl -fsLS get.chezmoi.io)"

# 2. 一键初始化（替换成你的 GitHub 用户名）
chezmoi init 你的用户名/dotfiles --apply
```

### 私有仓库

```bash
# 需要用 SSH URL
chezmoi init git@github.com:你的用户名/dotfiles.git --apply
```

**执行完毕后，自动获得**：

- ✅ `~/.claude/CLAUDE.md` 全局配置
- ✅ `~/.claude/settings.json` 插件设置
- ✅ 所有配置的 GitHub skills
- ✅ Marketplace 插件会在首次启动 Claude Code 时自动安装

---

## 十二、日常维护命令

```bash
# 查看状态（本地和 chezmoi 源是否一致）
chezmoi status

# 查看差异
chezmoi diff

# 应用更新（本地 → 目标位置）
chezmoi apply

# 从远程拉取 + 应用
chezmoi update

# 强制刷新所有 external skills
chezmoi apply --refresh-externals

# 添加新文件到 chezmoi 管理
chezmoi add ~/.claude/新文件

# 编辑 chezmoi 中的文件（会自动打开编辑器）
chezmoi edit ~/.claude/CLAUDE.md

# 编辑后应用
chezmoi apply
```

---

## 十三、常见问题

### Q1: `chezmoi apply` 报权限错误

**原因**：目标文件被 Claude Code 占用

**解决**：关闭 Claude Code 再试

```bash
# 或者强制覆盖
chezmoi apply --force
```

### Q2: external skills 没有更新

**原因**：还没到 `refreshPeriod`（默认 7 天）

**解决**：强制刷新

```bash
chezmoi apply --refresh-externals
```

### Q3: 私有仓库拉取失败

**原因**：用了 HTTPS，没有认证

**解决**：改用 SSH

```bash
chezmoi init git@github.com:你的用户名/dotfiles.git --apply
```

### Q4: hostname 匹配不上

**原因**：有些系统 hostname 带 `.local` 后缀，有些不带

**解决**：用 `contains` 代替 `eq`

```
{{ if contains .chezmoi.hostname "mini" }}
...
{{ end }}
```

### Q5: 本地改了配置，怎么同步回 chezmoi？

**方法 1**：重新添加

```bash
chezmoi add ~/.claude/CLAUDE.md
```

**方法 2**：直接编辑 chezmoi 源

```bash
chezmoi edit ~/.claude/CLAUDE.md
# 编辑完后
chezmoi apply
```

### Q6: Marketplace 插件没有自动安装

**这是正常的**。Marketplace 插件（如 obsidian、frontend-design）存在 `settings.json` 里，但实际插件需要 Claude Code 启动后才会下载。

chezmoi 只负责同步 `settings.json`，插件下载是 Claude Code 的事。

---

## 十四、完整配置文件参考

### 目录结构

```
~/.local/share/chezmoi/
├── .chezmoiignore              # 忽略规则
├── .chezmoiexternal.toml.tmpl  # External skills 配置（模板）
├── private_dot_claude/
│   ├── CLAUDE.md               # 全局指令
│   └── settings.json           # 插件配置
└── README.md                   # 可选：说明文档
```

### settings.json 示例

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

### CLAUDE.md 示例

```markdown
# Python Web Backend Rules

## 环境要求
- 使用 uv 管理依赖
- 使用 .venv/ 虚拟环境
- 运行命令用 uv run

## 项目结构
app/
  __init__.py
  routes.py
  models.py
main.py
tests/

## 常用命令
- 启动服务: uv run uvicorn app.main:app --reload
- 运行测试: uv run pytest
- 格式化: uv run ruff format .
```

---

## 十五、总结

| 痛点 | 解决方案 |
|-----|---------|
| 多机器配置不一致 | chezmoi 统一管理，Git 同步 |
| Skills 手动安装麻烦 | external 自动从 GitHub 拉取 |
| 不同机器要不同配置 | 模板语法按 hostname 区分 |
| 新机器配置耗时 | `chezmoi init --apply` 一条命令 |
| Skills 需要手动更新 | refreshPeriod 自动检查 |

**一句话总结**：

> dotfiles 管理器 + GitHub = Claude Code 配置的终极同步方案

---

## 附录：推荐的 Claude Code Skills

| Skill | 用途 | GitHub |
|-------|-----|--------|
| gstack | 模拟完整工程团队（CEO/Designer/QA） | garrytan/gstack |
| humanizer | 去除 AI 生成痕迹 | blader/humanizer |
| marketingskills | CRO/SEO/文案/增长（34个技能） | coreyhaines31/marketingskills |
| frontend-slides | 创建动画 HTML 演示文稿 | zarazhangrui/frontend-slides |
| baoyu-skills | 宝玉的技能包（翻译、公众号等） | JimLiu/baoyu-skills |

---

## 延伸：不止 Claude Code

chezmoi 能管理的远不止 Claude Code 配置。同样的方法可以用来同步：

- **zshrc / bashrc** — 你的 shell 配置
- **git config** — 全局 Git 设置
- **SSH config** — 服务器连接配置
- **Vim / Neovim** — 编辑器配置
- **tmux** — 终端复用器配置
- **其他任何 dotfiles**

一旦用上了 chezmoi，你会发现再也回不去手动复制配置的日子了。

---

## 懒人方案

如果你懒得一步步配，可以直接把这篇文章丢给你的 Claude Code 或 OpenClaw（小龙虾），让 AI 帮你搞定：

> "帮我按照这篇教程配置 chezmoi，同步我的 Claude Code 配置到 GitHub。"

AI 会帮你创建仓库、添加配置、设置 external——你只需要最后确认一下就行。

---

*如果这篇文章帮到了你，欢迎分享给也在用 Claude Code 的朋友。*
