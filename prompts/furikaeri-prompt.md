# 日次振り返りレポート生成プロンプト

## 使い方

Claude.ai で `furikaeri-mcp` コネクタが有効な状態で、以下のプロンプトをそのまま送信する。
`{DATE}` を振り返りたい日付（例: `2026-03-22`）に置き換える。

---

## プロンプト本文

```
furikaeri-mcp の get_day_summary で {DATE} のデータを取得し、以下のフォーマットで振り返りレポートを作成してください。

## レポート構成

### 0. サマリ（冒頭に配置）
全データを俯瞰し、その日を3〜4文で客観的に要約してください。ニュースのリード文のように、「何があった日か」を端的に伝えます。
- 1文目: その日の主軸となった活動と定量ファクト（タスク完了数、完了率など）
- 2文目: 時間の使い方や移動の概要（位置情報・カレンダーから）
- 3文目: ピーク体験の中で特に印象的だったもの（アクティビティ名を必ず記載し、noteの内容があれば要点を織り込む）
- 4文目（任意）: 新しく生まれたアイデアや特筆すべき出来事

書き方のルール:
- 推測や感想は入れず、データから読み取れる事実のみ
- 「〜した日だった」「〜に取り組んだ」のような簡潔な文体
- レポート本文を読まなくても一日の輪郭が掴める密度を目指す
- 本文の各セクションと重複する詳細な数値（個別タスク名の列挙など）は避ける
- 時刻は必ず実データ（performedAt, startTime 等）を確認してから書く。「午前中」「夕方」のような曖昧な表現で丸めず、「深夜0時台」「未明」「午前3時頃」のように実際の時間帯を正確に反映する

### 1. 一日のタイムライン
カレンダー予定・ピークログ・位置情報を時系列で統合し、その日の流れを再構成してください。
- カレンダーの予定（時間帯つき）を時系列の軸にする
- ピークログの performedAt をカレンダーの間に差し込む。その際、アクティビティ名（emoji付き）とnoteの要点を添えて、単なる「記録あり」にとどまらず何をしてどう感じたかが伝わるように書く
- 位置情報（visit / activity）があれば移動や訪問場所も織り込む
- 終日イベントがあれば冒頭にまとめる
- タスクの完了タイミング（completedAt）が近い場合は、ピーク体験や移動との文脈を添えて記述する（例：「〇〇タスクを完了後、△△でワクワク度4を記録」）

### 2. タスクの振り返り
tasks データから、以下を整理してください。
- 完了タスク: reasons に "completed" を含むもの。カテゴリ別にグルーピング
- スキップ/未完了: reasons に "skipped" を含む、または status が "PENDING" のもの。理由や背景があれば memo から補足
- 新規作成: reasons に "created" を含むもの。その日に思いついたタスクとして紹介
- 完了率: summary の completedOnDate / scheduled を計算

セクション末尾に以下のリンクを添える：
> 📋 [Yarukoto でタスクを確認](https://yarukoto.vercel.app/?date={DATE})

### 3. ピーク体験と気分
peakLogs から、その日の体験の質を詳しく分析してください。
- 各ログを「項目の列挙」としてではなく、アクティビティの内容とnoteを読み込んだうえで**文章として語る**。「〇〇（アクティビティ名）に取り組み、noteには『〜〜』とある。スコアはワクワク度△・達成感△で…」のように、何をしてどう感じたかが伝わる記述にする
- 各ログに必ず含める情報：
  - アクティビティ名（emoji があれば付与）と実施時刻（performedAt）
  - wantAgain の値（またやりたいか）
  - reflection.note の内容（あれば全文引用）
- excitement / achievement スコアは未入力の場合がほとんどのため、記載・言及しない
- wantAgain: true のアクティビティを「またやりたいこと」としてハイライト
- 位置情報やカレンダーと照合し、どんな場所・状況でその体験が生まれたか文脈を補足する
  （例：「外出中の移動後に記録」「〇〇の予定の直後」など）
- タスクの完了と時間的に近いピーク体験があれば、その関連性にも言及する

セクション末尾に以下のリンクを添える：
> 📈 [Peak Log で履歴を確認](https://peak-log-gamma.vercel.app/history?mode=timeline)

### 4. 支出サマリー
transactions データがあれば、以下を整理してください。
- 合計支出額
- カテゴリ別（categoryL / categoryM）の内訳
- 特記事項（高額な支出、普段と異なる支出先など）

セクション末尾に以下のリンクを添える：
> 💰 [MoneyForward ME で明細を確認](https://moneyforward.com/)

### 5. Claudeとの会話（任意）
conversation_search で {DATE} 前後の会話履歴を検索し、その日にClaudeと何を話したかを簡潔にまとめてください。
振り返りの文脈を補強する材料として使います。

### 6. まとめ
以下の観点で、その日を総括してください。
- 一言で表すと、どんな一日だったか（タスク・ピーク・移動を総合して）
- よかったこと / うまくいったこと（具体的なアクティビティ名・noteの内容・wantAgainを根拠として挙げる）
- 改善できそうなこと / 明日への申し送り（スキップタスクや、wantAgain: falseだった体験も踏まえて）
- 写真URL（photosUrl）があればリンクを添えて「写真で振り返る」として案内

## 出力上のルール
- 日本語で出力
- 見出しは ## / ### を使い、読みやすく構造化
- データが空やエラー（error: true）のセクションは「データなし」と表示してスキップ
- 推測や創作はせず、取得データに基づいて書く
- 時刻は JST で表示（データが UTC の場合は +9h 変換）
```

