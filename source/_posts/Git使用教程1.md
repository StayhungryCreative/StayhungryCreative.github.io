---
title: Git 使用教程1：git入门操作
date: 2025-01-05 18:07:06
excerpt: Github 仓库代码管理教程，经常使用！
tags:
  - Git
  - Github
categories:
  - 工具
---



[toc]

# git 常用操作记录

## git config

配置本机信息

```bash
## 配置本机的name和email
git config --global user.name "github用户名"
git config --global user.email "github注册使用的邮箱地址" 

## 查看当前配置
git config -l # 等同于 git config --list
```



## git init 

将**本地当前目录初始化**为**本地 Git 空仓库**，初始化后自动将该仓库设置为master。

如果没有初始化为 git 仓库，后面所有操作都会报错。

具体内容参考 [关于Git这一篇就够了_git push origin --delete-CSDN博客](https://blog.csdn.net/bjbz_cxy/article/details/116703787) ，此处不做详细说明。

```bash
git init
```



## git add

将**本地文件添加到缓存区**

```bash
git add --all #最简单无脑的操作，区域所有文件修改都提交，包括删除文件
git add . # 同上，但不记录删除操作
```



## git commit

提交本地文件**修改的描述信息**到**本地 Git 仓库**。

通常是本地仓库迭代一个版本时进行，还**需结合git push推到远端仓库**。

```bash
git commit -m "the 1st try"	# 生成id，将git add提交到缓存区的文件提交到本地仓库，便于回滚。

git commit --amend 			# 修改提交的注释
```



## git remote 

**添加远端 Git 仓库**。用于将本地项目连接到远端 Git仓库。

```bash
## 添加远端仓库并指定名称
git remote add <远端仓库别名> <remote_url>
git remote add origin git@github.com:StayhungryCreative/test.git # 此处的origin是远端仓库的别名，可以改成任意其她名称

## 删除关联指定名称的远端仓库
git remote rm <远端仓库别名>

# 远程主机重命名
git remote rename <原主机名> <新主机名> 

## 验证远程Git仓库是否添加成功
git remote		# 列出所有远程主机
git remote -v	# 列出远程主机+网址
```



## git push

将**本地 Git 仓库**的更新**推送到远程Git仓库**

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



## git clone

克隆远端仓库数据到本地

```python
git clone ssh://git@xxx.xxx:/project.git
```

