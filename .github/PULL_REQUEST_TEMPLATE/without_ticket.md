<!-- If AI generates this PR content, include both Japanese and English. -->
<!-- Use the reviewee's language for PR review comments. If unknown, write both Japanese and English. -->
<!-- PRでのレビューコメントはレビュイーの言語でコメントする。言語が分からなければ、日本語と英語の併記でコメントする。 -->
<!-- Write the PR body based on verified facts; do not fill unknowns with guesses. -->
<!-- PR本文は確認済みの事実ベースで記載し、未確認事項を推測で埋めない。 -->
<!-- Check only applicable checklist items; add a short reason for non-applicable items when needed. -->
<!-- チェックリストは該当項目のみチェックし、必要に応じて非該当理由を簡潔に記載する。 -->
<!-- For verification, prioritize reproducible evidence (commands, environment, links). -->
<!-- 動作確認は再現可能な証跡（実行コマンド、環境、リンク）を優先して記載する。 -->
<!-- Include both rationale and decision reason in 1-2 lines. -->
<!-- 変更理由と判断理由を1-2行で明記する。 -->
<!-- Explicitly state security/permission/data impact; write \"none\" when there is no impact. -->
<!-- セキュリティ/権限/データ影響は明示し、影響がない場合も「なし」と記載する。 -->
<!-- For review feedback responses, separate addressed items and intentionally unaddressed items with reasons. -->
<!-- レビュー指摘への対応は、対応済みと未対応（理由付き）を分けて記載する。 -->

# Pull Request (Without Ticket)

## 📝 Background & Purpose / 背景・目的

> Describe the background/purpose
>
> 対応理由を必ず記載

Description / 説明文

---

## 🔄 Changes / 変更内容

> List the main changes in bullet points
>
> 主な変更点を箇条書きで

-

---

## 🖥️ Verification / 動作確認

> Evidence of developer-led verification in the development environment.
>
> 開発環境で開発者が行った動作確認。

- [ ] Include screenshots or videos in the "Implementation Results" section  
  「🧾 Implementation Results / 実装結果」セクションにスクリーンショット or 動画を記載
- [ ] Unit test added → Execution results (screenshot/CI link)  
  単体テスト追加 → 実行結果 (スクショ/CIリンク)
- [ ] Integration/E2E test added → Execution results (screenshot/CI link)  
  統合/E2Eテスト追加 → 実行結果 (スクショ/CIリンク)
- [ ] Log file snippets  
  ログファイルの抜粋
- [ ] API request & response samples  
  APIリクエスト&レスポンスのサンプル
- [ ] Database records (before & after)  
  DBレコード(変更前後)
- [ ] Performance metrics  
  パフォーマンスメトリクス
- [ ] REPL/Debugger output  
  REPL/デバッガーの実行結果

---

## 🧾 Implementation Results / 実装結果

> Summarize implementation outcomes for requesters and reviewers
>
> 依頼者・レビュアー向けに実装結果を簡潔に記載

---

## 🖥️ Verification Steps / 動作確認手順

> Provide concise steps that reviewers can minimally verify
>
> レビュアーが最低限確認できる手順を簡潔に

1. Check out the branch  
   ブランチをチェックアウト
2. Start the environment with `make start`  
   `make start` で環境起動
3. Access `/xxx` and execute ○○ to see △△ displayed  
   `/xxx` にアクセスして ○○ を実行すると △△ が表示される

---

<!-- Keep this list in sync with "2. システム影響範囲" in docs/ja/ISSUE.md -->
## 📌 System Impact / システム影響範囲

> Select all applicable items. If needed, add brief context.
> See details: [ISSUE.md - 2. System Impact Scope](../../docs/en/ISSUE.md#2-system-impact-scope)
>
> 該当する項目をすべて選択し、必要に応じて補足を記載してください
> 詳細: [ISSUE.md - 2. システム影響範囲](../../docs/ja/ISSUE.md#2-システム影響範囲)

- [ ] UI / UX
- [ ] API
- [ ] External Systems / 外部システム
- [ ] Authorization / Authentication / 認可 / 認証
- [ ] Business Logic / Domain Rules / ビジネスロジック / ドメインルール
- [ ] Configuration / Feature Flags / 設定 / Feature Flag
- [ ] Data Migration / Existing Data / データ移行 / 既存データ
- [ ] DB Schema / DBスキーマ
- [ ] Batch Jobs / Schedulers / バッチ処理 / スケジューラ
- [ ] Logging / Monitoring / Alerts / ログ / 監視 / アラート
- [ ] CI / CD
- [ ] Deployment Process / Environments / デプロイ手順 / 環境
- [ ] DevOps Tooling & Infrastructure / DevOpsツール / インフラ
- [ ] Local Development Environment / Developer Setup / ローカル開発環境 / 開発者セットアップ
- [ ] Documentation / Help / ドキュメント / ヘルプ

---

## 📋 Nature of this PR / このPRの性質

> Select all applicable categories
>
> 該当するカテゴリをすべて選択

<!-- prettier-ignore -->
- [ ] Feature / Enhancement  
  機能追加 / 改善
- [ ] Bug Fix  
  バグ修正
- [ ] Refactoring (no behavior change)  
  リファクタリング（振る舞い変更なし）
- [ ] Performance / Reliability Improvement  
  パフォーマンス / 信頼性改善
- [ ] Test-only Change  
  テストのみの変更
- [ ] Build / Deployment / CI / Tooling / Dependency Update  
  ビルド / デプロイ / CI / ツール / 依存関係の更新

---

## 🔍 Checklist / チェックリスト

<!-- prettier-ignore -->
- [ ] CI is green  
  CIがグリーンである
- [ ] Verification by developer is documented  
  開発者による動作確認を記載した
- [ ] Implementation results are documented  
  実装結果を記載した
- [ ] Verification steps are documented  
  動作確認手順を記載した
- [ ] System impact is selected  
  システム影響範囲を選択した
- [ ] Security/permissions/performance impact considered  
  セキュリティ/権限/パフォーマンス影響を考慮した
- [ ] Known constraints/TODOs that should be communicated to reviewers are documented  
  レビュアーに伝えるべき既知の制約・TODOを記載した

---

> 🛈 For detailed review criteria, please refer to [REVIEW_POLICY.md](../../docs/en/REVIEW_POLICY.md)  
> 🛈 詳細なレビュー基準については [REVIEW_POLICY.md](../../docs/ja/REVIEW_POLICY.md) を参照してください
