# 🎧 YouTube Music プレイリスト自動生成ガイド

## 概要

YouTube再生履歴（Google Takeout）を分析し、ytmusicapi用のJSONプレイリストデータを生成した。
このドキュメントは他セッションでの再利用・追加生成のためのリファレンス。

---

## JSONスキーマ仕様

```json
{
  "playlist_name": "プレイリスト名",
  "description": "説明文",
  "tracks": [
    {
      "title": "曲名",
      "artist": "アーティスト名",
      "priority": "high | medium | low",
      "tags": ["ジャンルタグ"],
      "play_count": 28,
      "mood": "energy | chill | mixed | neutral | refresh",
      "march_2026_plays": 7
    }
  ]
}
```

### フィールド定義

| フィールド | 必須 | 型 | 説明 |
|-----------|------|-----|------|
| `title` | ✅ | string | 曲名（YouTube Music上の表記に合わせる） |
| `artist` | ✅ | string | アーティスト名（Topic名から`- Topic`を除いた形） |
| `priority` | ✅ | string | `high`=15回以上or直近ヘビロテ / `medium`=8〜14回 / `low`=7回以下 |
| `tags` | ✅ | string[] | ジャンル・サブジャンルタグ（下記タグ一覧参照） |
| `play_count` | ✅ | number | 全期間の再生回数 |
| `mood` | ✅ | string | `energy` / `chill` / `mixed` / `neutral` / `refresh` |
| `march_2026_plays` | - | number | 2026年3月の再生回数（最近のトレンド指標。0の場合は省略） |

### タグ一覧

**メインジャンル:** `日本語ラップ` / `海外ラップ` / `J-POP` / `EDM` / `R&B`

**サブジャンル:** `trap` / `drill` / `UK` / `classic` / `other`

フィルタ時は `--tags 日本語ラップ` のようにOR条件で絞り込み可能。

### mood定義

| mood | 意味 | 代表アーティスト |
|------|------|-----------------|
| `energy` | アガる・テンション高め | Watson, BAD HOP, Pop Smoke, Central Cee |
| `chill` | メロウ・落ち着いた | Kid Cudi, 6LACK, Frank Ocean, Mac Miller |
| `mixed` | 曲によって両方 | ZUTOMAYO, Tyler The Creator |
| `neutral` | 分類困難 | A Tribe Called Quest, 2Pac |
| `refresh` | J-POPなど息抜き系 | ずとまよ, ヨルシカ, ポルカ |

---

## 生成済みプレイリスト一覧（8本）

| # | ファイル名 | プレイリスト名 | 曲数 | 内容 |
|---|-----------|--------------|------|------|
| 1 | `01_heavyrotation_top30.json` | ヘビロテ TOP30 | 30 | 全期間play_count上位 |
| 2 | `02_jp_rap_only.json` | 日本語ラップ Only | 30 | 日本語ラップのヘビロテ曲 |
| 3 | `03_foreign_rap_only.json` | 海外ラップ Only | 30 | 海外ヒップホップのヘビロテ曲 |
| 4 | `04_march2026_mix.json` | 2026年3月 ヘビロテMIX | 30 | 直近の再生データ基準 |
| 5 | `05_chill_mix.json` | チル系ミックス | 25 | Kid Cudi, 6LACK, Frank Ocean系 |
| 6 | `06_energy_mix.json` | エナジー系ミックス | 25 | Watson, BAD HOP, Pop Smoke系 |
| 7 | `07_classic_hiphop.json` | クラシック ヒップホップ | 20 | 2Pac, Biggie, Dre, ATCQ |
| 8 | `08_jpop_refresh.json` | J-POP 息抜き | 20 | ずとまよ, ヨルシカ, ポルカ |

---

## アーティスト分類マスター

### 日本語ラップ（主要）

Watson, Yvngboi P, eyden, GREEN KIDS, Playsson, Eric.B.Jr., BAD HOP, ¥ellow Bucks, KEIJU, DJ KANJI, Masato Hayashi, Tee Shyne, The Mikado, Lunv Loyal, Sad Kid Yaz, Yvng Patra, Kaneee, Bene Baby, LEX, Kohjiya, Jin Dogg, Young zetton, RYKEY, Jinmenusagi, MonyHorse, Bonbero, YZERR, Leon Fanourakis, Awich, ANARCHY, Seeda, MC TYSON, AK-69, JP THE WAVY, MIYACHI, Benjazzy, YOU THUG, DADA, Elle Teresa, Kvi Baba, IO, STUTS, Tokyo Young Vision, 03- Performance, BON-K, Candee, BADSAIKUSH, DJ CHARI, DJ Ryow, AKLO

