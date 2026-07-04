---
name: idea-to-github-pages
description: |
  零基础→网页→链接。用户说「帮我做个网页」，Agent 像朋友聊天一样问几个问题，自动生成页面，自动在浏览器预览，自动变成别人能点开的链接。用户全程只需要打字回答+在浏览器点确认。不需要任何技术知识。
  触发词：帮我做一个网页、帮我做个页面、帮我做个网站、帮我搞个主页。
  agent_created: true
---

# Idea to GitHub Pages — 零基础到链接

## 核心原则

用户只会用浏览器和微信。他不知道什么是代码、终端、命令行。

**你的工作：** 所有技术操作在后台自动完成。用户只需要做三件事：打字回答你、在浏览器里点「授权」、点开你给的链接。

**禁止对用户说这些词：** GitHub、终端、命令行、仓库、部署、提交、推送、CLI、token、Pages、git、npm、brew。

**应该说：** 「变成链接」「在浏览器里确认一下」「给链接起个名」「搞定，发给大家就行」。

---

## 触发条件

优先匹配：`帮我做一个网页` `帮我做个页面` `帮我做个网站` `帮我搞个主页`。

也匹配包含「做」+「页面/网页/网站/主页」+「部署/发布/上线/链接」的组合描述。

---

## 前置声明

本 Skill 的设计规则融合了以下开源项目的精华：

- **design-taste-frontend** — 反 AI 模板化前端设计
- **taste-design** — Google Stitch 语义化设计系统
- **frontend-design** — 大胆美学方向指引
- **ui-ux-pro-max** — 风格/配色/字体设计数据库

感谢这些项目的作者对开源设计的贡献。

---

## 阶段 1：聊天式需求引导

一次一个问题。用大白话。

### Q1 — 做什么

「你想做一个什么样的页面？比如介绍你的店、介绍你自己、展示你的作品——或者你直接说想法就行。」

### Q2 — 给谁看

「这个页面主要给谁看？客人？朋友？还是随便看看的陌生人？」

### Q3 — 他们看完做什么

「你希望看完的人做什么？加你微信？打电话？记住你的名字？」

### Q4 — 喜欢什么感觉

「你希望页面给人的感觉是什么样的？

- **暖色的**，像咖啡馆花店那种温暖的
- **清爽的**，白底干干净净像杂志
- **深色的**，黑底白字酷酷的
- **活泼的**，颜色鲜艳有活力

或者直接说你喜欢的感觉。」

### Q5 — 手机还是电脑

「主要是在手机上给别人看，还是电脑上？」

### Q6 — 有现成的图吗

「你有现成的照片、logo、或者写好的文字吗？有的话发给我，没有我帮你编。」

### 确认

「好的，我确认一下：你想做一个 [XX] 的页面，给 [XX] 看，风格想要 [XX]，主要是 [手机/电脑] 上看，[有/没有现成的图和字]。对吗？」

---

## 阶段 2：结构确认

用大白话给用户看页面结构：

```
你的页面大概长这样（从上到下）：

最上面 — 店名 / 名字 + 一句话介绍
中间 — 你的服务或特点（3-4 条）
下面 — 详细介绍
最下面 — 联系方式

可以吗？想加什么或删什么直接说。
```

---

## 阶段 3：设计确认

后台用 `design-taste-frontend` Skill 或 `references/design-rules.md` 做设计决策，然后翻译成大白话给用户确认：

「我帮你选了一种感觉：

- 底色：奶油白
- 重点地方：暖橘色
- 字体：清晰好读

你看看可以吗？想换就告诉我。」

---

## 阶段 4：生成页面

生成 `index.html` + `styles.css` + `app.js`（如需）。

内联 SVG 做装饰和图标。CSS 变量统一配色。响应式适配。字体用系统字体栈或 Google Fonts CDN。动效仅 CSS。

---

## 阶段 5：自动预览

生成后，**不要让用户手动打开文件**。自动在浏览器打开页面：

**方式一：HTTP 服务器**（macOS / Linux 通常自带 python3）

```bash
cd <project-dir>
python3 -m http.server 8080 &
sleep 1
# macOS
open http://localhost:8080 2>/dev/null
# Linux
xdg-open http://localhost:8080 2>/dev/null
# Windows (git bash / WSL)
start http://localhost:8080 2>/dev/null
```

**方式二：直接打开 HTML 文件**（没有 python3 时的备选）

```bash
# macOS
open <project-dir>/index.html
# Linux
xdg-open <project-dir>/index.html
# Windows
start <project-dir>/index.html
```

**方式三：用 Node.js**（如果 Node.js 可用）

```bash
npx serve <project-dir> -p 8080 &
sleep 1
open http://localhost:8080
```

按优先级尝试，哪个能跑用哪个。

告诉用户：「页面写好了，你的浏览器应该已经打开了。看看颜色、排版、内容——哪里要改直接告诉我。」

如果用户说没看到，换端口重试或换方式。反复改到用户满意。

---

## 阶段 6：变成链接

**以下全部在后台自动完成。不对用户提任何技术词汇。**

### 6.1 安装必需工具（自动）

**检查 git：**
```bash
which git
```
未安装时：
- macOS：`xcode-select --install 2>/dev/null` 或 git 随 Xcode CLI 自动安装
- Windows：`winget install Git.Git`
- Linux：`sudo apt install git -y` 或 `sudo dnf install git -y`

