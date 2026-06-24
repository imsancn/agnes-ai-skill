# Agnes AI 帮助中心

> Agnes AI，让世界级 AI 属于每一个人。

本仓库是 Agnes AI API 的**非官方帮助资源集合**，包含交互式接入助手、完整 API 文档、以及通用支持 Skill，帮助开发者快速接入 Agnes AI 全模态 API（文本、图像、视频）。

---

## 📢 重要公告

**Agnes 2.0 全模态模型 API 正式开放全球免费调用！**

- ✅ 不限期、全模态、API 调用完全免费（RPM 20 以内）
- ✅ 注册官网 → 生成 KEY → 直接调用
- ✅ 文本、图像、视频全能适配
- ✅ 模型持续升级并保持免费

**官方平台：** <a href="https://platform.agnes-ai.com" target="_blank">https://platform.agnes-ai.com</a>

---

## 📦 仓库内容

| 文件 | 说明 | 用途 |
|------|------|------|
| `agnes-ai-assistant.html` | 交互式接入助手 | 浏览器打开，自助完成接入、排查问题（**自动检查 GitHub 更新**） |
| `SKILL.md` | 通用支持 Skill | 兼容 OpenClaw / Claude Code / Claude Desktop / Hermes / Codex / WorkBuddy / Cherry Studio / Opencode / Kimi Work（**加载时检查更新**） |

---

## 🚀 快速开始

### 方式一：Skill（⭐ 推荐，AI 工具链集成）

将 `SKILL.md` 放入你的 AI 工具 Skill 目录，即可让 Agent 自动：

- 指导新用户完成 Agnes AI 接入
- 诊断 401/400/429/500/503 等错误
- 排查视频排队过长、图像生成失败等问题
- 指导 Thinking 模式、流式输出、工具调用等高级功能
- 指导 Codex、OpenClaw、Hermes、Claude CLI/Desktop、WorkBuddy、Cherry Studio、Opencode 等工具集成

**兼容工具：** OpenClaw、Claude Code、Claude Desktop、Hermes、Codex、WorkBuddy、Cherry Studio、Opencode、Kimi Work

**📋 一键安装（复制发给 Agent）：**
```
请读取并安装 Agnes AI 支持 Skill：https://github.com/imsancn/agnes-ai-skill
```

安装完成后，只需配置 Agnes API Key 即可使用。告诉 Agent"我要生图""我要生视频"，Agent 会自动判断并执行。

### 方式二：Agnes 创意空间（推荐自助创作与排查）

无需下载，直接访问 **Agnes 创意空间** 在线使用：

**🔗 <a href="https://ailabing.cn/tools/agnes-creative-space/" target="_blank">https://ailabing.cn/tools/agnes-creative-space/</a>**

**功能：**
- ✨ **生成模式**：文生图、图生图、文生视频、图生视频
- 💬 **对话模式**：Agnes AI 智能助手，支持图片分析、自助排查问题、接入指导
- 🎨 **参数调节**：支持多种尺寸比例、自定义参数调整
- 🔑 **API Key 设置**：输入自己的 Agnes API Key 即可直接使用

**适用场景：**
- 快速体验 Agnes AI 图像/视频生成能力
- 通过对话模式自助排查问题、获取接入建议
- 无需搭建环境，浏览器直接创作

---

### 方式三：交互式 HTML 助手（推荐新手）

1. 下载 `agnes-ai-assistant.html`
2. 用浏览器直接打开（无需服务器）
3. 跟随 4 步向导完成接入
4. 遇到问题用左侧「问题排查」工具自助诊断

**HTML 助手包含：**
- ⚡ 快速接入向导（4 步）
- 🤖 模型速查与选择
- 🛠️ 交互式代码生成器
- 💡 Thinking 模式详解
- 🎬 视频参数调节指南
- ❓ 可搜索 FAQ（20+ 常见问题）
- 🚨 错误码速查表
- 🔍 8 类问题智能诊断
- 📡 接口端点汇总
- ✅ 接入检查清单

### 方式四：API 文档（推荐 Agent / 开发者）

直接查阅 Agnes AI 官方文档，获取最新、最完整的接口信息：

**🔗 <a href="https://agnes-ai.com/doc/overview" target="_blank">https://agnes-ai.com/doc/overview</a>**

官方文档涵盖：
- 概述与核心能力
- Agnes 2.0 Flash 完整参数与示例
- Agnes Image 2.0 / 2.1 Flash 完整参数与示例
- Agnes Video V2.0 完整参数与示例（含异步轮询）
- 各工具链接入指南（OpenClaw / Hermes / Claude CLI / Cherry Studio 等）
- 隐私政策与服务条款

---

### 自动更新提醒

本仓库的所有文件都支持**版本检查与更新提醒**：

- **HTML 助手**：每次打开页面时自动检查 GitHub 是否有新版本，有更新则显示顶部横幅提醒
- **SKILL.md**：每次被 Agent 加载时，Agent 会检查 GitHub 最新 commit，有更新则在回复开头提醒用户

**手动检查更新方式：**
1. 访问 GitHub 仓库查看最新 commit：<a href="https://github.com/imsancn/agnes-ai-skill" target="_blank">https://github.com/imsancn/agnes-ai-skill</a>
2. 或直接让 Agent 执行：`请检查 Agnes AI 支持 Skill 是否有更新`

---

## 🔑 接入要点速记

```
Base URL: https://apihub.agnes-ai.com/v1
Headers:
  Authorization: Bearer YOUR_API_KEY
  Content-Type: application/json
```

