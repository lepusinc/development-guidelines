# プルリクエスト

このドキュメントは、GitHub テンプレートとプルリクエストの**操作規約**（使用する PR テンプレート・課題テンプレートの選択、PR タイトルとチェックリストの整合）を定義します。

テンプレートは全リポジトリで共通化するため lepusinc/.github で管理しています。

- PR テンプレート: <https://github.com/lepusinc/.github/tree/main/.github/PULL_REQUEST_TEMPLATE>
- 課題テンプレート: <https://github.com/lepusinc/.github/tree/main/.github/ISSUE_TEMPLATE>

## 1. PR テンプレート

- PR テンプレート（`with_ticket.md` または `without_ticket.md`）は <https://github.com/lepusinc/.github/tree/main/.github/PULL_REQUEST_TEMPLATE> を使用します。
- チケットがある場合は `with_ticket.md`、ない場合は `without_ticket.md` を使用します。
- リリース/運用調整を主目的とする変更（リリースマージ、CI 変更、ローカル開発環境更新、ドキュメント更新）は `release_or_ops.md` を使用します（<https://github.com/lepusinc/.github/blob/main/.github/PULL_REQUEST_TEMPLATE/release_or_ops.md>）。
- 本番ビルド/デプロイ変更は `with_ticket.md` または `without_ticket.md` を使用します。
- 選択したテンプレートに合わせて、PR タイトル形式とチェックリストを満たしてください。

## 2. 課題テンプレート

課題タイプに対応するテンプレートを選択してください。

| タイプ | テンプレート |
| --- | --- |
| Epic | [epic.md](https://github.com/lepusinc/.github/blob/main/.github/ISSUE_TEMPLATE/epic.md) |
| Story | [story.md](https://github.com/lepusinc/.github/blob/main/.github/ISSUE_TEMPLATE/story.md) |
| Task | [task.md](https://github.com/lepusinc/.github/blob/main/.github/ISSUE_TEMPLATE/task.md) |
| Bug | [bug.md](https://github.com/lepusinc/.github/blob/main/.github/ISSUE_TEMPLATE/bug.md) |
| Sub-task | [sub_task.md](https://github.com/lepusinc/.github/blob/main/.github/ISSUE_TEMPLATE/sub_task.md) |

課題タイプの**定義と使い分け**（各タイプの意味と使いどころ）は、Dev Playbook（Confluence）の「Jira 課題運用ガイド」を参照してください: <https://lepus.atlassian.net/wiki/spaces/DEVPB/pages/41615364/Jira>。Jira カスタム項目「システム影響範囲」については [課題](./ISSUE.md) を参照してください。
