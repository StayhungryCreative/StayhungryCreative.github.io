---
title: Git使用教程2：关于标签和分支
date: 2025-02-04 10:45:20
excerpt: Github 仓库代码管理教程，经常使用！
tags:
  - Git
  - Github
categories:
  - 工具
---



## 标签和分支的异同点总结

[参考资料：Git中标签与分支的区别，我应该在这里使用哪个](https://geek-docs.com/git/git-questions/371_git_how_is_a_tag_different_from_a_branch_in_git_which_should_i_use_here.html)

1. 用于**记录重要里程碑、版本发布**，分支用于**支持并行开发、任务切换**。
2. 切换分支和“切换”标签完全不同！



## 一、分支的常用操作

```python
## 创建并切换分支
git checkout -b <新分支名>
# 等同于下面两步
git branch <新分支名>			# 创建分支
git checkout <新分支名>			# 切换到分支

## 创建分支并推送到远端
git branch <新分支名> 			# 本地创建分支
git push <远程主机名> <本地分支名>:<远程分支名> 	# 推送本地分支到远端
git branch -r

## 删除远端的分支（两种方法）
git push <远程主机名> --delete <远程分支名>
git push <远程主机名> :<远程分支名>	# 冒号前的空字符串表示本地无对应分支，因此会删除远程分支
```





## 二、标签的常用操作

```python
## 切换标签，注意：和切换分支完全不同！！！
git checkout release-C10	# 切换到标签 release-C10，处于“detached HEAD”的状态

## 本地操作
git tag 							# 查看本地有哪些标签，按q退出
git tag release-C01					# 创建标签 release-C01，默认加在最新的commit上——轻量标签
git tag -a release-C01 -m "标签说明"	# 本地创建标签 release-C01，并附上标注——附注标签
git tag -d release-C01 				# 删除标签 release-C01

## 创建标签并推送到远端
git tag release-C01							# 本地创建标签 release-C01
git push <远程主机名> <标签名>		# 推送本地标签到远端
git push <远程主机名> --tags		  # 推送本地所有标签到远端


## 删除远端的标签
git push <远程主机名> :refs/tags/<标签名>	# 和删除分支不同，删除标签时需要明确指定引用路径 refs/tags/
```



### 2.1 切换标签详解

在 Git 中，标签（tag）是**一个指向特定提交的静态引用**，通常用于标记重要的版本（如 `v1.0.0`）。**标签本身是不可移动的**，因此不能像分支一样直接“切换”到标签。

```python
git checkout release-C10	# 切换到标签 release-C10，处于“detached HEAD”的状态
```

分离头指针。这意味着当前状态不在任何分支上，新的提交不会属于任何分支。

如果想要在标签的基础上继续开发，可以基于标签创建一个新分支。

```python
git checkout -b <新分支名> release-C10 # 基于标签 release-C10 创建新的分支
```

