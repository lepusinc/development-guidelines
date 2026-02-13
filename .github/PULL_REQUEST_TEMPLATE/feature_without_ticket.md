<!-- If AI generates this PR content, include both Japanese and English. -->

# Future Pull Request (Without Ticket)

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

## 🧪 Outputs / 作業成果

> At least one of the following is required. Leave at least one.
>
> 以下のいずれか必須。最低1つ以上は必ず残す

- [ ] Unit test added → Execution results (screenshot/CI link)  
  単体テスト追加 → 実行結果 (スクショ/CIリンク)
- [ ] Integration/E2E test added → Execution results (screenshot/CI link)  
  統合/E2Eテスト追加 → 実行結果 (スクショ/CIリンク)
- [ ] Manual verification screenshot or video  
  手動確認のスクリーンショット or 動画
- [ ] Reproduction steps described (reviewers can reproduce)  
  再現手順を記載（レビュアーが追試可能な形）

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

<!-- prettier-ignore -->
- [ ] Bug fix only (no refactoring)  
  バグ修正のみ（リファクタなし）
- [ ] Bug fix + **minimal mechanical refactoring** (separated into different commits)  
  バグ修正 + **最小の機械的リファクタ**（別コミットに分離済み）
- [ ] Preparatory refactoring (behavior unchanged) ※ Fix will be done in a separate PR  
  準備リファクタ（振る舞い不変）※修正は別PRで実施

---

## 🔍 Checklist / チェックリスト

<!-- prettier-ignore -->
- [ ] CI is green  
  CIがグリーンである
- [ ] Outputs are documented  
  作業成果を記載した
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
