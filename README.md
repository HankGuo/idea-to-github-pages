# idea-to-github-pages

> 一个 WorkBuddy Skill：用自然语言描述想法，AI 自动生成网页并部署到 GitHub Pages。

[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![WorkBuddy Skill](https://img.shields.io/badge/WorkBuddy-skill-blueviolet.svg)](https://www.workbuddy.cn)

---

## 是什么

把「想法 → 网页 → 上线」整个流程自动化：

1. 你用自然语言描述想要什么页面（产品介绍、个人主页、活动页……）
2. AI 拆解需求、设计方案、生成完整 HTML/CSS/JS
3. 自动创建 GitHub 仓库、推送代码、开启 GitHub Pages
4. 返回可访问的 URL

全程对话完成，不需要手写代码。

---

## 安装

在 WorkBuddy 对话中发送：

```
安装 skill idea-to-github-pages
```

或手动将本仓库克隆到你的 skills 目录：

```bash
cd ~/.workbuddy/skills
git clone https://github.com/HankGuo/idea-to-github-pages.git
```

---

## 使用方法

安装后，在 WorkBuddy 对话中直接描述你的需求：

```
帮我做一个产品介绍页，产品叫 XX，主打 YY 功能
```

```
做一个个人主页，简约风格，深色系
```

```
做一个活动 landing page，活动是 XX，7月举办
```

Skill 会自动：
- 拆解需求并输出需求文档让你确认
- 设计前端方案（配色/布局/动效）
- 生成完整静态页面
- 部署到 GitHub Pages 并返回链接

---

## 触发词

以下关键词会自动触发此 Skill：

`帮我做一个网页` · `上线一个页面` · `部署到 GitHub Pages` · `把这个做成网站` · `做产品介绍页` · `做个人主页` · `build and deploy`

---

## 文件结构

```
idea-to-github-pages/
├── SKILL.md                      # Skill 主文件（AI 执行指令）
├── references/
│   ├── design-rules.md          # 前端设计规则（design-taste-frontend 不可用时的 fallback）
│   └── github-pages-deploy.md  # GitHub Pages 部署步骤参考
└── README.md                    # 本文件
```

---

## 依赖

- [WorkBuddy](https://www.workbuddy.cn) — 运行环境
- GitHub 账号 + `gh` CLI（部署时需要）
- 可选：[design-taste-frontend](https://workbuddy.cn/skills/design-taste-frontend) Skill（获得更高质量的设计输出）

---

## License

[MIT](LICENSE) — 完全开源，免费使用、修改、分发。

---

## 作者

HANK · 五花样 · 公众号「算力白肉」

如有问题或建议，欢迎提 Issue。

---

## ☕ 赞助作者喝杯咖啡

这个 Skill 完全免费，代码随便用，改了也行，删了也行，拿去卖钱我也管不着。

但如果你用着顺手，省了时间，或者单纯心情好——可以考虑请我喝杯咖啡。

不是为了赚多少钱，主要是看看有没有人真的用这个东西，以及……咖啡比白开水好喝，就这么简单。

<p align="center">
  <img src="buy-me-a-coffee.jpg" width="280" alt="微信打赏" />
  <br>
  <sub>扫码打赏，金额随意，一块也是爱，一百也是爱，不打赏也是完全正常的选择</sub>
</p>