**模型名称：**
- 编程/Agent/推理：`agnes-2.0-flash`（默认启用 Thinking 模式）
- 图像生成（推荐）：`agnes-image-2.1-flash`
- 视频生成：`agnes-video-v2.0`

**⚠️ 视频接口重要提醒：**
- **必须用 `video_id` 查询视频结果**：`GET /agnesapi?video_id=<ID>`
- **不要用 `task_id` 查询**，会导致排队异常延长（超过 5 分钟大概率是接口搞错了）

---

## 📖 Agnes Help Skill 教程图

> 一图看懂：如何安装 Skill、如何用 Skill 生图/生视频、如何用 Skill 接入工具、如何用 Skill 排查问题

![Agnes Help Skill 教程信息图](https://raw.githubusercontent.com/imsancn/agnes-ai-skill/main/assets/agnes-help-skill-tutorial.png)

---

## 📚 官方与社区资源

| 资源 | 链接 |
|------|------|
| 官方平台 | <a href="https://platform.agnes-ai.com" target="_blank">https://platform.agnes-ai.com</a> |
| 官方 GitHub | <a href="https://github.com/AgnesAI-Labs/Agnes-AI" target="_blank">https://github.com/AgnesAI-Labs/Agnes-AI</a> |
| 官方模型目录 | <a href="https://github.com/AgnesAI-Labs/Agnes-AI/blob/main/MODEL_CATALOG.md" target="_blank">MODEL_CATALOG.md</a>（RPM / 配额 / 模型规格） |
| 官方文档 | <a href="https://agnes-ai.com/doc/overview" target="_blank">https://agnes-ai.com/doc/overview</a> |
| 操作手册 | <a href="https://agnes-ai.com/doc/常用接入文档" target="_blank">https://agnes-ai.com/doc/常用接入文档</a> |
| 视频文档 | <a href="https://agnes-ai.com/doc/agnes-video-v20" target="_blank">https://agnes-ai.com/doc/agnes-video-v20</a> |
| OpenClaw 接入 | <a href="https://agnes-ai.com/doc/cid1" target="_blank">https://agnes-ai.com/doc/cid1</a> |
| Hermes 接入 | <a href="https://agnes-ai.com/doc/cid2" target="_blank">https://agnes-ai.com/doc/cid2</a> |
| Claude CLI 接入 | <a href="https://agnes-ai.com/doc/cid3" target="_blank">https://agnes-ai.com/doc/cid3</a> |
| Claude Desktop 接入 | <a href="https://agnes-ai.com/doc/cid4" target="_blank">https://agnes-ai.com/doc/cid4</a> |
| WorkBuddy 接入 | <a href="https://agnes-ai.com/doc/cid5" target="_blank">https://agnes-ai.com/doc/cid5</a> |
| Cherry Studio 接入 | <a href="https://agnes-ai.com/doc/cid6" target="_blank">https://agnes-ai.com/doc/cid6</a> |
| Opencode 接入 | <a href="https://agnes-ai.com/doc/cid7" target="_blank">https://agnes-ai.com/doc/cid7</a> |
| Codex++ 接入 | <a href="https://agnes-ai.com/doc/cid8" target="_blank">https://agnes-ai.com/doc/cid8</a> |
| 常见问题 QA | <a href="https://icn1d2hdv39m.feishu.cn/wiki/R7TEwjadJibD62kWeS9cubtpnPi" target="_blank">https://icn1d2hdv39m.feishu.cn/wiki/R7TEwjadJibD62kWeS9cubtpnPi</a> |
| 官方反馈（GitHub Issues） | <a href="https://github.com/AgnesAI-Labs/Agnes-AI/issues" target="_blank">https://github.com/AgnesAI-Labs/Agnes-AI/issues</a> |
| 反馈进度看板 | <a href="https://github.com/users/AgnesAI-Labs/projects/1" target="_blank">https://github.com/users/AgnesAI-Labs/projects/1</a> |
| 支持邮箱 | support@agnes-ai.com |

---

## 🔄 版本历史

| 版本 | 日期 | 更新内容 |
|------|------|----------|
| v2.3.0 | 2026-06-24 | 版本检查升级为5步流程；新增官方问题反馈渠道；新增 API 错误码参考（22个状态码）；常见错误速查表增加反馈引导；触发词增加反馈/错误码 |
| v2.2.0 | 2026-06-23 | 新增「访问与限制」章节（RPM速查表+订阅配额）；增加官方GitHub链接；来源声明精简 |
| v2.1.0 | 2026-06-22 | 512K Context + 64K Max Output + 4K图片全面支持；图像默认9:16 2K分辨率；移除1:1比例 |

查看 <a href="https://github.com/imsancn/agnes-ai-skill/releases" target="_blank">Releases</a> 页面获取完整版本历史与更新说明。

---

## 📎 来源声明

本仓库基于以下项目修改整合而成：

1. <a href="https://github.com/lj1270998580-crypto/Agnes-help-skill" target="_blank">lj1270998580-crypto/Agnes-help-skill</a> — 原始 Skill 框架与文档
2. <a href="https://github.com/cnskycn/agnes-api-skill" target="_blank">cnskycn/agnes-api-skill</a> — 重要参数修正
3. <a href="https://github.com/AgnesAI-Labs/Agnes-AI" target="_blank">AgnesAI-Labs/Agnes-AI</a> — 官方模型目录与 RPM 参考

原始仓库版权归原作者所有，本仓库仅作个人定制使用。

---

## ⚠️ 免责声明

本仓库为社区整理的非官方资源，内容基于 Agnes AI 官方公开文档。如有与官方文档冲突之处，请以官方文档为准。

> Agnes AI 官方公司：Sapiens AI / Agnes AI
