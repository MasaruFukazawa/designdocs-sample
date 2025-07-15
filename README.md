# Design Documents Sample

このリポジトリは、ソフトウェア設計ドキュメントの作成と管理における実験的なアプローチのサンプルです。

## 概要

設計ドキュメントを「コードと同様に扱う」というアプローチを実践するためのサンプルプロジェクトです。
以下のような課題に対する一つの解決策を提案しています：

- ドキュメントの更新が実装に追いつかない
- バイナリ形式のドキュメントでは変更履歴の追跡が困難
- チーム内での設計の共有と理解が属人化しやすい
- レビューとフィードバックのプロセスが不明確
- ドキュメントとコードの整合性維持が困難

## 特徴

1. プレーンテキストベースのドキュメント管理
   - reStructuredText形式での記述
   - Gitによるバージョン管理
   - Mermaidによる図表の自動生成

2. GitHubを中心としたワークフロー
   - Pull Requestベースのレビュープロセス
   - コードとドキュメントの同時レビュー
   - 変更履歴の追跡と管理

## 必要なツール

- Python 3.8以上
- Cursor（AI エディタ）
- Git
- Github MCP Server

## セットアップ

```bash
# リポジトリのクローン
git clone https://github.com/MasaruFukazawa/designdocs-sample.git
cd designdocs-sample

# 依存パッケージのインストール
pip install -r requirements.txt

# ドキュメントのプレビュー開始
sphinx-autobuild docs/design/source docs/design/build/html
```

ブラウザで http://127.0.0.1:8000 にアクセスすると、生成されたドキュメントを確認できます。

## プロジェクト構造

```
designdocs-sample/
├── docs/                 # Sphinxドキュメント
│   └── design/
│       └── source/      # 設計ドキュメント
│           ├── user_story/  # ユーザーストーリー
│           ├── usecase/     # ユースケース
│           ├── database/    # データベース設計
│           └── mail/        # メール定義
├── .cursor/             # 設計ルール
│   └── rules/          
└── requirements.txt     # 依存パッケージ
```

## 設計ドキュメントの作成手順

1. GitHubでIssueを作成
2. Cursorを使用して設計ドキュメントを生成
3. Sphinxでドキュメントをビルド
4. Pull Requestでレビュー

## ライセンス

このプロジェクトはMITライセンスの下で公開されています。

## 作者

- Masaru Fukazawa

## 参考資料

- [Sphinx](https://www.sphinx-doc.org/)
- [reStructuredText](https://docutils.sourceforge.io/rst.html)
- [Mermaid](https://mermaid-js.github.io/mermaid/)
- [Github MCP Server](https://github.com/github/github-mcp-server) 