**检查 gh CLI：**

```bash
which gh
```

如果已安装 → 跳过。

如果未安装 → **根据操作系统自动选择安装方式**：

**macOS:**
```bash
# 尝试 1: brew (如果装了的话很快)
if brew install gh 2>/dev/null; then
  echo "gh installed via brew"
# 尝试 2: 下载官方 pkg 安装包
else
  curl -fsSL https://cli.github.com/packages/mac/gh_latest_macOS_amd64.pkg -o /tmp/gh.pkg
  sudo installer -pkg /tmp/gh.pkg -target /
fi
```
如果 sudo 需要密码 → 告诉用户：「我需要装一个小工具，系统会弹出一个密码框，输入你电脑的开机密码就行。我帮你打开下载页面。」然后用 `open https://github.com/cli/cli/releases/latest` 让用户自己下载安装包双击安装。

**Windows:**
```powershell
winget install --id GitHub.cli
```
如果 winget 不可用 → 「帮我打开这个下载页面：https://github.com/cli/cli/releases/latest 你下载 Windows 版，双击安装就行。装好告诉我。」

**Linux (Debian/Ubuntu):**
```bash
(type -p wget >/dev/null || sudo apt-get install wget -y) \
&& sudo mkdir -p -m 755 /etc/apt/keyrings \
&& wget -qO- https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
&& sudo apt update \
&& sudo apt install gh -y
```

**Linux (Fedora/RHEL):**
```bash
sudo dnf install gh -y
```

如果自动安装失败 → **引导用户到官网下载页**：用 `open` / `start` / `xdg-open` 打开 `https://github.com/cli/cli/releases/latest`，告诉用户：「我需要装一个小工具，帮你打开了下载页面——选你电脑对应的版本下载安装就行。装好之后告诉我。」

### 6.2 检查登录

```bash
gh auth status 2>&1
```

已登录 → 跳过。

未登录 → **用浏览器 OAuth，不需要用户碰终端：**

```
我需要你在浏览器里确认一下身份，我帮你打开网页。
```

然后执行：
```bash
gh auth login --web --hostname github.com
```
这会自动打开浏览器。用户只需在浏览器里点「Authorize」。之后检查 `gh auth status` 确认登录成功。

**如果浏览器 OAuth 失败**（比如在无桌面环境）：降级为 token 模式。引导用户：

1. 用 `open https://github.com/settings/tokens/new?scopes=repo&description=Page+Deploy` 打开 token 创建页面
2. 告诉用户：「帮你在浏览器里打开了页面。点最下面的『Generate token』，把生成的那串字符复制发给我就行。」
3. 拿到 token 后回传：`echo "<token>" | gh auth login --with-token`

### 6.3 获取 GitHub 用户名

```bash
gh api user --jq '.login'
```

### 6.4 给链接起名

「给你的链接起个简单的英文名？比如你的店叫『阳光花坊』，英文就叫 `sunny-flowers`。这个会出现在最终链接里。」

### 6.5 后台执行（全自动）

```bash
git init
git add .
git commit -m "Initial page"

gh repo create <repo-name> --public --source=. --remote=origin --push

gh api repos/<username>/<repo-name>/pages -X POST \
  -f "source[branch]=main" -f "source[path]=/"
```

如果 `gh repo create --push` 失败（如已有 remote）：
```bash
git remote add origin https://github.com/<username>/<repo-name>.git 2>/dev/null
git remote set-url origin https://github.com/<username>/<repo-name>.git
git branch -M main
git push -u origin main
```

### 6.6 等待部署

```bash
# 轮询检查，最多等 3 分钟
for i in $(seq 1 18); do
  status=$(gh api repos/<username>/<repo-name>/pages --jq '.status' 2>/dev/null)
  if [ "$status" = "built" ]; then break; fi
  sleep 10
done
```

### 6.7 告诉用户

「好了！你的页面可以看了，把这个链接发给大家就行：

**https://[用户名].github.io/[仓库名]/**

刚发可能要等一两分钟才能打开。以后想改内容，直接跟我说就行。」

---

## 阶段 7：后续修改

「以后想改内容的话，直接跟我说『帮我把那页的 XX 改成 XX』，我帮你更新，链接不变。」

---

## 故障处理

### 自动安装 gh 失败

不要放弃。降级为引导用户手动安装：

「我需要装一个小工具，帮你打开了下载页面。选你电脑的版本，下载安装就行——跟装普通软件一样。装好之后告诉我。」

用 `open https://cli.github.com` 打开官网。

### 浏览器 OAuth 登录失败

降级为 token 模式（见 6.2）。

### 链接打不开

后台自动重试部署状态检查。不要跟用户说「你检查一下 GitHub Pages 设置」——自己去检查。如果确实有问题，后台修复。

### 网络超时

国内用户可能连不上 GitHub。检查 `curl --connect-timeout 5 https://github.com`。如果超时，检查是否有代理。不要跟用户提「代理」「翻墙」等词——直接说「网络不太稳定，等一会儿我再试。」

---

## 文件结构

```
idea-to-github-pages/
├── SKILL.md
└── references/
    ├── design-rules.md
    └── github-pages-deploy.md
```