### 海外ラップ（主要）

Central Cee, Drake, Pop Smoke, A$AP Rocky, Eminem, Dave, Kendrick Lamar, J. Cole, XXXTENTACION, Lil Tjay, Future, Juice WRLD, Tyler The Creator, Kid Cudi, Roddy Ricch, Lil Uzi Vert, Childish Gambino, Travis Scott, A Tribe Called Quest, The Notorious B.I.G., 2Pac, Dr. Dre, 50 Cent, Ice Spice, Lil Baby, Jack Harlow, Meek Mill, JAY-Z, D-Block Europe, Freddie Gibbs, Mac Miller, 21 Savage

### J-POP

ZUTOMAYO, Polkadot Stingray, Yorushika, あいみょん, Kenshi Yonezu, YOASOBI, Rokudenashi, tofubeats, AZU

### EDM

DJ Snake, Major Lazer, Marshmello, Avicii

### R&B

Justin Bieber, The Weeknd, Post Malone, Akon, 6LACK, Frank Ocean, Jhené Aiko, Ed Sheeran, Russ

---

## ユーザーの音楽嗜好サマリー

### コアプロファイル

- **メインジャンル:** ヒップホップ / ラップ（全体の7〜8割）
- **日本語 vs 海外:** ほぼ半々。やや日本語ラップ寄り
- **好みのスタイル:** ストリート系・トラップ寄り。リアル志向
- **データ期間:** 2024年8月〜2026年3月（本格利用は2025年11月〜）
- **総再生数:** 23,756回（うちYouTube Music: 5,570回）

### TOP5アーティスト（全期間）

1. **Watson** — 347回。日本語ラップで圧倒的1位
2. **Central Cee** — 311回。UK drill/ラップのメイン
3. **Yvngboi P** — 146回。最近（2026年3月）最もハマっている
4. **eyden** — 106回
5. **ZUTOMAYO** — 105回。ヒップホップ以外で最多

### 最近の傾向（2026年3月）

Yvngboi Pが急上昇（月81回）。Watson, Central Ceeは安定。Sad Kid Yaz, RYKEY, Eric.B.Jr.への関心が増加中。Pop Smokeの名曲を繰り返し聴く傾向。

### 好みのキーワード

- トラップビート、ドリル、ストリート
- 日本語ラップのリアル志向（Watson的な世界観）
- UK ドリル / グライム（Central Cee, Dave）
- 90sクラシック（ATCQ, Biggie, 2Pac）も好む
- チルタイムはKid Cudi, 6LACK, Frank Ocean
- 息抜きにずとまよ、ヨルシカ

---

## ytmusic_dj.py での使い方

```powershell
# ドライラン（検索テスト）
python ytmusic_dj.py playlists/01_heavyrotation_top30.json --dry-run

# 本番実行
python ytmusic_dj.py playlists/04_march2026_mix.json

# タグ絞り込み
python ytmusic_dj.py playlists/01_heavyrotation_top30.json --tags drill
python ytmusic_dj.py playlists/01_heavyrotation_top30.json --tags 日本語ラップ

# ムード絞り込み
python ytmusic_dj.py playlists/01_heavyrotation_top30.json --mood energy

# 優先度絞り込み
python ytmusic_dj.py playlists/01_heavyrotation_top30.json --priority high

# 組み合わせ
python ytmusic_dj.py playlists/01_heavyrotation_top30.json --tags 海外ラップ --mood chill
```

---

## 新しいプレイリストを追加生成する場合

Claudeへのプロンプト例:

```
以下のJSON形式でプレイリストデータを生成して。
用途はytmusicapiでYouTube Musicプレイリストを自動作成するスクリプトに食わせること。

{
  "playlist_name": "プレイリスト名",
  "description": "説明",
  "tracks": [
    {
      "title": "曲名",
      "artist": "アーティスト名",
      "priority": "high",
      "tags": ["日本語ラップ", "trap"],
      "play_count": 28,
      "mood": "energy"
    }
  ]
}

条件: [ここに条件を書く]
参考: このチャットの音楽嗜好サマリーとアーティスト分類マスターを使って。
```
