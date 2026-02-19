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
<!-- Do not delete comment lines in this template. -->
<!-- このテンプレートのコメント行は削除しない。 -->
<!-- Do not modify or remove existing option texts. -->
<!-- テンプレート内の既存選択肢テキストの変更および選択肢の削除はしない。 -->
<!-- Toggle checklist states (on/off) as appropriate. -->
<!-- チェックの on / off は適切に行う。 -->
<!-- Adding options is allowed; append "（追加）" to added option text so additions are identifiable. -->
<!-- 選択肢の追加は許可するが、追加したことが分かるよう選択肢のテキストに「（追加）」サフィックスを付与する。 -->

# Pull Request (Release / Ops)

> Use this template for release/operational coordination changes where the primary value is process/runbook/workflow updates.
>
> このテンプレートは、プロセス・Runbook・ワークフロー更新など、リリース/運用調整が主目的の変更に使用。

## 📝 Background & Purpose / 背景・目的

> Describe why this operational change is needed.
>
> この運用変更が必要な理由を記載。

-

---

## 🔄 Changes / 変更内容

> List key changes and affected files/workflows.
>
> 主な変更点と影響するファイル/ワークフローを記載。

-

---

## 📋 Nature of this PR / このPRの性質

> Select all applicable categories.
>
> 該当するカテゴリをすべて選択。
> For release merge PRs, summarize commit scope and release-note updates rather than repeating implementation details.
>
> リリースマージPRでは、実装詳細の再説明よりも、取り込みコミット範囲とリリースノート更新内容を要約。

<!-- prettier-ignore -->
- [ ] Release merge (`release` → `main`)  
  リリースマージ（`release` → `main`）
- [ ] GitHub Actions change  
  GitHub Actions変更
- [ ] Local developer setup update  
  ローカル開発環境の更新
- [ ] Documentation update  
  ドキュメント更新

---

## 🖥️ Verification / 動作確認

> Describe what was validated and provide light evidence (log, screenshot, CI run link).
> Examples: CI run link, local verification steps/results, screenshots, logs.
>
> 何を確認したかと証跡（ログ・スクリーンショット・CI実行リンク）を簡潔に記載。
> 例: CI実行リンク、ローカル確認手順/結果、スクリーンショット、ログ。

---

## 🔍 Checklist / チェックリスト

<!-- prettier-ignore -->
- [ ] Nature of this PR is selected  
  「このPRの性質」を選択した
- [ ] Changes are documented  
  変更内容を記載した
- [ ] Verification results are documented with evidence/link  
  動作確認結果を証跡/リンク付きで記載した
- [ ] Confirmed this PR is focused on release/operational coordination  
  このPRがリリース/運用調整を主目的とすることを確認した
- [ ] Related documentation is updated if needed  
  必要に応じて関連ドキュメントを更新した

---

> 🛈 For detailed review criteria, please refer to [REVIEW_POLICY.md](../../docs/en/REVIEW_POLICY.md)  
> 🛈 詳細なレビュー基準については [REVIEW_POLICY.md](../../docs/ja/REVIEW_POLICY.md) を参照してください
