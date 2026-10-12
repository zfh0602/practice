# Git 动手练习：第一课

本练习使用 Windows PowerShell。逐条运行命令，先预测结果，再观察输出。
练习文件已经准备好，仓库初始化和提交留给你亲手完成。

## 1. 初始化仓库

```powershell
Set-Location 'D:\git_practice\lesson-01'
git init -b main
git status
```

预期：当前分支为 main，还没有提交；README.md 和 notes.txt 为未跟踪文件。
未跟踪表示 Git 看到了文件，但尚未把它们加入版本管理。

## 2. 检查提交身份

```powershell
git config user.name
git config user.email
```

如果没有输出，或信息不符合你的用途，执行下面的命令。先把占位文字换成自己的真实信息，不要原样复制占位内容。

```powershell
git config user.name '替换成你的姓名'
git config user.email '替换成你的邮箱'
```

这里没有使用 --global，配置只对本练习仓库生效。
提交身份用于记录作者，不等于远程平台的登录凭据。

## 3. 保存第一个版本

```powershell
Get-Content -LiteralPath notes.txt -Encoding UTF8
git add README.md notes.txt
git status
git diff --staged
git commit -m 'docs: 创建 Git 学习笔记'
git status
git log --oneline
```

预期：

- add 后，两个文件进入暂存区，等待提交。
- diff --staged 展示这次将提交的内容。
- commit 后，工作区干净（working tree clean）。
- log 显示一条提交，前面的字符是提交 ID 的缩写。

如果 diff 或 log 进入分页界面，按 q 返回命令行。

## 4. 观察工作区与暂存区的区别

先给笔记增加一行：

```powershell
Add-Content -LiteralPath notes.txt -Value '第一条：git status 用来查看当前状态。' -Encoding UTF8
git status
git diff
git add notes.txt
git diff
git diff --staged
```

预期：add 前，git diff 能看到新增行；add 后，git diff 没有输出，而 git diff --staged 能看到新增行。

再增加一行，暂时不要 add：

```powershell
Add-Content -LiteralPath notes.txt -Value '第二条：git add 保存的是执行时选中的修改。' -Encoding UTF8
git status
git diff
git diff --staged
```

预期：同一个文件同时出现在“已暂存”和“未暂存”的修改中。

- git diff 展示刚增加的第二条笔记。
- git diff --staged 展示之前暂存的第一条笔记。
- 此刻若提交，只会提交已暂存的内容。

把第二条也加入暂存区，然后提交：

```powershell
git add notes.txt
git diff --staged
git commit -m 'docs: 记录状态检查与暂存机制'
git status
git log --oneline
```

预期：现在有两条提交，工作区干净。

## 5. 验证自己真的理解了

先独立回答，再对照上面的实验：

1. 修改文件之后，不执行 add 就 commit，这些修改会被提交吗？（本课使用普通 git commit，不带 -a。）
2. add 之后继续修改同一文件，新增修改会自动进入暂存区吗？
3. git diff 和 git diff --staged 分别比较什么？
4. commit 成功后，代码是否已经上传到远程？

通过标准：能解释工作区、暂存区和提交之间的关系；两次提交均能在 log 中找到；最终 status 干净。

## 后续实操路线

每次练习 20～30 分钟，通过一课再进入下一课。

1. 本课：初始化、检查状态、暂存、提交、查看差异。
2. 分支：创建 feat/notes 分支，提交新笔记，切换分支观察文件，再合并回 main。
3. 冲突：让两个分支修改同一行，合并并手动解决冲突。
4. 撤销：练习 restore --staged、stash 和 revert，区分每种操作作用的范围。
5. 协作：创建本地 bare 仓库模拟远程，用两个克隆目录模拟两位同事，练习 fetch、pull 和 push。
6. 团队流程：在自己的代码托管平台练习仓库中创建功能分支，推送并发起 PR/MR。

本课所有命令均在 lesson-01 目录内执行。出现不理解的错误时，先保留输出，不要随意尝试强制覆盖或删除命令。
