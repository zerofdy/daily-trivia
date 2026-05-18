# Daily Trivia

毎朝、Cowork (Claude) が自動で雑学リレーを生成し、このサイトに追記していくアーカイブ。

## 公開URL

GitHub Pages 有効化後、`https://zerofdy.github.io/daily-trivia/` でアクセス可能。

## 構成

```
daily-trivia/
├── index.html      # 単一ファイル静的サイト（カードUI＋検索）
├── data.json       # データベース（Cowork が日次で append）
└── README.md
```

`index.html` は起動時に `data.json` を fetch し、日付降順でカード表示する。

## data.json スキーマ

```json
{
  "entries": [
    {
      "date": "YYYY-MM-DD",
      "general": [
        { "title": "...", "image": "URL or ''", "body": "..." },
        { "title": "...", "image": "...", "body": "..." },
        { "title": "...", "image": "...", "body": "..." }
      ],
      "it": [
        { "title": "...", "image": "...", "body": "..." },
        { "title": "...", "image": "...", "body": "..." },
        { "title": "...", "image": "...", "body": "..." }
      ]
    }
  ]
}
```

各 `entries[]` は1日分。`general` / `it` ともに連想リレー3トピック。

## セットアップ手順（初回のみ）

1. このリポジトリを GitHub に作成（`zerofdy/daily-trivia`、Public）
2. 3ファイルをコミット＆push
   ```bash
   git init
   git add index.html data.json README.md
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/zerofdy/daily-trivia.git
   git push -u origin main
   ```
3. **Settings → Pages** → Source を「Deploy from a branch」、Branch を `main` / `(root)` にして Save
4. 数十秒後、`https://zerofdy.github.io/daily-trivia/` で公開される
5. Cowork で使う Personal Access Token (PAT) を作成し、Cowork に渡す
   - 詳細は会話を参照（Contents: Read & Write）

## 日次更新フロー

Scheduled task `daily-trivia-web` が毎朝07:00に起動：

1. 雑学6トピック（一般3 + IT3）を生成
2. リポジトリをshallow clone
3. `data.json` の `entries[]` の先頭に今日分を追加
4. commit & push
5. GitHub Pages が自動再配信

メール送信は廃止。
