
## 打开Git
---
1. 打开git bash
2. `cd /d/cs61b`


## 使用Git Bash提交至GitHub
---
### Q: 如何连接代码目录和GitHub仓库
吕天白@DESKTOP-81VN84D MINGW64 /d/cs61b $ git status fatal: not a git repository (or any of the parent directories): .git我好像没有把代码目录和GitHub仓库[https://github.com/blackblake/cs61b连接起来，我该怎么做？](https://github.com/blackblake/cs61b连接起来，我该怎么做？)

### A:

1.  首先，在你的本地目录（/d/cs61b）初始化 Git 仓库：

```bash
git init
```

1.  添加远程仓库：

```bash
git remote add origin https://github.com/blackblake/cs61b.git
```

1.  拉取远程仓库的内容（如果远程仓库已经有内容）：

```bash
git pull origin main
```

（注意：如果你的主分支是 `master` 而不是 `main`，请使用 `git pull origin master`）

1.  如果你已经在本地有文件，你可以添加并提交它们：

```bash
git add --all #添加所有更改
git commit -m "初始化仓库"
```

1.  最后，将本地内容推送到 GitHub：

```bash
git remote set-url origin git@github.com:blackblake/cs61b.git
```

（同样，如果主分支是 `master`，请使用 `git push -u origin master`）

这样就完成了本地目录与 GitHub 仓库的连接。执行 `git status` 应该能够看到正常的状态信息了。
