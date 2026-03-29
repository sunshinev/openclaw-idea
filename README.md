# OpenClaw Idea - 科技猎手博客

> 本仓库由 OpenClaw Agent 自动管理，用于发布科技日报和博客文章。

---

## ⚠️⚠️⚠️ 核心规则（必须遵守）

### 文章目录

```
✅ 正确:  content/posts/
❌ 错误:  blog/content/posts/

绝对禁止创建 blog/ 子目录！
```

### 构建命令

```bash
cd /root/.openclaw/workspace/openclaw-idea
hugo --gc --minify    # ✅ 正确

# ❌ 错误: cd blog && hugo
```

---

## 📁 项目结构

```
openclaw-idea/
├── hugo.yaml                    # Hugo 配置文件
├── content/posts/               # 📝 文章源文件目录
│   ├── hello-world.md
│   └── tech-hunter-xxx.md
├── themes/PaperMod/             # 主题目录
├── docs/                        # 🚀 构建输出（GitHub Pages）
└── README.md                    # 本文件
```

---

## 🚀 发布流程

### Step 1: 创建文章

```bash
cd /root/.openclaw/workspace/openclaw-idea/content/posts/
```

创建 `.md` 文件：

```markdown
---
title: "文章标题"
date: 2026-03-29
draft: false
categories:
  - 科技日报
tags:
  - AI
---

文章内容...
```

### Step 2: 构建

```bash
cd /root/.openclaw/workspace/openclaw-idea
hugo --gc --minify
```

### Step 3: 推送

```bash
git add -A
git commit -m "feat: 添加文章《文章标题》"
git push origin main
```

---

## 📋 OpenClaw 操作指南

### ✅ 允许的操作

| 操作 | 方法 |
|------|------|
| 发布新文章 | 在 `content/posts/` 创建 `.md` 文件 |
| 修改文章 | 编辑 `content/posts/` 下的文件 |
| 删除文章 | 删除 `content/posts/` 下的文件 |
| 发布日报 | 生成 Markdown 到 `content/posts/` |

### ❌ 禁止的操作

| 禁止项 | 原因 |
|--------|------|
| 创建 `blog/` 目录 | 正确路径是 `content/` |
| 使用 `blog/content/posts/` | 路径错误 |
| 命令 `cd blog && hugo` | blog 目录不存在 |
| 修改 `docs/` 目录 | docs 是构建输出 |
| 添加 submodule | 主题已是普通目录 |

---

## 📊 配置信息

| 项目 | 值 |
|------|-----|
| 仓库 | `git@github.com:sunshinev/openclaw-idea.git` |
| 博客地址 | https://sunshinev.github.io/openclaw-idea/ |
| 源文件目录 | `content/posts/` |
| 构建输出 | `docs/` |
| Hugo 版本 | 0.134.0 extended |
| 主题 | PaperMod v7.0 |

---

## 🔧 常见问题

### Q: 文章发布后看不到？

检查：
- `draft: false` 是否设置
- 文件是否在 `content/posts/` 目录（不是 `blog/content/posts/`）
- 是否执行了 `hugo --gc --minify` 构建

### Q: 构建报错找不到目录？

确认你在项目根目录：
```bash
cd /root/.openclaw/workspace/openclaw-idea
pwd  # 应该显示 .../openclaw-idea
```

---

*此 README 供 OpenClaw Agent 读取，了解项目结构和操作规范。*
