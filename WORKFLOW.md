# ワークフロー（Workflow）

n8nで実行される自動分析フローの設計。

## 全体フロー

```
[Schedule Trigger]
      ↓ 毎朝9時
[データ取得]
  ├── US10Y（米10年債）
  ├── JP10Y（日本10年債）
  ├── USD/JPY（ドル円）
  └── ニュース（RSS / NewsAPI）
      ↓
[正規化（Set ノード）]
  → JSON_SCHEMA.md の入力スキーマに整形
      ↓
[Claude API 呼び出し（HTTP Request）]
  → PROMPT.md のプロンプトに変数を埋め込んで送信
      ↓
[レスポンス解析（Function ノード）]
  → JSONパース・バリデーション
      ↓
[ログ保存]
  ├── ローカルファイル or n8n内部DB
  └── spread_yesterday として翌日に引き継ぎ
      ↓
[通知・投稿]（Bフェーズ以降）
  ├── Notion
  └── Slack / メール
```

## Aプロトタイプの最小フロー

```
[Manual Trigger or Schedule]
      ↓
[Set ノード（固定値でテスト）]
  {
    "us10y": 4.35,
    "jp10y": 1.55,
    "usd_jpy": 143.20,
    "spread_yesterday": 2.75,
    "news_list": ["テストニュース1", "テストニュース2"]
  }
      ↓
[HTTP Request（Claude API）]
      ↓
[結果確認（n8n実行ログ）]
```

## Claude API 呼び出し設定（HTTP Request ノード）

| 項目 | 値 |
|---|---|
| Method | POST |
| URL | `https://api.anthropic.com/v1/messages` |
| Header: `x-api-key` | `{{ $env.ANTHROPIC_API_KEY }}` |
| Header: `anthropic-version` | `2023-06-01` |
| Header: `content-type` | `application/json` |
| Body | JSON（下記参照） |

```json
{
  "model": "claude-sonnet-4-6",
  "max_tokens": 1024,
  "messages": [
    {
      "role": "user",
      "content": "{{ プロンプト（変数埋め込み済み） }}"
    }
  ]
}
```

## spread_yesterday の引き継ぎ方法

Aフェーズ：前日の `spread` 値を手動でSetノードに入力。
Bフェーズ：前日ログファイルから自動取得 → SetノードへFunctionで渡す。
