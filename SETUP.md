# セットアップガイド

## 1. Formspree の設定（フォームID取得）

1. [https://formspree.io](https://formspree.io) にアクセス
2. 無料アカウントを作成（GitHubアカウントでもOK）
3. 「New Form」を作成 → フォーム名を入力（例：`translator-contact`）
4. 発行された **Form ID**（`xxxxxxxx` の8文字）をコピー
5. プロジェクト内の **全contact.mdファイル**を開き、以下を一括置換：

```
YOUR_FORM_ID  →  取得した実際のID
```

対象ファイル：
- `ja/contact.md`
- `en/contact.md`
- `zh-cn/contact.md`
- `zh-tw/contact.md`

> **無料プランの制限**：月50件まで。超える場合は有料プラン（月$8〜）へ。

---

## 2. GitHub Pages の設定

### リポジトリ作成

1. GitHub で新規リポジトリを作成（例：`translation-site`）
2. このフォルダの内容をプッシュ：

```bash
cd honkit-site
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/あなたのID/translation-site.git
git push -u origin main
```

### GitHub Actions でビルド自動化

`.github/workflows/deploy.yml` を作成（下記）→ `main` にプッシュするたびに自動ビルド＆公開されます。

```yaml
name: Deploy Honkit to GitHub Pages

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm install
      - run: npm run build
      - uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./_book
```

3. GitHub リポジトリの **Settings → Pages → Source** を `gh-pages` ブランチに設定

公開URL：`https://あなたのID.github.io/translation-site/`

---

## 3. 独自ドメインの接続（移行時）

現在のドメインをGitHub Pagesに向ける場合：

1. ドメインのDNS設定で `CNAME` レコードを追加：
   - `www` → `あなたのID.github.io`
2. リポジトリのルートに `CNAME` ファイルを作成（内容：`www.あなたのドメイン.com`）
3. GitHub Settings → Pages → Custom domain に入力

> **安定したら旧レンタルサーバーを解約**するタイミングでDNSを切り替えればOKです。

---

## 4. NotebookLMのスライド画像を挿入する方法

1. NotebookLM でスライド（PDF）を生成・ダウンロード
2. PDFをJPGに変換（例：Adobe、ILovePDF、または `pdftoppm` コマンド）
3. 変換した画像を `assets/images/` フォルダに配置
4. 各Markdownファイルのコメントアウト部分を有効化：

```markdown
<!-- コメントを外してファイル名を合わせる -->
![説明テキスト](../../assets/images/ファイル名.jpg)
```

---

## 5. 料金を入力する箇所

以下のファイルの `¥X,XXX` / `●●` プレースホルダーを実際の値に置き換えてください：

- `ja/services/README.md` — 日本語料金表
- `en/services/README.md` — 英語料金表
- `zh-cn/services/README.md` — 中国語（簡体字）料金表
- `ja/portfolio/README.md` — 実績件数
- `ja/books/README.md` — 書籍タイトル・リンク
- `book.json` — あなたの名前・メールアドレス
