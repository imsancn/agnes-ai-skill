---
name: agnes-ai-support
version: "2.5.0"
description: |
  Agnes AI API 接入支持与问题排查 Skill。帮助新用户完成 Agnes AI API 的接入配置，
  诊断和解决接入过程中遇到的认证、参数、响应、图像生成、视频生成等各类问题。
  涵盖 Thinking 模式开启、视频长度调节、流式输出、工具调用等高级功能指导。
  兼容 OpenClaw、Claude Code、Hermes、Codex 等工具链。
  触发词：Agnes AI、agnes、接入、API Key、401、400、排查、错误、视频生成、图像生成、
  thinking、stream、tool calling、模型选择、Base URL、响应格式、timeout、超时、轮询、
  FAQ、问题排查、调试、免费调用、RPM、video_id、task_id、Prompt 模板、提示词、
  生图、生视频、图生图、文生图、图生视频、文生视频、灰度自检、1M 上下文、4K 图片、4K 图、4K、灰度、自检、反馈、Bug 反馈、问题反馈、错误码、Token Plan、订阅、Starter、Plus、Pro。
default-enabled: true
---

# Agnes AI API 接入支持与问题排查

> **Skill 版本：** v2.6.0
> **适用工具：** OpenClaw / Claude Code / Claude Desktop / Hermes / Codex / WorkBuddy / Cherry Studio / Opencode / Kimi Work
> **更新日期：** 2026-06-30
> **GitHub 仓库：** https://github.com/imsancn/agnes-ai-skill
> **官方平台：** https://platform.agnes-ai.com
> **官方 GitHub：** https://github.com/AgnesAI-Labs/Agnes-AI
> **官方文档：** https://agnes-ai.com/doc/overview
> **操作手册：** https://agnes-ai.com/doc/%E5%B8%B8%E7%94%A8%E6%8E%A5%E5%85%A5%E6%96%87%E6%A1%A3
> **社区教程：** https://github.com/Yacey/agnes-ai-generation-skill
> **社区 Skill：** https://github.com/kangarooking/agnes-free-model-skills
> **ComfyUI 节点：** https://github.com/16nic/comfyui-agnes-ai
> **常见问题 QA（飞书）：** https://icn1d2hdv39m.feishu.cn/wiki/R7TEwjadJibD62kWeS9cubtpnPi

---

## ⚙️ 用户档位配置

> **根据你的 Token Plan 订阅档位修改下方配置，AI 会据此调整所有 RPM、配额和使用建议。**
> 本 Skill 默认假设 **Starter** 档位（$4/月，推荐个人用户）。

**配置方法：** 将下方 `user_tier` 改为你的实际档位即可。

```
# === 用户档位配置 ===
# 可选值：free | starter | plus | pro
# 默认值：starter
user_tier: starter
```

### 各档位参数速查

| 参数 | Free | Starter ⭐ | Plus | Pro |
|------|------|-----------|------|-----|
| 月费 | $0 | $4 | $10 | $50 |
| 文本 RPM（实际可执行） | 20 | 1,000 | 1,000 | 1,000 |
| 图像 1K RPM | 20 | 100 | 100 | 100 |
| 图像 2K RPM | 10 | 80 | 80 | 80 |
| 视频 RPM（实际可执行） | 1 | 5 | 5 | 5 |
| 文本配额 | 无限 | 1,500 次/5小时；15,000 次/周 | 7,500 次/5小时；75,000 次/周 | 30,000 次/5小时；300,000 次/周 |
| 图像配额 | 无限 | 4,000 张/天 | 4,000 张/天 | 4,000 张/天 |
| 视频配额 | 无限 | 500 秒/天 | 500 秒/天 | 500 秒/天 |
| 429 退避建议 | ≥ 3s | ≥ 1s | ≥ 1s | ≥ 1s |

> ⭐ **Starter 是推荐默认档位**：$4/月即可获得文本 RPM 1,000（Free 的 50 倍）、视频 RPM 5、1,500 请求/5小时、4,000 张图/天、500 秒视频/天。

**AI 行为规则：**
1. 读取 `user_tier` 配置值
2. 根据 `user_tier` 查上表获取对应的 RPM 和配额参数
3. 在回复 RPM/配额相关问题时，使用对应档位的数值（而非固定值）
4. 如果用户未配置，默认使用 `starter` 档位参数

### 如何切换默认档位

安装本 Skill 后，默认档位为 `starter`。如果你的 Token Plan 订阅不是 Starter，需要手动切换：

**方法一：直接告诉 AI**
> 「把我的档位切换到 Plus」或「我的档位是 Free」

AI 会自动修改 SKILL.md 中的 `user_tier` 配置，无需你手动编辑文件。

**方法二：手动编辑 SKILL.md**
1. 打开本 Skill 的 SKILL.md 文件
2. 找到头部的 `user_tier: starter` 这一行
3. 将 `starter` 改为你的实际档位（`free` / `plus` / `pro`）
4. 保存文件，下次对话即生效

**方法三：HTML 助手切换**
如果你使用 HTML 助手页面，在侧边栏的「档位」下拉框中选择即可，设置会自动保存到 `localStorage`。

