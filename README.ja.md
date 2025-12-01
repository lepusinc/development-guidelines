# LEPUS 開発ガイドライン ![version](https://img.shields.io/badge/バージョン-1.0-blue)

🇯🇵 **日本語** | 🇺🇸 [English](README.md)

このリポジトリには、**レプスグループ エンジニアリングチームの開発ガイドラインとベストプラクティス** が含まれています。  
コードレビュー、プルリクエスト、テスト、チームコラボレーションに関する原則を定義しています。

---

## 📖 利用可能なドキュメント

- [レビューポリシー](docs/ja/REVIEW_POLICY.md)
  - [PR 単位の原則](docs/ja/REVIEW_POLICY.md#pr単位の原則)
  - [レビューの心得](docs/ja/REVIEW_POLICY.md#-レビューの心得)
  - [簡易チェックリスト](docs/ja/REVIEW_POLICY.md#-簡易チェックリスト)
  - [レベル判定ガイド](docs/ja/REVIEW_POLICY.md#-レベル判定ガイド)
  - [RACI マトリクス](docs/ja/REVIEW_POLICY.md#-raciマトリクス)
- [プルリクエストテンプレート](docs/ja/PULL_REQUEST_TEMPLATE.md)
  - [テスト証跡](docs/ja/PULL_REQUEST_TEMPLATE.md#-テスト証跡)
  - [チェックリスト](docs/ja/PULL_REQUEST_TEMPLATE.md#-チェックリスト)
- [サードパーティパッケージ選定基準](docs/ja/DEPENDENCIES_GUIDELINES.md)
  - [目的](docs/ja/DEPENDENCIES_GUIDELINES.md#目的)
  - [信頼性](docs/ja/DEPENDENCIES_GUIDELINES.md#1-信頼性popularity--ecosystem-trust)
  - [セキュリティ](docs/ja/DEPENDENCIES_GUIDELINES.md#2-セキュリティsecurity--compliance)
  - [コード品質](docs/ja/DEPENDENCIES_GUIDELINES.md#3-コード品質code-quality)
  - [プロジェクト継続性](docs/ja/DEPENDENCIES_GUIDELINES.md#4-プロジェクト継続性project-sustainability)
  - [ライセンス](docs/ja/DEPENDENCIES_GUIDELINES.md#5-ライセンスlicense-compliance)
  - [選定プロセス](docs/ja/DEPENDENCIES_GUIDELINES.md#6-選定プロセスevaluation-process)
  - [継続的チェック](docs/ja/DEPENDENCIES_GUIDELINES.md#7-継続的チェックongoing-review)
  - [例外ルール](docs/ja/DEPENDENCIES_GUIDELINES.md#8-例外ルールexception-rules)

---

## 🏗️ 対象範囲

このガイドラインには以下が含まれます：

- コードレビューの責務と原則
- プルリクエストのテンプレートとワークフロー
- テストと証跡に関する必須要件
- 効果的なコラボレーションのためのコミュニケーション心得

---

## 📂 ディレクトリ構成

```
.
├── docs/
│   ├── en/               # 英語ドキュメント
│   └── ja/               # 日本語ドキュメント
├── LICENSE               # リポジトリのライセンス
├── README.md             # 英語版 README
└── README.ja.md          # このファイル (日本語版 README)
```

---

## 🤝 貢献について

改善提案はいつでも歓迎します。  
ただし、**レプスグループ 外部からのプルリクエストは受け付けていません**。

詳細は [CONTRIBUTING.md](CONTRIBUTING.md) を参照してください。  
社内メンバーは `main` ブランチに向けてプルリクエストを作成してください。

---

## 📜 ライセンス

このリポジトリは [MIT License](LICENSE) のもとで公開されています。

---

© レプスグループ – エンジニアリングチーム
