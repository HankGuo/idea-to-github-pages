---
name: idea-to-github-pages
description: |
  端到端「想法→网页→上线」流水线。用户用自然语言描述一个想法（产品介绍页、个人主页、活动页面、作品展示等），Skill 自动拆解需求、调用 design-taste-frontend 设计前端、生成 SVG 图形、产出完整 HTML+CSS+JS，然后通过 GitHub CLI 部署到 GitHub Pages 并返回最终网址。
  触发词：帮我做一个网页、上线一个页面、部署到 GitHub Pages、把这个做成网站、做产品介绍页、做个人主页、build and deploy。
  agent_created: true
---

# Idea to GitHub Pages — 从想法到上线

## 概述

用户说「帮我做一个 XX 页面」，从需求拆解到 GitHub Pages 上线全自动流水线。四个工具全部用上：

- **Markdown** — 输出需求文档，用户确认方向
- **design-taste-frontend** — 设计前端（配色/排版/布局/动效）
- **SVG** — 生成图表、插图、装饰元素
- **HTML + CSS + JS** — 产出完整可部署的静态页面
- **GitHub CLI** — 自动创建仓库、推送代码、开启 Pages

---

## 阶段 0：判断是否适用此 Skill

**适用场景**：单页或少量页面的静态展示页。如产品介绍页、个人主页、活动 landing page、作品展示页、简历页、团队介绍页。

**不适用场景**：需要后端/数据库的页面、需要登录认证的页面、多页复杂应用、React/Next.js 项目（这些不适合直接 GitHub Pages 部署）。

---

## 阶段 1：需求拆解

### 1.1 理解用户意图

从用户描述中提取关键信息：
- **页面类型**：产品介绍 / 个人主页 / 活动页 / 作品集 / 其他
- **目标受众**：谁在看这个页面？B2B 客户 / 消费者 / 招聘者 / 社群成员？
- **核心信息**：用户想让受众知道什么？做什么？
- **风格偏好**：用户有没有提到「简约」「科技感」「温暖」「专业」等风格词？
- **内容素材**：用户是否提供了 logo、图片、文案？

### 1.2 追问（最多 3 个问题，逐个问）

如果信息不足，逐个追问。不要一次扔一堆问题。

优先追问顺序：
1. **页面用途不清时**：「这个页面主要给谁看？想让他们看完之后做什么？」
2. **风格不清时**：「偏好简约干净的，还是更有视觉冲击力的？」
3. **内容缺失时**：「有现成的 logo 或品牌色吗？有的话发给我」

### 1.3 输出需求文档

确认理解后，用 **Markdown 格式**输出需求文档，包含：

```markdown
# <页面名称> — 需求文档

## 基本信息
- 页面类型：
- 目标受众：
- 核心目标：

## 页面结构
1. 封面/首屏 — <标题> + <副标题>
2. <section 2>
3. <section 3>
...

## 设计方向
- 风格：
- 主色调：
- 参考：

## 内容清单
- 文案：
- 图片：
- logo：
- 其他素材：
```

输出后等待用户确认或修改，确认后进入设计阶段。

---

## 阶段 2：前端设计

### 2.1 尝试加载 design-taste-frontend

先尝试调用 `design-taste-frontend` Skill。

**如果加载成功**：将其设计决策映射到本项目的 Design Read / Dials / 配色 / 字体 / 布局方案。**只取设计决策层**，不取 React 组件代码。

**如果加载失败**（用户未安装该 Skill）：直接使用内置设计规则 `references/design-rules.md`。读取该文件，按同样的流程产出 Design Read → Dials → 配色 → 字体 → 布局方案。

两种路径产出的设计文档格式完全相同。

### 2.2 设计产出物格式

```markdown
## 设计决策

**Design Read**：<一句话>（来源：design-taste-frontend 或 references/design-rules.md）

**三档位**：
- VARIANCE: <1-10>
- MOTION_INTENSITY: <1-10>
- VISUAL_DENSITY: <1-10>

**配色**：
- 主色: #XXXXXX
- 辅色: #XXXXXX
- 背景: #XXXXXX
- 文字: #XXXXXX
- 强调文字: #XXXXXX

**字体**：
- 标题: <font-family>
- 正文: <font-family>

**布局方案**：
- Hero: <方案名>
- 各 Section 布局描述
```

