---
title: "Git"
date: 2020-07-05T22:23:02+08:00
tags: ["git操作"]
---

## git区别

git pull 等价于 git fetch + git merge

git pull --rebase 等价于 git fetch + git rebase

> 在不同分支用merge(个人不赞同)

> 在同一分支用rebase

`git pull --rebase`

## merge与rebase区别

### merge

假设我们有一个主分支 master 及一个开发分支 deve，仓库历史就像这样：

![image](https://upload-images.jianshu.io/upload_images/178692-857161ce33f79a87.png?imageMogr2/auto-orient/strip|imageView2/2/w/459/format/webp)

现在如果在 master 分支上 git merge dev：Git 会自动根据两个分支的共同祖先即 e381a81 这个 commit 和两个分支的最新提交即 8ab7cff 和 696398a 进行一个三方合并，然后将合并中修改的内容生成一个新的 commit，即下图的 78941cb

![image](https://upload-images.jianshu.io/upload_images/178692-8862813b57f221df.png?imageMogr2/auto-orient/strip|imageView2/2/w/457/format/webp)

### rebase

如果是在 master 分支上 git rebase deve：

Git 会从两个分支的共同祖先 3311ba0 开始提取 master 分支（当前所在分支）上的修改，即 85841be、a016f64 与 e53ec51，再将 master 分支指向 deve 的最新提交（目标分支）即 35b6708 处，然后将刚刚提取的修改依次应用到这个最新提交后面。

操作会舍弃 master 分支上提取的 commit，同时不会像 merge 一样生成一个合并修改内容的 commit，相当于把 master 分支（当前所在分支）上的修改在 deve 分支（目标分支）上原样复制了一遍,操作完成后的版本历史就像这样：

![image](https://upload-images.jianshu.io/upload_images/178692-29cebbe428d8a196.png?imageMogr2/auto-orient/strip|imageView2/2/w/454/format/webp)

可以看见 master 分支从 deve 分支最新提交 35b6708 开始依次提交了自己的三个 commit（由于是提取修改后重新依次提交，故 commit 的 hash 码与上面的85841be、a016f64、e53ec51 不同）

### 冲突处理策略的不同

* merge 遇见冲突后会直接停止，等待手动解决冲突并重新提交 commit 后，才能再次 merge

* rebase 遇见冲突后会暂停当前操作，开发者可以选择手动解决冲突，然后 git rebase --continue 继续，或者 --skip 跳过（注意此操作中当前分支的修改会直接覆盖目标分支的冲突部分），亦或者 --abort 直接停止该次 rebase 操作
