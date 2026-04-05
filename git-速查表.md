# Git 速查表

> 适合已经知道 Git 基本概念时，快速查命令、查流程、查排错方法。

## 快速入口

- [常用日常命令](#常用日常命令)
- [分支操作](#分支操作)
- [合并与冲突](#合并与冲突)
- [远程仓库](#远程仓库)
- [双平台同步推送](#双平台同步推送)
- [stash / 回滚 / 恢复](#stash--回滚--恢复)
- [常见排错](#常见排错)

---

## 常用日常命令

### 查看状态

```bash
git status
```

### 添加到暂存区

```bash
git add 文件名
git add .
```

### 提交

```bash
git commit -m "feat: 提交说明"
```

### 查看历史

```bash
git log
git log --oneline --graph --decorate
```

### 查看改动

```bash
git diff
git diff --cached
```

### 最常用工作流

```bash
git status
git add .
git commit -m "fix: 修复问题"
git push
```

---

## 分支操作

### 查看分支

```bash
git branch
```

### 创建分支

```bash
git branch dev
```

### 切换分支

```bash
git switch dev
```

### 创建并切换分支

```bash
git switch -c feature/login
```

### 删除本地分支

```bash
git branch -d feature/login
```

### 删除远程分支

```bash
git push origin --delete feature/login
```

---

## 合并与冲突

### 合并分支

```bash
git switch main
git merge feature/login
```

### 变基

```bash
git switch feature/login
git rebase main
```

### 冲突解决流程

```bash
# 1. 打开冲突文件，手动修改
# 2. 删除冲突标记
# 3. 标记已解决

git add 冲突文件

# merge 场景
git commit

# rebase 场景
git rebase --continue
```

### 放弃 rebase

```bash
git rebase --abort
```

### 冲突标记长这样

```text
<<<<<<< HEAD
当前分支内容
=======
要合并进来的内容
>>>>>>> main
```

---

## 远程仓库

### 查看远程仓库

```bash
git remote -v
```

### 添加远程仓库

```bash
git remote add origin 仓库地址
```

### 拉取远程更新

```bash
git fetch
git pull
```

### 首次推送

```bash
git push -u origin main
```

### 后续推送

```bash
git push
```

---

## SSH 配置

### 生成 SSH key

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

### 查看公钥

```bash
cat ~/.ssh/id_ed25519.pub
```

### 测试连接

```bash
ssh -T git@github.com
ssh -T git@gitee.com
```

---

## 双平台同步推送

### 给同一个 origin 配多个 push 地址

```bash
git remote add origin git@github.com:用户名/仓库.git
git remote set-url --add --push origin git@github.com:用户名/仓库.git
git remote set-url --add --push origin git@gitee.com:用户名/仓库.git
```

### 查看配置结果

```bash
git remote -v
```

预期类似：

```text
origin  git@github.com:用户名/仓库.git (fetch)
origin  git@github.com:用户名/仓库.git (push)
origin  git@gitee.com:用户名/仓库.git (push)
```

### 后续直接推送

```bash
git push
```

### 双 key 配置

```bash
ssh-keygen -t ed25519 -C "github" -f ~/.ssh/github_key
ssh-keygen -t ed25519 -C "gitee" -f ~/.ssh/gitee_key
```

`~/.ssh/config` 示例：

```sshconfig
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_key
    IdentitiesOnly yes

Host gitee.com
    HostName gitee.com
    User git
    IdentityFile ~/.ssh/gitee_key
    IdentitiesOnly yes
```

---

## stash / 回滚 / 恢复

### 临时保存现场

```bash
git stash
git stash list
git stash pop
git stash apply
```

### 撤销工作区修改

```bash
git restore 文件名
```

### 撤销暂存区

```bash
git restore --staged 文件名
```

### 回退最近一次提交，但保留改动

```bash
git reset --soft HEAD~1
```

### 回退最近一次提交，并取消暂存

```bash
git reset --mixed HEAD~1
```

### 丢弃提交和本地修改

```bash
git reset --hard HEAD~1
```

### 安全撤销已 push 的提交

```bash
git revert 提交ID
```

### 选择口诀

- 没 push：优先考虑 `reset`
- 已 push：优先考虑 `revert`

---

## 标签与进阶命令

### 标签

```bash
git tag v1.0.0
git tag
git push origin v1.0.0
git push origin --tags
```

### 查看某次提交

```bash
git show 提交ID
```

### 查看某一行是谁改的

```bash
git blame 文件名
```

### 挑选某个提交

```bash
git cherry-pick 提交ID
```

---

## 常见排错

### push 权限错误

先检查：

```bash
ssh -T git@github.com
ssh -T git@gitee.com
git remote -v
```

### 分支不存在

```bash
git branch
```

### pull / merge 冲突

- 打开冲突文件
- 删除冲突标记
- 保留最终正确内容
- `git add 冲突文件`
- merge 用 `git commit`
- rebase 用 `git rebase --continue`

### 不小心提交错了

没 push：

```bash
git reset --soft HEAD~1
```

已 push：

```bash
git revert 提交ID
```

---

## 推荐提交信息

```text
feat: 新功能
fix: 修复问题
docs: 文档更新
refactor: 重构
chore: 杂项维护
```

---

## 一句话速记

```text
查看状态：git status
加入暂存：git add
生成提交：git commit
查看历史：git log
推送远程：git push
拉取更新：git pull
```

---

## 对应完整版

如果你需要图解、案例和详细解释，请看完整版：

- [Git 教程完整版](readme.md)
