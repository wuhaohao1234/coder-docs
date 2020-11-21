---
title: "Git操作"
date: 2020-11-03T18:37:58+08:00
---

## 适合小型团队的git操作

项目中的操作

1. 以master分支为稳定分支，所有开发人员均以dev分支为主进行开发，待版本发布时候，将dev分支并入master分支

2. 使用pull request作为code review作为代码审查工具

### 具体实施

开发人员开发一个新功能，修补一个bug均基于当前最新的远程dev分支的代码，另外开一个分支名称

远程为dev

开发人员

`git pull origin dev`

创建新分支

`git checkout -b feature-clock`

完成开发

`git push origin feature-clock`

提交pull request

代码审查人员进行code review

code review通过后，merge到dev分支

然后删除远程feature-clock分支

当审核不通过，则提出修改意见，并打回

开发人员进行修补后，重复以上行为

期间要注意：

1. 所有人均要关注dev分支代码是否更新

当dev分支代码更新的时候，切换到dev分支，拉取最新的dev分支，然后在本地解决代码冲突，建议在每次代码提交之前均检查一遍

需要用到的命令:

```
git checkout dev

git pull origin dev --rebase

git checkout feat-clock

git merge dev

此时可能需要解决feat-clock分支与dev分支发生的代码冲突
```

2. 最好明确每个分支所做的事情

这样的好处是，对于新人学习旧代码是很友好的，可以很好的追踪问题来源，及时版本回退与bug修补

### 独立开发者或者双人开发的方式

这里采用结对编程比较合适，都在dev分支进行开发

### 需要注意的点

1. git pull = git fetch + git merge

当不明确时，建议使用git fetch

2. 可以使用git cherry-pick切换到一个commit，这个时候会产生一个新的分支，基于此commit进行开发

3. 可以使用git remote查看当前跟踪的存储库

4. 可以使用gitk查看当前的git树形图