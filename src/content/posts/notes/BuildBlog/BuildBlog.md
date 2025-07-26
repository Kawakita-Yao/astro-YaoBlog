---
title: Astro + Fuwai 主题 + Netlify 部署的 GitHub 项目
published: 2025-07-15
description: ''
image: './astro.jpg'      # cover image
tags: [Astro]
category: 'Notes'
draft: false 
---



# Intro



# 准备工作

## 环境准备
- __Node.js和pnpm(perfomant node package manager)__
	
  去<a href="https://nodejs.org" title="nodejs.org">Node官网</a>下载LTS（long term support）版本，直接CV bash script即可，会自动下载好**nvm(node version manager)**和**pnpm**
	
  > PS：如果要安装其他版本的Node（eg. v20），用**nvm**
  >
  > ```shell
  > nvm install 20
  > nvm use 20
  > nvm ls 									# 查看你安装过的 Node.js 版本
  > nvm alias default 22 		# 默认v22
  > nvm uninstall <version> 
  > ```

## <a href="https://docs.astro.build/zh-cn/tutorial/0-introduction/" title="贴心的astro教程">Astro DOCS Tutorial: Build a blog</a>

- :::warning
  **Failed to connect to github.com port 443**问题
  :::

  - **本质原因：Git不自动使用你系统的“全局代理”**

    Git 命令行工具（如 `git clone`）默认不会自动走你的系统代理或翻墙工具，它**需要手动配置代理地址**

    > 没有为 Git 单独设置代理
    > 设置的代理地址或端口不对（比如 Clash 开的不是 7890）

  - **解决方案**

    1. 确认本地代理端口

       在 Clash 的设置中确认 HTTP 代理端口

    2. 为 Git 设置代理（HTTP + HTTPS）

       ```shell
       git config --global http.proxy http://127.0.0.1:7890
       git config --global https.proxy http://127.0.0.1:7890
       ```

       > PS：127.0.0.1 是特殊保留地址，和localhost等价

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
     

# 开始搭建

## title

1. 通过配置文件 `src/config.ts` 自定义博客
2. 执行 `pnpm new-post <filename>` 创建新文章，并在 `src/content/posts/` 目录中编辑
3. 参考[官方指南](https://docs.astro.build/zh-cn/guides/deploy/)将博客部署至 Vercel, Netlify, GitHub Pages 等；部署前需编辑 `astro.config.mjs` 中的站点设置。



|           **Command**            | **Action**                             |
| :------------------------------: | :------------------------------------: |
| `pnpm install && pnpm add sharp` | 安装依赖                               |
|            `pnpm dev`            | 在 `localhost:4321` 启动本地开发服务器 |
|           `pnpm build`           | 构建网站至 `./dist/`                   |
|          `pnpm preview`          | 本地预览已构建的网站                   |
|    `pnpm new-post <filename>`    | 创建新文章                             |
|         `pnpm astro ...`         | 执行 `astro add`, `astro check` 等指令 |
|       `pnpm astro --help`        | 显示 Astro CLI 帮助                    |