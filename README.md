# prompt-patterns

Claude（Web版・Claude Code）で再利用するためのプロンプト・ガイドをまとめたリポジトリ。

---

## ディレクトリ構成

```
prompt-patterns/
├── claude-code/                        # Claude Code 向け
│   ├── claude-workflow.md              # セッション管理・Git ワークフロー設計ガイド
│   ├── versioning.md                   # バージョン更新作業ガイド
│   ├── claude-md-template.md           # CLAUDE.md 雛形（コピーして使う）
│   └── coding-rule.md                  # コーディングルール（Next.js / TypeScript）
└── prompts/                            # 汎用プロンプト
    ├── snippets.md                     # よく使う依頼文スニペット集
    ├── app-intro-article.md            # アプリ紹介ブログ記事の執筆プロンプト
    ├── record-music-identification.md  # 録音ファイルから曲を特定するガイド
    └── playlist_generation_guide.md    # YouTube Musicプレイリスト自動生成ガイド
```

---

## ファイル詳細

### claude-code/

| ファイル | 概要 | 使う場面 |
|----------|------|----------|
| [claude-workflow.md](./claude-code/claude-workflow.md) | セッション管理・Git ワークフロー・ドキュメント設計の全体ガイド | 新規プロジェクトに Claude Code を導入するとき |
| [versioning.md](./claude-code/versioning.md) | バージョン更新の手順・CHANGELOG 記述ルール | `package.json` と `CHANGELOG.md` を更新するとき |
| [claude-md-template.md](./claude-code/claude-md-template.md) | `CLAUDE.md` の雛形（プロジェクト固有部分を書き換えて使う） | 新規プロジェクトのセットアップ時 |
| [coding-rule.md](./claude-code/coding-rule.md) | Next.js / TypeScript プロジェクト向けコーディングルール（型・設計・ファイル構成・エラーハンドリング） | CLAUDE.md に貼り付けてコーディング規約を Claude に伝えるとき |

### prompts/

| ファイル | 概要 | 使う場面 |
|----------|------|----------|
| [snippets.md](./prompts/snippets.md) | コードレビュー・バグ調査・実装依頼などの短い依頼文テンプレート | 毎日のやり取りで使い回す |
| [app-intro-article.md](./prompts/app-intro-article.md) | アプリ紹介ブログ記事の執筆プロンプト | 自作アプリの紹介記事を書くとき |
| [record-music-identification.md](./prompts/record-music-identification.md) | 録音ファイルから曲名・YouTubeリンクを特定するガイド | 音声ファイルの曲調査をするとき |
| [playlist_generation_guide.md](./prompts/playlist_generation_guide.md) | YouTube再生履歴からytmusicapi用プレイリストJSONを生成するガイド（スキーマ・アーティスト分類・音楽嗜好サマリー含む） | ytmusicapiでYouTube Musicのプレイリストを自動作成するとき |

---

## 使い方

### Claude Code 向けファイルを新規プロジェクトに適用する

```bash
# CLAUDE.md 雛形をプロジェクトにコピー
cp path/to/prompt-patterns/claude-code/claude-md-template.md your-project/CLAUDE.md

# バージョン管理ガイドをコピー
cp path/to/prompt-patterns/claude-code/versioning.md your-project/docs/versioning.md
```

その後 `CLAUDE.md` のプロジェクト固有セクションを書き換える。
詳細な導入手順は [claude-workflow.md](./claude-code/claude-workflow.md) を参照。

### Claude Web で使う

`prompts/snippets.md` から目的に合った依頼文をコピーして貼り付ける。
各プロンプトファイルのテンプレートセクションも同様にコピーして使う。