---

## 阶段 3：SVG 生成

根据页面需要生成 SVG 图形。常见场景：
- **插图/装饰**：Hero 区域的抽象图形、背景纹理
- **图标**：优先使用图标库（Phosphor / Tabler 的内联 SVG），需要自定义图形时才手写 SVG
- **数据图表**：柱状图、折线图、流程图
- **logo 占位**：如果用户没有 logo，生成一个简洁的文字标

SVG 要求：
- 直接内联在 HTML 中，不单独引用外部文件
- `viewBox` 正确设置
- 颜色使用 CSS 变量或硬编码在 SVG 内部（后期可改）

---

## 阶段 4：HTML 生成

### 4.1 输出结构

生成以下文件：
- `index.html` — 主页面，包含所有内容、内联 SVG
- `styles.css` — 样式文件
- `app.js` — 交互逻辑（如有翻页、动画等）

如果页面是单页无复杂交互，可以将 CSS 内联在 `<style>` 标签中。

### 4.2 HTML 规范

- 使用语义化标签（`<header>`, `<section>`, `<footer>`）
- 所有样式集中管理，使用 CSS 变量定义配色和排版
- 响应式设计：至少覆盖手机（<768px）和桌面（>1024px）
- 图片使用占位符或用户提供的 URL
- **不收**纯文本占位——如果用户没给图，用 SVG 图形代替，不写 `<div>` 模拟图片
- 字体使用系统字体栈或 Google Fonts，不使用需要 npm 安装的字体包

### 4.3 CSS 变量规范

```css
:root {
  --primary: #XXXXXX;
  --secondary: #XXXXXX;
  --bg: #XXXXXX;
  --text: #XXXXXX;
  --text-muted: #XXXXXX;
  --sans: "...", system-ui, sans-serif;
  --mono: "...", monospace;
}
```

### 4.4 动效规范

- 仅使用 CSS transition/animation 或 vanilla JS
- **不使用** React/Motion/GSAP 等框架（目标输出是静态 HTML）
- 动效必须符合 `prefers-reduced-motion` 媒体查询
- 动效强度对应 MOTION_INTENSITY dial

---

## 阶段 5：GitHub Pages 部署

### 5.1 收集 GitHub 信息

按顺序确认：

1. **GitHub 用户名**：先尝试 `gh auth status` 获取已登录用户。如果未登录 → 引导用户执行 `gh auth login`
2. **仓库名**：建议 `<project-slug>` 或 `<project-slug>-pages`。如果用户已有仓库 → 使用现有仓库。如果没有 → 创建新仓库。确认仓库**公开**（Pages 免费方案要求）
3. **确认推送**：告知用户将要推送的文件列表，确认后执行

### 5.2 执行部署

详细步骤见 `references/github-pages-deploy.md`。

核心命令序列：
```bash
# 1. 确保 gh 已登录
gh auth status

# 2. 创建仓库并推送（如果没有仓库）
gh repo create <repo-name> --public --source=. --remote=origin --push

# 3. 如果已有仓库，直接推送
git add . && git commit -m "Deploy from idea-to-github-pages" && git push

# 4. 开启 GitHub Pages
gh api repos/<username>/<repo-name>/pages -X POST \
  -f "source[branch]=main" -f "source[path]=/"

# 5. 检查部署状态
gh api repos/<username>/<repo-name>/pages
```

### 5.3 返回结果

部署成功后返回：
- **GitHub Pages URL**：`https://<username>.github.io/<repo-name>/`
- **仓库地址**：`https://github.com/<username>/<repo-name>`
- **备用说明**：如果 `gh api` 开启 Pages 失败，给出手动开启的步骤链接

---

## 阶段 6：收尾

在项目目录下新建 `.workbuddy/memory/` 记录本次部署信息（仓库名、URL、创建时间），方便后续维护。

---

## 参考资源

- `references/design-rules.md` — 前端设计规则（design-taste-frontend 精简版，作为 Skill 不可用时的 fallback）
- `references/github-pages-deploy.md` — GitHub Pages 部署详细步骤、故障排查
