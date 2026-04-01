---
title: "OpenSpec 技术架构深度解析｜Spec-Driven Development 框架"
date: 2026-03-31T21:00:00+08:00
draft: false
tags: ["技术架构", "OpenSpec", "SDD", "AI编程", "规范驱动"]
categories: ["技术深度"]
---

> OpenSpec 是一个轻量级、开源的 Spec-Driven Development (SDD) 框架，专为 AI 编码助手设计。本文将深度拆解其技术架构、核心组件和运行原理。

---

## 一、框架概述

### 1.1 什么是 OpenSpec？

OpenSpec 是由 Fission-AI 开发的 **规范驱动开发框架**，旨在解决 AI 辅助编程中的三大痛点：

| 痛点 | OpenSpec 解决方案 |
|------|-------------------|
| **上下文丢失** | 规范文件持久化存储于代码仓库 |
| **需求漂移** | 结构化的变更管理流程 |
| **AI 幻觉** | 明确的约束和验证机制 |

**核心理念**：
- ✅ Fluid not rigid（灵活而非僵化）
- ✅ Iterative not waterfall（迭代而非瀑布）
- ✅ Easy not complex（简单而非复杂）
- ✅ Built for brownfield（支持遗留代码库）
- ✅ Scalable（从个人项目到企业级）

---

## 二、核心架构组件

### 2.1 目录结构设计

OpenSpec 采用 **文件系统作为存储层**，所有规范以 Markdown 形式存储：

```
project_root/
├── openspec/
│   ├── AGENTS.md          # AI 指令集（机器可读的 README）
│   ├── project.md         # 全局项目上下文
│   ├── config.yaml        # 项目配置（可选）
│   ├── specs/             # 系统当前状态的 Source of Truth
│   │   ├── auth-login/
│   │   │   └── spec.md    # 登录模块规范
│   │   └── payment/
│   │   │   └── spec.md    # 支付模块规范
│   ├── changes/           # 活跃变更工作区
│   │   └── add-oauth-login/
│   │   │   ├── proposal.md    # 变更意图
│   │   │   ├── design.md      # 技术设计
│   │   │   ├── tasks.md       # 实现任务清单
│   │   │   └── specs/         # 规范增量（Delta）
│   │   └── archive/           # 已归档变更
```

#### 设计原理

1. **Source of Truth (specs/)**
   - 存储系统当前的业务逻辑和技术约束
   - 按功能模块（Capability）组织
   - AI 修改前必须先读取现有规范，防止"灾难性遗忘"

2. **Change Workspace (changes/)**
   - 类似 Git 分支的变更隔离机制
   - 新功能/修复以独立文件夹存在
   - 实现并验证后才合并到主规范

3. **Agent Interface (AGENTS.md)**
   - 机器可读的行为指令集
   - 指导 AI 如何读取上下文、格式化输出
   - 支持无原生集成的 AI 工具通过读取此文件工作

---

### 2.2 状态机模型

OpenSpec 强制执行 **四阶段软件演化状态机**：

