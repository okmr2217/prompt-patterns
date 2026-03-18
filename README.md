# prompt-patterns

Claude（Web版・Claude Code）で再利用するためのプロンプト・ガイドをまとめたリポジトリ。

---

## ファイル一覧

| ファイル | 用途 | 使う場面 |
|----------|------|----------|
| [claude-workflow.md](./claude-workflow.md) | Claude Code でのセッション管理・Git ワークフロー設計ガイド | 新規プロジェクトに Claude Code を導入するとき |
| [versioning.md](./versioning.md) | バージョン更新作業の手順ガイド（Claude Code 参照用） | `CHANGELOG.md` と `package.json` を更新するとき |
| [app-intro-article.md](./app-intro-article.md) | アプリ紹介ブログ記事の執筆プロンプト | 自作アプリの紹介記事を書くとき |
| [record_music_identification_method.md](./record_music_identification_method.md) | 録音ファイルから曲を特定するプロンプト設計ガイド | 音声ファイルから曲名・YouTubeリンクを調べるとき |

---

## 使い方

### Claude Code で使う場合

`claude-workflow.md` と `versioning.md` は、プロジェクトの `CLAUDE.md` や `docs/versioning.md` としてそのままコピーして使える。

```bash
# 例: 新規プロジェクトにワークフローを適用する
cp path/to/prompt-patterns/versioning.md your-project/docs/versioning.md
```

### Claude Web で使う場合

各ファイルのテンプレートセクションをコピーして、Claude のチャットに貼り付ける。

---

## 改善メモ

- ファイル名を `kebab-case` に統一する（`record_music_identification_method.md` → `record-music-identification.md`）
- 用途別にディレクトリを分ける
  ```
  claude-code/   # Claude Code 向け（workflow, versioning）
  prompts/       # 汎用プロンプト（app-intro-article, music-identification）
  ```
