---
title: "Git冲突解决"
date: 2020-12-15T10:35:40+08:00
---

## git冲突解决

1. git reset 到远程分支，保持与本地同步

```
git reset --hard origin/master
```

2. git cherry-pick到本地最新的commit id

```
git cherry-pick commit-id
```

3. git push