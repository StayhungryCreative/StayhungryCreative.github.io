---
title: Git 使用教程
date: 2025-01-05 18:07:06
excerpt: Github 仓库代码管理教程，经常使用！
tags:
  - Git
  - Github
categories:
  - 工具
---

按照 git 常用的操作来记录

1、git config 配置本机信息

```bash
## 配置本机的name和email
git config --global user.name "github用户名"
git config --global user.email "github注册使用的邮箱地址" 
## 查看当前配置
git config -l # 或者 git config --list
```



2、git init 将**本地当前目录初始化**为**本地 Git 仓库**

```bash
git init #如果没有初始化为 git 仓库，后面所有操作都会报错
```



3、git add 将**本地文件添加到缓存区**

```bash
git add --all #最简单无脑的操作，区域所有文件修改都提交，包括删除文件
git add . # 同上，但不记录删除操作
```



4、git commit **提交本地文件的更改**到**本地 Git 仓库**

```bash
git commit -m "the 1st try"
```



5、git remote **添加远程 Git 仓库**	

```bash
# 添加远程仓库并指定名称
git remote add <远程仓库别名> <remote_url>
## 举例
git remote add origin git@github.com:StayhungryCreative/test.git # 此处的origin是远程仓库的别名，可以改成任意其她名称

# 删除关联指定名称的远程仓库
git remote rm <远程仓库别名>
## 举例
git remote rm origin

## 验证远程Git仓库是否添加成功
git remote
git remote -v
```



6、git push 将**本地 Git 仓库**的更新**推送到远程Git仓库**

```python
git push <远程仓库名> <本地仓库分支名>:<远程仓库分支名> 
## 举例
git push origin master # 🌳 省略了远程分支名，结果是：本地的master分支推送到origin主机的master分支，如果远程不存在master分支，则新建。
git push origin master:20250105

# 删除远程主机的分支
git push <远程仓库名> --delete <远程仓库分支名>
git push <远程仓库名> :<远程仓库分支名>
## 举例
git push origin :master # 删除远程主机的master分支

# 将本地的master分支推动到远程origin仓库，同时指定origin为默认远程仓库
git push -u origin master 
```



