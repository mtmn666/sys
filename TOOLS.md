# 使用ツール（Tools）

プロジェクトを構成する主要ツールと役割。

## VSCode

- コード編集・ローカル管理
- 拡張機能：Claude Code（このアシスタント）
- プロジェクトルート：`c:/Users/user/source/repos/SYS/`

## Claude API

- 分析エンジン
- エンドポイント：`https://api.anthropic.com/v1/messages`
- 現在使用モデル：`claude-sonnet-4-6`
- 認証：`x-api-key` ヘッダー

## n8n

- 自動実行スケジューラー（毎朝9時）
- 実行フロー：
  1. スケジュールトリガー起動
  2. 外部APIからデータ取得
  3. JSON正規化
  4. Claude API呼び出し
  5. レスポンス解析・ログ保存
  6. Notion投稿（Bフェーズ以降）
- アクセス先：`http://localhost:5678`（ローカル）

## GitHub

- バージョン管理
- 管理対象：プロンプト・JSONスキーマ・n8nワークフロー・ドキュメント
- リポジトリ：`SYS`

## ngrok

- ローカルのn8nを外部アクセス可能にするトンネル
- Claude API からのwebhookレスポンス受信に使用
- 起動コマンド例：`ngrok http 5678`
- 注意：無料プランはURLが再起動のたびに変わる

## データソース候補（Bフェーズ）

| データ | ソース候補 |
|---|---|
| 米10年債利回り | FRED API / Yahoo Finance |
| 日本10年債利回り | Bloomberg / 日本銀行 |
| ドル円レート | ExchangeRate API / Yahoo Finance |
| ニュース | NewsAPI / RSS（Reuters, Bloomberg） |
