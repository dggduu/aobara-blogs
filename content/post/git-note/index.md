+++
title = 'Git Note'
date = 2024-10-17T08:13:05+08:00
draft = true
+++
## 安装
```javascript
scoop install git
winget install git
choco install git
```
## Git 创建仓库
使用当前目录作为 Git 仓库  
git init 
使用当指定目录作为 Git 仓库
git init [Name]
```javascript
git add *.c
git add README
git commit -m '提交说明'
```
注： 在 Linux 系统中，commit 信息使用单引号 '，Windows 系统，commit 信息使用双引号 "。