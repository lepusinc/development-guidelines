# 課題

このドキュメントは、Jira カスタム項目「システム影響範囲」を定義します。

課題タイプの**定義と使い分け**（Epic / Story / Task / Bug / Sub-task の意味とそれぞれの使いどころ）や、Jira 課題運用の目的・背景・ワークフローでの使い方・承認フロー・記録方針については、Dev Playbook（Confluence）の「Jira 課題運用ガイド」を参照してください。これらの文脈情報はそちらが正（Single Source of Truth）です。
<https://lepus.atlassian.net/wiki/spaces/DEVPB/pages/41615364/Jira>

課題・PR の**テンプレート**（タイプごとに使用するテンプレート）については [プルリクエスト](./PULL_REQUEST.md) を参照してください。

## 1. システム影響範囲

Jira のカスタム項目「システム影響範囲」では、以下の汎用選択肢を使用します。該当する項目を選択してください。

| No. | 項目 | 説明 |
| --- | --- | --- |
| 1 | UI / UX | 画面構成・コンポーネント・文言・操作フローなど、ユーザーインターフェースや体験に変更があるか。 |
| 2 | API | 既存APIのインターフェース（エンドポイント、パラメータ、レスポンス形式）や新規APIの追加・変更が発生するか。 |
| 3 | External Systems | 他システムとの連携や外部サービス/APIとのインテグレーションに影響があるか。 |
| 4 | Authorization / Authentication | 認証・認可の仕組みやアクセス制御に影響があるか。 |
| 5 | Business Logic / Domain Rules | ドメインルールや計算ロジック、状態遷移など、ビジネスロジックそのものが変わるか。 |
| 6 | Configuration / Feature Flags | 環境設定やコンフィグ、Feature Flag の追加・変更が必要か。 |
| 7 | Data Migration / Existing Data | 既存データの補正・移行や、一括更新などが必要か。 |
| 8 | DB Schema | テーブル構成やカラム定義、インデックス、制約などのスキーマ変更が必要か。 |
| 9 | Batch Jobs / Schedulers | バッチ処理や定期実行ジョブ、スケジューラ設定に影響があるか。 |
| 10 | Logging / Monitoring / Alerts | ログ出力内容やメトリクス収集、アラート条件・通知設定に変更が必要か。 |
| 11 | CI / CD | ビルド・テスト・デプロイのパイプライン定義や、そのフローに影響があるか。 |
| 12 | Deployment Process / Environments | 本番・ステージングなど各環境へのデプロイ手順や構成、リリース方法に影響があるか。 |
| 13 | DevOps Tooling & Infrastructure | CIツール、デプロイツール、監視基盤、IaC（Terraform 等）やインフラ構成に影響があるか。 |
| 14 | Local Development Environment / Developer Setup | 開発者のローカル環境構築手順やツール構成に変更が必要か。 |
| 15 | Documentation / Help | 仕様書、設計書、運用手順書、ユーザーマニュアル、FAQ などのドキュメント更新が必要か。 |
