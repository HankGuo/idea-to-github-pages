# GitHub Pages 部署参考

## 前置条件

1. GitHub 账号已注册
2. 本地已安装 `gh` CLI 并已登录 (`gh auth login`)
3. 项目文件已就绪（HTML + CSS + JS + 资源文件）

## 部署流程

### 步骤 1：检查 GitHub 认证状态

```bash
gh auth status
```

如果未登录，引导用户执行 `gh auth login`。

### 步骤 2：收集必要信息

需要确认以下信息：
- **GitHub 用户名**：从 `gh auth status` 或 `git config github.user` 获取
- **仓库名**：如果没有现有仓库，创建新仓库。命名建议：`<project-name>` 或 `<project-name>-pages`
- **仓库可见性**：公开（public）——GitHub Pages 免费方案要求公开仓库

### 步骤 3：初始化 Git 仓库并推送

如果目录还不是 Git 仓库：
```bash
cd <project-dir>
git init
git add .
git commit -m "Initial commit"
```

添加 remote 并推送：
```bash
git remote add origin https://github.com/<username>/<repo-name>.git
git branch -M main
git push -u origin main
```

如果 remote 已存在但指向其他仓库：
```bash
git remote set-url origin https://github.com/<username>/<repo-name>.git
git push -u origin main
```

### 步骤 4：创建 GitHub 仓库（如需要）

```bash
gh repo create <repo-name> --public --source=. --remote=origin --push
```

参数说明：
- `--public`：公开仓库（Pages 免费方案要求）
- `--source=.`：使用当前目录作为源
- `--remote=origin`：设置 origin remote
- `--push`：自动推送

### 步骤 5：开启 GitHub Pages

方式 A — 使用 gh CLI：
```bash
gh api repos/<username>/<repo-name>/pages -X POST -f "source[branch]=main" -f "source[path]=/"
```

方式 B — 用户手动操作（给出清晰指引）：
1. 打开 `https://github.com/<username>/<repo-name>/settings/pages`
2. Source 选择 "Deploy from a branch"
3. Branch 选择 `main`, 目录选择 `/ (root)`
4. 点击 Save

### 步骤 6：等待部署

GitHub Pages 部署通常需要 1-2 分钟。检查状态：
```bash
gh api repos/<username>/<repo-name>/pages
```

### 步骤 7：获取最终 URL

部署完成后，页面地址为：`https://<username>.github.io/<repo-name>/`

## 常见问题

### push 超时或失败

- 检查代理是否运行（国内环境常见问题）
- 如使用代理：`git config --local http.proxy http://127.0.0.1:7890`
- 如不使用代理：`git config --local --unset http.proxy`

### 页面 404

- 确认仓库 settings > Pages 中 Branch 选择了正确的分支
- 确认 index.html 在仓库根目录（不是子目录）
- 等待 1-2 分钟后刷新

### CNAME 自定义域名

如果用户不使用自定义域名，不需要 CNAME 文件。如果已存在 CNAME 文件，删除它即可使用默认 `github.io` 域名。
