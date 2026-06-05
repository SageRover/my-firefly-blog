---
title: Astro + Fuwari 静态博客部署记录
published: 2025-08-24
description: '使用 Astro 框架和 Fuwari 主题搭建静态博客，部署到 Vercel 并绑定自定义域名的完整流程。'
image: ''
tags: ["Astro", "Fuwari", "Vercel", "博客", "Cloudflare"]
category: '技术博客'
draft: false
---

## 🏗️ 技术栈

| 层 | 技术 |
|---|---|
| 框架 | **Astro** |
| 主题 | **Fuwari** |
| 部署 | **Vercel** |
| 域名 | `blog.20001114.xyz`（Cloudflare DNS） |
| 仓库 | `github.com/SageRover/my-fuwari-blog` |

## 📋 部署步骤

### 1️⃣ 项目初始化

- 通过 Fuwari 的 [GitHub Template](https://github.com/saicaca/fuwari) 生成自己的仓库
- 或 `git clone` 后推送到新建空仓库

### 2️⃣ Vercel 部署

- 从 GitHub 仓库导入，Vercel 自动识别 Astro 项目
- 部署成功获得 `*.vercel.app` 临时域名，点击访问测试

### 3️⃣ 自定义域名配置

- Vercel 项目设置 → Domains → 添加自定义域名
- Cloudflare 配置 DNS 解析（CNAME 指向 `cname.vercel-dns.com` 或 A 记录）

### 4️⃣ 本地开发

```bash
git clone <你的仓库地址>
cd my-fuwari-blog
pnpm install
```

- 修改 `astro.config.mjs` 中的 `site` 为你的域名
- 添加 `.md` 文章到 `src/content/posts/` 目录
- 遵循 Fuwari 的 YAML frontmatter 格式要求

### 5️⃣ 自动化部署

- 确保 Vercel 的 GitHub App 已安装
- GitHub 仓库关联 Vercel 项目后自动配置 Webhook
- 每次 `git push` 自动触发重新部署

## ⚠️ 注意事项

- 本地 git 配置的邮箱需与 GitHub 和 Vercel 账户一致
- Markdown 文件需包含正确的 frontmatter（`title`、`published`、`tags`、`category` 等）
- 如果部署后内容未更新，检查 Vercel 构建日志排查

## 💡 使用体验

**Fuwari** 设计简洁，加载速度快，内置代码高亮、搜索、标签分类等功能。

**Astro** 的 Islands 架构按需加载交互组件，默认输出零 JS，页面性能优秀。

**Vercel** 的自动部署流程省心，push 即上线，配合 GitHub 使用非常流畅。
