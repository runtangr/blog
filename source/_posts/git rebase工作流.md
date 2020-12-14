---
title: git rebase 工作流
tags:
  - git
categories: 技术
abbrlink: 27141bww
date: 2020-12-07 00:00:00
---

## 起因
* 在工作中，使用git rebase工作流，对git的使用有一些理解，在此记录下。

## git rebase 工作流

* git clone 远程代码
* git checkout -b feature/#1{issue id} 
    * 已是最新代码可直接checkout
    * 在开发新功能前需要创建issue，issue创建最好有一些模板
* git add
* git commit -m "{message}"
    * message规则
        * Fix: 功能修复简短描述
        * Feat: 新功能简短描述
        * Style: 代码风格描述
        * ......
* git checkout develop
* git pull
    * 更新开发分支
* git checkout feature/#1
* git rebase develop
    * git rebase --continue 修复冲突
    * git rebase --abort 中止rebase
* git push origin feature/#1
* 在git上提交pr
    * 添加描述、指定issue
    * 指定reviewers


### 思考
* 当开发个人的项目可以随意的add、commit。但是随着项目开发人员的增多，必要的开发规范，是有必要的。