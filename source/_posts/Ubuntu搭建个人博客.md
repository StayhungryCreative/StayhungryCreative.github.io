---
title: Ubuntu搭建个人博客
date: 2025-01-05 15:55:01
excerpt: 搭建博客的步骤，一路踩坑，填，再填，越挫越勇
tags:
  - ubuntu
  - hexo
categories:
  - 工具
---



> 参考资料
>
> https://blog.csdn.net/wang_da_bing/article/details/82818445
>
> https://www.cnblogs.com/microxiami/p/12641163.html



资料讲的很详细，因此我只简单总结，大概会是我gitlab的第一篇博文。

完成 1-3 将获得一个空白的 gitlab 博客界面

完成 4 将创建自己的第一篇博客，撒花！！！

## 1、预备工作

```bash
# 1、安装git
sudo apt-get install git-core

# 2、安装nvm
## 安装依赖包
sudo apt-get update
sudo apt-get install build-essential libssl-dev
## 安装nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
## 验证nvm装好了
nvm -v 或者 nvm --version

# 3、安装node、npm
nvm install node # nvm install stable（功能和上一步相同，也是安装 node，pass）
## 验证node、npm安装好了
node -v
npm -v

# 4、安装hexo，一个博客框架，用来生成静态网页
npm install -g hexo-cli
## 建站
hexo init <folder> #hexo init StayhungryCreative
cd <folder> #cd StayhungryCreative
npm install
## 生成静态页面
hexo g # 或者 hexo generate
## 启动服务器
hexo s # 或者 hexo server
## 用 http://localhost:4000/ 预览
```



## 2、ssh 和 Git 配置

生成ssh密钥对，本地配置 Git 仓库信息，步骤省略，以后忘记/有需要再补充

```bash
git config --global user.name "github用户名"
git config --global user.email "github注册使用的邮箱地址" 
ssh -T git@github.com
> Hi StayhungryCreative! You've successfully authenticated, but GitHub does not provide shell access.
```



## 3、博客部署到 Git

以下操作都是在本地生成的博客文件夹StayhungryCreative中进行

1、命令行安装hexo插件

```bash
npm install hexo-deployer-git --save
```

2、文件_config.yml，打开找到Deployment，按照如下修改

```bash
deploy:
#type: git
#repo: git@github.com:用户名/用户名.github.io.git
#branch: master

type: git
repo: git@github.com:StayhungryCreative/StayhungryCreative.github.io.git
branch: master
```

3、将博客部署到git

```bash
hexo clean
hexo g # 或者 hexo generate
hexo d # 或者 hexo deploy
```

4、补充内容

```bash
# 下载的一个博客主题（使用略）
git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```



## 4、创建新博文

以下操作都是在本地生成的博客文件夹StayhungryCreative中进行

```bash
# 1、创建新文章
hexo new "Ubuntu搭建个人博客"		# 文件路径在 source/_posts/Ubuntu搭建个人博客.md
## 打开该md文件撰写即可

# 2、本地预览，确认内容是否有问题
hexo server

# 3、博文部署到 github pages
hexo clean
hexo g # 或者 hexo generate
hexo d # 或者 hexo deploy
```
