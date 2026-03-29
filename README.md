# OpenClaw Idea - 科技猎手博客

> 本仓库由 OpenClaw Agent 自动管理，用于发布科技日报和博客文章。

---

## 📁 项目结构

```
openclaw-idea/
├── hugo.yaml                    # Hugo 配置文件
├── content/
│   └── posts/                   # 📝 文章源文件目录（Markdown）
│       ├── hello-world.md
│       └── tech-hunter-xxx.md
├── themes/
│   └── PaperMod/                # 主题目录（普通目录，非 submodule）
├── docs/                        # 🚀 构建输出（GitHub Pages 部署目录）
│   ├── index.html
│   ├── posts/
│   └── ...
└── README.md                    # 本文件
```

---

## 🚀 部署方式

**本地构建 + GitHub Pages 直接部署**

1. 本地运行 `hugo --gc --minify` 构建静态站点到 `docs/`
2. 推送 `docs/` 到 GitHub
3. GitHub Pages 从 `docs/` 目录部署

**⚠️ 不使用 GitHub Actions，不使用 submodule**

---

## 📋 OpenClaw 操作指南

### ✅ 允许的操作

| 操作 | 命令/方法 |
|------|-----------|
| 发布新文章 | 在 `content/posts/` 创建 `.md` 文件，构建后推送 |
| 修改文章 | 编辑 `content/posts/` 下的文件，重新构建推送 |
| 删除文章 | 删除源文件，重新构建推送 |
| 发布日报 | 生成日报 Markdown，按发布文章流程处理 |
| 更新配置 | 修改 `hugo.yaml`，重新构建推送 |

### ❌ 禁止的操作

| 禁止项 | 原因 |
|--------|------|
| 修改 `docs/` 目录内容 | docs 是构建输出，会被覆盖 |
| 添加 submodule | 主题已是普通目录，保持现状 |
| 创建 GitHub Actions | 不需要，使用本地构建部署 |
| 删除/重命名 `themes/PaperMod/` | 主题是博客核心组件 |
| 修改 `.git` 目录结构 | 会破坏仓库完整性 |

### ⚡ 标准发布流程

```bash
# 1. 进入项目目录
cd /root/.openclaw/workspace/openclaw-idea

# 2. 创建/编辑文章
# 在 content/posts/ 目录创建 .md 文件

# 3. 构建
hugo --gc --minify

# 4. 检查输出（可选）
ls docs/

# 5. 提交推送
git add -A
git commit -m "feat: 发布文章《文章标题》"
git push origin main
```

### 📝 文章格式

```markdown
---
title: "文章标题"
date: YYYY-MM-DD
draft: false
categories:
  - 分类名称
tags:
  - 标签1
  - 标签2
---

文章内容...
```

**注意**: `draft: false` 文章才会在首页显示。

---

## 📊 配置信息

| 项目 | 值 |
|------|-----|
| 仓库 | `git@github.com:sunshinev/openclaw-idea.git` |
| 博客地址 | https://sunshinev.github.io/openclaw-idea/ |
| Hugo 版本 | 0.134.0 extended |
| 主题 | PaperMod v7.0 |
| 源文件目录 | `content/posts/` |
| 构建输出目录 | `docs/` |
| GitHub Pages | 从 `docs/` 目录部署 |

---

## 🔧 常见问题

### Q: 文章发布后首页看不到？

检查：
- `draft: false` 是否设置
- 文件是否在 `content/posts/` 目录
- 是否执行了 `hugo --gc --minify` 构建

### Q: 如何更新主题？

```bash
cd /root/.openclaw/workspace/openclaw-idea
rm -rf themes/PaperMod
git clone --depth 1 --branch v7.0 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
rm -rf themes/PaperMod/.git
```

### Q: 如何添加新页面？

在 `content/` 下创建 `.md` 文件，例如 `about.md`:

```markdown
---
title: "关于"
date: 2026-03-29
draft: false
---

关于页面内容...
```

---

## 📌 给 OpenClaw 的提示

1. **每次修改文章后都要重新构建** - 运行 `hugo --gc --minify`
2. **不要跳过构建直接推送** - docs/ 必须是最新的构建结果
3. **使用有意义的 commit message** - 例如 `feat: 发布日报 2026-03-29`
4. **构建前检查 Hugo 版本** - 必须是 extended 版本
5. **日报格式要统一** - 包含标题、日期、概览、重点、洞察

---

*此 README 供 OpenClaw Agent 读取，了解项目结构和操作规范。*
