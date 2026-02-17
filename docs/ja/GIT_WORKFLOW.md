# Gitワークフローガイドライン

このドキュメントは、Lepus Engineering Team の Git 運用ガイドを定義します。
各リポジトリに個別の Git ワークフローまたはブランチ運用ガイドが定義されている場合は、そのリポジトリの定義を優先します。

## 1. ブランチ戦略

- `main`: 本番反映済みの安定ブランチ
- `develop`: 開発統合ブランチ（テスト環境反映対象）
- `feature/*`: 機能開発ブランチ
- `bugfix/*`: バグ修正ブランチ
- `hotfix/*`: 緊急リリース対応ブランチ
- `release/*`: リリース候補ブランチ
- `docs/*`: ドキュメント更新ブランチ
- `refactor/*`: 振る舞いを変えない構造変更ブランチ
- `test/*`: テスト追加・更新ブランチ
- `chore/*`: メンテナンス作業ブランチ（ツール、設定、クリーンアップ）
- `ops/*`: 運用変更ブランチ（CI ワークフロー、環境設定）

## 2. 開発ワークフロー

1. `develop` から作業ブランチ（`feature/*` など）を作成します。
2. 作業ブランチで変更を実装します。
3. `feature -> develop` の Pull Request を作成します。
4. レビュー OK 後、squash merge します。

### 2.1 競合解決

- `feature` ブランチの同期は、`develop` をマージせず `git rebase origin/develop` を原則とします。
- `feature -> develop` PR の競合解消で他 feature への依存が生じた場合、PR に `dependency` ラベルを付与し、説明欄に `depends-on: <PR番号 or チケットキー>` を記載します。

## 3. リリースワークフロー

1. `main` から `release/<topic>` ブランチを作成します。
2. `develop` からリリース対象コミットを `cherry-pick` します（原則 1 ブランチ = 1 squash commit）。
3. `release/<topic> -> main` のリリース PR を作成します。
4. レビュー OK 後、squash merge します。
5. 必要に応じて `main` の変更を `develop` に反映して差分を同期します。

## 4. PR運用

- PR テンプレート（`.github/PULL_REQUEST_TEMPLATE/with_ticket.md` または `.github/PULL_REQUEST_TEMPLATE/without_ticket.md`）を使用します。
- チケットがある場合は `with_ticket.md`、ない場合は `without_ticket.md` を使用します。
- リリース/運用調整を主目的とする変更（リリースマージ、CI 変更、ローカル開発環境更新、ドキュメント更新）は `release_or_ops.md` を使用します。
- 本番ビルド/デプロイ変更は `with_ticket.md` または `without_ticket.md` を使用します。
- 選択したテンプレートに合わせて、PR タイトル形式とチェックリストを満たしてください。

## 5. ブランチ命名

形式:

`<type>/<ticket-or-topic>-<short-description>`

例:

- `feature/PROJ-123-add-review-level-checklist`
- `bugfix/PROJ-456-correct-pr-template-link`
- `hotfix/PROJ-789-patch-critical-release-issue`
- `docs/update-readme-document-index`
- `release/2026-02-docs-sync`

`type` は「1. ブランチ戦略」で定義したブランチ種別を使用します。

命名:

- チケットがある場合は先頭にチケットキーを入れます（例: `PROJ-123`）。
- チケットがない場合は、内容が分かるトピック語句を使います。
- `ticket-or-topic` と `short-description` は lowercase の kebab-case を使用します。
- 空白、アンダースコア、特殊文字は使用しません。
- 可能な限り 60 文字以内に収めます。

## 6. コミットメッセージ

- `test:`: 再現テスト・回帰テストに関する変更
- `fix:`: バグ修正
- `refactor:`: 振る舞いを変えないリファクタリング
- チケットがある場合は、コミットメッセージにチケット番号とチケットタイトルを含めます。  
  形式: ``<type>: [<チケット番号>] <チケットタイトル>``  
  例: ``fix: [PROJ-123] ログイン失敗時にエラーメッセージを表示する``
- 自動整形や大量リネームは、専用 PR または先行 PR で分離します。