```
┌─────────────────────────────────────────────────────────────┐
│                    OpenSpec 状态机                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Phase 1: Proposal                                          │
│  ├─ 命令: /opsx:propose 或 /opsx:new                        │
│  ├─ 输出: proposal.md, tasks.md, design.md                  │
│  ├─ 目的: 明确意图、制定计划、分解任务                        │
│  │                                                          │
│  ↓                                                          │
│                                                             │
│  Phase 2: Definition                                        │
│  ├─ 命令: /opsx:ff 或 openspec validate                     │
│  ├─ 输出: specs/ (Delta)                                    │
│  ├─ 目的: 编写具体规范增量                                   │
│  │                                                          │
│  ↓                                                          │
│                                                             │
│  Phase 3: Apply                                             │
│  ├─ 命令: /opsx:apply                                       │
│  ├─ 输出: 源代码变更                                         │
│  ├─ 目的: AI 根据任务清单和规范编写代码                       │
│  │                                                          │
│  ↓                                                          │
│                                                             │
│  Phase 4: Archive                                           │
│  ├─ 命令: /opsx:archive                                     │
│  ├─ 输出: 合并后的 specs/, 归档文件夹                        │
│  ├─ 目的: 固化变更、清理工作区                               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**关键设计**：
- 阶段不可逆，确保文档不落后于代码
- 解决软件工程中"文档腐烂"的慢性问题
- 每个阶段产出可审计的制品

---

## 三、OPSX Workflow 系统

### 3.1 什么是 OPSX？

OPSX 是 OpenSpec 的 **新一代工件驱动工作流系统**，相比旧版有重大改进：

| 特性 | Legacy Workflow | OPSX Workflow |
|------|-----------------|---------------|
| 指令存储 | 硬编码在 TypeScript | schema.yaml + templates/*.md |
| 定制性 | 无法修改 | 可编辑模板即时生效 |
| 测试粒度 | 全量命令 | 可单独验证每个工件 |
| 工作流 | 固定结构 | 自定义工件和依赖 |

### 3.2 Artifact Graph Engine

OPSX 的核心是 **Artifact Graph Engine** —— 一个基于 DAG（有向无环图）的依赖管理系统：

```
         proposal
             │
             ↓
          specs
             │
             ↓
          design
             │
             ↓
          tasks
             │
             ↓
        implement
```

**运行原理**：

1. **Schema 定义**：定义工件类型和依赖关系
2. **文件扫描**：扫描文件系统检测已完成工件
3. **依赖计算**：基于 DAG 计算哪些工件可创建
4. **状态跟踪**：实时更新工件完成状态

**代码示例**（schema.yaml）：

```yaml
artifacts:
  proposal:
    dependencies: []
    template: templates/proposal.md
    
  specs:
    dependencies: [proposal]
    template: templates/specs.md
    
  design:
    dependencies: [specs]
    template: templates/design.md
    
  tasks:
    dependencies: [design]
    template: templates/tasks.md
```

---

### 3.3 核心命令解析

| 命令 | 功能 | 工作原理 |
|------|------|----------|
| `/opsx:propose` | 创建变更 + 生成规划工件 | 一步完成 Proposal 阶段 |
| `/opsx:explore` | 思考、调研、澄清需求 | 无结构化输出，纯思考伙伴 |
| `/opsx:new` | 创建变更脚手架 | Expanded workflow，仅创建文件夹 |
| `/opsx:continue` | 创建下一个工件 | 基于依赖图决定下一步 |
| `/opsx:ff` | 快进生成所有规划工件 | 批量生成 proposal/specs/design/tasks |
| `/opsx:apply` | 实现任务 | 读取 tasks.md，逐条执行 |
| `/opsx:verify` | 验证实现 | Expanded workflow，对照规范验证 |
| `/opsx:sync` | 同步 Delta 到主规范 | Expanded workflow，可选步骤 |
| `/opsx:archive` | 归档变更 | 移动到 archive/，合并规范 |
| `/opsx:bulk-archive` | 批量归档 | Expanded workflow，多变更归档 |
| `/opsx:onboard` | 引导式端到端演示 | Expanded workflow，新手教程 |

---

## 四、Context 注入机制

### 4.1 全局上下文锚点

`project.md` 是 AI 理解项目的"世界观"：

```markdown
# Project Context

## Tech Stack
- TypeScript 5.0
- React 18
- Node.js 20.x

## Architecture Patterns
- 所有数据库访问必须通过 Repository 层
- Controller 直接查询严格禁止

## Code Standards
- ESLint + Prettier
- 严格 TypeScript 配置

## Business Domain
- [行业术语和规则]
```

**注入原理**：
- OpenSpec 将此内容注入每个工件的指令前缀
- 包裹在 `<project-context>` 标签中
- 减少 AI "幻觉"，使生成代码符合项目约定

---

### 4.2 配置驱动的 Rules 注入

```yaml
# openspec/config.yaml

schema: spec-driven

