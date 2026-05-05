# プロジェクト概要

HP制作フリーランス向けの **クライアント提案用ポートフォリオ** と、関連する **運用検証用サイト** の2リポジトリ構成。

ターゲットクライアント: 中小・小売店オーナー（IT慣れていない層）。
販売モデル: **売り切り納品**（保守はオプション）。

---

## リポジトリ構成

### 1. hp_portfolio（このディレクトリ）— クライアント提案用ギャラリー
- パス: `c:\Users\kosuk\dev\hp_portfolio`
- GitHub: https://github.com/shibata21/hp-portfolio
- 公開URL: https://shibata21.github.io/hp-portfolio/
- 役割: 4テーマのデザインサンプルを並べて、クライアントに好みを選んでもらう

### 2. hp_test — 運用検証用サイト
- パス: `c:\Users\kosuk\dev\hp_test`
- GitHub: https://github.com/shibata21/hp-test
- 公開URL: https://shibata21.github.io/hp-test/
- 役割: 「お問い合わせ」「お知らせ」機能の実動作を検証する単独サイト（Naturalサンプルがベース）

---

## 技術スタック（共通）

- **純粋なHTML / CSS / JavaScript**（Node.js不要・ビルド工程なし）
- **Tailwind CSS** via CDN
- **Google Fonts**（テーマごとに使い分け）
- **picsum.photos** で画像プレースホルダ
- **GitHub Pages** でホスティング
- **Git** のみあれば運用可能

理由: クライアント納品後にクライアント側で技術的な保守が発生しないため、シンプル構成を最優先。

---

## hp_portfolio の構造

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
contact.htmlのフォームは `https://formspree.io/f/XXXXXXXX` のプレースホルダのまま（実際の運用は hp_test 参照）。

---

## hp_test の構造

```
hp_test/
├── index.html / about.html / products.html  ← 静的（Naturalサンプル流用）
├── news.html        ← Googleスプレッドシートから動的取得
├── contact.html     ← Googleフォームに送信
├── config.js        ← Google接続先URL・entry IDの設定
└── README.md        ← セットアップ手順
```

---

## 運用機能の設計（hp_test）

### お問い合わせ → Google フォーム
- **カスタムHTMLフォーム** → Google フォームの `formResponse` URLにPOST
- `mode: 'no-cors'` + `FormData` で送信
- 通知: Gmail（フォーム設定で「新しい回答のメール通知」ON）
- 回答記録: 連携先のGoogleスプレッドシート
- **完全無料**

### お知らせ → Google スプレッドシート
- スプレッドシートを **CSV形式でウェブ公開**
- ブラウザJSがCSVを取得 → **PapaParse** でパース → 描画
- 列構成: `日付` / `カテゴリ` / `タイトル` / `本文`
- カテゴリは `お知らせ` / `イベント` / `新商品` / `季節` で自動色分け
- **完全無料**

### config.js
- すべてのGoogle接続先設定を1ファイルに集約
- クライアントごとに値を差し替えるだけで他クライアント用に転用可能

---

## 落とし穴（必ず守る）

### Google フォームの選択肢ミスマッチ問題（重要）
- プルダウン/ラジオの `<option value="...">` は **Googleフォーム側のオプション文字列と完全一致** させる必要がある
- 1文字違うだけで **送信全体がサイレント拒否** される
- クライアントへの引き渡し時に「**Googleフォームの選択肢は勝手に変えないでください**」と必ず伝える

### 画像について
- 試行錯誤の結果、`picsum.photos` に落ち着いた
- `source.unsplash.com` は2024年5月廃止
- `Pollinations.ai` はseed指定で全部トラ画像が生成された（ハマった）
- **本番納品時はクライアントの実写真に差し替え前提**

### GitHub Pages
- `git push` で自動デプロイ
- GitHub Actions の不調時は数十分〜時間単位で詰まることあり（自分のミスじゃないので落ち着いて待つ）

### 改行コード
- Windowsでpowershell経由で書いたファイルはCRLFになりがち
- gitが警告を出すが動作上は問題なし

---

## クライアント納品時のチェックリスト

引き渡し前の作業:
1. config.js の `NEWS_SHEET_CSV_URL` をクライアント用シートのURLに置換
2. config.js の `CONTACT_FORM.url` をクライアント用フォームのURLに置換
3. config.js の `CONTACT_FORM.fields` の5つのentry IDを取得・差し替え
   - 取得方法: フォーム編集画面 → 「事前入力したリンクを取得」 → 適当な値を入れて「リンクを取得」 → URL内の `entry.XXXXXXX` を抽出
4. contact.html の `<option>` の値を、Googleフォーム側の選択肢と **1文字単位で完全一致** させる
5. 各ページの店舗情報（住所・電話・営業時間）をクライアント情報に差し替え
6. 画像URL（picsum）をクライアントの実写真に差し替え

引き渡し時の説明事項:
- Googleアカウントを使ってお知らせ更新（スプレッドシート編集）
- お問い合わせはGmailに自動通知
- **「Googleフォームの選択肢は勝手に変更しない」** ことを念押し

---

## ビジネスモデルメモ

- **売り切り** が基本。保守はオプション扱い。
- Formspree などの有料サービスを使うとフリーランス側で月額費用が発生するため、**Google製品（無料）に集約** する設計に至った。
- お知らせ機能の管理は **Googleスプレッドシート**（クライアントの大半がGmail所有 = 操作に慣れている）。