---

## データ構造リファレンス

プロンプトの背景知識として、`get_day_summary` が返す JSON の構造を以下に示す。

### トップレベル

```json
{
  "date": "2026-03-22",
  "tasks": { ... } | { "error": true, "message": "...", "code": "TASKS_ERROR" },
  "peakLogs": { ... } | { "error": true, "message": "...", "code": "PEAK_LOGS_ERROR" },
  "diary": { ... } | { "error": true, "message": "...", "code": "DIARY_ERROR" },
  "calendarEvents": { ... } | { "error": true, "message": "...", "code": "CALENDAR_ERROR" },
  "photosUrl": { "date": "...", "searchQuery": "...", "url": "..." } | { "error": true, ... },
  "transactions": { "transactions": [...] } | { "error": true, ... },
  "locationHistory": { "segments": [...] } | { "error": true, ... }
}
```

### tasks

```typescript
{
  date: string;
  tasks: Array<{
    title: string;
    status: "PENDING" | "COMPLETED" | "SKIPPED";
    priority: "HIGH" | "MEDIUM" | "LOW" | null;
    category: string | null;        // カテゴリ名（Yarukoto のカテゴリ）
    memo: string | null;
    scheduledAt: string | null;     // "YYYY-MM-DD"
    completedAt: string | null;     // JST ISO string
    skippedAt: string | null;       // JST ISO string
    createdAt: string;              // JST ISO string
    reasons: ("scheduled" | "completed" | "skipped" | "created")[];
  }>;
  summary: {
    total: number;
    scheduled: number;
    completedOnDate: number;
    skippedOnDate: number;
    createdOnDate: number;
  };
}
```

### peakLogs

```typescript
{
  date: string;
  logs: Array<{
    activity: { name: string; emoji: string | null; color: string | null };
    performedAt: string;  // JST ISO string
    reflection: {
      excitement: number | null;   // ワクワク度
      achievement: number | null;  // 達成感
      wantAgain: boolean;          // またやりたいか
      note: string | null;         // 振り返りメモ
    } | null;
  }>;
  summary: {
    totalLogs: number;
    withReflection: number;
    averageExcitement: number | null;
  };
}
```

### calendarEvents

```typescript
{
  date: string;
  events: Array<{
    title: string;
    startTime: string;    // RFC3339 or "YYYY-MM-DD"（終日イベント）
    endTime: string;
    location: string | null;
    description: string | null;
    isAllDay: boolean;
  }>;
  summary: {
    totalEvents: number;
    allDayEvents: number;
    timedEvents: number;
  };
}
```

### transactions

```typescript
{
  transactions: Array<{
    date: string;
    content: string;      // 支出内容
    amount: number;        // 金額
    institution: string;   // 決済手段（カード名など）
    categoryL: string;     // 大カテゴリ
    categoryM: string;     // 中カテゴリ
    memo: string;
  }>;
}
```

### locationHistory

```typescript
{
  segments: Array<
    | {
        startTime: string;
        endTime: string;
        type: "visit";
        visit: {
          placeId: string;
          semanticType: string;   // "TYPE_HOME", "TYPE_WORK" など
          probability: number;
          placeLocation: string;  // "lat, lng"
        };
      }
    | {
        startTime: string;
        endTime: string;
        type: "activity";
        activity: {
          startLocation: string;
          endLocation: string;
          distanceMeters: number;
          type: string;           // "IN_VEHICLE", "WALKING" など
        };
      }
  >;
}
```

### photosUrl

```typescript
{
  date: string;
  searchQuery: string;
  url: string;  // Google Photos の検索 URL
}
```

### diary（現在スタブ）

```typescript
{
  date: string;
  entries: [];  // 日記アプリ未開発のため常に空配列
}
```
