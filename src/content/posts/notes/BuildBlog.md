---
title: Astro + Fuwai 主题 + Netlify 部署的 GitHub 项目
published: 2025-07-15
description: "A step-by-step guide to building a blog using Astro and deploying it on Netlify."
image: "/images/astro.jpg" # cover image
tags: [Astro, Blog, Netlify, Deploy, Static Site, GitHub]
category: "Static Site"
draft: false
---

# Intro

本文记录了如何使用 Astro 框架，配合 Fuwai 博客主题，并部署到 Netlify 上，构建一个现代化的个人博客。<br/>

# 准备工作

## 1. 环境准备

> 本教程开发环境版本参考：<br/>
> Node.js <= 22<br/>
> pnpm <= 9

### 安装 Node.js 和 pnpm(perfomant node package manager)

去<a href="https://nodejs.org" title="nodejs.org">Node 官网</a>下载 LTS（long term support）版本，直接 CV bash script 即可，会自动下载好**nvm(node version manager)**和**pnpm**

:::tip
如果要安装其他版本的 Node（eg. v20），用**nvm**

```shell
nvm install 20
nvm use 20
nvm ls 									# 查看你安装过的 Node.js 版本
nvm alias default 22 		# 默认v22
nvm uninstall <version>
```

:::

### 可选：Astro 官方教程

> <a href="https://docs.astro.build/zh-cn/tutorial/0-introduction/" title="贴心的astro教程">Astro DOCS Tutorial: Build a blog</a>

## 2. 常见问题：Failed to connect to github.com port 443

:::important
Git 没有正确设置代理
:::

### 本质原因：Git 不自动使用你系统的“全局代理”

Git 命令行工具（如 `git clone`）默认不会自动走你的系统代理或翻墙工具，它**需要手动配置代理地址**

> 没有为 Git 单独设置代理<br/>
> 设置的代理地址或端口不对（比如 Clash 开的不是 7890

### 解决方案

1. 确认 Clash 的 HTTP 代理端口

2. 为 Git 设置代理（HTTP + HTTPS）

   ```shell
   git config --global http.proxy http://127.0.0.1:7890
   git config --global https.proxy http://127.0.0.1:7890
   ```

   :::note
   PS：127.0.0.1 是特殊保留地址，和 localhost 等价
   :::

3. 可选：关闭 SSL 验证（避免某些证书问题）

   ```shell
   git config --global http.sslVerify false
   ```

4. 验证

   ```shell
   git config --global --get http.proxy
   git config --global --get https.proxy
   # 尝试克隆
   git clone https://github.com/github/gitignore.git
   ```

:::tip
取消代理设置（恢复默认直连）
:::

```shell
git config --global --unset http.proxy
git config --global --unset https.proxy
```

# 开始搭建 Blog

## 1. 使用 create-fuwari 在本地初始化项目

```shell
# pnpm
pnpm create fuwari@latest
```

## 2. 配置博客基础信息

- 编辑 `src/config.ts` 自定义博客标题、副标题、作者名等
- 添加 favicon、封面图、作者头像等资源

## 3. 创建新文章

- `pnpm new-post <filename>` 创建新文章，并在 `src/content/posts/` 中编辑

## 4. 常见 Command

|           **Command**            |               **Action**               |
| :------------------------------: | :------------------------------------: |
| `pnpm install && pnpm add sharp` |                安装依赖                |
|            `pnpm dev`            | 在 `localhost:4321` 启动本地开发服务器 |
|           `pnpm build`           |          构建网站至 `./dist/`          |
|          `pnpm preview`          |          本地预览已构建的网站          |
|    `pnpm new-post <filename>`    |               创建新文章               |
|         `pnpm astro ...`         | 执行 `astro add`, `astro check` 等指令 |
|       `pnpm astro --help`        |          显示 Astro CLI 帮助           |

# 部署到 Netlify

将博客部署到 [Netlify](https://www.netlify.com/) 上

## Step1: 提交项目到 GitHub

初始化 Git 仓库（如果还没有）：

```sh
git init
git remote add origin https://github.com/<your-username>/<repo-name>.git
git add .
git commit -m "init: blog project"
git push -u origin main #!
```

:::note
`git push -u origin main`这个命令：
| 参数/部分 | 含义说明 |
|---------------|--------------------------------------------------------------------------|
| `-u` | （全称 `--set-upstream`）设置默认的远程分支与本地分支的跟踪关系 |
| `origin` | 远程仓库的名称，通常默认是 `origin` |
| `main` | 推送的本地分支名称，通常是 `main`，也可能是 `master` 或其他自定义分支 |

:::

## Step2: 确保 astro.config.mjs 中配置了网站地址（用于正确生成 sitemap 和路径）：

```js
export default defineConfig({
  site: "https://<your-name>.netlify.app", // 改成你 Netlify 分配的地址
});
```

根据[官方 Guides](https://docs.astro.build/en/guides/deploy/)部署到 Netlify，运行以下命令

```shell
pnpm astro add netlify
```

## Step3: 最后在 Netlify 上导入 GitHub 里面的 Repo 即可

![Netlify部署](/images/netlify.png)
