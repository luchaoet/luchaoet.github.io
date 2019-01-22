---
title: git命令
date: 2018-03-06 17:35:16
tags: git
summary: git常用命令总结
---
### 创建并切换到该分支 
git checkout -b dev

### 切换分支
git checkout dev

### 查看分支
git branch  /\*本地分支\*/
git branch -a /\*本地和远程分支\*/

### 合并dev分支
git merge dev

### 删除分支
git branch -d dev /\*删除本地分支\*/
git branch -D dev /\*强行删除没有合并过的分支\*/
git push branch -d dev /\*删除远程分支\*/

### 重新命名本地分支
git branch -m dev1 dev2 /\*本地将dev1分支命名为dev2，推送远程后，dev1 dev2同时存在\*/

### git add

| 命令 | 修改的文件 | 新文件 | 删除的文件 |
| --- | --- | --- | --- |
| git add . | ✔ | ✔ | ✘ |
| git add -u | ✔ | ✘ | ✔ |
| git add -A | ✔ | ✔ | ✔ |

### 回到历史某版本
git reset --hard d97e
git reset --hard HEAD^ /\*回到上一个版本\*/

### 打开提交记录后台
gitk /\*mac上没有该命令\*/

### bug分支处理
场景：正在dev上开发，需要去其他分支处理紧急问题
1.在dev上储存为提交的代码 
git stash save "说明"
2.切换到其他分支去处理问题
3.且回到dev 查看存储的工作现场 
git stash list
4.恢复 
git stash pop /\*恢复 同时删除stash内容\*/

| git stash apply stash@{0} | 恢复时不删除stash内容 |
| --- | --- |
| git stash drop | 删除最近一个 |
| git stash clear | 删除所有的 |
| git stash drop stash@{0} | 删除特定某一个 |

### 各个分支作用

| master | 主分支 | 时刻与远程同步 |
| --- | --- | --- |
| dev | 开发分支 | 团队开发的代码都要合并到该分支 时刻与远程同步 |
| bug | bug修复分支 | bug修复完毕 合并到dev 没必要推送到远程 |

### 标签管理
git tag /\*查看所有的标签\*/
git tag v1.0 /\*打标签\*/
git tag v1.0 c80cc /\*给某历史版本打标签\*/
git show v1.0 /\*查看v1.0标签信息\*/
git tag -d v1.0 /\*删除v1.0标签\*/
git push origin v1.0 /\*推送标签到远程\*/
git push origin :refs/tags/v1.0 /\*删除远程tag标签\*/