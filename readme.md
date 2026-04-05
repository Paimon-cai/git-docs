# Git 教程：从入门到双平台同步推送

## 🎯 这份教程能帮你做什么

这是一份面向初学者到进阶用户的 Git 教程，目标是让你掌握：

- Git 的基本概念
- 日常提交与版本管理
- 分支与合并
- 远程仓库协作
- SSH 配置
- 一个项目同时同步到 GitHub 和 Gitee

如果你只想解决“双平台一次 push 同步两个仓库”，可以直接跳到：

- [第十二章：SSH 配置](#十二ssh-配置)
- [第十四章：一个项目同步推送到-github-和-gitee](#十四一个项目同步推送到-github-和-gitee)

---

# 一、Git 是什么

Git 是一个**分布式版本控制系统**（Distributed Version Control System）。

它的作用是：

- 记录文件变化
- 回退到历史版本
- 支持多人协作
- 通过分支安全地开发新功能

你可以把 Git 理解成：

> 给代码拍时间线快照，并且允许你随时切换、对比、合并。

## 图解理解

```text
你写代码的过程：

第 1 天  o
第 2 天  o
第 3 天  o
第 4 天  o
第 5 天  o

Git 做的事：

o──o──o──o──o
↑  ↑  ↑  ↑  ↑
每一次提交（commit）都是一个可回退的历史点
```

所以 Git 不只是“存文件”，而是在保存**文件随时间变化的历史**。

---

# 二、Git 的核心概念

Git 最重要的不是命令，而是理解它的三个区域：

```text
工作区（Working Directory）
    ↓ git add
暂存区（Staging Area / Index）
    ↓ git commit
本地仓库（Repository）
```

## 图解：三区模型

```text
┌────────────────────┐
│ 工作区              │
│ 你正在修改的文件     │
└─────────┬──────────┘
          │ git add
          ↓
┌────────────────────┐
│ 暂存区              │
│ 准备提交的内容       │
└─────────┬──────────┘
          │ git commit
          ↓
┌────────────────────┐
│ 本地仓库            │
│ 已保存的历史版本     │
└────────────────────┘
```

## 1. 工作区

就是你当前正在编辑的文件夹。

## 2. 暂存区

用于“准备提交”的区域。你可以只把一部分改动放进去。

## 3. 本地仓库

真正保存提交历史的地方。

## 常见误区

很多人以为修改文件后直接 `git commit` 就行，其实通常流程是：

```bash
git add 文件名
git commit -m "提交说明"
```

也就是说：

- `git add`：把改动放进暂存区
- `git commit`：把暂存区内容生成一个历史版本

## 图解：一次提交发生了什么

```text
你修改了 login.js
        │
        ▼
工作区里有改动
        │
        │ git add login.js
        ▼
暂存区准备好提交
        │
        │ git commit -m "fix: 修复登录问题"
        ▼
仓库中新增一个提交点
```

---

# 三、安装与初始配置

## 1. 检查 Git 是否已安装

```bash
git --version
```

示例输出：

```bash
git version 2.43.0
```

## 2. 设置身份信息

第一次使用 Git，建议先设置用户名和邮箱。

### 全局配置（推荐）

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

查看配置：

```bash
git config --global user.name
git config --global user.email
```

### 当前项目单独配置

如果某个项目需要使用不同身份：

```bash
git config user.name "项目专用用户名"
git config user.email "项目专用邮箱"
```

查看当前项目配置：

```bash
git config user.name
git config user.email
```

## 3. 查看全部配置

```bash
git config --list
```

## 图解：全局配置 vs 当前项目配置

```text
全局配置（--global）
├─ 项目 A：默认使用
├─ 项目 B：默认使用
└─ 项目 C：默认使用

当前项目配置（不带 --global）
└─ 只覆盖当前这个仓库
```

---

# 四、创建你的第一个仓库

## 方法 1：新建本地仓库

```bash
mkdir my-project
cd my-project
git init
```

示例输出：

```bash
Initialized empty Git repository in /path/to/my-project/.git/
```

这表示 Git 已经开始跟踪这个目录。

## 方法 2：克隆已有仓库

```bash
git clone 仓库地址
```

例如：

```bash
git clone git@github.com:用户名/仓库.git
```

## 图解：init 和 clone 的区别

```text
git init
你本地原来有一个普通文件夹
        ↓
把它变成 Git 仓库


git clone
远程已经有一个仓库
        ↓
复制一份到你的本地
```

---

# 五、最常用的日常操作

这是最核心的一组命令。

## 1. 查看状态

```bash
git status
```

它会告诉你：

- 哪些文件被修改了
- 哪些文件还没加入暂存区
- 哪些文件已经准备提交

## 2. 添加到暂存区

```bash
git add 文件名
```

添加全部改动：

```bash
git add .
```

## 3. 提交

```bash
git commit -m "提交说明"
```

例如：

```bash
git commit -m "feat: 添加登录页面"
```

## 4. 查看历史

```bash
git log
```

更常用的精简格式：

```bash
git log --oneline --graph --decorate
```

示例输出：

```bash
* a1b2c3d (HEAD -> main) feat: 添加登录页面
* e4f5g6h init project
```

## 5. 查看改动

查看工作区改动：

```bash
git diff
```

查看已暂存改动：

```bash
git diff --cached
```

## 图解：最常用 5 个命令在干什么

```text
git status   → 看现在是什么状态
git add      → 把改动放进暂存区
git commit   → 生成一个历史版本
git log      → 查看历史提交
git diff     → 对比改动内容
```

---

# 六、Git 基本工作流

你日常最常见的流程如下：

```text
修改文件
   ↓
git status
   ↓
git add
   ↓
git commit
   ↓
git push
```

对应命令示例：

```bash
git status
git add .
git commit -m "fix: 修复首页布局问题"
git push
```

## 图解：完整提交流程

```text
[修改代码]
    ↓
[git status]
    ↓
[git add]
    ↓
[git commit]
    ↓
[git push]
    ↓
[远程仓库更新]
```

可以把它记成一句话：

> 先检查，再暂存，再提交，最后推送。

---

# 七、文件管理与忽略规则

## 1. 忽略不需要提交的文件

创建 `.gitignore` 文件：

```gitignore
node_modules/
dist/
.env
*.log
.vscode/
```

常见应该忽略的内容：

- 依赖目录
- 构建产物
- 日志文件
- 本地环境变量
- IDE 临时文件

## 图解：为什么要用 .gitignore

```text
项目目录
├─ src/              ✅ 应提交
├─ package.json      ✅ 应提交
├─ node_modules/     ❌ 不应提交
├─ .env              ❌ 不应提交
└─ dist/             ❌ 通常不提交
```

## 2. 删除文件

```bash
git rm 文件名
```

如果只是删除工作区文件但不想停止跟踪，要谨慎使用。

## 3. 重命名文件

```bash
git mv 旧文件名 新文件名
```

---

# 八、分支基础

分支（branch）是 Git 最强大的功能之一。

你可以把它理解成：

> 在主线之外开一条独立开发线，开发完成后再合并回来。

## 1. 查看分支

```bash
git branch
```

## 2. 创建分支

```bash
git branch dev
```

## 3. 切换分支

```bash
git checkout dev
```

或使用更新写法：

```bash
git switch dev
```

## 4. 创建并切换

```bash
git checkout -b feature/login
```

或：

```bash
git switch -c feature/login
```

## 5. 分支图示

```text
main:    A --- B --- C
                  \
feature:           D --- E
```

此时 `feature` 在独立开发，尚未影响 `main`。

## 图解：为什么要开分支

```text
main
A --- B --- C ------------------

如果直接在 main 上开发：
A --- B --- C --- X --- Y --- Z
               ↑
          半成品也会混进去

如果新开分支：
main     A --- B --- C ----------
                    \
feature              X --- Y --- Z

开发完成后再合并，更安全
```

---

# 九、合并与变基

## 1. merge（合并）

把一个分支的改动合并到当前分支：

```bash
git checkout main
git merge feature/login
```

图示：

```text
A --- B --- C -------- M   main
           \        /
            D ---- E      feature/login
```

优点：

- 历史完整
- 比较安全
- 适合新手

## 2. rebase（变基）

把当前分支“接”到另一个分支最新提交后面：

```bash
git checkout feature/login
git rebase main
```

图示：

```text
原来：
A --- B --- C   main
       \
        D --- E feature

rebase 后：
A --- B --- C --- D' --- E' feature
```

优点：

- 历史更直
- 看起来更整洁

注意：

- 不要随便 rebase 已经共享给别人的公共分支

## 图解：merge 和 rebase 的直观区别

```text
merge：保留分叉历史

A --- B --- C -------- M
       \            /
        D --- E ----

rebase：把分支“搬到”主线后面

A --- B --- C --- D' --- E'
```

一句话记忆：

- `merge`：两条线汇合
- `rebase`：把支线接到主线末尾

---

# 十、冲突怎么解决

当 Git 无法自动合并时，就会出现冲突。

## 常见场景

两个人同时修改了同一文件的同一部分。

## 冲突标记示例

```text
<<<<<<< HEAD
当前分支内容
=======
另一个分支内容
>>>>>>> feature/login
```

## 解决步骤

1. 打开冲突文件
2. 手动保留正确内容
3. 删除冲突标记
4. 重新加入暂存区
5. 完成合并或变基

例如：

```bash
git add 冲突文件
git commit
```

如果是 rebase 过程中：

```bash
git add 冲突文件
git rebase --continue
```

如果想放弃本次 rebase：

```bash
git rebase --abort
```

## 图解：冲突是怎么来的

```text
同一行代码：

main 分支：     color = "blue"
feature 分支：  color = "red"

Git 不知道该保留哪一个
        ↓
      发生冲突
        ↓
需要你手动决定最终内容
```

---

# 十一、远程仓库基础

远程仓库就是托管在 GitHub、Gitee、GitLab 等平台上的仓库。

## 1. 查看远程

```bash
git remote -v
```

## 2. 添加远程仓库

```bash
git remote add origin 仓库地址
```

## 3. 拉取远程更新

```bash
git fetch
```

只下载，不自动合并。

## 4. 拉取并合并

```bash
git pull
```

相当于：

```bash
git fetch
git merge
```

## 5. 推送到远程

```bash
git push -u origin main
```

以后通常只需要：

```bash
git push
```

## 图解：本地和远程的关系

```text
你的电脑                         GitHub / Gitee
┌──────────────┐                ┌──────────────┐
│ 工作区        │                │ 远程仓库      │
│ 暂存区        │ -- git push -->│              │
│ 本地仓库      │<-- git pull ---│              │
└──────────────┘                └──────────────┘
```

---

# 十二、SSH 配置

如果你不想每次 push 都输入密码，推荐使用 SSH。

## 1. 生成 SSH key

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

如果系统太旧不支持 ed25519，也可以用 rsa，但优先推荐 ed25519。

生成后常见文件：

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

## 2. 查看公钥

```bash
cat ~/.ssh/id_ed25519.pub
```

把输出内容复制到代码托管平台的 SSH Keys 页面。

## 3. 测试连接

GitHub：

```bash
ssh -T git@github.com
```

Gitee：

```bash
ssh -T git@gitee.com
```

成功时通常会看到欢迎信息。

## 图解：SSH 是怎么工作的

```text
本地电脑
├─ 私钥：留在自己电脑里，不上传
└─ 公钥：上传到 GitHub / Gitee

连接时：
本地私钥  <----配对验证---->  平台上的公钥

匹配成功
→ 允许你访问仓库
```

---

# 十三、多人协作的常见流程

一种简单实用的流程如下：

## 1. 从 main 切新分支

```bash
git switch main
git pull
git switch -c feature/login
```

## 2. 开发并提交

```bash
git add .
git commit -m "feat: 添加登录接口"
```

## 3. 推送分支

```bash
git push -u origin feature/login
```

## 4. 发起 Pull Request / Merge Request

让同事审查代码后再合并。

## 推荐习惯

- 不要直接在 `main` 上做大改动
- 一个功能一个分支
- 提交信息尽量清晰
- 合并前先同步主分支最新代码

## 图解：团队协作流程

```text
main
  ↓
拉最新代码
  ↓
新建功能分支
  ↓
开发 + 提交
  ↓
push 到远程分支
  ↓
发起 PR / MR
  ↓
代码审查通过
  ↓
合并回 main
```

---

# 十四、一个项目同步推送到 GitHub 和 Gitee

这是本教程的重点之一。

目标是实现：

- 一个项目
- 两个平台（GitHub + Gitee）
- 一次 `git push` 同时推送两个远程仓库

## 图解：最终目标

```text
                ┌──────────────┐
                │   GitHub     │
                └──────▲───────┘
                       │
                       │ git push
                       │
本地仓库 ──────────────┼─────────────
                       │
                       │ git push
                       │
                ┌──────▼───────┐
                │    Gitee     │
                └──────────────┘
```

也就是：

> 一次 push，两个平台同时更新。

---

## 1. 先准备两个远程仓库

假设你已经在：

- GitHub
- Gitee

分别创建了同名或不同名仓库。

示例地址：

```bash
git@github.com:用户名/仓库.git
git@gitee.com:用户名/仓库.git
```

---

## 2. 给当前项目设置身份

进入项目目录：

```bash
cd ~/projects/tunex
```

设置项目身份：

```bash
git config user.name "你的用户名"
git config user.email "你的邮箱"
```

验证：

```bash
git config user.name
git config user.email
```

说明：

- 这里设置的是**当前仓库身份**
- 不会影响其他仓库（除非用了 `--global`）

---

## 3. 添加远程仓库

先添加主远程：

```bash
git remote add origin git@github.com:用户名/仓库.git
```

如果已经有 `origin`，先查看：

```bash
git remote -v
```

如确实需要删除再重建：

```bash
git remote remove origin
```

---

## 4. 给同一个 origin 配置多个 push 地址

这是关键操作：

```bash
git remote set-url --add --push origin git@github.com:用户名/仓库.git
git remote set-url --add --push origin git@gitee.com:用户名/仓库.git
```

解释：

- `origin` 仍然只有一个名字
- fetch 地址通常保留一个
- push 地址可以配置多个
- 执行 `git push` 时，会依次推送到所有 push URL

## 图解：一个 origin，多个 push 地址

```text
origin
├─ fetch → GitHub
├─ push  → GitHub
└─ push  → Gitee
```

这就是为什么一次 `git push` 可以同步两个平台。

---

## 5. 验证配置

```bash
git remote -v
```

你应该看到类似输出：

```text
origin  git@github.com:用户名/仓库.git (fetch)
origin  git@github.com:用户名/仓库.git (push)
origin  git@gitee.com:用户名/仓库.git (push)
```

---

## 6. 第一次提交与推送

```bash
git add .
git commit -m "init"
git push -u origin main
```

如果默认分支不是 `main`，请替换成你的实际分支名。

---

## 7. 后续使用

以后就正常提交：

```bash
git add .
git commit -m "update"
git push
```

这时一次 `git push` 就会同时推送到：

- GitHub
- Gitee

## 图解：同步推送流程

```text
[本地提交完成]
      ↓
[git push]
      ↓
[push 到 GitHub]
      ↓
[push 到 Gitee]
      ↓
[两个平台都更新]
```

---

# 十五、GitHub 和 Gitee 使用不同 SSH key 的配置

如果你希望 GitHub 和 Gitee 使用不同 SSH key，可以这样配置。

## 1. 生成两个 key

```bash
ssh-keygen -t ed25519 -C "github" -f ~/.ssh/github_key
ssh-keygen -t ed25519 -C "gitee" -f ~/.ssh/gitee_key
```

---

## 2. 配置 SSH

编辑文件：

```bash
nano ~/.ssh/config
```

写入：

```sshconfig
# GitHub
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_key
    IdentitiesOnly yes

# Gitee
Host gitee.com
    HostName gitee.com
    User git
    IdentityFile ~/.ssh/gitee_key
    IdentitiesOnly yes
```

## 图解：双 key 配置关系

```text
GitHub  <---- github_key ---- 本地电脑 ---- gitee_key ---->  Gitee
```

意思是：

- GitHub 用 `github_key`
- Gitee 用 `gitee_key`
- 两个平台互不干扰

---

## 3. 设置权限

```bash
chmod 600 ~/.ssh/github_key
chmod 600 ~/.ssh/gitee_key
```

---

## 4. 添加公钥到平台

```bash
cat ~/.ssh/github_key.pub
cat ~/.ssh/gitee_key.pub
```

分别复制到：

- GitHub SSH Keys
- Gitee SSH Keys

---

## 5. 测试连接

```bash
ssh -T git@github.com
ssh -T git@gitee.com
```

如果都成功，说明双 key 配置完成。

---

# 十六、stash：临时保存当前修改

当你改了一半代码，突然要切分支处理别的问题，可以用：

```bash
git stash
```

查看暂存列表：

```bash
git stash list
```

恢复最近一次暂存：

```bash
git stash pop
```

如果只想恢复但保留 stash：

```bash
git stash apply
```

## 图解：stash 像什么

```text
当前改到一半
   ↓
git stash
   ↓
先把现场收起来
   ↓
切去处理别的任务
   ↓
git stash pop
   ↓
再把现场拿回来
```

---

# 十七、回滚与撤销

这是 Git 里最容易混淆的一部分。

## 1. 撤销工作区修改

```bash
git checkout -- 文件名
```

更新写法：

```bash
git restore 文件名
```

## 2. 撤销暂存区

```bash
git restore --staged 文件名
```

## 3. 回退提交

### 保留修改，只回退提交记录

```bash
git reset --soft HEAD~1
```

### 回退提交并取消暂存

```bash
git reset --mixed HEAD~1
```

### 丢弃提交和本地修改

```bash
git reset --hard HEAD~1
```

注意：

- `--hard` 很危险，会直接丢失未保存改动

## 4. 安全回滚公共历史

如果某个提交已经 push 给别人了，更推荐：

```bash
git revert 提交ID
```

它会新建一个“反向提交”，而不是粗暴改历史。

## 图解：reset 和 revert 的区别

```text
reset
A --- B --- C
        ↑
      回到这里，后面的提交不保留历史

revert
A --- B --- C --- D
                ↑
        新增一个反向提交 D 来抵消 C
```

简单记：

- `reset`：改历史
- `revert`：补一个反向提交

---

# 十八、标签 tag

标签通常用于标记版本，例如：`v1.0.0`。

## 创建标签

```bash
git tag v1.0.0
```

查看标签：

```bash
git tag
```

推送标签：

```bash
git push origin v1.0.0
```

推送全部标签：

```bash
git push origin --tags
```

## 图解：tag 是什么

```text
A --- B --- C --- D
          \
           v1.0.0

标签就是给某个提交打一个“版本标记”
```

---

# 十九、常用进阶命令

## 1. 查看某次提交内容

```bash
git show 提交ID
```

## 2. 查看谁改了某一行

```bash
git blame 文件名
```

## 3. 挑选某个提交到当前分支

```bash
git cherry-pick 提交ID
```

适合把某个修复单独搬过来。

## 图解：cherry-pick 的作用

```text
main:     A --- B --- C
feature:        \--- D --- E

只想把 D 拿过来
        ↓
main:     A --- B --- C --- D'
```

---

# 二十、常见问题与排错

## 1. push 时提示权限错误

先检查：

```bash
ssh -T git@github.com
ssh -T git@gitee.com
```

再检查远程地址：

```bash
git remote -v
```

## 2. 提示分支不存在

查看当前分支：

```bash
git branch
```

有些仓库默认分支是 `master`，有些是 `main`。

## 3. pull 时出现冲突

说明远程和本地都修改了同一位置，需要手动解决冲突。

## 4. 不小心把不该提交的文件提交了

如果只是最近一次提交，且还没 push，可以考虑：

```bash
git reset --soft HEAD~1
```

然后重新整理后再提交。

如果已经 push，优先考虑：

```bash
git revert 提交ID
```

## 图解：排错思路

```text
push 失败
  ↓
先看报错信息
  ↓
是权限问题？ → 检查 SSH
是地址问题？ → 检查 remote
是分支问题？ → 检查 branch
是冲突问题？ → 手动解决冲突
```

---

# 二十一、最佳实践

## 1. 提交信息写清楚

推荐格式：

```text
feat: 新功能
fix: 修复问题
docs: 文档更新
refactor: 重构
chore: 杂项维护
```

例如：

```bash
git commit -m "fix: 修复登录接口参数错误"
```

## 2. 小步提交

不要把很多不相关修改塞进一次 commit。

## 3. 不直接在 main 上做复杂开发

新建分支更安全。

## 4. 提交前先看状态

```bash
git status
git diff
```

## 5. 统一开发环境

如果你使用 WSL，就尽量一直在 WSL 里操作，不要混用 Windows 和 WSL，避免路径、权限、SSH 配置混乱。

## 图解：推荐习惯

```text
好习惯：
改一点 → 提交一点 → 信息写清楚 → 再 push

不推荐：
攒一大堆修改 → 一次全提交 → 自己都看不懂改了什么
```

---

# 二十二、速查表

## 初始化与配置

```bash
git init
git clone 仓库地址
git config --global user.name "名字"
git config --global user.email "邮箱"
```

## 日常提交

```bash
git status
git add .
git commit -m "说明"
git push
```

## 历史查看

```bash
git log
git log --oneline --graph --decorate
git show 提交ID
git diff
```

## 分支操作

```bash
git branch
git switch -c 新分支
git switch main
git merge 分支名
```

## 回滚恢复

```bash
git restore 文件名
git restore --staged 文件名
git reset --soft HEAD~1
git revert 提交ID
```

## 远程仓库

```bash
git remote -v
git remote add origin 地址
git pull
git push -u origin main
```

## 双平台同步推送

```bash
git remote set-url --add --push origin git@github.com:用户名/仓库.git
git remote set-url --add --push origin git@gitee.com:用户名/仓库.git
git push
```

---

# 🎉 总结

学 Git 最重要的不是死记命令，而是建立这套理解：

- 文件先在工作区修改
- 再进入暂存区
- 再提交到本地仓库
- 最后推送到远程仓库

而双平台同步的本质也很简单：

- 一个 `origin`
- 多个 `push URL`
- 一次 `git push`
- 同步多个平台

如果你愿意，我下一步还可以继续帮你升级成：

1. **加入真实命令输出示例版**
2. **加入 Git 冲突实战演示版**
3. **整理成更漂亮的目录导航版**

你选一个，我继续直接帮你改。