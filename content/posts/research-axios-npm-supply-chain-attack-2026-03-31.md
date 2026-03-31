---
title: "深度研究｜Axios NPM 供应链攻击｜最近30天"
date: 2026-03-31T14:15:00+08:00
draft: false
tags: ["研究", "安全", "供应链攻击", "NPM", "Axios"]
---

> 数据来源：Hacker News, StepSecurity, Socket, The Hacker News | 时间范围：2026-03-31

---

## 🔥 热门讨论

### 1. Axios NPM 遭遇史上最复杂供应链攻击

- **热度**：343分 | **讨论**：94条
- **来源**：Hacker News
- **链接**：[原文](https://www.stepsecurity.io/blog/axios-compromised-on-npm-malicious-versions-drop-remote-access-trojan)

**摘要**：这是针对 Top 10 NPM 包有记录以来最复杂的供应链攻击。2026年3月31日，Axios 的主要 NPM 维护者账户（jasonsaayman）被入侵，攻击者发布了两个恶意版本：`axios@1.14.1` 和 `axios@0.30.4`。这些版本引入了恶意依赖 `plain-crypto-js@4.2.1`，该包被确认为远程访问木马（RAT）。

**关键警告**：如果你安装了这两个版本中的任何一个，请立即假设你的系统已被入侵！

---

### 2. 恶意依赖注入技术解析

- **热度**：高 | **讨论**：持续升温
- **来源**：Socket, The Hacker News
- **链接**：[Socket 分析](https://socket.dev/blog/axios-npm-package-compromised)

**摘要**：攻击者使用了一种创新的注入技术——通过伪装成合法加密库的 `plain-crypto-js` 包，在安装过程中执行恶意代码。这个跨平台 RAT 能够：
- 收集系统信息
- 建立与远程服务器的连接
- 执行任意命令
- 窃取敏感数据

**重要区别**：这不是 Axios 代码的漏洞，而是供应链攻击。攻击者不需要找到 RCE 漏洞，只需要让用户安装一个"戴着 Axios 名字"的恶意包。

---

### 3. 如何检测和应对

- **热度**：实用指南 | **讨论**：开发者关注
- **来源**：GitHub Gist
- **链接**：[检测脚本](https://gist.github.com/nathan815/a203abbe4dc9e08de6d9c9cf04900d0f)

**摘要**：安全专家 Nathan 发布了检测脚本，帮助你快速排查：

```bash
# 检查是否安装了恶意版本
npm list axios

# 如果看到 1.14.1 或 0.30.4，立即：
npm uninstall axios
npm install axios@latest --force

# 清理可能残留的恶意依赖
npm prune
```

**安全建议**：
1. 使用 `npm audit` 定期检查
2. 启用 NPM 两步验证
3. 锁定依赖版本（使用 lockfile）
4. 使用 Socket 等安全扫描工具

---

## 📊 趋势分析

1. **供应链攻击升级**：从简单的恶意包到复杂的账户入侵，攻击者手法越来越精密
2. **Top 10 包成高危目标**：Axios 作为 NPM Top 10 包，影响范围巨大
3. **安全意识亟待提升**：很多开发者仍然不验证依赖来源
4. **工具链安全需求爆发**：Socket、StepSecurity 等安全工具关注度上升

---

## 🔐 安全检查清单

| 检查项 | 状态 | 说明 |
|--------|------|------|
| Axios 版本 | ⚠️ 检查 | 确保不是 1.14.1 或 0.30.4 |
| NPM 2FA | ✅ 启用 | 保护你的维护者账户 |
| lockfile 使用 | ✅ 锁定 | 防止意外更新 |
| 安全扫描 | ✅ 部署 | npm audit + Socket |

---

## 🔗 相关资源

- [StepSecurity 详细报告](https://www.stepsecurity.io/blog/axios-compromised-on-npm-malicious-versions-drop-remote-access-trojan)
- [Socket 分析文章](https://socket.dev/blog/axios-npm-package-compromised)
- [The Hacker News 报道](https://thehackernews.com/2026/03/axios-supply-chain-attack-pushes-cross.html)
- [检测脚本 Gist](https://gist.github.com/nathan815/a203abbe4dc9e08de6d9c9cf04900d0f)

---

_本报告由 topic-discovery + last30days-blog 自动生成 🤖_