context: |
  Tech stack: TypeScript, React, Node.js
  API conventions: RESTful, JSON responses
  Testing: Vitest for unit tests, Playwright for e2e

rules:
  proposal:
    - Include rollback plan
    - Identify affected teams
  specs:
    - Use Given/When/Then format for scenarios
  design:
    - Include sequence diagrams for complex flows
```

**注入优先级**（从高到低）：
1. CLI flag (`--schema`)
2. 变更元数据 (`.openspec.yaml`)
3. 项目配置 (`openspec/config.yaml`)
4. 默认值 (`spec-driven`)

**注入流程**：
```
Context (prepend to all artifacts)
   ↓
<project-context>...</project-context>
   ↓
Rules (artifact-specific)
   ↓
<artifact-rules>...</artifact-rules>
   ↓
Template content
```

---

## 五、Spec Delta 评审机制

### 5.1 什么是 Spec Delta？

Spec Delta 是 **变更的可视化表示**，类似于代码 diff：

```markdown
### Requirement: Session expiration

- The system SHALL expire sessions after a configured duration.
+ The system SHALL support configurable session expiration periods.

#### Scenario: Default session timeout

- GIVEN a user has authenticated
- WHEN 24 hours pass without activity
+ WHEN 24 hours pass without "Remember me"
- THEN invalidate the session token

+ #### Scenario: Extended session with remember me
+ - GIVEN user checks "Remember me" at login
+ - WHEN 30 days have passed
+ - THEN invalidate the session token
+ - AND clear the persistent cookie
```

**符号含义**：
- `-` 删除的行
- `+` 新增的行

### 5.2 评审价值

1. **理解变更**：无需阅读代码，高层理解变更意图
2. **快速审查**：秒级理解影响范围
3. **追溯能力**：每个变更都有明确的意图记录

---

## 六、与 AI 工具的集成模式

### 6.1 支持的工具

OpenSpec 支持 **20+ AI 编码助手**：

| 工具 | 集成方式 |
|------|----------|
| Claude Code | Slash Commands (`/opsx:*`) |
| Cursor | `.cursorrules` + Slash Commands |
| GitHub Copilot | `.github/prompts/` |
| Windsurf | `.windsurf/workflows/` |
| Cline | 配置文件自动生成 |
| Amazon Q | 通用 AGENTS.md |
| Codex | Slash Commands |
| ... | 更多工具支持 |

### 6.2 集成原理

```
┌─────────────────────────────────────────────────────────────┐
│                    AI Tool Integration                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  openspec init                                              │
│       │                                                     │
│       ↓                                                     │
│  选择工具类型                                                │
│       │                                                     │
│       ↓                                                     │
│  生成工具特定配置                                             │
│  ├─ Claude Code → .claude/skills/openspec-*                │
│  ├─ Cursor → .cursor/rules + .cursor/commands              │
│  ├─ Copilot → .github/prompts/openspec-*.md                │
│  ├─ Windsurf → .windsurf/workflows/                        │
│  └                                                     │
│       ↓                                                     │
│  AGENTS.md 通用指令（所有工具可读）                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**关键设计**：
- AGENTS.md 是"机器可读的 README"
- 工具特定配置是可选优化层
- 即使无原生集成，AI 也能通过读取 AGENTS.md 工作

---

## 七、CLI 技术实现

### 7.1 安装与初始化

```bash
# 全局安装（需要 Node.js 20.19.0+）
npm install -g @fission-ai/openspec@latest

# 项目初始化
cd your-project
openspec init

# 非交互式初始化（CI/CD 友好）
openspec init --tools claude,cursor
```

### 7.2 核心 CLI 命令

| 命令 | 功能 |
|------|------|
| `openspec init` | 项目初始化 |
| `openspec update` | 刷新 AI 指令 |
| `openspec validate` | 验证规范一致性 |
| `openspec config profile` | 配置工作流模式 |
| `openspec schemas --json` | 查看可用工件类型 |

---

## 八、与其他框架对比

