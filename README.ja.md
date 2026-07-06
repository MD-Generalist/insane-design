[English](README.md) | [한국어](README.ko.md) | [中文](README.zh.md) | 日本語 | [Español](README.es.md)

# insane-design

<div align="center">
  <img src="assets/hero.png" width="860" alt="シネマティックなキーアート：Web サイトの表面が砕け、カラースウォッチ（#8B5CF6、#22FF88、#FFB35C）、Aa のタイプ見本、角丸チップ、スペーシング定規が「insane-design — RIPS THE DESIGN OUT」と表示されたターミナルへ吸い込まれていく。color / type / design.md のチェックリスト付き。">
</div>

> **どんな Web サイトからも、デザインシステムを引き剥がす。コマンド一発で。**

「Stripe っぽくして」は AI には通じない——必要なのは雰囲気ではなくトークンだから。insane-design は推測しません。実際の CSS を取得し、すべてのカスタムプロパティを解析し、タイポグラフィ・スペーシング・シャドウ・角丸・グラデーションを抽出して、AI エージェントが本当にビルドに使える `design.md` を渡します。さらに、クリックでコピーできるスウォッチ付きのインタラクティブ HTML レポートも。

100 サイトの分析済みデータを同梱（Stripe、Apple、Linear、Toss、Notion...）——もちろん任意の URL を指定してすぐ実行することもできます。

**[ギャラリーを見る — 100 のデザインシステムレポート →](https://fivetaku.github.io/insane-design/)**

[クイックスタート](#クイックスタート) • [なぜ insane-design？](#なぜ-insane-design) • [仕組み](#仕組み) • [機能](#機能) • [動作要件](#動作要件)

---

## クイックスタート

### 1. マーケットプレイスを追加（初回のみ）

```
/plugin marketplace add https://github.com/fivetaku/gptaku_plugins.git
```

### 2. インストール

```
/plugin install insane-design
```

### 3. Claude Code を再起動

プラグインの読み込みに必要です。

### 4. 実行

```
/insane-design https://stripe.com
```

自然言語でも動きます。「このサイトのデザインシステムを分析して」——コマンドが意図を汲み取ります。

---

## なぜ insane-design？

- **LLM の推測ではない** —— CSS を直接取得して解析。Hex 値はハルシネーションではなく、実際のスタイルシート由来。
- **AI がそのまま使える出力** —— `design.md` は AI エージェント向けに構造化済み。どんなプロンプトにも添付でき、雰囲気ではなく再現可能な結果が返ってくる。
- **100 サイトの分析済みデータを同梱** —— Stripe、Apple、Linear、Vercel、Toss、Nike、Figma、Notion ほか 92 サイト。分析を走らせずにそのまま適用できる。
- **2 つのモード、1 つのコマンド** —— 新しい URL の分析も、既存デザインシステムのプロジェクトへの適用も、同じコマンドが両方こなす。
- **貼るだけの CSS と Tailwind 設定** —— 出力には、そのまま貼り付けられる CSS 変数と Tailwind テーマ拡張を含む。手作業での変換は不要。
- **インタラクティブ HTML レポート** —— クリックでコピーできるカラースウォッチ、ライブタイポグラフィプレビュー、スペーシングバー、シャドウのデモカード——視覚要素のすべてを 1 ファイルに。
- **Quick Start チートシート** —— 「この 3 つを適用すれば 80% は再現できる」。最小の労力で最大の効果が出るよう優先順位付け済み。

---

## 仕組み

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

出力は `{site-slug}/` に生成されます：

```
stripe/
├── design.md              # AI agent reference — 16-section structured doc
├── report.ko.html         # interactive HTML report
└── screenshots/
    └── hero-cropped.png   # 1280×800 screenshot
```

---

## 機能

### コマンド

| コマンド | 説明 |
|---------|-------------|
| `/insane-design [URL]` | サイトを分析——デザインシステムを抽出し、`design.md` + HTML レポートを生成 |
| `/insane-design [slug]` | 分析済みデザインをプロジェクトに適用 |
| `/insane-design:analysis [URL]` | 分析モードのみ |
| `/insane-design:apply [slug]` | 適用モードのみ |
| `/insane-design:build [design.md]` | `design.md` から新しいページ／コンポーネントをスキャフォールド（URL からのオンザフライ合成も可） |
| `/insane-design:verify [job_id]` | 非同期ベリファイアをポーリング——§18 DON'T 違反 + スクリーンショット差分を収集 |

自然言語でも OK。「このサイトを分析して」で分析モード、「Stripe のスタイルを適用して」で適用モードが起動します。

### 抽出されるデザイントークン

| トークン種別 | 抽出内容 |
|------------|----------------|
| カラー | ブランドランプ、ニュートラルランプ、アクセント系統、セマンティックエイリアス |
| タイポグラフィ | 見出し／本文スケール——サイズ、ウェイト、行間、字間 |
| スペーシング | スペーシングトークン一式 |
| 角丸 | border-radius スケール |
| シャドウ | box-shadow レイヤー（多層スタックを含む） |
| フォントスタック | カスタムフォントの特定 + フォールバックチェーン |

### design.md の構成（16 セクション）

| セクション | 内容 |
|---------|---------|
| 01 Quick Start | 3 ステップの CSS スニペット——「5 分で 80% 再現」 |
| 02 Provenance | ソース URL、取得日、CSS バイト数 |
| 03 Tech Stack | フレームワークと DS 名前空間の検出 |
| 04–05 Typography | フォントスタック + フルスケール |
| 06 Colors | ブランドランプ · ニュートラル · アクセント · セマンティックレイヤー |
| 07–09 Spacing / Radius / Shadows | 全トークンテーブル |
| 12 Components | BEM クラスパターンとコンポーネントバリアント |
| 14 Drop-in CSS | そのまま貼れる `:root {}` CSS 変数 |
| 15 Tailwind Config | `theme.extend` 設定 |
| 16 DO / DON'T | そのサイトで最もやりがちなミスと回避策 |

### 適用モード —— トークンをプロジェクトに注入

適用モードはプロジェクト（globals.css、Tailwind 設定、素の CSS）をスキャンし、既存のセットアップを上書きせずにデザイントークンを注入します。

| 対象 | 動作 |
|--------|-------------|
| `:root {}` がある CSS | 既存ブロック内にトークンを追記 |
| `:root {}` がない CSS | ファイル先頭に新しい `:root {}` を作成 |
| Tailwind 設定 | `theme.extend.colors`、`fontFamily`、`borderRadius` を更新 |
| 再適用 | `/* insane-design */` ブロックをクリーンに置換 |

### 100 の分析済みサイト

Stripe、Apple、Linear、Vercel、Notion、Figma、GitHub、Discord、Slack、Spotify、Toss、Baemin、Daangn、Kakao、Naver、Nike、Gucci、Chanel、Tesla、Ferrari、BMW ほか 79 サイト。分析を走らせずに、どれでも直接適用できます。

---

## 動作要件

### 必須

- [Claude Code](https://docs.anthropic.com/claude-code)
- Python 3.11+
- Pillow (`pip install Pillow`)

### 任意

- **Playwright MCP** —— スクリーンショットのキャプチャ品質が向上します。なくても insane-design は curl ベースのキャプチャにフォールバックして動作します。

---

## ライセンス

MIT

---

<div align="center">

**本物の CSS。雰囲気じゃない。**

</div>
