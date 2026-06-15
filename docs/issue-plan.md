# Issue計画

## Issue 1: リポジトリ作成・Next.js初期セットアップ

### 概要

コーディング試験対策アプリの開発基盤を構築する。

### 作業内容

- Githubリポジトリを作成する
- Next.js を導入する
- TypeScript を導入する
- Tailwind CSS を導入する
- App Router 構成にする
- `src` ディレクトリ構成にする
- `docs` ディレクトリを作成する
- 要件定義書を配置する
- 基本設計書を配置する
- Issue計画書を配置する

### 完了条件

- `npm run dev` で起動できる
- `http://localhost:3000\` で画面を確認できる
- `docs/product-requirements.md` が存在する
- `docs/basic-design.md` が存在する
- `docs/issue-plan.md` が存在する
- main ブランチに初回コミットがpushされている

---

## Issue 2: Docker開発環境構築

### 概要

ローカル開発環境を Docker化する。

### 作業内容

- NExt.js 用コンテナを作成する
- PostgreSQL コンテナを作成する
- `docker-compose.yml` を作成する
- 環境変数を整理する
- ローカル起動確認を行う

#### 完了条件

- `docker compose up` で起動できる
- Next.js にアクセスできる
- PostgreSQL に接続できる

---

## Issue 3: Prisma導入・DB接続

### 概要

Prisma を導入し、PostgreSQL と接続する。

### 作業内容

- Prisma を導入する
- PostgreSQL 接続設定を行う
- `prisma/schema.prisma` を作成する
- 初回 migration を実行する
- Prisma Client を生成する
- Prisma Client の共通接続処理を作成する

### 完了条件

- Prisma Client が利用できる
- migration が成功する
- DB接続確認ができる

---

### Issue 4: Auth.js導入

### 概要

認証基盤を構築する。

### 作業内容

- Auth.jsを導入する
- 認証関連テーブルを作成する
- ログイン機能を実装する
- ログアウト機能を実装する
- セッション管理を実装する

### 完了条件

- ログインできる
- ログアウトできる
- セッションが保持される

---

## Issue 5: 問題・テストケース実装

### 概要

問題管理機能の基盤を実装する。

### 作業内容

- `problems` テーブルを作成する
- `test_cases` テーブルを作成する
- Prismaモデルを作成する
- 問題一覧APIを実装する
- 問題詳細APIを実装する
- seed データを作成する

### API

```http
GET /api/problems
GET /api/problems/{problemId}
```

### 完了条件

- 問題一覧を取得できる
- 問題詳細を取得できる
- サンプルケースのみ返却される
- seed データで動作確認できる

---

## Issue 6: 問題一覧・問題詳細画面実装

### 概要

問題閲覧画面を実装する。

### 作業内容

- 問題一覧画面を作成する
- 問題詳細画面を作成する
- 問題文を表示する
- 入力形式を表示する
- 出力形式を表示する
- 制約を表示する
- サンプルケースを表示する
- 難易度を表示する

### 画面

```txt
/problems
/problems/[problemId]
```

### 完了条件

- 問題一覧が表示される
- 問題詳細が表示される
- サンプルケースが表示される

---

### Issue 7: Monaco Editor導入

### 概要

ブラウザ上でコードを記述できるエディタを実装する。

### 作業内容

- Monaco Editor を導入する
- 言語選択機能を実装する
- PHP の初期コードテンプレートを表示する
- TypeScript の初期コードテンプレートを表示する
- エディタ状態管理を実装する

### 対応言語

- PHP
- TypeScript

### 完了条件

- Monaco Editor が表示される
- 言語切替ができる
- コード入力ができる

---

## Issue 8: PHP / TypeScript 実行基盤の試作

### 概要

提出コードを実行するための基盤を試作する。

### 作業内容

- PHP 実行環境を構築する
- TypeScript 実行環境を構築する
- 標準入力を渡せるようにする
- 標準出力を取得する
- 期待出力と実際の出力を比較する
- タイムアウト制御を実装する
- 実行エラーを扱う

### 判定ステータス

```txt
accepted
wrong_answer
runtime_error
timeout
internal_error
```

### 完了条件

- PHPコードが実行できる
- TypeScriptコードが実行できる
- 標準入力を渡せる
- 判定結果を返却できる

---

## Issue 9: 提出・実行結果保存機能の実装

### 概要

ユーザーの提出コードと採点結果を保存する。

### 作業内容

- `submissions` テーブルを作成する
- `execution_results` テーブルを作成する
- 提出APIを実装する
- 実行結果保存処理を実装する
- 提出履歴取得APIを実装する

### API

```http
POST /api/submissions
GET /api/submissions
```

### 完了条件

- コードを提出できる
- 提出内容が保存される
- 実行結果が保存される
- 提出履歴が取得できる

---

## Issue 10: ダッシュボード・提出履歴画面実装

### 概要

ユーザーの学習状況と提出履歴を表示する。

### 作業内容

- ダッシュボード画面を作成する
- 提出履歴一覧を表示する
- 正解済み問題数を表示する
- 直近の提出履歴を表示する

### 画面

```txt
/dashboard
```

### 完了条件

- ログインユーザーの提出履歴が表示される
- 正解済み問題数が表示される
- 直近の提出状況が確認できる

---

## Issue 11: Vitest導入・テスト基盤構築

### 概要

テスト実行環境を構築する。

### 作業内容

- Vitest を導入する
- テスト用設定を作成する
- ユーティリティ関数のテストを追加する
- 判定ロジックのテストを追加する

### 完了条件

- `npm run test` でテストが実行できる
- 判定ロジックのテストが通る

---

## Issue 12: Github Actions CI導入

### 概要

Github Actions によるCIを構築する。

### 作業内容

- Github Actions workflow を作成する
- lint を実行する
- typecheck を実行する
- test を実行する

### 完了条件

- Pull Request 時にCIが実行される
- lint が成功する
- typecheck が成功する
- test が成功する