### 8.1 对比表

| 维度 | OpenSpec | Spec Kit (GitHub) | Kiro (AWS) | BMAD |
|------|----------|-------------------|------------|------|
| **设置时间** | 5 分钟 | 30 分钟 | 15 分钟 | 数天 |
| **依赖** | 无（仅需 Node.js） | Python + 复杂依赖 | AWS IDE | 多代理系统 |
| **定制性** | ✅ 高（模板可编辑） | ❌ 低（硬编码） | ❌ 低（锁定 Claude） | ✅ 高 |
| **Brownfield** | ✅ 核心设计 | ⚠️ 有限支持 | ⚠️ 有限支持 | ✅ 支持 |
| **API Keys** | ❌ 不需要 | ❌ 不需要 | ✅ 需要 | ❌ 不需要 |
| **工具锁定** | ❌ 无锁定 | ❌ 无锁定 | ✅ AWS IDE 锁定 | ❌ 无锁定 |

### 8.2 核心优势

1. **轻量级**：无复杂依赖，无 SaaS 平台
2. **隐私友好**：无 API Keys，数据本地存储
3. **高定制**：模板可编辑，即时生效
4. **Brownfield-first**：专为遗留代码库设计

---

## 九、最佳实践

### 9.1 变更范围判断

```
                    ┌─────────────────────────────────────┐
                    │ Is this the same work?              │
                    └──────────────┬──────────────────────┘
                                   │
          ┌────────────────────────┼────────────────────────┐
          │                        │                        │
          ↓                        ↓                        ↓
      Same intent?            >50% overlap?        Can original
      Same problem?           Same scope?          be "done" without
          │                        │                these changes?
          │                        │                        │
    ┌────┴────┐              ┌────┴────┐            ┌───────┴───────┐
    │         │              │         │            │               │
   YES       NO             YES       NO           NO             YES
    │         │              │         │            │               │
    ↓         ↓              ↓         ↓            ↓               ↓
  UPDATE    NEW            UPDATE    NEW          UPDATE          NEW
```

### 9.2 Context Hygiene

- 实现前清空上下文窗口
- 使用高推理模型（Opus 4.5, GPT 5.2）
- 保持规范简洁（约 250 行 vs 其他工具 800 行）

---

## 十、技术总结

### 10.1 核心技术栈

| 层级 | 技术 |
|------|------|
| **存储层** | Markdown 文件系统 |
| **依赖管理** | DAG (有向无环图) |
| **工作流引擎** | OPSX Artifact Graph |
| **上下文注入** | XML 标签包裹 + YAML 配置 |
| **变更表示** | Spec Delta (Diff 格式) |
| **工具集成** | AGENTS.md + 工具特定配置 |

### 10.2 设计哲学总结

| 原则 | 实现 |
|------|------|
| **结构即代码** | 规范与代码同仓库 |
| **变更可审计** | 每个变更有完整生命周期 |
| **AI 可控** | 明确约束减少幻觉 |
| **工具自由** | 无厂商锁定 |
| **渐进式** | Brownfield-first 设计 |

---

## 十一、快速开始

```bash
# 1. 安装
npm install -g @fission-ai/openspec@latest

# 2. 初始化项目
cd your-project
openspec init

# 3. 创建变更
/opsx:propose "Add dark mode support"

# 4. 生成规划文档
/opsx:ff

# 5. 实现
/opsx:apply

# 6. 归档
/opsx:archive
```

---

## 参考资源

- [OpenSpec 官网](https://openspec.dev/)
- [GitHub 仓库](https://github.com/Fission-AI/OpenSpec)
- [OPSX Workflow 文档](https://github.com/Fission-AI/OpenSpec/blob/main/docs/opsx.md)
- [OpenSpec Pro 教程](https://openspec.pro/)
- [Deep Dive 深度解析](https://redreamality.com/garden/notes/openspec-guide/)

---

_本文基于 OpenSpec 官方文档和社区资源整理，深度拆解其技术架构和核心原理。_