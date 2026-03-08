# 設計ドキュメント リポジトリ

Webアプリケーションの設計ドキュメントを管理するリポジトリです。

## 📁 ディレクトリ構成

```
docs/
├── system-design/       # システム設計書
├── sequence-diagrams/   # シーケンス図
├── er-diagrams/         # ER図
└── api-specs/           # API仕様書
```

## 📄 ドキュメント一覧

| ドキュメント | パス | 説明 |
|---|---|---|
| システム設計書 | [docs/system-design/](./docs/system-design/) | アーキテクチャ・コンポーネント設計 |
| シーケンス図 | [docs/sequence-diagrams/](./docs/sequence-diagrams/) | 処理フローのシーケンス図（Mermaid） |
| ER図 | [docs/er-diagrams/](./docs/er-diagrams/) | データベース設計（Mermaid） |
| API仕様書 | [docs/api-specs/](./docs/api-specs/) | REST API仕様（OpenAPI） |

## 🛠️ ツール・記法

- **シーケンス図 / ER図**: [Mermaid](https://mermaid.js.org/) 記法
- **API仕様書**: [OpenAPI 3.0](https://swagger.io/specification/) (YAML)
- **システム設計書**: Markdown

## 🔄 ドキュメント更新ルール

1. ブランチを切って作業する（例: `docs/update-auth-sequence`）
2. プルリクエストでレビューを依頼する
3. レビュー承認後にマージする

## 📝 Mermaid プレビュー

GitHubはMermaid記法をネイティブでレンダリングします。  
ローカルで確認する場合は [Mermaid Live Editor](https://mermaid.live/) を利用してください。
