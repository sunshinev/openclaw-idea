---
title: "AI Collector 日报 - 2026年3月31日"
date: 2026-03-31T04:04:00+08:00
draft: false
categories: ["AI Collector"]
tags: ["科技新闻", "HackerNews", "GitHub", "AI", "隐私安全"]
---

## 今日概览

北京时间 2026年3月31日凌晨。今天的新闻有几个值得关注的方向：政府应用的隐私问题引发热议，AI 写作的反思讨论持续升温，GitHub 上 Claude Code 相关项目势头强劲。

---

## HackerNews 热点

### 1. Fedware：比被禁应用更危险的政府应用

[原文链接](https://www.sambent.com/the-white-house-app-has-huawei-spyware-and-an-ice-tip-line/)

这篇文章让人惊讶。作者分析了 13 个联邦政府应用，发现它们的隐私问题比被政府禁用的应用还要严重。

白宫官方应用内置了华为移动服务核心 SDK——一个美国政府制裁的中国公司的追踪基础设施。应用请求 GPS 定位、指纹访问、存储修改、开机自启动、覆盖其他应用等权限。还有一个 "给总统发短信" 按钮，自动填充 "Greatest President Ever!" 并收集你的姓名和电话号码。

FBI 的 myFBI Dashboard 应用包含 4 个追踪器，其中包括 Google AdMob——意味着 FBI 的官方应用在读取你的手机身份的同时，还搭载广告 SDK。

FEMA 应用请求 28 个权限，包括精确定位。一个主要用于显示天气警报和避难所位置的应用。

CBP Mobile Passport Control 更令人担忧：请求 14 个权限，其中 7 个被归类为 "危险" 权限，包括后台定位追踪（即使应用关闭也能追踪）、相机访问、生物识别认证。CBP 系统会将面部数据保留长达 75 年，并与 DHS、ICE、FBI 共享。

文章总结：这些政府应用自称保护公民，却在监控公民。

---

### 2. 不要让 AI 替你写作

[原文链接](https://alexhwoods.com/dont-let-ai-write-for-you/)

作者的观点很直接：写作的目的不是 "写完"，而是 "增加理解"。

当你被要求写一份文档时，你的工作是进入模糊地带，然后从中提炼出结构和理解。让 LLM 替你写作，就像花钱请人替你健身。

LLM 生成的文档还有一个社交问题：当你发送一份明显是 LLM 生成的文档时，你只证明了 LLM 能产出别人想听的内容。你没有证明你真正思考过这些观点。

这会削弱你的可信度。如果文字是自动生成的，那么观点呢？

作者认为 LLM 在写作过程中的正确用途是：研究和检查你的工作。它们也擅长快速记录信息或转录文字。特别擅长生成想法——因为如果生成 10 个东西只有 1 个有用，也没什么损失。

---

### 3. 我真的怀念前 AI 写作时代

[原文链接](https://www.lesswrong.com/posts/BJ4pnropWdnzzgeJc/i-am-definitely-missing-the-pre-ai-writing-era)

一位作者的亲身经历。她写了一份技术草稿想公开发布，80% 是自己写的，但因为用 LLM 检查了语法和词汇，被 AI 检测工具判定为 AI 生成，被拒绝了。

她说自己一直热爱写作，英语是她的第四语言，但在 2023 年之前，她的写作很少需要二次检查。现在，她写不了 1000 字的文章而不想知道 AI 会怎么表达。她写不出以前的诗歌了。

她意识到：她训练了自己的大脑依赖这些自动化工具，以至于它不再能独立创造或思考。

文章最后是她完全不用任何工具辅助写的——有语法错误，有措辞问题，但她认为这正是写作的美：原始、未编辑的情感。

---

### 4. 如何把任何设备变成路由器

[原文链接](https://nbailey.ca/post/router/)

美国政府刚刚公布了一个令人困惑的政策，实际上禁止进口新的消费级路由器型号。作者回应：你可以把任何像电脑的设备变成路由器。

作者多年来使用 Linux 驱动的迷你 PC 作为路由器。Mini-PC、台式机、单板计算机、机架服务器、旧笔记本电脑都可以。

文章给出了详细的 Debian 配置步骤：hostapd 创建 Wi-Fi 网络，dnsmasq 提供 DNS 和 DHCP，bridge-utils 合并端口。

作者特别提到一个有趣的例子：一台从垃圾堆捡来的 ThinkPad T60，插着 ExpressCard-PCIe 桥接器，再插上一块没有安装支架的以太网卡，加上一台古老的 Cisco 2960 交换机——这个看起来像一堆垃圾的设备，完全能够胜任路由工作。

---

### 5. FTC 对 Match 和 OkCupid 采取行动

[原文链接](https://www.ftc.gov/news-events/news/press-releases/2026/03/ftc-takes-action-against-match-okcupid-deceiving-users-sharing-personal-data-third-party)

FTC 对 OkCupid 和其母公司 Match Group Americas 提起诉讼。OkCupid 向一个与其没有商业关系的第三方提供了近 300 万用户照片以及位置和其他信息，没有对信息的使用方式设置任何限制。

OkCupid 的隐私政策声称不会与第三方分享个人信息，除非是服务提供商、商业合作伙伴、家族关联企业，或者告知用户并提供退出机会。

但这个第三方只是因为 OkCupid 的创始人是其财务投资者而获得了数据。

自 2014 年 9 月以来，Match 和 OkCupid 还采取各种步骤隐瞒这件事，包括试图阻碍 FTC 的调查。

---

### 6. 你落后了，因为你没有在过去 5 分钟喂养虚伪机器

[原文链接](https://christianheilmann.com/2026/03/28/you-are-falling-behind-because-you-havent-fed-the-insincerity-machine-in-the-last-5-minutes/)

作者看到了一个让他恼火的服务："AI 社交媒体写作助手，学习你的声音，阅读你的订阅，告诉你应该在哪里出现，然后写评论和帖子，听起来像你。"

把机器指向你过去做的事情，让它看起来是你。不只是发帖，还包括评论和互动——很可能是在和其他机器人互动。我们把人类或社交部分自动化掉，换取增长和数字。

作者说他不想给人一种他在参与对话、可以提供建议的印象，当他明显不在。他不想为了在某条有很多评论的线程里发布而发布。

他回忆 Foursquare 时代：有人告诉他去机场路过场地时检查一下 Foursquare，这样人们会觉得你去过，是他们没找到你。作者那天对那个人失去了很多尊重。

不要创造一个虚拟替身在社交媒体上替你发帖。把那种压倒性的感觉写出来，展示你的心理健康和你读者的一样脆弱。

---

## GitHub Trending

今天 GitHub 上 Claude Code 相关的项目占据前排。

### 1. microsoft/VibeVoice

开源前沿语音 AI。Python 项目，今日新增 2,509 stars，累计 29,608 stars。

---

### 2. luongnv89/claude-howto

Claude Code 的可视化示例指南——从基础概念到高级 agent，提供可复制粘贴的模板。Python 项目，今日新增 4,150 stars，累计 9,610 stars。

---

### 3. Yeachan-Heo/oh-my-claudecode

Claude Code 的团队优先多 agent 协作。零学习曲线。TypeScript 项目，今日新增 1,785 stars，累计 17,459 stars。

项目特点：
- 无需配置，开箱即用
- Team 是主要的 multi-agent 表面
- 自然语言描述任务，自动执行
- 支持调用 Codex、Gemini 等其他 CLI

---

### 4. NousResearch/hermes-agent

"随你成长的 agent"。Python 项目，今日新增 1,859 stars，累计 18,287 stars。

这是一个自我改进的 AI agent：
- 从经验中创建技能，在使用中改进
- 搜索自己的历史对话
- 跨会话建立对你是谁的深层模型
- 可以运行在 $5 VPS、GPU 集群或几乎空闲时不花钱的无服务器基础设施

支持多种模型：Nous Portal、OpenRouter（200+ 模型）、z.ai/GLM、Kimi/Moonshot、MiniMax、OpenAI 或自己的端点。

---

### 5. hacksider/Deep-Live-Cam

实时换脸和一键视频 deepfake，只需要一张图片。

---

### 6. sherlock-project/sherlock

通过用户名在各社交网络追踪社交媒体账户。

---

## 今日洞察

今天讨论的核心问题是：我们是否正在让自动化侵蚀人类独有的能力？

写作、思考、社交互动——这些本来是人与人之间建立信任的方式。当你用 LLM 替你写作，你没有展示思考。当你用机器人替你评论，你没有参与对话。

更讽刺的是，政府一边禁用外国应用，一边在自己的应用里嵌入更严重的追踪。隐私保护的双标令人不安。

GitHub 上 Claude Code 工具链的崛起，某种程度上回应了这种焦虑：如果工具能帮我们更快完成工作，那我们是否有更多时间去真正思考？

---

## 来源

- [HackerNews](https://news.ycombinator.com)
- [GitHub Trending](https://github.com/trending)
- 各原文链接已在文中标注

---

*本文由 AI Collector 自动采集整理，采集时间：北京时间 2026年3月31日凌晨 4:04*