> 💡 **提示**：切换后无需重启，下次对话时 AI 会读取最新配置。如果不确定自己的档位，可访问 [Agnes AI Token Plan](https://agnes-ai.com/pricing) 查看。

### 订阅变更后如何更新档位

如果用户的 Token Plan 订阅发生变化（升级或降级），需要更新档位配置：

1. **确认用户的实际档位**：询问用户当前订阅的是 Free / Starter / Plus / Pro 哪一档
2. **修改 SKILL.md 头部配置**：将 `user_tier` 改为用户实际档位，例如 `user_tier: plus`
3. **如果用户使用 HTML 助手**：提醒用户在 HTML 助手侧边栏的档位下拉框中同步切换
4. **验证生效**：修改后，AI 在后续回复中将自动使用新档位的 RPM 和配额参数

---

## 版本检查与更新提醒

**每次加载本 Skill 时，请执行以下检查：**

### 步骤 1：检测版本

1. 读取 GitHub 仓库最新 commit 信息：
   ```
   https://api.github.com/repos/imsancn/agnes-ai-skill/commits?per_page=1
   ```

2. 比较远程最新 commit 的 `sha` 与本地记录的版本（当前 `v2.6.0`）。

3. 如果 `sha` 不匹配 → 说明有新版本，继续步骤 2。

### 步骤 2：读取更新内容（重要）

如果检测到更新，**必须读取 README.md 的更新记录**来获取本次更新内容：

```
https://raw.githubusercontent.com/imsancn/agnes-ai-skill/main/README.md
```

从 README 中提取「更新记录」表格的第一行（最新的那条），获取：
- 更新日期
- 更新内容

### 步骤 3：向用户报告（在回复开头）

> 📢 **本 Skill 有新版本可用！**
> 
> **当前版本：** v2.6.0（您的版本）
> **最新版本：** [远程版本号]
> 
> **本次更新内容：**
> [从 README 更新记录读取的具体内容]
> 
> **是否需要更新？**
> 如果您想获取最新功能，请回复「更新 Skill」，我会帮您安装最新版。
> 
> **一键更新指令：**
> ```
> 请读取并安装 Agnes AI 支持 Skill 最新版：https://raw.githubusercontent.com/imsancn/agnes-ai-skill/main/SKILL.md
> ```
> 
> 如果您暂时不想更新，也可以继续使用当前版本。

### 步骤 4：用户选择更新

如果用户回复「更新 Skill」或「安装最新版」：
1. 读取最新版 SKILL.md 的 URL
2. 指导用户完成安装（或直接帮用户安装，取决于工具能力）
3. 安装完成后确认：「已更新至最新版本 vX.X.X」

如果用户不更新：
- 继续正常帮助用户，不强制更新
- 但建议用户在方便时更新以获得最新功能和修复

### 步骤 5：HTML 助手和 API 文档检查

如果用户正在使用 HTML 助手或 API 文档，同样提醒：
> 检测到 HTML 助手 / API 文档也有更新，建议从仓库下载最新版。

---

### 版本发布规则

**本 Skill 不会主动发布新版本到 GitHub。** 任何涉及版本号的修改（功能更新、配置调整等），必须：
1. 先在本地完成所有修改
2. 向用户报告修改内容和变更摘要
3. 等待用户明确确认「发布」或「推送」后，才执行 GitHub 推送和 Release 发布
4. 用户未确认时，仅保留本地修改，不自动推送

---


## Agnes AI 官方问题反馈渠道

> **注意：以下渠道是 Agnes AI 官方的反馈渠道，不是本 Skill 的反馈渠道。**
> 
> 当用户遇到 **Agnes AI API 官方服务**的问题（如 API 返回错误、功能异常、模型行为不符合预期等）时，主动提供以下官方渠道。

### 情况 1：用户遇到 Bug 或异常

如果用户报告了 API 异常、返回错误、功能不正常等：

> 🐛 **遇到 Bug 了？** 您可以通过以下官方渠道反馈：
> 
> **1. GitHub Issues（推荐）**
> - 地址：https://github.com/AgnesAI-Labs/Agnes-AI/issues
> - 用途：提交 Bug 反馈、API 使用咨询、需求与建议
> - 团队会按优先级标记处理
> 
> **2. 反馈处理进度看板**
> - 地址：https://github.com/users/AgnesAI-Labs/projects/1
> - 用途：查看问题修复进度、新模型上线、文档更新等
> - 建议收藏，可随时查看
> 
> **3. 支持邮箱**
> - 邮箱：support@agnes-ai.com
> - 适用于：需要详细描述的问题
> 
> **反馈前准备：**
> - 错误码（如 400、500）
> - 请求参数（脱敏 API Key）
> - 复现步骤
> - 期望行为 vs 实际行为

### 情况 2：用户有需求或建议

如果用户提出了功能需求、改进建议：

> 💡 **有建议？** 欢迎通过以下渠道提交：
> 
> **GitHub Issues：** https://github.com/AgnesAI-Labs/Agnes-AI/issues
> - 标题格式：`[Feature Request] 您的建议标题`
> - 内容描述：使用场景 + 期望功能 + 优先级
> - 团队会评估并纳入开发排期
> 
> 也可以在反馈进度看板查看需求处理状态：
> https://github.com/users/AgnesAI-Labs/projects/1

### 情况 3：用户想查询问题处理进度

> 📋 **查询反馈进度：**
> 
> https://github.com/users/AgnesAI-Labs/projects/1
> 
> 看板包含：
> - 所有已提交的 Issues 状态
> - 问题修复进度
> - 新模型上线计划
> - 文档更新进度
> - 开发排期
> 
> 建议收藏此链接，可随时查看官方最新动态。

---

## 如何使用本 Skill

> 本 Skill 是你的 Agnes AI 接入助手。安装后，Agent 会自动理解 Agnes API 的完整信息，帮你完成接入、生成内容、排查问题。

![Agnes Help Skill 教程信息图](https://raw.githubusercontent.com/imsancn/agnes-ai-skill/main/assets/agnes-help-skill-tutorial.png)

### 一、用 Skill 让 Agent 帮你生图 / 生视频

**步骤：**
1. **安装 Skill** → 把本 Skill 加载到 Agent（OpenClaw / Claude Code / Kimi Work 等）
2. **配置 API Key** → 告诉 Agent 你的 Agnes API Key
3. **告诉 Agent 需求** → "帮我生成一张赛博朋克城市夜景图" / "帮我生成一段 5 秒产品展示视频"
4. **Agent 自动执行** → Agent 会根据 Skill 中的参数说明自动选择模型、填写参数、调用 API

**示例对话：**
```
用户：帮我安装这个 skill，并帮我配置 Agnes API Key，然后帮我生成一张图片
Agent：（读取 Skill → 指导配置 → 调用 API → 返回图片）

用户：我已经装好 skill，帮我生成一个 5 秒视频
Agent：（读取 Skill → 选择视频模型 → 提交任务 → 轮询 → 返回视频）
```

> 💡 **你无需选择模型**，Agent 会根据你的需求自动判断并选择合适的模型执行。

> ⚠️ **视频查询一定要用 video_id，不要用 task_id**

---

### 二、用 Skill 让 Agent 接入你开发的工具

**场景：** 你有自己的工具、API 或服务，需要让 Agent 帮你接入和使用。

**步骤：**
1. **安装 Agnes Skill** → 让 Agent 以本 Skill 为参考来理解 Agnes 接口
2. **描述你的工具** → 告诉 Agent 你的工具是什么、需要什么能力
3. **Agent 生成接入方案** → Agent 会参考 Skill 中的接口信息，帮你生成接入代码、示例、调用方法
4. **联调修复** → 根据 Agent 提供的步骤调试，直至成功接入

**示例对话：**
```
用户：我想在我的 Next.js 项目中接入 Agnes AI 的图像生成功能
Agent：（参考 Skill → 生成 Next.js API Route 代码 → 提供前端调用示例）

用户：我的工具需要同时支持文本对话和图像生成
Agent：（参考 Skill → 生成多模态接入方案 → 提供统一封装代码）
```

> ⭐ **Skill 的价值：让 Agent 更懂你的工具，更快帮你完成接入。**

---

### 三、用 Skill 来排查问题，快速拿到方案

**步骤：**
1. **描述问题** → 把错误码、日志、截图发给 Agent，清晰描述你遇到了什么问题
2. **Agent 分析** → Agent 会参考 Skill 中的错误码表、常见问题、排查指南进行诊断
3. **给出方案** → Agent 输出：原因分析 + 修复步骤 + 示例代码

**高效提问模板：**
```
现在【正在做】：我在做什么
目标【期望结果/需求】：我需要什么
问题：遇到了什么问题
已尝试：【做过什么】
请给出：【原因 + 修复步骤 + 示例】
```

**常见错误码速查（Agent 会自动匹配）：**

| 错误码 | 含义 | 常见原因 |
|--------|------|----------|
| 401 | 认证错误 | API Key 错误、过期、格式不对 |
| 400 | 参数错误 | 必填参数缺失、类型错误、response_format 放错位置 |
| 429 | 频率限制 | 文本/图像 RPM 超限，或视频 RPM 超限（Free: 文本/图像 20, 视频 1；Starter: 文本 1,000, 视频 5） |
| 503 | 服务繁忙 | 服务端负载高，稍后重试 |

> **🔔 如果以上错误排查均无效？**
> 
> 请通过 **Agnes AI 官方反馈渠道**提交问题：
> - **GitHub Issues（推荐）：** https://github.com/AgnesAI-Labs/Agnes-AI/issues
> - **反馈进度看板：** https://github.com/users/AgnesAI-Labs/projects/1
> - **支持邮箱：** support@agnes-ai.com
> 
> **反馈前准备：** 错误码 + 请求参数（脱敏 Key）+ 复现步骤 + 期望行为 vs 实际行为
---

### 补充建议

- **新手可先安装 Skill 再使用** → 快速上手，降低使用门槛
- **API 文档可交给 Agent 参考** → 提升理解准确度与效率
- **有更新时查看 GitHub 仓库最新版** → 获取最新功能与修复

---

## 0. 重要公告（2026-06 更新）

### Agnes 2.0 全模态模型 API 正式开放全球免费调用

> **不限期、全模态、API 调用完全免费**
> - 文本 / 图像模型：Free RPM 20，Starter RPM 1,000
> - 视频模型：Free RPM 1，Starter RPM 5
> - 推荐订阅 **Starter $4/月**，获取更高并发和配额
- 注册官网 → 生成 KEY → 直接调用
- 文本、图像、视频全能适配，高效便捷
- 模型会持续升级并保持免费
- 欢迎大家多用、多吐槽

**平台直达：** https://platform.agnes-ai.com

### 视频接口重要更新

🔴 **必须使用 video_id 查询视频结果，禁止使用 task_id：**
```
❌ GET /v1/videos/{task_id}     ← 会导致排队异常延长（超过5分钟）
✅ GET /agnesapi?video_id=<ID>  ← 唯一正确方式，秒级响应
```
- 创建视频任务后，响应中会同时返回 `task_id` 和 `video_id`，**必须使用 video_id 查询结果**
- 使用 `task_id` 查询会导致视频排队异常延长（超过 5 分钟大概率是查询接口用错了）
- 视频文档：https://agnes-ai.com/doc/agnes-video-v20
- 社区 Skill 中的视频接口可能未更新，请以官方文档为准

### ⚠️ Agnes-2.0-Flash 上下文窗口说明（2026-06-22 更新）

> **当前规格：** Agnes-2.0-Flash 上下文窗口参考值为 **512K**，Max Output **65.5K**。
>
> - Context：**512K**（⚠️ **官方文档页面已标注**，但 [官方 MODEL_CATALOG.md](https://github.com/AgnesAI-Labs/Agnes-AI/blob/main/MODEL_CATALOG.md) 仍标注为 256K 并声明 1M 已回滚，两处存在矛盾）
> - Max Output：65.5K
>
> **使用建议：** 默认可尝试 512K 上下文；如遇到与上下文长度相关的错误（如 413/400），请回退到官方标注的 **256K** 作为上限，等待官方正式公告后再使用更大窗口。
>
> ⚠️ **1M 上下文状态**：官方 MODEL_CATALOG.md 声明 "The temporary 1M context window was rolled back in June 2026 for stability"，但 [官方文档页面](https://agnes-ai.com/zh-Hans/docs/agnes-20-flash) 仍标注上下文 512K / 最大输出 65.5K。两处存在矛盾，建议以官方文档页面为准，保留 512K 内测标注。

---

## 1. 快速接入流程（4 步）

### Step 1 — 创建 API Key
1. 访问 https://platform.agnes-ai.com 注册/登录
2. Settings → API Keys → Create new secret key
3. **只显示一次**，立即复制保存
4. 安全提醒：不要暴露在代码仓库、前端代码、截图、公开文档中

### Step 2 — 选择模型

| 场景 | 推荐模型 | 端点 |
|------|----------|------|
| 编程 / Agent / 推理 / 图片理解 | `agnes-2.0-flash`（默认启用 Thinking） | `/v1/chat/completions` |
| 图像生成 / 编辑（推荐） | `agnes-image-2.1-flash` | `/v1/images/generations` |
| 图像快速生成 | `agnes-image-2.0-flash` | `/v1/images/generations` |
| 视频生成 | `agnes-video-v2.0` | `/v1/videos` |

### Step 3 — 配置请求

```
Base URL: https://apihub.agnes-ai.com/v1
Headers:
  Authorization: Bearer YOUR_API_KEY
  Content-Type: application/json
```

### Step 4 — 测试请求

🔴 **CHECKPOINT：执行前确认以下三项全部通过再发送请求：**
1. API Key 已复制且不含多余空格/换行
2. Base URL 为 `https://apihub.agnes-ai.com/v1`（不含 `/chat/completions`）
3. 网络可访问 `apihub.agnes-ai.com`（如有代理/VPN，确认已放行）

```bash
curl https://apihub.agnes-ai.com/v1/chat/completions \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"agnes-2.0-flash","messages":[{"role":"user","content":"Hello!"}],"chat_template_kwargs":{"enable_thinking":true}}'
```

**如果 curl 返回错误 → 按 Section 2 诊断树排查，不要反复重试同一请求。**

> 💡 **agnes-2.0-flash 默认启用 Thinking 模式**，所有请求应包含 `chat_template_kwargs.enable_thinking: true`，以获得最佳推理质量。

---

## 2. 问题诊断决策树

> 🔴 **STOP：排查前先确认 Base URL 正确（`https://apihub.agnes-ai.com/v1`），90% 的"无法连接"是 URL 写错。**

### 2.1 认证类问题

**401 Unauthorized → 按以下顺序逐项检查，某步通过则停止：**
1. ✅ Header 格式正确 → `Authorization: Bearer YOUR_KEY`（Bearer 后有空格）→ 若仍 401 → 下一步
2. ✅ Key 未过期/删除 → 控制台 Settings → API Keys 查看 → 若仍 401 → 下一步
3. ✅ Key 无多余空格/换行 → 重新复制一次 → 若仍 401 → 下一步
4. ✅ 账户未冻结 → 查看控制台账户状态 → 若仍 401 →
5. 🔴 **CHECKPOINT：以上全部确认仍 401 → 用 curl 裸测排除 SDK 问题：**
   ```bash
   curl -s https://apihub.agnes-ai.com/v1/models \
     -H "Authorization: Bearer YOUR_KEY"
   ```
   - 返回模型列表 → Key 有效，问题在 SDK/框架配置
   - 仍 401 → Key 确实无效，到控制台重新创建

**API Key 泄露 → 立即执行（不可逆操作前确认）：**
- 🔴 **STOP：先在控制台删除旧 Key，再创建新 Key**
- 用 `git filter-repo` / BFG 清理代码仓库历史中的敏感信息
- 检查环境变量是否被意外提交到公开仓库

### 2.2 请求类问题（400 Bad Request）

**通用排查清单：**
1. JSON 格式是否合法（用 jsonlint 校验）
2. 必填参数是否缺失
3. 参数类型是否正确

**Chat 模型 400：**
- 必填：`model` + `messages`
- `messages` 必须是数组，元素含 `role` 和 `content`
- `temperature` 范围 0-2
- 图片 URL 输入时，`content` 必须是数组格式

**图像模型 400 → 按以下分支排查：**
- 文生图必填：`model` + `prompt` + `size` → 缺任一项 → 补齐后重试
- 图生图必填：`model` + `prompt` + `size` + `extra_body.image` → 缺 `extra_body.image` → **🔴 STOP：image 必须放在 extra_body 中，不能放顶层！放顶层必 400**
- **response_format 必须放在 `extra_body` 中** → 放根级 → 400，移入 `extra_body` 重试
- 图生图**不需要**传 `tags: ["img2img"]` → 传了可能出错 → 删除该参数
- `size` 格式错误 → 见下方尺寸列表 → 改为正确格式重试

**视频模型 400 → 按以下分支排查：**
- 必填：`model` + `prompt` → 缺任一项 → 补齐后重试
- `num_frames` 不满足约束 → 只支持 10 个合法值：81/121/161/201/241/281/321/361/401/441 → 改为合法值
- 🔴 **num_frames 使用非法值不会报错，任务会卡在队列永不执行**
- `frame_rate` 超范围 → 必须 1-60 → 改为有效值
- 图生视频 `image` URL 不可达 → 确认公网可访问后重试

### 2.3 响应类问题

**无响应 / 超时 → 按以下分支处理：**
- Chat 请求超时（>30s）→ 检查 `max_tokens` 是否过大 → 缩减后重试 → 仍超时 → 检查网络能否访问 `apihub.agnes-ai.com`
- 图像请求超时（>360s）→ 正常范围，继续等待 → 超过 360s → 检查 `size` 是否过大或网络问题
- 视频任务超时 → 异步任务，必须轮询 → 如果轮询超过 10 分钟无结果 → 检查是否用了 task_id 而非 video_id（见 2.4）
- 网络层面 → `curl -sI https://apihub.agnes-ai.com` 无响应 → 检查防火墙/代理/DNS

**503 Service Unavailable → 指数退避重试：**
- 第1次：等 1s 重试 → 仍 503 →
- 第2次：等 2s 重试 → 仍 503 →
- 第3次：等 4s 重试 → 仍 503 →
- 第4次：等 8s 重试 → 仍 503 → 🔴 **STOP：连续 4 次均 503 → 服务端异常，等待 30 分钟后重试或联系 support@agnes-ai.com**

**429 Rate Limited → 降频处理：**
- 文本 / 图像模型：Free RPM 20，Starter RPM 1,000（详见第 5 节）
- 视频模型：Free RPM 1（每分钟只能生成 1 个视频），Starter RPM 5
- 超限 → 实现客户端队列，Free 请求间隔 ≥ 3s，Starter ≥ 1s
- 持续 429 → 检查是否有并发请求 → 合并为顺序请求

### 2.4 视频排队过长（> 5 分钟）

🔴 **STOP：视频排队超过 5 分钟，99% 是查询接口用错了。按以下流程排查：**

1. 检查查询接口 → 是 `GET /agnesapi?video_id=<ID>` 吗？
   - **是 video_id →** 正确接口，继续等待或检查视频 ID 是否有效
   - **是 task_id →** 🔴 **这是根因！** 立即切换为 video_id 查询方式：
     ```
     ❌ 错误：GET /v1/videos/{task_id}     ← 会导致排队异常延长（超过5分钟）
     ✅ 正确：GET /agnesapi?video_id=<ID>  ← 秒级响应
     ```
2. 确认 video_id 获取方式 → 创建任务后，响应体中**同时返回 `task_id` 和 `video_id`**，必须取 `video_id` 字段
3. 检查 num_frames → 是否为 10 个合法值之一？非法值会导致任务卡在队列永不执行
4. 轮询间隔：5 秒一次，不要频繁轮询

### 2.5 输出质量问题

- 优化 Prompt 结构：`[Role] + [Task] + [Context] + [Requirements] + [Output Format]`
- 编程/推理任务开启 Thinking 模式
- 调整 `temperature`：确定性任务 0.1-0.3，创意任务 0.7-1.0
- 检查 `max_tokens` 是否足够，避免截断
- 图像任务：提供详细的场景、风格、光照、构图描述

---

## 3. 高级功能指南

### 3.1 Thinking 模式（agnes-2.0-flash）

**OpenAI 兼容格式：**
```json
{
  "model": "agnes-2.0-flash",
  "messages": [...],
  "chat_template_kwargs": {
    "enable_thinking": true
  }
}
```

**Anthropic 兼容格式：**
```json
{
  "model": "agnes-2.0-flash",
  "messages": [...],
  "thinking": {
    "type": "enabled",
    "budget_tokens": 2048
  }
}
```

**budget_tokens 建议：**
- 简单编码：2048
- 复杂调试/重构：4096+
- 多步骤 Agent：4096+

### 3.2 流式输出（SSE）

请求体加 `"stream": true`
- 响应逐块返回，每块以 `data:` 开头
- 最后以 `data: [DONE]` 结束
- 需正确解析 `choices[0].delta.content` 并拼接

### 3.3 工具调用

1. 请求中提供 `tools` 数组（含 function 定义）
2. 模型返回 `finish_reason: "tool_calls"`
3. 解析 `choices[0].message.tool_calls`
4. 执行函数后，以 `role: tool` 回传结果
5. 模型根据工具结果生成最终回复

> ⚠️ **Responses API 多轮函数调用不可靠**：Agnes 的 Responses API 多轮 function calling 不适用于 Agent 自动工具循环。实测表明 provider 可能在 `status=completed` 时返回 `function_call`，而使用 `function_call_output` + `previous_response_id` 提交可能失败。建议使用 Chat Completions 路径进行工具调用，将 Responses API 的 function calling 仅视为尽力而为的请求格式兼容。

### 3.4 视频长度调节

**公式：** `seconds = num_frames / frame_rate`

**约束：**
- `num_frames ≤ 441`
- `num_frames` 仅支持以下 **10 个合法值**：**81, 121, 161, 201, 241, 281, 321, 361, 401, 441**（必须满足 8n + 1 且 ≥ 81）
- 🔴 **使用非法值会导致任务卡在队列永不执行，不会报错！**
- `frame_rate`：1-60，推荐 24

**常用配置：**
| 时长 | num_frames | frame_rate |
|------|-----------|------------|
| ~3s | 81 | 24 |
| ~5s | 121 | 24 |
| ~7s | 161 | 24 |
| ~8s | 201 | 24 |
| ~10s | 241 | 24 |
| ~12s | 281 | 24 |
| ~13s | 321 | 24 |
| ~15s | 361 | 24 |
| ~17s | 401 | 24 |
| ~18s | 441 | 24 |

**更长视频：** 增加 num_frames 或降低 frame_rate

**参数推荐场景表：**

| 使用场景 | 推荐设置 |
|----------|----------|
| 标准视频 | `width: 1152, height: 768, num_frames: 121, frame_rate: 24` |
| 短视频社交 | `num_frames: 81 或 121, frame_rate: 24` |
| 更平滑运动 | 更高的 frame_rate（24 或 30） |
| 可复现结果 | 设置固定 seed |
| 关键帧过渡 | `extra_body.mode: "keyframes"` |
| 避免不需要的内容 | 使用 negative_prompt |

### 3.5 视频音频说明

> 💡 **生成视频自带音频**（音画同出，AAC 编码），无需额外配音或参数设置。模型会根据 prompt 描述自动生成环境音效、背景音乐，以及口语对话。**支持中英文双语**，实测可在 prompt 中描述具体台词（如"说'欢迎光临，请坐'"），模型会尝试按描述内容生成。
>
> ⚠️ 台词内容由模型按场景理解生成，不一定 100% 复现 prompt 中指定的文字。如需精确配音/旁白，建议后期用 TTS 工具合成。

### 3.6 视频任务完整流程与代码示例

**创建任务 → 获取 video_id → 轮询查询 → 下载视频**

```python
import requests, time

API_KEY = "your-api-key"
HEADERS = {
    "Authorization": f"Bearer {API_KEY}",
    "Content-Type": "application/json"
}

# 1. 创建视频任务
resp = requests.post("https://apihub.agnes-ai.com/v1/videos", headers=HEADERS, json={
    "model": "agnes-video-v2.0",
    "prompt": "A cinematic shot of a cat walking on the beach at sunset",
    "height": 768,
    "width": 1152,
    "num_frames": 121,
    "frame_rate": 24
})
data = resp.json()
task_id = data["task_id"]
video_id = data["video_id"]  # 🔴 必须使用 video_id 查询！
print(f"Task created: {task_id}")
print(f"Video ID: {video_id}")

# 2. 轮询结果（使用 video_id + /agnesapi）
while True:
    result = requests.get(
        f"https://apihub.agnes-ai.com/agnesapi?video_id={video_id}",
        headers=HEADERS
    ).json()
    status = result["status"]
    progress = result.get("progress", 0)
    print(f"Status: {status}, Progress: {progress}%")

    if status == "completed":
        # 视频 URL 在 remixed_from_video_id 字段
        video_url = result.get("video_url") or result.get("remixed_from_video_id")
        print(f"Video URL: {video_url}")
        break
    elif status == "failed":
        print(f"Failed: {result.get('error')}")
        break
    time.sleep(5)
```

**完成响应示例：**
```json
{
  "id": "task_YOUR_TASK_ID",
  "model": "agnes-video-v2.0",
  "object": "video",
  "status": "completed",
  "progress": 100,
  "seconds": "10.0",
  "size": "1280x768",
  "remixed_from_video_id": "https://storage.googleapis.com/agnes-aigc/aigc/videos/2026/06/03/video_xxxxxx.mp4"
}
```

> ⚠️ **视频 URL 字段**：完成时视频下载链接存储在 `remixed_from_video_id` 字段，**不是** `video_url` 字段。
>
> ⚠️ **速率限制**：轮询可能返回 `429 Too Many Requests`，建议在代码中处理重试。

### 3.7 图像输出格式

**URL 输出：**
```json
{
  "extra_body": {
    "response_format": "url"
  }
}
```

**Base64 输出（文生图）：**
```json
{
  "return_base64": true
}
```

**Base64 输出（图生图）：**
```json
{
  "extra_body": {
    "response_format": "b64_json"
  }
}
```

---

## 4. 各模型参数速查

### agnes-2.0-flash
- 端点：`POST /v1/chat/completions`
- Context：512K（⚠️ 官方文档页面已标注，MODEL_CATALOG.md 仍为 256K 并声明 1M 已回滚，遇上下文错误请回退 256K）
- Max Output：65.5K
- 额外参数：stream, tools, tool_choice, chat_template_kwargs, thinking
- 支持图片 URL 输入（messages[].content 数组格式）
- 支持工具调用、Thinking 模式、流式输出
- **默认启用 Thinking 模式**（`chat_template_kwargs.enable_thinking: true`）
- 价格：Input $0.1/1M, Output $0.2/1M（**现价 $0**，RPM 详见第 5 节）

### agnes-image-2.0/2.1-flash
- 端点：`POST /v1/images/generations`
- 必填：model, prompt, size
- 图生图必填：`extra_body.image`（URL 数组或 Data URI Base64，**必须放在 extra_body 中！**）
- size：见下方尺寸表
- 输出：URL 或 Base64
- 价格：$0.003/image（**现价 $0**，RPM 详见第 5 节）

**图像尺寸表：**

> 💡 **默认使用 9:16 2K 分辨率**（`1440x2560`），仅当用户明确要求时才使用其他比例/分辨率。4K 图片已全面支持（所有用户可用）。

| 比例 | 2K（默认） | 4K | 说明 |
|------|-----------|-----|------|
| 4:3 | 1024x768 | — | 标准横版 |
| 3:4 | 768x1024 | — | 标准竖版 |
| 16:9 | 2560x1440 | 3840x2160 | 宽屏横版 |
| 9:16 | 1440x2560 | 2160x3840 | 宽屏竖版（默认） |

### agnes-video-v2.0
- 创建：`POST /v1/videos`
- 🔴 **查询（唯一正确方式）：`GET /agnesapi?video_id=<ID>`**
- ❌ **禁止使用：`GET /v1/videos/{task_id}`**（会导致排队异常延长）
- 创建任务后响应中同时返回 `task_id` 和 `video_id`，**必须使用 video_id 查询**
- 参数：model, prompt, image, mode, height(768), width(1152), num_frames, frame_rate(24), num_inference_steps, seed, negative_prompt, extra_body.image, extra_body.mode
- 🔴 **num_frames 仅支持 10 个值**：81, 121, 161, 201, 241, 281, 321, 361, 401, 441（非法值会导致任务卡在队列永不执行）
- 价格：$0.005/second（**现价 $0**，RPM 详见第 5 节）

---

## 5. 访问与限制

> 以下 RPM 和配额为 **2026-06-28 官方公开参考值**（[官方 MODEL_CATALOG.md](https://github.com/AgnesAI-Labs/Agnes-AI/blob/main/MODEL_CATALOG.md) · [官方 Token Plan FAQ](https://github.com/AgnesAI-Labs/Agnes-AI/blob/main/docs/TOKEN_PLAN_FAQ.md)），可能随定价或容量调整而变化。以官方平台控制台为最终依据。

> 🎯 **默认假设：本 Skill 推荐参数基于 Starter 档位**（$4/月），适合大多数个人用户。如果你使用 Free 档位，请注意 RPM 更低（文本/图像 20/min, 视频 1/min）。推荐订阅 Starter 以获得最佳体验。

### 用户类型

| 用户类型 | 说明 |
|----------|------|
| Free / 默认 | 未订阅 Token Plan 或未企业认证的用户 |
| **Starter** ⭐ | **$4/月**，推荐个人用户，文本 RPM 1,000，视频 RPM 5 |
| Enterprise | 已完成企业认证的用户，更高基准限制 |
| Token Plan | 已订阅 Token Plan 的用户（Starter/Plus/Pro），更高 RPM 和订阅配额 |

> 💡 一个用户可以同时持有多种类型的 Key（Free Key、Enterprise Key、Token Plan Key），每种 Key 使用独立的限制池。同类型多 Key 共享同一池，不会增加总 RPM 或配额。

### 文本模型 RPM

| 用户类型 | 公开请求 RPM | 实际可执行 RPM |
|----------|------------:|--------------:|
| Free / 默认 | 30 | 20 |
| **Starter** ⭐ | 1,000 | 1,000 |
| Enterprise | 60 | 40 |
| Token Plan (Plus/Pro) | 1,000 | 1,000 |

### 图像模型 RPM

> 图像模型 RPM 因分辨率而异。更高分辨率通常 RPM 更低，因为消耗更多计算资源。

| 用户类型 | 分辨率 | 公开请求 RPM | 实际可执行 RPM |
|----------|--------|------------:|--------------:|
| Free / 默认 | 1K | 30 | 20 |
| Free / 默认 | 2K | 20 | 10 |
| Free / 默认 | 3K | 2 | 1 |
| Free / 默认 | 4K | 1 | 1 |
| **Starter** ⭐ | 1K | 120 | 100 |
| **Starter** ⭐ | 2K | 120 | 80 |
| **Starter** ⭐ | 3K | 2 | 1 |
| **Starter** ⭐ | 4K | 2 | 1 |
| Enterprise | 1K | 60 | 40 |
| Enterprise | 2K | 40 | 20 |
| Enterprise | 3K | 2 | 1 |
| Enterprise | 4K | 2 | 1 |
| Token Plan (Plus/Pro) | 1K | 120 | 100 |
| Token Plan (Plus/Pro) | 2K | 120 | 80 |
| Token Plan (Plus/Pro) | 3K | 2 | 1 |
| Token Plan (Plus/Pro) | 4K | 2 | 1 |

### 视频模型 RPM

> 视频生成同时受 RPM 限制和每日视频时长配额约束。

| 用户类型 | 公开请求 RPM | 实际可执行 RPM |
|----------|------------:|--------------:|
| Free / 默认 | 2 | 1 |
| **Starter** ⭐ | 6 | 5 |
| Enterprise | 2 | 2 |
| Token Plan (Plus/Pro) | 6 | 5 |

### Token Plan 订阅配额

| 方案 | 价格 | agnes-2.0-flash | agnes-image-2.0/2.1-flash | agnes-video-v2.0 |
|------|-----:|----------------|--------------------------|------------------|
| Starter ⭐ | $4/月 | 1,500 请求/5小时；15,000 请求/周 | 4,000 张/天 | 500 秒/天 |
| Plus | $10/月 | 7,500 请求/5小时；75,000 请求/周 | 4,000 张/天 | 500 秒/天 |
| Pro | $50/月 | 30,000 请求/5小时；300,000 请求/周 | 4,000 张/天 | 500 秒/天 |

> ⭐ **Starter 是推荐默认档位**：$4/月即可获得文本 RPM 1,000（是 Free 的 50 倍）、视频 RPM 5、1,500 请求/5小时、4,000 张图/天、500 秒视频/天。

> **配额计算方式：**
> - 文本配额按请求计数
> - 图像配额按生成图片计数
> - 视频配额按生成视频时长（秒）计数
> - Token Plan 用户同时受 RPM 限制和订阅配额约束

### API Key 类型与限制池

| Key 类型 | 适用用户 | 限制行为 |
|----------|----------|----------|
| Free / 默认 Key | 所有用户 | 使用 Free/默认 RPM 池 |
| **Starter Key** ⭐ | Token Plan Starter 用户 | 使用 Starter RPM 和订阅配额池 |
| Enterprise Key | 企业认证用户 | 使用 Enterprise RPM 池 |
| Token Plan Key (Plus/Pro) | Token Plan Plus/Pro 用户 | 使用 Token Plan RPM 和订阅配额池 |

> ⚠️ 同类型多 Key 共享同一限制池，创建多个同类型 Key 不会增加总 RPM 或配额。不同类型 Key 使用独立池。

### RPM 字段说明

| 字段 | 含义 |
|------|------|
| 公开请求 RPM | 用户每分钟允许发起的请求数 |
| 实际可执行 RPM | 经服务端调度和容量约束后，每分钟实际可执行的请求数 |

---

## 5A. API 错误码参考（官方）

> 以下内容整理自 [Agnes AI 官方错误码文档](https://github.com/AgnesAI-Labs/Agnes-AI/blob/main/docs/ERROR_CODES.md)（2026-06-22 更新）。
> 生产环境相关的模型权限、速率限制和配额规则可能随时间变化，请以官方文档或平台控制台为准。

| 状态码 | 含义 | 常见原因 | 建议处理 |
|--------|------|----------|----------|
| 400 | 请求参数错误 | JSON 格式错误、必填参数缺失、参数类型不正确、上下文超限、图片/视频参数异常 | 检查 JSON 与必填参数，压缩上下文，核对尺寸、帧率、时长等模型参数 |
| 401 | 鉴权失败 | API Key 错误/失效/未正确配置，Header 未携带 Authorization | 使用正确的 Bearer 格式，检查多余空格或换行，确认 Key 有效 |
| 402 | 余额/额度不足 | 余额不足、Token Plan 额度不足、请求消耗超过可用额度 | 检查余额、Token Plan 和订阅状态，充值或升级 |
| 403 | 无访问权限 | 未开通模型权限、企业认证或订阅未生效、IP/地区/网络受限 | 确认账号与模型权限，检查 Key 归属，更换网络后重试 |
| 404 | 接口/模型/资源不存在 | Base URL 错误、路径错误、重复拼接 `/v1`、模型名称错误 | 检查 Base URL、接口路径、模型名，确认工具未重复拼接 `/v1` |
| 405 | 请求方法不支持 | 应使用 POST 却使用 GET，或工具请求方法配置错误 | 查看接口文档，修改客户端或工具的请求方法 |
| 408 | 请求超时 | 网络不稳定、请求内容过大、长任务耗时较长、客户端超时过短 | 重试并检查网络，缩短输入或降低生成规格，增加客户端超时 |
| 409 | 请求冲突 | 重复提交同一任务、相同请求正在处理中 | 避免短时间重复提交，等待当前任务完成 |
| 413 | 请求体过大 | 输入上下文过长、Base64 图片过大、一次性提交内容过多 | 压缩请求内容，减少上下文，分批提交；优先使用公网图片 URL |
| 415 | 文件格式不支持 | 上传文件格式不支持、Content-Type 设置错误 | 检查支持格式，图片建议使用 PNG/JPG/JPEG/WEBP |
| 422 | 参数内容不合法 | 参数值异常、seed/尺寸/帧率超出范围、图片 URL 无法访问、Base64 异常 | 查看具体报错信息，检查图片/视频参数、seed 范围、URL 可访问性 |
| 429 | 速率限制（详见第 5 节） | 文本/图像 RPM 超限、视频 RPM 超限（Free: 视频 1/min；Starter: 视频 5/min）、并发请求过多、自动重试频率过快 | 等待后重试，降低频率和并发，视频需排队等待，使用指数退避 |
| 431 | 请求 Header 过大 | 自定义 Header 过长、Authorization 异常 | 移除不必要的 Header，检查 Authorization |
| 499 | 客户端断开连接 | 客户端超时、工具中断、网络断开 | 重新请求，增加客户端超时，长任务使用异步轮询 |
| 500 | 服务器内部错误 | 请求参数异常触发服务异常、临时上游问题 | 检查参数，确认图片/视频尺寸符合要求，简化请求后重试 |
| 502 | 网关错误 | 本地网络异常、DNS 解析异常、代理配置异常 | 检查网络和代理配置，更换网络，修改 DNS |
| 503 | 服务暂时不可用 | 模型名称错误、路由配置异常、服务短时间不可用 | 检查模型名，重新获取模型列表，选择兜底模型，稍后重试 |
| 504 | 网关等待上游超时 | 长上下文/图片/视频任务耗时过长、服务负载较高 | 稍后重试，缩短输入，降低生成规格，减少并发 |
| 520 | 未知网关错误 | 网络链路异常、上游服务临时波动 | 使用退避重试，检查网络、代理和 DNS |
| 522 | 网关连接超时 | 网络连接超时、网关到上游连接异常 | 重试，检查本地网络、代理和 DNS 设置 |
| 524 | 上游处理超时 | 长上下文任务、图片/视频生成耗时过长 | 稍后重试，降低请求复杂度，使用异步轮询 |

> **接入建议：**
> - 对 408、429、500、502、503、504、520、522、524 等临时性错误加入指数退避重试
> - 视频生成请先创建任务，再使用 `video_id` 轮询查询结果
> - 图片和视频请求在重试前，请先检查尺寸、宽高比、帧率、时长、公网 URL 可访问性

---

## 6. 工作流决策树

> 当用户提出生成需求时，按以下决策树选择模型和参数：

```
用户请求生成/编辑内容
│
├─ 用户需要生成图片或编辑图片？
│   ├─ 用户提供了图片 URL 或本地图片路径？
│   │   ├─ 是 → 使用 agnes-image-2.0-flash（图生图/编辑）
│   │   │   ├─ 单图编辑 → extra_body.image 传单张 URL
│   │   │   └─ 多图合成 → extra_body.image 传多张 URL
│   │   └─ 否 → 使用 agnes-image-2.1-flash（文生图）
│   └─ 结果 → 图片 URL 或 base64
│
├─ 用户需要生成视频？
│   ├─ 纯文本描述 → 文生视频（无 image 参数）
│   ├─ 单张图片动画化 → 图生视频（image 参数传单 URL）
│   ├─ 多张图片引导 → 多图视频（extra_body.image）
│   └─ 关键帧过渡 → 关键帧动画（extra_body.mode: "keyframes"）
│   └─ 结果 → 异步任务：POST /v1/videos → 🔴 用 video_id + GET /agnesapi 轮询
│
└─ 用户需要纯文本对话？
    ├─ 通用对话 → 使用 agnes-2.0-flash
    ├─ 需要流式输出 → 设置 `stream: true`
    ├─ 需要思考过程 → 设置 `chat_template_kwargs: { enable_thinking: true }`（默认已启用）
    └─ 需要工具调用 → 提供 `tools` 参数
```

---

## 7. 常见错误速查表

| 状态码 | 含义 | 常见原因 | 解决方案 |
|--------|------|----------|----------|
| 400 | 请求无效 | 参数错误、JSON 格式、必填缺失 | 检查格式、参数类型、必填字段 |
| 401 | 未授权 | API Key 错误/过期 | 检查 Authorization 头，确认 Key 有效 |
| 404 | 不存在 | 视频/任务 ID 错误 | 确认 ID 正确 |
| 429 | 速率限制 | 文本/图像请求过于频繁，或视频 RPM 超限（Free: 文本/图像 20/min, 视频 1/min；Starter: 文本 1,000/min, 视频 5/min） | 降低频率，视频需排队等待，实现退避重试（详见第 5 节） |
| 500 | 服务器错误 | 服务端异常 | 稍后重试，持续出现联系支持 |
| 503 | 服务繁忙 | 负载高或维护 | 指数退避重试，关注公告 |

---

## 8. 接入检查清单

### 通用
- [ ] 已注册账户并创建 API Key（https://platform.agnes-ai.com）
- [ ] Base URL 正确：`https://apihub.agnes-ai.com/v1`
- [ ] 请求头包含 Authorization 和 Content-Type
- [ ] API Key 未暴露在公开代码中
- [ ] 已实现错误处理和重试逻辑
- [ ] 了解 RPM 限制（Free: 文本/图像 20/min, 视频 1/min；Starter: 文本 1,000/min, 视频 5/min，详见第 5 节）

### Chat
- [ ] 模型名称拼写正确
- [ ] messages 数组格式正确
- [ ] **agnes-2.0-flash 已默认启用 Thinking 模式（chat_template_kwargs.enable_thinking: true）**
- [ ] 图片 URL 使用数组 content 格式
- [ ] 流式输出能正确解析 SSE
- [ ] 工具调用能处理 finish_reason: tool_calls

### 图像
- [ ] 模型名称正确（推荐 2.1）
- [ ] 文生图传 model + prompt + size（支持多种比例与分辨率，见尺寸表）
- [ ] 图生图传了 `extra_body.image` 数组（**不能放顶层！**）
- [ ] response_format 在 extra_body 中
- [ ] 未传 tags: ["img2img"]
- [ ] 输入图片 URL 公网可访问
- [ ] 客户端超时 ≥ 60s

### 视频
- [ ] 模型名称：agnes-video-v2.0
- [ ] num_frames 为 10 个合法值之一：81/121/161/201/241/281/321/361/401/441
- [ ] frame_rate 在 1-60
- [ ] 图生视频图片 URL 公网可访问
- [ ] **已实现轮询查询（间隔 5s）**
- [ ] **使用 video_id 查询结果（不要用 task_id）**
- [ ] 视频排队超过 5 分钟时检查查询接口是否正确

---

## 9. 最佳实践

### 反例与黑名单（禁止事项）

> 🔴 以下是使用 Agnes AI 时的常见错误和禁止操作，务必避免：

| # | 禁止操作 | 正确做法 | 后果 |
|---|----------|----------|------|
| 1 | 图生图时 `image` 放在请求体顶层 | 放在 `extra_body.image` 中 | 400 Bad Request |
| 2 | `response_format` 放在请求体根级 | 放在 `extra_body.response_format` 中 | 400 Bad Request |
| 3 | 用 `task_id` 查询视频结果 | 用 `video_id` 查询：`GET /agnesapi?video_id=<ID>` | 视频排队异常延长（>5分钟），永不返回 |
| 4 | Base URL 写到 `/v1/chat/completions` | 只写到 `/v1` | 请求路径错误，404 或重复路径 |
| 5 | API Key 字段填写 `Bearer sk-xxx` | 只填 `sk-xxx`，SDK 自动添加 Bearer | 401 Unauthorized |
| 6 | 图生图时传 `tags: ["img2img"]` | 不需要该参数，删除即可 | 可能导致意外行为 |
| 7 | 前端代码硬编码 API Key | 存储在环境变量或密钥管理服务中 | Key 泄露风险 |
| 8 | `num_frames` 使用任意值 | 只用 10 个合法值：81/121/161/201/241/281/321/361/401/441 | 任务卡在队列永不执行，不会报错 |
| 9 | 429 后立即重试 | 等待后重试，Free ≥ 3s，Starter ≥ 1s，实现指数退避 | 持续被限流 |
| 10 | 503 后无限重试 | 最多 4 次指数退避（1s→2s→4s→8s），仍失败则等待 30 分钟 | 浪费请求配额 |
| 11 | 视频 Prompt 用中文 | 视频必须用英文 Prompt；中文会导致视频出现英文字幕/语音（底层模型以英文训练为主）。如必须中文，用 negative_prompt + 明确约束语言 | 画面出现非预期英文文字/语音 |

### Prompt 编写
- 使用结构化格式：`[Role] + [Task] + [Context] + [Requirements] + [Output Format]`
- 编程任务：提供语言、框架、错误信息、期望行为
- Agent 任务：清晰描述目标、可用工具、任务约束
- 图像任务：主体 + 场景 + 风格 + 光照 + 构图 + 质量要求
- 视频任务：主体 + 动作 + 场景 + 镜头运动 + 光照 + 风格
- 🔴 **视频 Prompt 必须用英文**：中文 Prompt 会导致视频中出现英文字母/语音，这是模型训练数据特性（底层视觉模型以英文为主，即使中文 prompt 也会"默认"生成英文内容）
- **中文 Prompt 缓解方法**（效果有限，仍建议优先用英文）：
  1. 在 prompt 中明确约束：`"The shop sign displays Chinese text '欢迎光临', NO English letters at all"`
  2. 使用 negative_prompt：加入 `"English text, English letters, English speech, western characters, alphabets"`
  3. 如需口语对话，在 prompt 中指定语言和内容：`She says "今天天气真好" in Chinese, soft voice`

### 性能优化
- 需要推理/编程使用 agnes-2.0-flash + Thinking（默认已启用）
- 图像生成设置合理超时（60-360s）
- 视频生成使用异步 + 轮询模式
- 实现指数退避重试（1s → 2s → 4s → 8s）
- 🔴 **视频查询必须用 video_id**，`task_id` 会导致排队异常延长永不返回
- **Starter 用户优势**：文本 RPM 1,000（Free 仅 20），视频 RPM 5（Free 仅 1），4,000 张图/天，500 秒视频/天

### 安全
- API Key 存储在环境变量或密钥管理服务中
- 前端代码中绝不硬编码 Key
- 定期轮换 API Key
- 监控异常用量

### 灰度自检（检查你的账户是否支持 1M 上下文）

> Agnes AI 曾灰度测试以下能力：
> - **1M Token 超长上下文**：⚠️ 官方 MODEL_CATALOG.md 声明已于 2026 年 6 月回滚（"rolled back for stability"），但 [官方文档页面](https://agnes-ai.com/zh-Hans/docs/agnes-20-flash) 仍标注上下文 512K。两处存在矛盾，建议以文档页面为准。
> - **4K 图片生成**：已全面支持（所有用户可用）
>
> 触发条件：当用户问"灰度自检"、"是否有1M上下文"、"能生成4K图吗"、"4K图片"等时，执行以下测试。
>
> **⚠️ 重要：1M 上下文测试是"尽力而为"的方法，结果仅供参考。官方已声明回滚 1M，当前基准为 512K。4K 图片无需灰度测试，已全面支持。**

---

**测试 1：1M 上下文自检**

> **原理**：需要发送超过 512K tokens 的文本。如果 API 成功返回 → 支持 1M；如果返回 413/400（上下文过长）→ 当前不支持。
>
> **注意**：agnes-2.0-flash Context 已开放至 512K（官方文档页面已标注，但 MODEL_CATALOG.md 声明 1M 已回滚），1M 测试需要发送 **超过 512K tokens**（约 **2M+ 字符**）的 payload。如 512K 已满足需求，无需测试 1M。

**curl 方法**（示例文本较短，实际测试需要更大payload，推荐用 Python 方法）：
```bash
# 此示例payload较小，仅展示调用方式
# 实际测试需要约 2M+ 字符的文本才能超过 512K tokens

curl https://apihub.agnes-ai.com/v1/chat/completions \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "agnes-2.0-flash",
    "messages": [
      {"role": "system", "content": "You are a helpful assistant."},
      {"role": "user", "content": "（此处需要放入约 2M+ 字符的文本）"}
    ],
    "max_tokens": 10
  }'
```

**Python 方法（推荐）**：
```python
import requests

key = "YOUR_API_KEY"

# 生成约 2.5M 字符（约 625K tokens，超过 512K 边界）
# 注意：实际 token 数取决于 tokenizer，但 2.5M 字符应足够超过 512K tokens
sentence = "Hello world this is a test for long context window capability. "
test_text = sentence * 42000  # 约 2.5M 字符

print(f"测试文本长度: {len(test_text)} 字符")

try:
    resp = requests.post(
        "https://apihub.agnes-ai.com/v1/chat/completions",
        headers={"Authorization": f"Bearer {key}", "Content-Type": "application/json"},
        json={
            "model": "agnes-2.0-flash",
            "messages": [
                {"role": "system", "content": "You are a helpful assistant."},
                {"role": "user", "content": test_text}
            ],
            "max_tokens": 10
        },
        timeout=60
    )

    if resp.status_code == 200:
        print("✅ 支持 1M 上下文（当前在灰度名单内）")
    elif resp.status_code == 413 or resp.status_code == 400:
        error_msg = resp.json().get("error", {}).get("message", resp.text[:200])
        if "context" in error_msg.lower() or "length" in error_msg.lower() or "too long" in error_msg.lower():
            print(f"❌ 当前不支持 1M 上下文（灰度未命中）：{error_msg}")
        else:
            print(f"⚠️ 请求失败（非上下文问题）：{resp.status_code} {error_msg}")
    else:
        print(f"⚠️ 请求失败：{resp.status_code} {resp.text[:200]}")
except Exception as e:
    print(f"⚠️ 请求异常（可能是payload过大导致客户端限制）：{e}")
```

---

**测试 2：4K 图片（已全面支持）**

> **状态：** 4K 图片生成已全面支持（所有用户可用），无需灰度自检。以下代码可直接用于 4K 图片生成。
> 4K 支持 16:9（3840x2160）和 9:16（2160x3840）尺寸。

```bash
# 4K 图片生成（已全面支持）
curl https://apihub.agnes-ai.com/v1/images/generations \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "agnes-image-2.1-flash",
    "prompt": "A beautiful landscape with mountains and lake, 4K ultra HD",
    "size": "3840x2160",
    "extra_body": {"response_format": "url"}
  }'
```

Python 方法：
```python
import requests

key = "YOUR_API_KEY"

resp = requests.post(
    "https://apihub.agnes-ai.com/v1/images/generations",
    headers={"Authorization": f"Bearer {key}", "Content-Type": "application/json"},
    json={
        "model": "agnes-image-2.1-flash",
        "prompt": "A beautiful landscape with mountains and lake, 4K ultra HD",
        "size": "3840x2160",
        "extra_body": {"response_format": "url"}
    }
)

if resp.status_code == 200:
    data = resp.json()
    if "data" in data and len(data["data"]) > 0 and data["data"][0].get("url"):
        print("✅ 4K 图片生成成功（已全面支持）")
    else:
        print("⚠️ 请求成功但无图片返回，请检查参数")
else:
    print(f"⚠️ 请求失败：{resp.status_code} {resp.text[:200]}")
```

---

**测试结果解读：**

| 测试 | 成功 | 失败 | 不确定 |
|------|------|------|--------|
| 1M 上下文 | ✅ 你的账户已开放 1M 上下文 | ❌ 当前仍为 512K | ⚠️ 请求异常（payload过大或网络问题） |
| 4K 图片 | ✅ 已全面支持（所有用户可用） | — | — |

> **注意：** 1M 上下文官方已声明回滚，当前基准为 512K（官方文档页面标注）。如 512K 已满足需求，无需测试 1M。4K 图片已全面开放，无需灰度测试。

---

### 本地图片处理

当用户提供本地图片路径进行图生图时，处理步骤：

1. 使用 Python 读取本地图片并转为 base64
2. 通过 data URI 格式传入 `extra_body.image` 字段
3. 或上传到可公网访问的临时存储，将 URL 传入

**从本地图片生成 data URI 示例：**
```python
import base64

with open("photo.jpg", "rb") as f:
    b64 = base64.b64encode(f.read()).decode("utf-8")
    uri = f"data:image/jpeg;base64,{b64}"
```

> 💡 data URI 格式支持 `image/jpeg`、`image/png`、`image/webp` 等。注意 base64 编码会使请求体变大，超大图片建议先压缩或上传到公网。

---

## 10. Prompt 模板库

> 复制以下模板，替换方括号中的内容即可直接使用。

### 10.1 文生图 Prompt 模板

**通用结构公式：**
```
[主体] + [场景/背景] + [风格] + [光照] + [构图] + [质量要求]
```

**模板 1 — 电商产品图**
```
A clean product photo of a [产品] on a [背景颜色] studio background, soft shadows, high detail, professional commercial photography, 8K quality
```
*示例：A clean product photo of a wireless earbuds case on a pure white studio background, soft shadows, high detail, professional commercial photography, 8K quality*

**模板 2 — 社交媒体海报**
```
A vibrant social media poster featuring [主题], bold typography space at [位置], [风格] color palette, eye-catching composition, modern design, 4K
```
*示例：A vibrant social media poster featuring summer sale, bold typography space at top, tropical color palette, eye-catching composition, modern design, 4K*

**模板 3 — 人物肖像**
```
Portrait of a [年龄/性别] [职业/角色], [表情], wearing [服装], [场景背景], [风格] lighting, cinematic composition, highly detailed, professional photography
```
*示例：Portrait of a young female scientist, confident smile, wearing a white lab coat, modern laboratory background, soft natural lighting, cinematic composition, highly detailed, professional photography*

**模板 4 — 风景/场景**
```
A breathtaking [场景类型] at [时间], [天气/氛围], [风格] style, dramatic lighting, ultra-wide angle, 8K resolution, photorealistic
```
*示例：A breathtaking mountain lake at golden hour, misty atmosphere, fantasy art style, dramatic lighting, ultra-wide angle, 8K resolution, photorealistic*

**模板 5 — 图标/插画**
```
A minimalist [风格] icon of [对象], flat design, [主色调] color scheme, clean lines, white background, vector style, UI/UX design
```
*示例：A minimalist line art icon of a rocket launching, flat design, blue and orange color scheme, clean lines, white background, vector style, UI/UX design*

---

### 10.2 图生图编辑指令模板

**通用结构公式：**
```
[编辑动作] + [保留元素] + [目标风格/场景] + [光照] + [构图]
```

**模板 1 — 风格转换**
```
Transform this image into [目标风格] style while preserving the main subject and composition
```
*示例：Transform this image into a cinematic cyberpunk style while preserving the main subject and composition*

**模板 2 — 颜色替换**
```
Change the [对象] color to [颜色] while keeping the original lighting and shadows
```
*示例：Change the car color to matte black while keeping the original lighting and shadows*

**模板 3 — 背景替换**
```
Replace the background with [新背景], keep the foreground subject unchanged, match the lighting
```
*示例：Replace the background with a sunset beach scene, keep the foreground subject unchanged, match the lighting*

**模板 4 — 添加元素**
```
Add [元素] to the scene, maintain the original style and perspective, natural integration
```
*示例：Add floating lanterns to the scene, maintain the original style and perspective, natural integration*

**模板 5 — 季节/时间转换**
```
Change the scene to [季节/时间], adjust lighting and atmosphere accordingly, preserve architecture
```
*示例：Change the scene to winter night with snowfall, adjust lighting and atmosphere accordingly, preserve architecture*

---

### 10.3 视频生成 Prompt 模板

**文生视频通用结构：**
```
[主体] + [动作] + [场景] + [镜头运动] + [光照] + [风格] + [时长暗示]
```

**模板 1 — 产品展示视频**
```
A [产品] rotating slowly on a [背景] platform, smooth 360-degree product showcase, soft studio lighting, clean and minimal, professional commercial video
```
*示例：A luxury watch rotating slowly on a marble platform, smooth 360-degree product showcase, soft studio lighting, clean and minimal, professional commercial video*

**模板 2 — 人物动作视频**
```
A [人物描述] [动作], natural movement, [场景], [镜头运动], cinematic lighting, realistic style
```
*示例：A young woman walking through a cherry blossom garden, natural movement, petals falling around her, slow tracking shot, golden hour lighting, realistic style*

**模板 3 — 场景转换视频**
```
A smooth cinematic transition from [场景A] to [场景B], maintaining visual consistency, dramatic lighting change, wide angle
```
*示例：A smooth cinematic transition from a rainy city street to a sunny countryside road, maintaining visual consistency, dramatic lighting change, wide angle*

**图生视频通用结构：**
```
Animate [元素] with [动作], keep [需保持的元素] consistent, [风格]
```
*示例：Animate the character with subtle breathing motion, hair moving gently, while keeping the face and outfit consistent, realistic style*

**关键帧动画结构：**
```
Generate a smooth cinematic transition between keyframes, maintaining [元素] consistent, [风格]
```
*示例：Generate a smooth cinematic transition between keyframes, maintaining character appearance consistent, fantasy art style*

---

### 10.4 文本模型 Prompt 模板

**模板 1 — 代码生成**
```
Role: Senior [语言] Developer
Task: Write a [功能描述]
Context: [框架/库], [已有代码/约束]
Requirements: [具体要求，如错误处理、类型安全、性能]
Output: Complete code with comments
```

**模板 2 — 代码调试**
```
Role: Debugging Expert
Task: Fix the bug in the following code
Context: [语言/框架], [错误信息], [期望行为]
Code: [粘贴代码]
Requirements: Explain the root cause, provide the fix, suggest prevention measures
```

**模板 3 — 内容创作**
```
Role: Professional [领域] Writer
Task: Write a [内容类型] about [主题]
Target Audience: [受众描述]
Tone: [语气，如专业/轻松/权威]
Requirements: [字数、结构、SEO关键词等]
```

**模板 4 — 数据分析**
```
Role: Data Analyst
Task: Analyze the following data and provide insights
Data: [粘贴数据或描述]
Requirements: [分析维度], identify trends, suggest actionable recommendations
Output: Structured report with key findings
```

**模板 5 — Agent 任务**
```
Role: [Agent 角色]
Goal: [明确目标]
Tools Available: [可用工具列表]
Constraints: [限制条件，如时间、预算、格式]
Step-by-step plan required: Yes
```

---

## 11. 社区资源

- **官方平台**：https://platform.agnes-ai.com
- **官方 GitHub**：https://github.com/AgnesAI-Labs/Agnes-AI
- **官方模型目录**：https://github.com/AgnesAI-Labs/Agnes-AI/blob/main/MODEL_CATALOG.md
- **官方文档**：https://agnes-ai.com/doc/overview
- **操作手册**：https://agnes-ai.com/doc/%E5%B8%B8%E7%94%A8%E6%8E%A5%E5%85%A5%E6%96%87%E6%A1%A3
- **视频文档**：https://agnes-ai.com/doc/agnes-video-v20
- **常见问题 QA**：https://icn1d2hdv39m.feishu.cn/wiki/R7TEwjadJibD62kWeS9cubtpnPi
- **社区教程**：https://github.com/Yacey/agnes-ai-generation-skill
- **社区 Skill**：https://github.com/kangarooking/agnes-free-model-skills
- **ComfyUI 节点**：https://github.com/16nic/comfyui-agnes-ai
- **支持邮箱**：support@agnes-ai.com
- **公司**：Sapiens AI / Agnes AI

---

## 12. 社区 Skill 接入指南

以下社区资源提供了更丰富的 Agnes AI 集成方式，可根据你的使用场景选择。

### 12.1 Yacey 的 Agnes AI 生成 Skill（推荐 Agent 用户）

**仓库：** https://github.com/Yacey/agnes-ai-generation-skill

**特点：**
- 封装 Agnes 官方文本、图片、视频 API 的标准 Skill
- 支持中文提示词自动翻译为英文（提升视频生成稳定性）
- 提供 Python 脚本直接调用
- 兼容 Codex、Claude Code、OpenClaw、Cursor、Windsurf

**安装方式：**
```bash
# 安装到当前 Agent
npx skills add Yacey/agnes-ai-generation-skill

# 安装到所有支持的 Agent
npx skills add Yacey/agnes-ai-generation-skill --all
```

**配置 API Key：**
```bash
# 临时配置（当前 PowerShell 会话）
$env:AGNES_API_KEY="YOUR_API_KEY"

# Windows 用户级持久配置
[Environment]::SetEnvironmentVariable("AGNES_API_KEY", "YOUR_API_KEY", "User")
```

**脚本也识别以下变量名：**
- `AGNES_API_KEY`
- `AGNES_API_TOKEN`
- `APIHUB_AGNES_API_KEY`

**使用示例：**
```bash
# 文本生成
python scripts/agnes_api.py text --prompt "Write a product tagline for an AI assistant."

# 文生图
python scripts/agnes_api.py image --prompt "A luminous floating city above a misty canyon at sunrise, cinematic realism" --size 1024x768

# 图生图
python scripts/agnes_api.py image --prompt "Turn the scene into a rainy cyberpunk night" --image https://example.com/input.png

# 文生视频（默认 num_frames=121, frame_rate=24）
python scripts/agnes_api.py video --prompt "A cinematic shot of a cat walking on the beach at sunset" --poll

# 图生视频
python scripts/agnes_api.py video --prompt "Animate subtle camera movement" --image https://example.com/image.png --poll

# 多图 / 关键帧视频
python scripts/agnes_api.py video --prompt "Create a smooth transition" --image https://example.com/a.png --image https://example.com/b.png --mode keyframes --poll

# 查询视频任务
python scripts/agnes_api.py video-get task_123456

# 运行轻量测试（默认不创建视频）
python scripts/agnes_api.py smoke-test

# 包含图生图测试
python scripts/agnes_api.py smoke-test --include-image-edit

# 测试单个视频模式
python scripts/agnes_api.py smoke-test --video-case text-to-video
```

**⚠️ 重要提醒：**
- 该 Skill 的视频查询默认使用 `task_id`，但 Agnes 官方已更新为推荐使用 `video_id` 查询
- 如视频排队超过 5 分钟，请检查是否使用了正确的查询方式
- 中文提示词会自动翻译为英文后再调用 API

---

### 12.2 kangarooking 的免费模型 Skills（推荐 Codex 用户）

**仓库：** https://github.com/kangarooking/agnes-free-model-skills

**特点：**
- 3 个独立 Skill：文本、图片、视频
- 可直接放入 Codex skills 目录使用
- 提供 Python 辅助脚本

**包含 Skill：**

| Skill | 能力 | 触发场景 |
|-------|------|----------|
| `agnes-free-text` | Agnes-2.0-Flash 文本模型，支持 Chat、流式、工具调用 | 用户说"用免费的文本模型""Agnes 文本" |
| `agnes-free-image` | Agnes Image 2.1 Flash 图片模型，支持文生图、图生图 | 用户说"用免费的图片模型""Agnes 图片" |
| `agnes-free-video` | Agnes-Video-V2.0 视频模型，异步任务+轮询+下载 | 用户说"用免费视频模型""Agnes 视频" |

**安装方式：**
```bash
# 克隆仓库
git clone https://github.com/kangarooking/agnes-free-model-skills.git

# 复制需要的 skill 到 Codex skills 目录
cp -R agnes-free-model-skills/agnes-free-text ~/.codex/skills/
cp -R agnes-free-model-skills/agnes-free-image ~/.codex/skills/
cp -R agnes-free-model-skills/agnes-free-video ~/.codex/skills/
```

**配置环境变量：**
```bash
export AGNES_API_KEY="your_api_key_here"
```

**备用变量名：**
- `AGNES_TOKEN`
- `AGNES_API_BASE`（可覆盖默认地址）

**使用示例：**
```bash
# 文本
python3 agnes-free-text/scripts/agnes_text.py chat --message "解释 Agent skill 是什么"

# 图片
python3 agnes-free-image/scripts/agnes_image.py generate --prompt "科技感插画，干净明亮"

# 视频
python3 agnes-free-video/scripts/agnes_video.py create --prompt "竖屏短视频，医疗科普画面"

# 视频轮询并下载
python3 agnes-free-video/scripts/agnes_video.py status --task-id task_123456 --wait --download-dir downloads
```

---

### 12.3 16nic 的 ComfyUI 节点（推荐 ComfyUI 用户）

**仓库：** https://github.com/16nic/comfyui-agnes-ai

**特点：**
- ComfyUI 自定义节点插件
- 7 个功能节点，覆盖 Agnes 全模态能力
- 支持 API Key 持久化、画质选择、宽高比选择
- 视频输出支持 ComfyUI 原生 VIDEO 类型

**功能节点：**

| 节点 | 功能 | 模型 |
|------|------|------|
| Agnes API Key Config | 持久化保存 API Key | — |
| Agnes LLM Chat | 文本对话 | agnes-2.0-flash |
| Agnes Image Reverse Prompt | 图像反推提示词 | agnes-2.0-flash (vision) |
| Agnes Image-to-Image | 图生图/编辑（支持多图） | agnes-image-2.1-flash |
| Agnes Text-to-Image | 文生图 | agnes-image-2.1-flash |
| Agnes Image-to-Video | 图生视频（支持多图/关键帧） | agnes-video-v2.0 |
| Agnes Text-to-Video | 文生视频 | agnes-video-v2.0 |

**安装方式：**
```bash
cd ComfyUI/custom_nodes
git clone https://github.com/16nic/comfyui-agnes-ai.git
cd comfyui-agnes-ai
pip install -r requirements.txt
```

**配置 API Key（推荐方式）：**
1. 在节点菜单 → **Agnes AI** → 添加 **Agnes API Key Config**
2. 在 `api_key` 输入框填入你的 API Key
3. 运行一次（Ctrl+Enter / Queue Prompt）
4. Key 自动保存到 `api_key_config.json`
5. 之后其他 Agnes 节点的 `api_key` 字段留空即可自动加载

**API Key 加载优先级：**
```
环境变量 AGNES_API_KEY > api_key_config.json > 运行时回退加载 > 节点输入框手动填写
```

**画质 × 宽高比对照表（图片）：**

| 比例 | 2K（默认） | 4K |
|------|-----------|-----|
| 4:3 | 1024×768 | — |
| 3:4 | 768×1024 | — |
| 16:9 | 2560×1440 | 3840×2160 |
| 9:16 | 1440×2560 | 2160×3840 |

**注意事项：**
- 视频生成是异步任务，通常需要 2-6 分钟
- 免费 API 高峰期可能有排队（503）或 GPU OOM（500）
- 建议直接复制平台提供的模型名称（区分大小写）
- 视频生成通常需要 2-6 分钟

---

## 13. Codex 集成指南

> **区分两个工具：**
> - **Codex++**（Agnes 官方文档推荐）：第三方 GUI 工具，通过管理界面配置供应商
> - **OpenAI Codex CLI**（命令行工具）：OpenAI 官方终端 AI 编程智能体，通过 `~/.codex/config.toml` 配置

---

### 13.1 Codex++ 配置（Agnes 官方推荐）

Codex++ 是一款支持多供应商的 GUI 管理工具，Agnes 官方文档提供完整接入指南。

**Step 1 — 下载 Codex++**

- 仓库：https://github.com/BigPizzaV3/CodexPlusPlus
- 最新版：https://github.com/BigPizzaV3/CodexPlusPlus/releases/latest
- 根据系统选择安装包：
  - Windows：`windows-x64-setup.exe`
  - Intel Mac：`macos-x64.dmg`
  - Apple Silicon Mac：`macos-arm64.dmg`

**Step 2 — 安装**

正常安装即可。Mac 用户若提示"已损坏，无法打开"，在终端执行：
```bash
sudo xattr -rd com.apple.quarantine "/Applications/Codex++.app"
sudo xattr -rd com.apple.quarantine "/Applications/Codex++ 管理工具.app"
```

**Step 3 — 打开管理工具，添加 Agnes 供应商**

🔴 **CHECKPOINT：添加供应商前确认 Key 格式正确：Key 只填 `sk-` 开头的密钥，不含 `Bearer` 前缀。**

打开"Codex++ 管理工具" → 左侧菜单"供应商配置" → "添加供应商"，填写：

| 配置项 | 填写内容 |
|--------|----------|
| 名称 | Agnes |
| 接入模式 | 纯 API |
| 测试模型 | agnes-2.0-flash |
| Base URL | `https://apihub.agnes-ai.com/v1` |
| Key | 你的 Agnes API Key（sk- 开头） |
| 上游协议 | Chat Completions |

**⚠️ 关键注意事项：**
- Key 只填写 `sk-` 开头的密钥，**不要**填写 `Bearer`
- Base URL 只填写到 `/v1`，**不要**填写 `/chat/completions`
- 上游协议必须选择 **Chat Completions**

**Step 4 — 启用并启动**

1. 保存配置后，回到供应商列表，选择 **Agnes**，点击"使用/切换到此供应商"
2. 左侧菜单"概览" → "启动 Codex++"（或右上角"重启 Codex"）
3. 启动完成后，新建会话测试：输入"你好，请介绍一下你自己"，正常回复即配置成功

---

### 13.2 OpenAI Codex CLI 配置（命令行用户）

如果你使用的是 OpenAI 官方的 `codex` 命令行工具，可通过以下方式接入 Agnes：

**Step 1 — 创建配置目录**
```bash
mkdir -p ~/.codex
```

**Step 2 — 编辑 `~/.codex/config.toml`**

```toml
#:schema https://developers.openai.com/codex/config-schema.json

# 使用 Agnes AI 作为后端
model = "agnes-2.0-flash"
model_provider = "openai"
openai_base_url = "https://apihub.agnes-ai.com/v1"

# 推荐日常配置
sandbox_mode = "workspace-write"
approval_policy = "on-request"
```

**Step 3 — 配置 API Key**

方式 A：环境变量（推荐）
```bash
# macOS / Linux
export OPENAI_API_KEY="YOUR_AGNES_API_KEY"

# Windows PowerShell
$env:OPENAI_API_KEY="YOUR_AGNES_API_KEY"
```

方式 B：认证文件
编辑 `~/.codex/auth.json`：
```json
{
  "OPENAI_API_KEY": "YOUR_AGNES_API_KEY"
}
```

**Step 4 — 验证配置**
```bash
codex doctor
```

`codex doctor` 会自动检测运行时、认证、网络连通性和 config.toml 格式，所有项目绿色即配置成功。

**多 Provider 配置（灵活切换）：**

```toml
# 默认使用 Agnes
model = "agnes-2.0-flash"
model_provider = "openai"
openai_base_url = "https://apihub.agnes-ai.com/v1"

# 同时保留官方 OpenAI 配置
[model_providers.openai]
name = "OpenAI Official"
base_url = "https://api.openai.com/v1"
env_key = "OPENAI_API_KEY"

# 或其他 Provider
[model_providers.deepseek]
name = "DeepSeek"
base_url = "https://api.deepseek.com/v1"
env_key = "DEEPSEEK_API_KEY"
```

切换 Provider 时只需修改：
```toml
model_provider = "openai"  # 使用 Agnes
# 或
model_provider = "deepseek"  # 使用 DeepSeek
```

---

### 13.3 使用 Agnes 的各模型

| 模型 | 配置值 |
|------|--------|
| 编程/Agent | `agnes-2.0-flash` |

**注意：** Codex/Codex++ 只支持 Chat Completions API。图像和视频生成必须通过 Agnes 的独立端点调用：
1. 安装 kangarooking 的 `agnes-free-image` / `agnes-free-video` Skill
2. 或直接用 curl / Python 脚本调用

---

### 13.4 常见问题

**Q: Codex++ 配置后无法连接？**
- 确认 Key 只填写 `sk-` 部分，不含 `Bearer`
- 确认 Base URL 只到 `/v1`，不含 `/chat/completions`
- 确认上游协议选择 **Chat Completions**
- 确认网络可正常访问 `https://apihub.agnes-ai.com`

**Q: OpenAI Codex CLI 提示 "自定义配置不生效"？**
- 确认 `config.toml` 路径正确（`~/.codex/config.toml`）
- 确认 `model_provider` 名称匹配
- 确认 `base_url` 完整（包含 `/v1`）
- 确认环境变量名与 `env_key` 一致

**Q: 如何同时使用 Agnes 和官方 OpenAI？**
- Codex++：在供应商列表中切换
- Codex CLI：使用多 Provider 配置（见 10.2）

**Q: Codex 的 Agent 模式能调用 Agnes 的工具调用吗？**
- 可以。Agnes 2.0 Flash 支持 OpenAI 兼容的工具调用格式
- 在 Codex 中正常使用 `tools` 即可
- 模型返回 `finish_reason: "tool_calls"` 时，Codex 会自动处理

---

> Agnes AI，让世界级 AI 属于每一个人。

---

## 14. OpenClaw 接入指南

> 官方文档：https://agnes-ai.com/doc/cid1

### 14.1 概述

OpenClaw 是一款支持自定义模型服务商的 Agent 工具。配置完成后，OpenClaw 可以通过 Agnes AI API 网关调用模型，用于本地 Agent 任务。

### 14.2 配置步骤

**Step 1 — 打开终端**

- macOS / Linux：使用终端
- Windows：使用命令提示符、PowerShell 或开发环境中的终端

**Step 2 — 进入 OpenClaw 配置界面**

```bash
openclaw config
```

**Step 3 — 选择本地配置**

在配置菜单中选择：`Local`，按 Enter。

**Step 4 — 进入模型配置**

选择：`Model`，按 Enter。

**Step 5 — 选择定制服务商**

选择：`Custom Provider`，按 Enter。

**Step 6 — 配置 API Base URL**

输入：
```
https://apihub.agnes-ai.com/v1
```

**Step 7 — 填写 API Key**

输入你的 Agnes API Key（`sk-` 开头）。

**⚠️ 注意：** 不要手动添加 `Bearer` 前缀，SDK 自动处理。除非 OpenClaw 明确要求输入完整的授权标头。

**Step 8 — 填写模型名称**

输入：
```
agnes-2.0-flash
```

**Step 9 — 保存配置**

完成所有必填项后，确认并保存。

### 14.3 完整配置示例

```
Provider Type: Custom Provider
API Base URL: https://apihub.agnes-ai.com/v1
API Key: YOUR_API_KEY
Model: agnes-2.0-flash
```

### 14.4 验证配置

配置完成后，运行一个 OpenClaw 任务或启动测试会话。如果能正常返回模型响应，说明配置成功。

### 14.5 常见问题

**Q: API 请求失败？**
- 检查 API Base URL 是否正确：`https://apihub.agnes-ai.com/v1`
- 确认 API Key 是否有效

**Q: 提示"缺乏模型"？**
- 确认模型名称填写正确，区分大小写
- 建议直接复制平台提供的模型名称

**Q: 鉴权失败？**
- 检查 API Key 是否过期
- 确认账户余额充足
- 确认该 Key 具备访问目标模型的权限

**Q: 网络错误？**
- 确认设备可正常访问 API
- 检查防火墙、代理或 VPN 设置

**Q: API Base URL 到底要不要填 `/chat/completions`？**
- **不要**。只填写到 `/v1`
- 正确：`https://apihub.agnes-ai.com/v1`
- 错误：`https://apihub.agnes-ai.com/v1/chat/completions`
- 除非 OpenClaw 明确要求输入完整接口地址

---

## 15. HermesAgents 接入指南

> 官方文档：https://agnes-ai.com/doc/cid2

### 15.1 概述

HermesAgents 是一款支持自定义模型服务商的 Agent 工具。配置完成后，HermesAgents 将模型请求发送到 Agnes AI API 网关，并使用 Agnes 模型完成 Agent 任务。

### 15.2 配置步骤

HermesAgents 使用命令行配置，格式为：
```bash
hermes config set <配置项> <值>
```

**Step 1 — 配置模型服务商**

```bash
hermes config set model.provider custom
```

**Step 2 — 配置 API Base URL**

```bash
hermes config set model.base_url https://apihub.agnes-ai.com/v1
```

**Step 3 — 配置 API Key**

```bash
hermes config set model.api_key YOUR_API_KEY
```

**⚠️ 注意：** 不要手动添加 `Bearer` 前缀，SDK 自动处理。除非 HermesAgents 明确要求输入完整的授权标头。

也可以通过环境变量配置：
```bash
hermes config set OPENAI_API_KEY YOUR_API_KEY
```

但在自定义模型服务商配置中，推荐优先使用 `model.api_key`。

**Step 4 — 配置模型名称**

```bash
hermes config set model.default agnes-2.0-flash
```

### 15.3 完整配置示例

```bash
hermes config set model.provider custom
hermes config set model.base_url https://apihub.agnes-ai.com/v1
hermes config set model.api_key sk-xxxxxxxxxxxxxxxx
hermes config set model.default agnes-2.0-flash
```

### 15.4 验证配置

配置完成后，运行一个 HermesAgents 任务或启动测试会话。如果能正常返回模型响应，说明配置成功。

### 15.5 常见问题

**Q: 鉴权失败？**
- 重新执行 `hermes config set model.api_key YOUR_API_KEY`
- 确认账户余额或积分充足

**Q: API 请求失败？**
- 确认 API Base URL 正确：`https://apihub.agnes-ai.com/v1`
- 确认网络、防火墙、代理或 VPN 设置正常

**Q: 模型名称不生效？**
- 确认模型 ID 区分大小写
- 建议直接复制平台提供的模型名称

**Q: API Base URL 要不要填 `/chat/completions`？**
- **不要**。只填写到 `/v1`
- 正确：`https://apihub.agnes-ai.com/v1`
- 错误：`https://apihub.agnes-ai.com/v1/chat/completions`

---

## 16. Claude CLI 接入指南（通过 CC-Switch）

> 官方文档：https://agnes-ai.com/doc/cid3

### 16.1 概述

Claude CLI 是 Anthropic 官方的终端 AI 编程工具。由于 Claude CLI 原生不支持自定义 OpenAI-Compatible API，需要通过 **CC-Switch** 作为代理路由，将请求转发到 Agnes AI API 网关。

**所需工具：**
- Claude CLI（已安装）
- CC-Switch v3.16.1+（下载：https://github.com/farion1231/cc-switch/releases）
- Agnes AI API Key

### 16.2 配置步骤

**Step 1 — 获取 Agnes API Key**

访问 https://platform.agnes-ai.com/，登录后进入 API Key 页面，创建并复制 API Key。

**Step 2 — 打开 CC-Switch**

启动 CC-Switch，在顶部工具栏选择：`Claude CLI`

**Step 3 — 添加新供应商**

点击右上角加号，添加新供应商：
- 供应商类型选择：`Claude Provider` → `Custom Provider`

**Step 4 — 填写配置信息**

| 配置项 | 填写内容 |
|--------|----------|
| API Key | 你的 Agnes API Key（`sk-` 开头） |
| 请求地址 | `https://apihub.agnes-ai.com/v1` |
| API 格式 | `OpenAI Chat Completions` |
| 认证字段 | 默认即可，或手动配置 `ANTHROPIC_AUTH_TOKEN` |

**⚠️ 注意：** 不要手动添加 `Bearer` 前缀，SDK 自动处理。

**Step 5 — 模型映射**

点击"获取模型列表"，确认可正常连接 Agnes API。

然后将 Claude CLI 的模型映射到 Agnes 模型：

| Claude 模型 | 映射到 Agnes 模型 |
|-------------|------------------|
| Sonnet | `agnes-2.0-flash` |
| Opus | `agnes-2.0-flash` |
| Haiku | `agnes-2.0-flash` |

**Step 6 — 添加自定义参数（重要）**

🔴 **STOP：Claude CLI 通过代理调用 OpenAI-Compatible API 时，必须添加以下自定义参数，否则会出现不兼容参数错误：**

在自定义参数中加入：

```json
{
  "allowed_openai_params": [
    "thinking",
    "context_management"
  ],
  "litellm_settings": {
    "drop_params": true
  }
}
```

**作用：**
- 允许指定的 OpenAI 参数通过
- 自动丢弃模型不兼容的未知参数
- 提高 Claude CLI 通过代理调用 OpenAI-Compatible API 时的兼容性

**Step 7 — 保存并启用**

- 保存供应商配置
- 进入 CC-Switch 设置 → `Route` → 打开 `Local Route`
- 在本地路由中启用 Claude 路由切换
- 返回供应商列表，启用 Agnes Provider

### 16.3 验证配置

打开 Claude CLI，执行一次测试对话或代码任务。如果能正常返回 Agnes 模型响应，说明配置成功。

### 16.4 常见问题

**Q: 无法获取模型列表？**
- 检查 API Base URL：`https://apihub.agnes-ai.com/v1`
- 确认 API Key 有效
- 确认网络可正常访问 Agnes API

**Q: Claude CLI 返回错误或不兼容参数？**
- 确认已添加自定义参数（`allowed_openai_params` + `litellm_settings.drop_params`）
- 确认 API 格式选择为 `OpenAI Chat Completions`

**Q: 配置保存后 Claude CLI 仍走官方 Anthropic？**
- 确认 CC-Switch 的 Local Route 已开启
- 确认 Claude 路由切换已启用
- 确认 Agnes Provider 已启用

---

## 17. Claude Desktop 接入指南（通过 CC-Switch）

> 官方文档：https://agnes-ai.com/doc/cid4

### 17.1 概述

Claude Desktop 是 Anthropic 官方的桌面端 AI 应用。与 Claude CLI 类似，需要通过 **CC-Switch** 作为代理路由，将请求转发到 Agnes AI API 网关。

**所需工具：**
- Claude Desktop（下载：https://claude.com/download）
- CC-Switch v3.16.1+（下载：https://github.com/farion1231/cc-switch/releases）
- Agnes AI API Key

### 17.2 配置步骤

**Step 1 — 安装 Claude Desktop**

根据系统选择安装包：
- Windows：`.exe`
- macOS：`.dmg`
- Linux：`AppImage`

**Step 2 — 打开 CC-Switch**

启动 CC-Switch，在顶部切换到：`Claude Desktop` 或 `ClaudeCode Desktop`

**Step 3 — 添加新供应商**

点击"添加新供应商"：
- 选择：`Custom Provider`

**Step 4 — 填写配置信息**

| 配置项 | 填写内容 |
|--------|----------|
| API Key | 你的 Agnes API Key（`sk-` 开头） |
| 请求地址 | `https://apihub.agnes-ai.com/v1` |
| API 格式 | `OpenAI Chat Completions` |

**Step 5 — 开启模型映射并获取模型列表**

- 开启"模型映射"功能
- 点击"获取模型列表"，确认可正常连接 Agnes API

**Step 6 — 配置模型映射**

| Claude 模型 | 映射到 Agnes 模型 |
|-------------|------------------|
| Sonnet | `agnes-2.0-flash` |
| Opus | `agnes-2.0-flash` |
| Haiku | `agnes-2.0-flash` |

**Step 7 — 保存并启用路由**

- 保存配置
- 进入 CC-Switch 设置 → `Route` → 打开本地路由开关
- 启用 Claude 路由
- 返回供应商列表，启用 Agnes Provider

### 17.3 验证配置

打开 Claude Desktop，发起一次测试对话。如果能正常通过 Agnes 模型返回响应，说明配置成功。

### 17.4 常见问题

**Q: Claude Desktop 无法调用模型？**
- 确认 CC-Switch 本地路由已开启
- 确认 Claude 路由已启用

**Q: 无法获取模型列表？**
- 检查 API Base URL：`https://apihub.agnes-ai.com/v1`
- 确认 API Key 有效

**Q: 模型映射失败？**
- 确认已开启模型映射功能
- 确认 Claude 模型已映射到 Agnes 模型

**Q: API 请求失败或鉴权失败？**
- 检查网络、防火墙、代理或 VPN 设置
- 确认 API Key 正确，账户余额充足

---

## 18. WorkBuddy 接入指南

> 官方文档：https://agnes-ai.com/doc/cid5

### 18.1 概述

WorkBuddy 是一款支持自定义模型的 AI 工作助手。配置完成后，WorkBuddy 可以直接调用 Agnes 文本模型进行对话、代码和 Agent 任务，也可以通过 Skill 方式使用 Agnes 图像和视频模型。

**本教程基于 WorkBuddy v4.24.5 版本。**

### 18.2 文本模型配置步骤

**Step 1 — 获取 Agnes API Key**

访问 https://platform.agnes-ai.com/，登录后进入 API Key 页面，创建并复制 API Key。

**Step 2 — 进入自定义模型配置**

打开 WorkBuddy，点击：`Auto` → `配置自定义模型`

**Step 3 — 选择自定义提供商**

在提供商列表中向下滑动到最后，选择：`其他` 或 `Custom`

**Step 4 — 添加 Agnes 文本模型**

点击"添加模型"，填写：

| 配置项 | 填写内容 |
|--------|----------|
| Provider | Custom |
| API Base URL | `https://apihub.agnes-ai.com/v1` |
| API Key | 你的 Agnes API Key |
| Model Name | `agnes-2.0-flash` |

**Step 5 — 保存并选择模型**

保存后，在 WorkBuddy 对话界面中选择：`agnes-2.0-flash`

**Step 6 — 验证**

发起一次普通对话，例如："你好，请介绍一下你自己。"正常返回即配置成功。

### 18.3 图像和视频模型配置（通过 Skill）

Agnes 的图像和视频模型可以通过创建 Skill 的方式在 WorkBuddy 中使用。

**创建 Skill 的提示词：**
```
我想要使用 Agnes Image 2.0 模型生图生视频，访问它的 API 平台 https://agnes-ai.com/doc/overview，并将它打包成一份 Skill。
```

WorkBuddy 会根据 Agnes API 文档自动生成图像和视频相关 Skill。

**使用图像 Skill：**
- 点击技能列表，选择图像生成 Skill（如 `agnes-image-gen`）
- 输入提示词，例如："生成一张赛博朋克城市夜景，霓虹灯光，电影感，高细节。"

**使用视频 Skill：**
- 选择视频生成 Skill（如 `agnes-video-gen`）
- 输入提示词，例如："生成一段亚洲女生模特在白色摄影棚中展示黑色连衣裙的视频，从全景缓慢切到半身特写，保持自然转身，5 秒。"

### 18.4 常见问题

**Q: 文本模型无法返回？**
- 检查 API Base URL：`https://apihub.agnes-ai.com/v1`
- 确认 API Key 有效

**Q: 模型列表中看不到 `agnes-2.0-flash`？**
- 确认模型名称填写正确，区分大小写
- 建议直接复制平台提供的模型名称

**Q: 图像或视频 Skill 创建失败？**
- 确认 WorkBuddy 可以正常访问 Agnes 文档地址
- 确认当前模型具备读取文档和创建 Skill 的能力

**Q: 视频任务长时间没有完成？**
- 视频生成通常需要 2-6 分钟，请耐心等待
- 确认使用 `video_id` 查询结果（不要用 `task_id`）

**Q: 鉴权失败？**
- 检查 API Key 是否正确
- 确认账户余额或 credits 充足

---

## 19. Cherry Studio 接入指南

> 官方文档：https://agnes-ai.com/doc/cid6

### 19.1 概述

Cherry Studio 是一款支持多模型服务商的 AI 客户端。配置完成后，Cherry Studio 可以调用 Agnes 文本模型进行对话，也可以通过技能或智能体绑定的方式使用 Agnes 图像和视频模型。

### 19.2 文本模型配置步骤

**Step 1 — 获取 Agnes API Key**

访问 https://platform.agnes-ai.com/，登录后进入 API Key 页面，创建并复制 API Key。

**Step 2 — 添加提供商**

进入 Cherry Studio 的 API 服务商或模型设置页面，点击：`添加提供商`

**Step 3 — 选择提供商类型**

提供商类型选择：`OpenAI`（或支持 OpenAI-Compatible API 的自定义类型）

**Step 4 — 填写配置信息**

| 配置项 | 填写内容 |
|--------|----------|
| 提供商名称 | `Agnes` 或 `Agnes AI` |
| API Key | 你的 Agnes API Key（`sk-` 开头） |
| API 地址 | `https://apihub.agnes-ai.com/v1` |

**⚠️ 注意：** 不要手动添加 `Bearer` 前缀，SDK 自动处理。

**Step 5 — 获取模型列表**

点击"获取模型列表"，确认可正常连接 Agnes API。选择文本模型：`agnes-2.0-flash`

**Step 6 — 保存并验证**

保存后，在 Cherry Studio 中新建对话框，选择 `agnes-2.0-flash`，发送测试消息。正常返回即配置成功。

### 19.3 图像和视频模型使用方式

Cherry Studio 中，图像和视频模型建议通过**创建技能并绑定智能体**的方式使用。

- 创建图像 Skill（如 `Agnes Image Skill`），绑定到智能体
- 创建视频 Skill（如 `Agnes Video Skill`），绑定到智能体
- 通过对话方式生成图片或视频

### 19.4 常见问题

**Q: 无法获取模型列表？**
- 检查 API 地址：`https://apihub.agnes-ai.com/v1`
- 确认 API Key 有效

**Q: 鉴权失败？**
- 确认 API Key 填写正确
- 确认账户余额充足

**Q: 文本模型可用，但图片或视频不能用？**
- Cherry Studio 的普通 OpenAI 兼容文本模型配置通常只适合文本对话
- 图像和视频模型建议通过技能或智能体工具绑定方式使用

**Q: API 地址要不要填 `/chat/completions`？**
- **不要**。只填写到 `/v1`
- 正确：`https://apihub.agnes-ai.com/v1`
- 错误：`https://apihub.agnes-ai.com/v1/chat/completions`

---

## 20. Opencode 接入指南

> 官方文档：https://agnes-ai.com/doc/cid7

### 20.1 概述

Opencode 是一款支持 OpenAI-Compatible API 的 AI 编程工具。当前 Agnes 官方文档中仅提供通用接入模板，以下为推荐配置参数。

### 20.2 推荐配置参数

如果 Opencode 支持 OpenAI-Compatible API，可以使用以下配置：

| 配置项 | 填写内容 |
|--------|----------|
| Provider Type | `OpenAI-Compatible` |
| API Base URL | `https://apihub.agnes-ai.com/v1` |
| API Key | 你的 Agnes API Key |
| Model | `agnes-2.0-flash` |

### 20.3 注意事项

- API Base URL 通常以 `/v1` 结尾
- **正确：** `https://apihub.agnes-ai.com/v1`
- **不推荐：** `https://apihub.agnes-ai.com/v1/chat/completions`
- 除非 Opencode 明确要求填写完整的接口地址

---

> Agnes AI，让世界级 AI 属于每一个人。
