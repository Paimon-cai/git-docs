
## 🎯 目标

实现：

- 一个项目
    
- 两个平台（GitHub + Gitee，不同账号）
    
- 一次 `git push` 同步两个仓库
    

---

# 🧹 一、清理环境（可选但推荐）

## 1. 清除 Git 全局身份

```bash
git config --global --unset user.name
git config --global --unset user.email
```

## 2. 删除当前仓库远程（如果有）

```bash
git remote remove origin
```

## 3. 清理 SSH（WSL）

```bash
rm ~/.ssh/config
rm ~/.ssh/id_*
rm ~/.ssh/known_hosts
```

---

# 🔑 二、创建 SSH 双账号

## 1. 生成两个 SSH key

```bash
ssh-keygen -t ed25519 -C "github" -f ~/.ssh/github_key
ssh-keygen -t ed25519 -C "gitee" -f ~/.ssh/gitee_key
```

---

## 2. 配置 SSH（关键）

编辑文件：

```bash
nano ~/.ssh/config
```

写入：

```bash
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

将内容分别添加到 GitHub 和 Gitee 的 SSH Keys

---

## 5. 测试连接

```bash
ssh -T git@github.com
ssh -T git@gitee.com
```

成功会显示欢迎信息

---

# 👤 三、设置项目身份

进入项目目录：

```bash
cd ~/projects/tunex
```

设置身份：

```bash
git config user.name "你的用户名"
git config user.email "你的邮箱"
```

验证：

```bash
git config user.name
git config user.email
```

---

# 🔗 四、绑定双远程仓库（核心步骤）

## 你的仓库地址示例：

- GitHub：
    
    ```
    git@github.com:用户名/仓库.git
    ```
    
- Gitee：
    
    ```
    git@gitee.com:用户名/仓库.git
    ```
    

---

## 配置命令：

```bash
# 添加 origin（主仓库）
git remote add origin git@github.com:用户名/仓库.git

# 添加第二个 push 地址（关键）
git remote set-url --add --push origin git@github.com:用户名/仓库.git
git remote set-url --add --push origin git@gitee.com:用户名/仓库.git
```

---

## 验证：

```bash
git remote -v
```

输出应包含：

```
origin  github地址 (fetch)
origin  github地址 (push)
origin  gitee地址 (push)
```

---

# 🚀 五、提交与推送

## 第一次提交：

```bash
git add .
git commit -m "init"
git push -u origin main
```

---

## 后续使用：

```bash
git add .
git commit -m "update"
git push
```

---

# ✅ 六、成功标志

推送时看到：

```
Everything up-to-date
```

或正常推送日志

👉 表示：

- GitHub 已更新 ✅
    
- Gitee 已同步 ✅
    

---

# ⚠️ 七、注意事项（非常重要）

## 1. 一个项目只设置一个身份

```bash
git config user.name
git config user.email
```

---

## 2. 必须使用不同 SSH key

```
github_key ≠ gitee_key
```

---

## 3. 不要混用 Windows 和 WSL

推荐：

👉 只在 WSL 操作项目

---

## 4. SSH config 必须配置

否则会出现权限错误

---

# 🧠 八、原理总结

- SSH config 决定用哪个 key
    
- remote 决定推送到哪里
    
- 一个 origin 可以有多个 push 地址
    
- git push 会同时推送所有 push URL
    

---

# 🎯 九、最终效果

```bash
git push
```

👉 自动：

- 推送到 GitHub
    
- 推送到 Gitee
    

---

# 🎉 完成

你现在已经拥有：

- 双账号 SSH 配置
    
- 双仓库同步能力
    
- 稳定的 Git 工作流
    

可长期使用 👍