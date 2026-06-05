---
title: 微信 + cc-connect + CC-Switch 连接 Claude Code 完整教程
published: 2026-06-01
description: '在微信上随时随地指挥 AI 编程助手 — Claude Code 桥接微信的全流程指南。'
image: ''
tags: ["Claude Code", "cc-connect", "微信", "AI编程", "教程"]
category: '技术博客'
draft: false
---

> 在微信上随时随地指挥 AI 编程助手

本教程涵盖四个核心组件，共同构成完整链路：**微信发消息 → 桥接服务 → AI 编程助手为你工作**。

## 一、整体架构

```mermaid
flowchart LR
    A[微信<br/>IM平台] <--> B[cc-connect<br/>桥接服务] <--> C[Claude Code<br/>AI Agent] <--> D[cc-switch<br/>模型路由]
    D --> E[DeepSeek<br/>自定义模型]
```

| 组件 | 职责 |
|------|------|
| **微信** | 日常聊天客户端，通过桥接直接向 Claude Code 发号施令 |
| **cc-connect** | 桥接服务，连接 IM 平台与 AI Agent，负责消息转换、会话管理 |
| **Claude Code** | Anthropic 官方 CLI AI 编程助手 — 整套方案的「大脑」 |
| **cc-switch** | 模型路由工具，接入 DeepSeek 等 40+ 第三方模型 |

## 二、CC-Switch — 接入自定义模型

CC-Switch 是一款开源桌面应用（GitHub 50K+ ⭐），让 Claude Code **一键切换模型供应商**。

### 安装

从 [GitHub Releases](https://github.com/farion1231/cc-switch/releases) 下载安装包，Windows 选 `.msi`。

### 添加 DeepSeek 模型

打开 CC-Switch，确保顶部选中 **"Claude Code"** 标签页：

1. 点击右上角 **"+"** → 选择 **DeepSeek**
2. 输入 API Key（需自行在 [DeepSeek 开放平台](https://platform.deepseek.com) 充值获取）
3. 配置模型映射：

| Claude Code 模型 | DeepSeek 模型 |
|:---|:---|
| Haiku | `deepseek-v4-flash` |
| Sonnet | `deepseek-v4-pro` |
| Opus | `deepseek-v4-pro` |

4. 保存并点击 **启用**

### 支持的 40+ 供应商

DeepSeek、智谱 GLM、千问 (Qwen)、Kimi、OpenAI、Anthropic、Google Gemini、百度文心、字节豆包等。

## 三、cc-connect — 桥接到微信

### 安装

```bash
# 需要 Node.js 18+，推荐安装 beta 版本（支持守护进程）
npm install -g cc-connect@beta

# 验证安装
cc-connect --version

# 首次运行生成默认配置
cc-connect
```

### 微信扫码绑定

```bash
# 执行微信设置命令
cc-connect weixin setup --project my-project

# 终端显示二维码，用微信扫描完成连接
# 扫码后给 Bot 发一条任意消息激活
```

### 启动服务

```bash
# 终端 1：启动服务
cc-connect

# 终端 2：启动 Web 可视化配置
cc-connect web
```

在 Web 页面中：左侧菜单 → **"项目"** → **"my-project"** → **"设置"**，将工作目录设为你的用户目录（如 `C:\Users\Sage`）。保存后即可在微信上测试。

## 四、日常使用

### 基础对话示例

> 👤 **你**：帮我看看当前项目结构
> 🤖 **Claude Code**：正在查看当前目录...当前共有 12 个文件。

> 👤 **你**：帮我把 README.md 改成中文
> 🤖 **Claude Code**：已修改完成，保留了原格式。

### 斜杠命令

| 命令 | 功能 |
|:---|:---|
| `/new [名称]` | 开启新会话 |
| `/list` | 列出所有会话 |
| `/switch` | 切换会话 |
| `/current` | 显示当前会话 |
| `/history [数量]` | 显示最近 N 条消息 |
| `/mode [名称]` | 切换权限模式（default/edit/plan/yolo） |
| `/quiet` | 切换思考消息显示 |
| `/allow <工具名>` | 预批准某个工具 |
| `/stop` | 停止当前执行 |
| `/help` | 显示可用命令 |

### 权限管理

- `allow` / `允许` — 批准本次请求
- `deny` / `拒绝` — 拒绝本次请求
- `allow all` / `允许所有` — 本次会话剩余请求全部自动批准

## 五、守护进程与开机自启

```bash
# 注册为守护进程
cc-connect daemon install --config ~/.cc-connect/config.toml

# 启动
cc-connect daemon start

# 其他管理命令
cc-connect daemon stop       # 停止
cc-connect daemon restart    # 重启
cc-connect daemon status     # 查看状态
cc-connect daemon logs -f    # 实时查看日志
```

## 相关链接

- [CC-Switch GitHub](https://github.com/farion1231/cc-switch)
- [cc-connect GitHub](https://github.com/chenhg5/cc-connect)
- [Claude Code 官方文档](https://docs.anthropic.com/en/docs/claude-code/overview)
- [DeepSeek API 文档](https://platform.deepseek.com/docs)
