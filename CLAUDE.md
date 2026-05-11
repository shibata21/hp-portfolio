# hp_portfolio — クライアント提案用ギャラリー

4テーマのデザインサンプルを並べて、クライアントに好みを選んでもらうための提案用サイト。

- GitHub: https://github.com/shibata21/hp-portfolio
- 公開URL: https://shibata21.github.io/hp-portfolio/

## 親プロジェクト

事業モデル・共通技術スタック・共通の落とし穴は親リポジトリの
[../CLAUDE.md](../CLAUDE.md) を参照。

## 構造

```
hp_portfolio/
├── index.html              ← ギャラリートップ（業種フィルター付き）
└── samples/
    ├── natural/            ← 花屋「Fleur」緑×クリーム / Noto Serif JP
    ├── modern/             ← セレクトショップ「BLANC」黒×ゴールド / Cormorant Garamond
    ├── warm/               ← 惣菜店「まるや」オレンジ系 / M PLUS Rounded 1c
    └── luxury/             ← ジュエリー「AURUM」黒×金 / Playfair Display
```

各サンプルは **5ページ構成**（index / about / products / news / contact）。
contact.html のフォームは `https://formspree.io/f/XXXXXXXX` のプレースホルダのまま。
実際の運用機能（フォーム送信・お知らせ更新）の動作確認は [hp_test](../hp_test/) で行う。

## 役割上の制約

- このリポジトリはあくまで **見せるためのギャラリー**
- 各サンプル内の動的機能は実装しない（プレースホルダで止める）
- テーマごとの世界観（配色・フォント・写真の雰囲気）の一貫性を最優先
