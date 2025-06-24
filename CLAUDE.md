# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト概要

これは Rails 7.1.3 ベースのブログ API プロジェクトです。

- Ruby バージョン: 3.3.2
- Rails バージョン: 7.1.3
- データベース: SQLite3
- Webサーバー: Puma

## 開発環境のセットアップ

### 初期セットアップ
```bash
bin/setup
```

このコマンドで以下が実行されます：
- Bundler のインストール
- 依存関係のインストール（bundle install）
- データベースの準備（db:prepare）
- ログとテンポラリファイルのクリア
- アプリケーションサーバーの再起動

### よく使うコマンド

#### サーバーの起動
```bash
bin/rails server
# または
bin/rails s
```

#### コンソールの起動
```bash
bin/rails console
# または
bin/rails c
```

#### データベース関連
```bash
# データベースの作成
bin/rails db:create

# マイグレーションの実行
bin/rails db:migrate

# データベースのリセット（削除→作成→マイグレーション→シード）
bin/rails db:reset

# シードデータの投入
bin/rails db:seed
```

#### テストの実行
```bash
# すべてのテストを実行
bin/rails test

# 特定のテストファイルを実行
bin/rails test test/models/user_test.rb

# 特定のテストメソッドを実行
bin/rails test test/models/user_test.rb:5
```

## アーキテクチャ概要

### ディレクトリ構造

- `app/` - アプリケーションのメインコード
  - `controllers/` - APIエンドポイントのコントローラー
  - `models/` - ActiveRecordモデル
  - `jobs/` - バックグラウンドジョブ
  - `mailers/` - メール送信クラス
  - `channels/` - Action Cable チャンネル

- `config/` - 設定ファイル
  - `routes.rb` - ルーティング定義
  - `database.yml` - データベース設定
  - `initializers/` - アプリケーション初期化設定
    - `cors.rb` - CORS設定（現在はコメントアウト）

- `db/` - データベース関連
  - `migrate/` - マイグレーションファイル
  - `seeds.rb` - シードデータ

### API モード

このプロジェクトは Rails API モードで作成されており、以下の特徴があります：

- View 層がない（API専用）
- ApplicationController が ActionController::API を継承
- 不要なミドルウェアが除外されている
- JSON レスポンスに最適化

### データベース

開発環境とテスト環境では SQLite3 を使用：
- 開発環境: `storage/development.sqlite3`
- テスト環境: `storage/test.sqlite3`
- 本番環境: `storage/production.sqlite3`

### Docker サポート

Dockerfileが含まれており、本番環境向けのコンテナ化が可能：
- Ruby 3.3.2 slim イメージを使用
- マルチステージビルドで最適化
- 非rootユーザー（rails）で実行
- ポート 3000 を公開

## 開発時の注意点

1. **API エンドポイントの追加**: `config/routes.rb` に新しいルートを定義
2. **モデルの追加**: `bin/rails generate model ModelName` を使用
3. **コントローラーの追加**: `bin/rails generate controller ControllerName` を使用
4. **CORS の有効化**: 必要に応じて `Gemfile` の `rack-cors` をアンコメントし、`config/initializers/cors.rb` を設定

## 現在の状態

- まだエンドポイントは定義されていません（ヘルスチェック `/up` のみ）
- モデルやコントローラーは未実装
- 基本的な Rails API の骨組みのみが存在

## 重要事項
- 今後すべての回答は日本語で回答してください
- Githubでのレビューや進捗状況も日本語で回答してください