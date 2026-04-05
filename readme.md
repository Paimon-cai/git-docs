# Git 教程：从入门到双平台同步推送

> 一份适合新手入门、也能作为日常查阅手册使用的 Git 教程。

[完整版教程](readme.md) | [速查表](git-速查表.md)

---

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

## 🗂️ 目录导航

### 第一部分：入门理解

- [一、Git 是什么](#一git-是什么)
- [二、Git 的核心概念](#二git-的核心概念)
- [三、安装与初始配置](#三安装与初始配置)
- [四、创建你的第一个仓库](#四创建你的第一个仓库)

### 第二部分：日常使用

- [五、最常用的日常操作](#五最常用的日常操作)
- [六、Git 基本工作流](#六git-基本工作流)
- [七、文件管理与忽略规则](#七文件管理与忽略规则)

### 第三部分：分支与协作

- [八、分支基础](#八分支基础)
- [九、合并与变基](#九合并与变基)
- [十、冲突怎么解决](#十冲突怎么解决)
- [十一、远程仓库基础](#十一远程仓库基础)
- [十二、SSH 配置](#十二ssh-配置)
- [十三、多人协作的常见流程](#十三多人协作的常见流程)

### 第四部分：双平台同步

- [十四、一个项目同步推送到 GitHub 和 Gitee](#十四一个项目同步推送到-github-和-gitee)
- [十五、GitHub 和 Gitee 使用不同 SSH key 的配置](#十五github-和-gitee-使用不同-ssh-key-的配置)

### 第五部分：进阶与排错

- [十六、stash：临时保存当前修改](#十六stash临时保存当前修改)
- [十七、回滚与撤销](#十七回滚与撤销)
- [十八、标签 tag](#十八标签-tag)
- [十九、常用进阶命令](#十九常用进阶命令)
- [二十、常见问题与排错](#二十常见问题与排错)
- [二十一、最佳实践](#二十一最佳实践)
- [二十二、速查表](#二十二速查表)

### 快速跳转

- [我只想学最常用命令](#五最常用的日常操作)
- [我只想看分支和合并](#八分支基础)
- [我只想看双平台同步推送](#十四一个项目同步推送到-github-和-gitee)
- [我只想看回滚撤销](#十七回滚与撤销)
- [我只想查命令](#二十二速查表)

---

## 🧭 阅读建议

如果你是第一次接触 Git，建议按这个顺序阅读：

1. [一、Git 是什么](#一git-是什么)
2. [二、Git 的核心概念](#二git-的核心概念)
3. [五、最常用的日常操作](#五最常用的日常操作)
4. [六、Git 基本工作流](#六git-基本工作流)
5. [八、分支基础](#八分支基础)
6. [十一、远程仓库基础](#十一远程仓库基础)
7. [十二、SSH 配置](#十二ssh-配置)
8. [十四、一个项目同步推送到 GitHub 和 Gitee](#十四一个项目同步推送到-github-和-gitee)

如果你已经会基本提交，建议重点看：

- [八、分支基础](#八分支基础)
- [九、合并与变基](#九合并与变基)
- [十、冲突怎么解决](#十冲突怎么解决)
- [十三、多人协作的常见流程](#十三多人协作的常见流程)
- [十七、回滚与撤销](#十七回滚与撤销)
- [二十、常见问题与排错](#二十常见问题与排错)

---

## 📝 使用说明

这份文档同时包含三种内容：

- **基础解释**：帮助你建立 Git 的整体理解
- **图解说明**：帮助你快速看懂概念和流程
- **实战案例**：帮助你在真实场景里知道该怎么做

建议你第一次阅读时先看主线流程，遇到不懂的地方再回头查对应章节。

如果你只是想快速查命令，也可以直接看：

- [Git 速查表](git-速查表.md)

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

示例输出：

```bash
On branch main
Changes not staged for commit:
  modified:   src/login.js

Untracked files:
  README-notes.txt

no changes added to commit
```

你可以这样理解：

- `modified`：这个文件改过了，但还没 `git add`
- `Untracked files`：这个文件 Git 还没开始跟踪

## 2. 添加到暂存区

```bash
git add 文件名
```

添加全部改动：

```bash
git add .
```

执行后再次 `git status`，常见输出会变成：

```bash
On branch main
Changes to be committed:
  modified:   src/login.js
  new file:   README-notes.txt
```

这说明文件已经进入暂存区，准备提交。

## 3. 提交

```bash
git commit -m "提交说明"
```

例如：

```bash
git commit -m "feat: 添加登录页面"
```

示例输出：

```bash
[main a1b2c3d] feat: 添加登录页面
 3 files changed, 45 insertions(+), 2 deletions(-)
```

你可以重点看：

- `a1b2c3d`：这次提交的 commit ID
- `3 files changed`：这次一共改了几个文件

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

含义：

- 最上面一行是最新提交
- `HEAD -> main` 表示你当前就在 `main` 分支最新位置

## 5. 查看改动

查看工作区改动：

```bash
git diff
```

示例输出：

```diff
- const title = "Login"
+ const title = "User Login"
```

查看已暂存改动：

```bash
git diff --cached
```

它会显示：

> 那些已经 `git add` 了、即将进入下一次提交的内容。

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

一套更真实的终端输出可能像这样：

```bash
$ git status
On branch main
Changes not staged for commit:
  modified:   src/home.css

$ git add .

$ git commit -m "fix: 修复首页布局问题"
[main b7c8d9e] fix: 修复首页布局问题
 1 file changed, 8 insertions(+), 2 deletions(-)

$ git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), done.
To github.com:user/repo.git
   a1b2c3d..b7c8d9e  main -> main
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

## 实战演示：一次最常见的合并冲突

下面用一个最简单的例子演示完整过程。

### 场景设定

当前仓库有一个文件 `config.txt`，初始内容是：

```text
theme=light
```

现在有两个人分别在不同分支修改它：

- `main` 分支改成 `theme=blue`
- `feature` 分支改成 `theme=dark`

因为改的是**同一行**，所以合并时会冲突。

### 第 1 步：在 main 分支修改并提交

```bash
git switch main
```

编辑 `config.txt`：

```text
theme=blue
```

提交：

```bash
git add config.txt
git commit -m "change: set theme blue"
```

### 第 2 步：切到 feature 分支做另一种修改

```bash
git switch feature
```

编辑 `config.txt`：

```text
theme=dark
```

提交：

```bash
git add config.txt
git commit -m "change: set theme dark"
```

### 第 3 步：把 main 合并到 feature

```bash
git merge main
```

这时 Git 可能会提示类似信息：

```text
Auto-merging config.txt
CONFLICT (content): Merge conflict in config.txt
Automatic merge failed; fix conflicts and then commit the result.
```

意思是：

- Git 发现 `config.txt` 两边都改了
- 但它不知道该保留哪一个
- 所以暂停，等你手动处理

### 第 4 步：打开冲突文件

这时 `config.txt` 很可能会变成这样：

```text
<<<<<<< HEAD
theme=dark
=======
theme=blue
>>>>>>> main
```

图解如下：

```text
<<<<<<< HEAD      ← 当前分支（你现在所在的分支）
theme=dark
=======
theme=blue
>>>>>>> main      ← 正在合并进来的分支
```

含义是：

- `HEAD` 这边是你当前分支 `feature` 的内容
- `main` 这边是你要合并进来的内容
- 你必须手动决定最终保留什么

### 第 5 步：手动修改成最终结果

比如你决定最终保留：

```text
theme=dark
```

那就把整个文件改成：

```text
theme=dark
```

如果你想折中，也可以改成：

```text
theme=blue-dark
```

关键点是：

- 删除 `<<<<<<<`、`=======`、`>>>>>>>`
- 文件里只保留你真正想要的最终内容

### 第 6 步：标记冲突已解决

```bash
git add config.txt
```

这一步的意思不是“再改一次”，而是告诉 Git：

> 这个文件的冲突我已经处理好了。

### 第 7 步：完成合并

```bash
git commit -m "merge: resolve conflict in config.txt"
```

到这里，冲突就解决完成了。

## 图解：完整冲突处理流程

```text
main 分支改了一行
        ↓
feature 分支也改了同一行
        ↓
git merge main
        ↓
Git 无法自动判断保留谁
        ↓
产生冲突标记
        ↓
你手动修改文件
        ↓
git add 冲突文件
        ↓
git commit
        ↓
冲突解决完成
```

## 如果是 rebase 里的冲突怎么办

流程几乎一样，只是最后一步不是 `git commit`，而是：

```bash
git add config.txt
git rebase --continue
```

如果不想继续这次变基：

```bash
git rebase --abort
```

## 新手最容易踩的坑

### 1. 只删了一半冲突标记

错误示例：

```text
<<<<<<< HEAD
theme=dark
=======
theme=blue
```

这样文件还是坏的，因为冲突标记没删干净。

### 2. 改完文件后忘了 `git add`

只改文件不 `git add`，Git 不知道你已经解决完冲突。

### 3. 没看清自己当前在哪个分支

冲突里的 `HEAD` 指的是**当前所在分支**，不是永远等于 `main`。

### 4. 一看到冲突就慌

大多数冲突本质上只是一个选择题：

- 保留我的版本
- 保留对方版本
- 两边合并成一个新版本

## 一个实用判断口诀

看到冲突时，按这个顺序想：

1. 我当前在哪个分支？
2. 哪一段是当前分支内容？
3. 哪一段是合并进来的内容？
4. 最终业务上应该保留什么？
5. 改完后有没有删干净冲突标记？

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

## 实战演示：从建分支到合并回 main 的完整流程

下面用一个常见例子串起来：

> 需求：新增一个登录按钮。

### 第 1 步：先切回 main 并拉最新代码

```bash
git switch main
git pull
```

常见输出：

```bash
Already on 'main'
Your branch is up to date with 'origin/main'.
Already up to date.
```

这一步的目的：

- 保证你是从最新主线开始开发
- 避免后面合并时冲突太多

### 第 2 步：创建功能分支

```bash
git switch -c feature/login-button
```

常见输出：

```bash
Switched to a new branch 'feature/login-button'
```

### 第 3 步：修改代码并查看状态

比如你修改了：

- `src/App.vue`
- `src/components/Header.vue`

然后执行：

```bash
git status
```

示例输出：

```bash
On branch feature/login-button
Changes not staged for commit:
  modified:   src/App.vue
  modified:   src/components/Header.vue
```

### 第 4 步：提交改动

```bash
git add .
git commit -m "feat: 添加登录按钮"
```

常见输出：

```bash
[feature/login-button a8b9c0d] feat: 添加登录按钮
 2 files changed, 18 insertions(+), 1 deletion(-)
```

### 第 5 步：推送到远程分支

```bash
git push -u origin feature/login-button
```

常见输出：

```bash
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Writing objects: 100% (5/5), done.
To github.com:user/repo.git
 * [new branch]      feature/login-button -> feature/login-button
branch 'feature/login-button' set up to track 'origin/feature/login-button'.
```

这说明：

- 远程已经有了这个功能分支
- 以后你再 push，直接 `git push` 就行

### 第 6 步：发起 PR / MR

这一步通常在 GitHub / Gitee 页面操作。

PR 标题可以写成：

```text
feat: 添加登录按钮
```

PR 描述可以简单写：

```text
## 变更内容
- 在顶部导航增加登录按钮
- 补充按钮点击逻辑

## 测试说明
- 页面可正常显示登录按钮
- 点击后可进入登录页
```

### 第 7 步：代码审查后合并

如果平台支持，你可以点击：

- Merge pull request
- Squash and merge
- Rebase and merge

新手最常用、也最容易理解的是普通合并。

### 第 8 步：回到本地同步 main

PR 合并后，回到本地：

```bash
git switch main
git pull
```

常见输出：

```bash
Switched to branch 'main'
Updating e4f5g6h..z9y8x7w
Fast-forward
 src/components/Header.vue | 10 ++++++++++
 1 file changed, 10 insertions(+)
```

这说明主分支已经拿到你刚刚合并进去的代码了。

### 第 9 步：可选，删除已完成的功能分支

本地删除：

```bash
git branch -d feature/login-button
```

远程删除：

```bash
git push origin --delete feature/login-button
```

如果你只是刚学 Git，也可以先不删，等熟悉后再整理。

## 图解：完整实战流程

```text
main
  ↓
git pull
  ↓
git switch -c feature/login-button
  ↓
修改代码
  ↓
git add + git commit
  ↓
git push -u origin feature/login-button
  ↓
发起 PR / MR
  ↓
审查通过并合并
  ↓
git switch main && git pull
```

## 这一套流程为什么推荐

因为它有几个明显好处：

- 主分支更稳定
- 每个功能的改动范围更清晰
- 更方便审查
- 出问题时更容易定位和回滚

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

生成时常见提示：

```bash
Generating public/private ed25519 key pair.
Your identification has been saved in /home/user/.ssh/github_key
Your public key has been saved in /home/user/.ssh/github_key.pub
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

常见成功输出类似：

```bash
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

如果都成功，说明双 key 配置完成。

---

# 十六、stash：临时保存当前修改

当你改了一半代码，突然要切分支处理别的问题，可以用：

```bash
git stash
```

示例输出：

```bash
Saved working directory and index state WIP on main: a1b2c3d feat: 添加登录页面
```

查看暂存列表：

```bash
git stash list
```

示例输出：

```bash
stash@{0}: WIP on main: a1b2c3d feat: 添加登录页面
```

恢复最近一次暂存：

```bash
git stash pop
```

常见输出：

```bash
On branch main
Changes not staged for commit:
  modified:   src/login.js
Dropped refs/stash@{0}
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

示例效果：

```bash
$ git reset --soft HEAD~1
$ git status
On branch main
Changes to be committed:
  modified:   src/login.js
```

这表示：

- 提交记录撤回了
- 但改动还在
- 而且还保留在暂存区

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

示例：

```bash
git revert a1b2c3d
```

常见输出：

```bash
[main c3d4e5f] Revert "feat: 添加登录页面"
 1 file changed, 12 deletions(-)
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

## 实战演示 1：刚提交错了，但还没 push

### 场景

你刚刚提交了一个错误版本：

```bash
git commit -m "feat: 添加登录页面"
```

后来发现：

- 有一个文件不该提交
- 或者提交信息写错了
- 或者代码还没改完

而且这次提交**还没 push**。

这时最适合用：

```bash
git reset --soft HEAD~1
```

### 操作过程

先看当前历史：

```bash
git log --oneline --graph --decorate
```

示例输出：

```bash
* a1b2c3d (HEAD -> main) feat: 添加登录页面
* e4f5g6h init project
```

执行回退：

```bash
git reset --soft HEAD~1
```

再看状态：

```bash
git status
```

示例输出：

```bash
On branch main
Changes to be committed:
  modified:   src/login.js
  modified:   src/router.js
```

这说明：

- 最新那次提交不见了
- 但代码改动还在
- 而且已经在暂存区里

如果你只是想重新提交一次，可以直接：

```bash
git commit -m "feat: 完善登录页面逻辑"
```

### 什么时候适合用它

适合：

- 刚提交完就发现问题
- 还没 push
- 想保留改动，只撤销提交记录

不适合：

- 已经 push 给别人了

## 实战演示 2：已经 push 了，想安全撤回

### 场景

你已经把某次提交推送到远程了：

```bash
git push
```

但后来发现这个提交有问题。

这时不要优先用 `reset` 改公共历史，更推荐：

```bash
git revert 提交ID
```

### 操作过程

先看历史：

```bash
git log --oneline --graph --decorate
```

示例输出：

```bash
* a1b2c3d (HEAD -> main, origin/main) feat: 删除旧接口
* e4f5g6h feat: 添加登录页面
* h7i8j9k init project
```

现在你想撤销 `a1b2c3d`：

```bash
git revert a1b2c3d
```

常见输出：

```bash
[main z9y8x7w] Revert "feat: 删除旧接口"
 1 file changed, 20 insertions(+), 18 deletions(-)
```

再看历史：

```bash
git log --oneline --graph --decorate
```

示例输出：

```bash
* z9y8x7w (HEAD -> main) Revert "feat: 删除旧接口"
* a1b2c3d (origin/main) feat: 删除旧接口
* e4f5g6h feat: 添加登录页面
```

这表示：

- 原来的错误提交还在历史里
- 但 Git 新增了一个“反向提交”把它抵消了
- 这种方式对团队协作更安全

最后正常推送：

```bash
git push
```

## 新手判断口诀

如果你拿不准该用哪个，先问自己两件事：

1. 这个提交已经 push 了吗？
2. 我是想“改历史”，还是想“补一个撤销提交”？

简单记：

- **没 push**：优先考虑 `reset`
- **已 push**：优先考虑 `revert`

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