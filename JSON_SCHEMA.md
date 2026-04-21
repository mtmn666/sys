# 最小構成JSON（Minimal Input Schema）

Aプロトタイプが動くための、最小・安定・拡張可能な入力構造。

## 入力スキーマ

```json
{
  "us10y": 0.0,
  "jp10y": 0.0,
  "usd_jpy": 0.0,
  "spread_yesterday": 0.0,
  "news_list": []
}
```

## フィールド定義

| フィールド | 型 | 説明 | 例 |
|---|---|---|---|
| `us10y` | float | 米10年債利回り（%） | 4.35 |
| `jp10y` | float | 日本10年債利回り（%） | 1.55 |
| `usd_jpy` | float | ドル円レート | 143.20 |
| `spread_yesterday` | float | 前日の金利差（us10y − jp10y） | 2.75 |
| `news_list` | array[string] | 今日の主要ニュース（最大10件） | ["Fed会合...", "日銀声明..."] |

## 出力スキーマ

```json
{
  "spread": "2.80%",
  "spread_change": "+0.05%（前日比）",
  "drivers": ["..."],
  "sentiment": "強気",
  "importance": 75,
  "top_news": ["...", "...", "..."]
}
```

## 拡張予定フィールド（Bフェーズ以降）

```json
{
  "us2y": 0.0,
  "jp2y": 0.0,
  "vix": 0.0,
  "sp500": 0.0,
  "nikkei": 0.0
}
```

> 現行の入力スキーマに追加するだけで動作する設計になっている。既存フィールドは変更不要。
