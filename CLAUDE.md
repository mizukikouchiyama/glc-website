# GLC Website — Project Context & Design System

## Project Overview
- **Brand**: GLC (Strategic Intelligence firm)
- **Site URL**: https://glc-website.netlify.app
- **GitHub**: https://github.com/mizukikouchiyama/glc-website
- **Stack**: Pure static HTML/CSS/JS (single `index.html`), no build tools
- **Animation**: GSAP 3.12.5 + ScrollTrigger (CDN)
- **Reference**: https://www.anthropic.com (design language, layout patterns, animation behaviors)

## Design Philosophy
Anthropic.com のアカデミック・ミニマリズムを踏襲。知的で洗練された、読書体験を阻害しない静謐なデザイン。box-shadow 一切禁止。装飾はフラットな色面と極細罫線(1px border)のみ。

## Typography System
| Role | Font | Weight | Usage |
|---|---|---|---|
| Sans-serif (UI) | **Inter** | 400-900 | ナビ、ヒーロー見出し、ボタン、セクションヘッダ、メタ情報 |
| Serif (Editorial) | **DM Serif Display** | 400 (+ italic) | リード文、記事タイトル、ダークセクション見出し、ミッションリンク |

### Heading Rules
- ヒーロー見出し: `Inter 900`, `clamp(3.2rem, 7.5vw, 6.5rem)`, `letter-spacing: -0.045em` (超タイト)
- ダークセクションタイトル: `DM Serif Display`, uppercase, `letter-spacing: 0.06em`, 一部 italic (lowercase)
- 下線アニメーション: `::after` 疑似要素で `width: 0→100%`

## Color System (CSS Variables)
```
--bg-paper: #F4EFE9        (ペーパーライクなオフホワイト背景)
--bg-card: #E6E1DA          (カード面、一段暗い)
--bg-dark: #0B0B0B          (フッター等)
Feature dark section: #000000 (純黒 — 土星画像の黒と完全一致)
--text-primary: #1C1C1C     (ダークチャコール、真黒を避ける)
--text-secondary: #5A5550
--text-on-dark: #F4EFE9
--border-fine: #C9C3BB      (極細罫線)
```

## Layout Architecture
- Container max-width: 1320px
- Gutter: `clamp(20px, 4vw, 48px)`
- Hero: CSS Grid `1.15fr 0.85fr`, gap `clamp(60px, 10vw, 160px)` — 左に巨大Sans-serif見出し、右下にSerif体リード文
- Cards: 3-column grid, フラットデザイン (no box-shadow), `border-radius: 10px`
- Mission: Grid `1fr 1.5fr`

## Animation System

### GSAP ScrollTrigger (Feature Dark Section)
- **Container expand**: margin 48px→0, borderRadius 40px→0, `scrub: 1`, start "top 80%" → end "top 15%"
- **Title parallax**: y: 60→-20, opacity: 0→1, `scrub: 1`
- **Planet image**: opacity: 0→1, scale: 0.88→1, deeper parallax scale 1→1.1
- **No pin**: 自然なスクロールフロー維持

### CSS/IO-based Animations (Other Sections)
- Hero見出し: 単語ごとスタガード (80ms間隔, `cubic-bezier(0.16, 1, 0.3, 1)`)
- ローテーションテキスト: 3秒ごとに "clarity→vision→strategy→precision" スライド切替
- カード: スタガード 150ms間隔フェードイン
- ミッションリンク: スタガード 120ms間隔
- ホバー: 200-300ms ease-in-out, opacity変化 + translateX(3px) のみ

### Accessibility
- `prefers-reduced-motion: reduce` で全アニメーション無効化
- GSAP `.set()` で最終状態を即座に適用
- セマンティックHTML、aria-label、focus-visible

## Assets
- `assets/saturn.jpg` — 1300×664px (1x, 55KB)
- `assets/saturn@2x.jpg` — 2600×1329px (2x Retina, 174KB)
- Source: NASA Cassini PIA06193 (Public Domain)
- 画像エッジ: `mask-image: radial-gradient(ellipse)` で純黒背景にシームレスにフェザリング

## Section Structure
1. **Header** — sticky, z-index:100, 常にダークセクションの上
2. **Hero** — アシンメトリーGrid、右上に装飾線(36px×2px)
3. **Feature Dark** — GSAP ScrollTrigger駆動、カード→全画面展開、土星画像
4. **Cards (Latest Insights)** — 3列フラットカード
5. **Mission** — 2列Grid、Serifリンクリスト
6. **Footer** — ダーク、4カラムGrid

## Infrastructure
- Netlify: `netlify deploy --prod --dir=.` (ビルド不要)
- GitHub → main push で自動デプロイ
- No build command, publish directory: `.`

## Design Constraints (MUST FOLLOW)
1. **box-shadow 完全禁止**
2. **CSSフレームワーク禁止** (Tailwind/Bootstrap等不使用)
3. **カラー/フォントは参考画像から自己推論**
4. **アニメーションは200-300ms ease-in-out、読書体験を阻害しない**
5. **セリフ体とサンセリフ体の明確なコントラスト**
6. **境界線は1px極細ソリッド罫線のみ**
