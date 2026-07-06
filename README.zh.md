[English](README.md) | [한국어](README.ko.md) | 中文 | [日本語](README.ja.md) | [Español](README.es.md)

# insane-design

<div align="center">
  <img src="assets/hero.png" width="860" alt="电影感主视觉：网站表面碎裂，色彩样片（#8B5CF6、#22FF88、#FFB35C）、Aa 字体样张、圆角样块和间距标尺被吸入一个终端，终端上写着 'insane-design — RIPS THE DESIGN OUT'，旁边是 color/type/design.md 清单">
</div>

> **从任何网站把设计系统整个撕下来。一条命令搞定。**

"做出 Stripe 的感觉"这种话对 AI 毫无用处——它需要的是设计令牌，而不是氛围。insane-design 从不靠猜：它抓取真实 CSS，解析每一个自定义属性，提取排版、间距、阴影、圆角和渐变，然后把一份真正能拿来构建的 `design.md` 交给你的 AI 智能体，外加一份支持点击复制色板的交互式 HTML 报告。

内置 100 个预分析站点（Stripe、Apple、Linear、Toss、Notion……）——也可以指向任意 URL 直接开跑。

**[查看画廊 — 100 份设计系统报告 →](https://fivetaku.github.io/insane-design/)**

[快速开始](#快速开始) • [为什么选 insane-design？](#为什么选-insane-design) • [工作原理](#工作原理) • [功能特性](#功能特性) • [环境要求](#环境要求)

---

## 快速开始

### 1. 添加插件市场（仅需一次）

```
/plugin marketplace add https://github.com/fivetaku/gptaku_plugins.git
```

### 2. 安装

```
/plugin install insane-design
```

### 3. 重启 Claude Code

必须重启，插件才会加载。

### 4. 运行

```
/insane-design https://stripe.com
```

也可以用自然语言："分析这个网站的设计系统"——命令会自动理解你的意图。

---

## 为什么选 insane-design？

- **不是 LLM 瞎猜** —— CSS 被直接抓取并解析。十六进制色值来自真实样式表，而非幻觉。
- **面向 AI 的输出** —— `design.md` 专为 AI 智能体结构化。把它附到任何提示词里，得到的是可复现的结果，而不是氛围感。
- **内置 100 个预分析站点** —— Stripe、Apple、Linear、Vercel、Toss、Nike、Figma、Notion，以及另外 92 个。无需运行分析即可直接应用。
- **两种模式，一条命令** —— 分析一个新 URL，或把已有设计系统应用到你的项目。同一条命令两者通吃。
- **即贴即用的 CSS 与 Tailwind 配置** —— 输出包含可直接粘贴的 CSS 变量和 Tailwind 主题扩展。无需手动转换。
- **交互式 HTML 报告** —— 点击即复制的色板、实时排版预览、间距条、阴影演示卡片——所有视觉信息集中在一个文件里。
- **Quick Start 速查表** —— "应用这三样，就有八成像了。" 按影响最大、投入最小的原则排好了优先级。

---

## 工作原理

```
URL
  ↓
Fetch HTML + CSS files (real stylesheet bundles, Chrome UA)
  ↓
Parse CSS custom properties
Extract: color ramps · typography scale · spacing tokens
         radius system · shadow layers · font stack
  ↓
Visual verification via screenshot (Playwright or curl fallback)
Confirm: brand color · light/dark theme · font rendering
  ↓
Generate design.md          Generate report.html
(structured for AI agents)  (interactive browser report)
```

输出会生成到 `{site-slug}/`：

```
stripe/
├── design.md              # AI agent reference — 16-section structured doc
├── report.ko.html         # interactive HTML report
└── screenshots/
    └── hero-cropped.png   # 1280×800 screenshot
```

---

## 功能特性

### 命令

| 命令 | 作用 |
|---------|-------------|
| `/insane-design [URL]` | 分析站点——提取设计系统，生成 `design.md` + HTML 报告 |
| `/insane-design [slug]` | 把预分析的设计应用到你的项目 |
| `/insane-design:analysis [URL]` | 仅分析模式 |
| `/insane-design:apply [slug]` | 仅应用模式 |
| `/insane-design:build [design.md]` | 基于 `design.md` 搭建新页面/组件（也可从 URL 即时合成） |
| `/insane-design:verify [job_id]` | 轮询异步校验器——收集 §18 DON'T 违规项 + 截图差异 |

自然语言同样有效。"分析这个网站"会触发分析模式，"应用 Stripe 风格"会触发应用模式。

### 提取的设计令牌

| 令牌类型 | 捕获内容 |
|------------|----------------|
| 颜色 | 品牌色阶、中性色阶、强调色系、语义别名 |
| 排版 | 标题/正文字号体系——字号、字重、行高、字距 |
| 间距 | 完整的间距令牌系统 |
| 圆角 | Border-radius 比例体系 |
| 阴影 | Box-shadow 层（含多层叠加） |
| 字体栈 | 自定义字体识别 + 回退链 |

### design.md 结构（16 个章节）

| 章节 | 内容 |
|---------|---------|
| 01 Quick Start | 3 步 CSS 片段——"5 分钟做到八成像" |
| 02 Provenance | 来源 URL、抓取日期、CSS 字节数 |
| 03 Tech Stack | 框架与设计系统命名空间检测 |
| 04–05 Typography | 字体栈 + 完整字号体系 |
| 06 Colors | 品牌色阶 · 中性色 · 强调色 · 语义层 |
| 07–09 Spacing / Radius / Shadows | 完整令牌表 |
| 12 Components | BEM 类名模式与组件变体 |
| 14 Drop-in CSS | 可直接粘贴的 `:root {}` CSS 变量 |
| 15 Tailwind Config | `theme.extend` 配置 |
| 16 DO / DON'T | 针对该站点最需避免的常见错误 |

### 应用模式——把令牌注入你的项目

应用模式会扫描你的项目（globals.css、Tailwind 配置或纯 CSS），在不覆盖现有设置的前提下注入设计令牌。

| 目标 | 行为 |
|--------|-------------|
| 含 `:root {}` 的 CSS | 令牌追加到现有块内 |
| 不含 `:root {}` 的 CSS | 在文件顶部新建 `:root {}` |
| Tailwind 配置 | 更新 `theme.extend.colors`、`fontFamily`、`borderRadius` |
| 重复应用 | 干净地替换 `/* insane-design */` 块 |

### 100 个预分析站点

Stripe、Apple、Linear、Vercel、Notion、Figma、GitHub、Discord、Slack、Spotify、Toss、Baemin、Daangn、Kakao、Naver、Nike、Gucci、Chanel、Tesla、Ferrari、BMW，以及另外 79 个。任何一个都可以直接应用，无需运行分析。

---

## 环境要求

### 必需

- [Claude Code](https://docs.anthropic.com/claude-code)
- Python 3.11+
- Pillow（`pip install Pillow`）

### 可选

- **Playwright MCP** —— 提升截图捕获质量。没有它，insane-design 会回退到基于 curl 的截图方案，依然可用。

---

## 许可证

MIT

---

<div align="center">

**要真实 CSS，不要凭感觉。**

</div>
