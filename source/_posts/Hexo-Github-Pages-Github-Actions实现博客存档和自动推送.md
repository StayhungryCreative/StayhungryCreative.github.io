---
title: Hexo + Github Pages + Github Actions实现博客存档和自动推送
date: 2025-01-08 08:42:30
excerpt: 搭建个人博客后的优化工作，实现博客存档和自动推送
tags:
  - Github Actions
  - Github Pages
  - Hexo
  - Ubuntu
categories:
  - 工具
---



## 1、Hexo 的推送策略

在使用 GitHub Actions 部署 Hexo 博客时，通常将 **Hexo 的源代码**和**生成的静态文件**分开管理，通过分支或独立仓库实现**隔离**。

1.1 Hexo 源代码仓库

- Hexo 的配置文件、主题文件等存储在源代码仓库（放在 main 分支）
- 通过 **deploy.yml** 文件**生成静态文件并推送到目标分支**。

1.2 静态文件部署仓库/分支

- 生成的静态文件通常会被**部署到 gh-pages 分支**，或一个独立的仓库。
- **GitHub Actions 自动处理文件生成与推送**，无需手动管理。



## 2、单仓库 多分枝管理

当前我采用这一策略，**将 Hexo 源代码保存在 main（或 master）分支，静态文件部署到 gh-pages 分支。**此处步骤记录不完整，如果你想从零开始搭建，可参考 `Ubuntu + Hexo + Github Pages 搭建个人博客`。

1、Hexo 项目本地初始化

```bash
# 建站，初始化 Hexo 项目
hexo init StayhungryCreative
cd StayhungryCreative
npm install

# 安装 Hexo 部署到 github 的插件
npm install hexo-deployer-git --save
```

2、配置 Hexo 项目根目录下的 _config.yml 文件，找到 deploy 配置项，修改如下：

```bash
deploy:
  type: git	# 部署方式，这里选择 git
  repo: git@github.com:StayhungryCreative/StayhungryCreative.github.io.git # 仓库地址，指向你的远程仓库
  branch: gh-pages #main 静态文件生成后推送到的分支
```

3、配置 .github/workflows/deploy.yml 文件

```bash
mkdir -p .github/workflows
touch .github/workflows/deploy.yml
```

deploy.yml 中**指定工作流触发的分支**，文件内容如下

```yaml
name: Deploy Hexo Blog  # Workflow 的名称，可自由修改为更具描述性的名字

on:  # 定义触发条件
  push:  # 当发生 push 操作时触发
    branches:  # 监控哪些分支
      - main  # 这里默认是主分支，如果你的分支名是master或其他名称，需要改成对应分支名

jobs:  # 定义工作流程的任务
  build:  # 工作任务的名称，可自由命名
    runs-on: ubuntu-latest  # 指定运行环境为最新的 Ubuntu 系统

    steps:  # 任务的步骤
    - name: Checkout code  # 检查代码
      uses: actions/checkout@v3  # 使用 GitHub 提供的官方代码检查工具

    - name: Setup Node.js  # 设置 Node.js 环境
      uses: actions/setup-node@v3  # 使用 GitHub 提供的 Node.js 设置工具
      with:
        node-version: '23'  # 指定 Node.js 版本（根据 Hexo 使用的版本修改）

    - name: Install dependencies  # 安装依赖
      run: npm install  # 在项目中安装 Hexo 和插件的依赖

    - name: Build Hexo  # 生成静态文件
      run: npx hexo generate  # 运行 Hexo 命令，生成 public 文件夹

    - name: Deploy to GitHub Pages  # 部署到 GitHub Pages
      uses: peaceiris/actions-gh-pages@v3  # 使用 peaceiris 提供的 GitHub Pages 部署工具
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}  # 使用 GitHub Actions 自动生成的 Token（权限令牌）
        publish_branch: gh-pages	# 指定静态文件生成后推送到 gh-pages 分支
        publish_dir: ./public  # 指定要部署的文件夹，Hexo 默认生成到 public 目录
```

4、首次手动部署

```bash
hexo clean
hexo generate
hexo deploy
```

5、提交代码并自动推送博客，后续只用执行这三行代码就可实现**存档+推送博客**

```bash
# 1、创建并初始化 Git 仓库
git init
git remote add origin git@github.com:StayhungryCreative/StayhungryCreative.github.io.git
git remote -v
git branch -M main ## ♥ 强制重命名，将本地仓库的分支名称从 master 改为 main

# 2、提交代码
git add .
git commit -m "Initialize Hexo Blog"
git push origin main	# 源代码提交到 main 分支
```

6、启用 GitHub Pages

打开仓库的 Settings > Pages 页面，选择 gh-pages 分支作为部署源，即可网页查看博客。



## 3、双仓库管理

没有实操过，但方法看着很合理，**和“单仓库 多分枝管理”步骤相同的部分直接略过，仅记录不同点**。

0、将 Hexo 源代码和静态文件分别存放在两个不同的仓库：

- **仓库 A**：存储 Hexo 源代码，例如 `blog-src`。
- **仓库 B**：存储生成的静态文件，例如 `blog-deploy`。

2、配置 Hexo 项目根目录下的 _config.yml 文件，找到 deploy 配置项，修改如下：

```bash
deploy:
  type: git	# 部署方式，这里选择 git
  repo: git@github.com:StayhungryCreative/blog-deploy.git # 仓库地址，指向存放静态文件的仓库
  branch: main #静态文件生成后推送到的分支
```

3、deploy.yml 文件内容如下

```yaml
    - name: Deploy to GitHub Pages  # 步骤名称：部署到 GitHub Pages
      uses: peaceiris/actions-gh-pages@v3  # 使用 peaceiris 提供的 GitHub Pages 部署工具
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}  # 使用 GitHub Actions 自动生成的 Token（权限令牌）
        external_repository: StayhungryCreative/blog-deploy # 指定部署静态文件的仓库
        publish_branch: main	# 指定静态文件生成后推送到仓库 B 的 main 分支
        publish_dir: ./public  # 指定要部署的文件夹，Hexo 默认生成到 public 目录
```

5、提交代码并自动推送博客，后续只用执行这三行代码就可实现**存档+推送博客**

```bash
# 1、创建并初始化 Git 仓库
git init
## 源代码提交到 blog-src 仓库
git remote add origin git@github.com:StayhungryCreative/blog-src.git
git remote -v
git branch -M main ## ♥ 强制重命名，将本地仓库的分支名称从 master 改为 main

# 2、提交代码
git add .
git commit -m "Initialize Hexo Blog"
git push origin main	# 源代码提交到 main 分支
```

6、启用 GitHub Pages

打开仓库 blog-deploy 的 Settings > Pages 页面，选择 main 分支作为部署源，即可网页查看博客。



## 4、方式对比

感觉两种方式实际使用应该没多大差别，表格可信度<4分（10分为满分）

|          功能           |       单仓库（多分支）       |               双仓库               |
| :---------------------: | :--------------------------: | :--------------------------------: |
|      **管理难度**       |             简单             |               稍复杂               |
|       **分离性**        | 源代码和静态文件在一个仓库内 |      源代码和静态文件完全分离      |
|      **适合场景**       |     小型博客，单用户维护     |        大型博客，多用户协作        |
| **GitHub Actions 支持** |           内置支持           | 增加一步，配置 external_repository |
