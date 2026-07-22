# Coding Test Practice

コーディング試験対策用の学習アプリです。

## 目的

業務経験はあるものの、アルゴリズム問題や標準入力形式のコーディング試験に不慣れなエンジニアが、段階的に練習できることを目的としています。

## 技術構成

- Next.js
- TypeScript
- Tailwind CSS
- PostgreSQL
- Prisma
- Auth.js
- Monaco Editor
- Vitest
- Docker
- GitHub Actions

## 開発フェーズ

現在は Phase1 として、基本的な問題演習・コード実行・提出履歴の保存を実装します。

## ローカル開発環境

このプロジェクトは Docker を利用した開発を前提としています。

> **Note**
>
> - ホスト環境で `npm install` を実行する必要はありません。
> - Node.js をローカルへインストールする必要もありません。
> - 依存パッケージは Docker コンテナ内で管理されます。

### 環境変数の設定

`.env.example` をコピーして `.env` を作成してください。

```bash
cp .env.example .env
```

### Docker コンテナの起動

以下のコマンドで Next.js と PostgreSQL のコンテナを起動します。

```bash
docker compose up --build
```

### 起動確認

ブラウザで以下のURLへアクセスしてください。

```text
http://localhost:3000
```

コンテナの状態は、以下のコマンドで確認できます。

```bash
docker compose ps
```

PostgreSQL コンテナが `healthy` と表示されていれば、正常に起動しています。

### Docker コンテナの停止

```bash
docker compose down
```

### 注意事項

- 現在の Dockerfile および `compose.yml` はローカル開発環境向けです。
- `.env` は Git 管理対象外です。
- `.env.example` の認証情報はローカル開発用です。本番環境では使用しないでください。