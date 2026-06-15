# 基本設計書

## 1. 概要

本ドキュメントは、コーディング試験対策アプリの基本設計を定義する。

本アプリは、ユーザーがブラウザ上でコーディング問題を解き、標準入力形式のテストケースに対してコードを実行し、
採点結果を確認できる学習アプリである。

Phase1では、初歩的なコーディング試験対策に必要な最小限の機能を実装する。

---

## 2. リポジトリ名

リポジトリ名は以下とする。

```txt
coding-test-practice
```
---

## 3. 技術構成

| 項目 | 採用技術 |
| --- | --- |
| フロントエンド | Next.js |
| 言語 | TypeScript |
| スタイリング | Tailwind CSS |
| バックエンド | Next.js Route Handlers |
| データベース | PostgreSQL |
| ORM | Prisma |
| 認証 | Auth.js |
| コードエディタ | Monaco Editor |
| テスト | Vitest |
| 開発環境 | Docker |
| CI | Github Actions |

---

## 4. 技術選定理由

### 4.1 Next.js

フロントエンドとバックエンドAPIを同一プロジェクト内で実装できるため採用する。

Phase1では開発速度を重視し、Express や NestJS などの別バックエンドは作成しない。

App Router と Route Handlers を利用して、画面表示とAPI実装を同一リポジトリ内で管理する。

### 4.2 TypeScript

型安全な開発を実現するため採用する。

問題、提出、実行結果などのデータ構造を型として管理し、実装ミスを減らすことを目的とする。

### 4.3 Tailwind CSS

画面開発速度を向上させるため採用する。

Phase1ではデザインよりも機能実装を優先する。

### 4.4 PostgreSQL

問題、提出、実行結果などのリレーショナルデータを扱うため採用する。

### 4.5 Prisma

TypeSctiptとの相性が良く、型安全なDBアクセスを実現できるため採用する。

### 4.6 Auth.js

Next.jsとの親和性が高く、認証機能を実装しやすいため採用する。

### 4.7 Monaco Editor

VS Codeに近い操作感を提供できるため採用する。

コーディング試験対策アプリとの相性が良い。

### 4.8 Vitest

TypeScriptとの親和性が高く、高速にテストを実行できるため採用する。

### 4.9 Docker

開発環境を統一するため採用する。

### 4.10 Github Actions

Lint、TypeCheck、Testを自動実行するため採用する。

---

## 5. アーキテクチャ方針

### 5.1 システム構成

本アプリは Next.js フルスタック構成を採用する。

```txt
ブラウザ
　　↓
Next.js
├─ 画面
├─ API(Route Handlers)
├─ Auth.js
├─ Prisma
　　↓
PostgreSQL
```

### 5.2 API実装方針

通常のAPIは Next.js Route Handlers を利用して実装する

対象:

- 問題一覧取得
- 問題詳細取得
- 提出履歴取得
- 認証関連処理

### 5.3 コード実行方針

ユーザーが提出したコードは Next.js 本体では実行しない。

理由:

- 任意コード実行によるセキュリティリスクを避けるため
- タイムアウト制御を行うため
- 実行環境を分離するため

コード実行は専用の Docker コンテナ内で行う。

```txt
Next.js
　　↓
コード実行依頼
　　↓
Docker実行環境
├─ PHP
├─ TypeScript
```

### 5.4 設計方針

Phase1では、DDDの考え方を参考にしつつ、過度に複雑なレイヤー分割や戦術的設計パターンの導入は避ける。

本プロジェクトでは、まず以下を重視する。

- 業務概念を中心に設計する
- ユビキタス言語を定義する
- ドメインモデルの責務を明確にする
- 機能単位でコードを整理する
- 将来的に境界づけられたコンテキストを分けられる構造にする

Phase1では、Entity、Value Object、Repository、Domain Service などの戦術的設計パターンを形式的に導入することを目的にしない。

ただし、問題管理、提出、採点、ユーザー管理など、将来的に関心ごとが分かれる領域を意識して設計する。

将来的なコンテキスト候補は以下とする。

- 学習コンテキスト
- 提出・採点コンテキスト
- ユーザー管理コンテキスト

---

### 6. ディレクトリ構成

```txt
coding-test-practice/
├── docs/
│   ├── product-requirements.md
│   ├── basic-design.md
│   ├── issue-plan.md
├── prisma/
│   ├── schema.prisma
│   ├── seed.ts
├── src/
│   ├── app/
│   ├── components/
│   ├── features/
│   ├── lib/
│   ├── constants/
│   ├── types/
├── tests/
├── docker/
├── docker-compose.yml
├── package.json
├── README.md
```

### 6.1 src/app

Next.js のルーティングを管理する。

主な責務:

- 画面ルーティング
- APIルーティング
- レイアウト管理

### 6.2 src/components

再利用可能なUIコンポーネントを管理する。

例:

- Button
- Card
- Header
- Editor

### 6.3 src/features

機能単位でコードを管理する。

例:

```txt
features/problems
features/submissions
features/auth
```

### 6.4 src/lib

アプリ全体で利用する共通処理を管理する。

例:

- prisma.ts
- auth.ts
- utils.ts

### 6.5 prisma

Prismaスキーマと初期データを管理する。

---

## 7. モデリング方針

本アプリでは、ユーザーがコーディング問題を解き、提出したコードを採点する業務を中心にモデリングを行う。

モデリングとは、システムで扱う重要な概念と、それらの関係性および責務を整理することである。

本アプリの主要な業務フローは以下である。

```txt
ユーザーが問題を選択する
↓
コードを記述する
↓
コードを提出する
↓
テストケースで実行する
↓
結果を確認する
```

上記の業務フローから主要なドメインを抽出する。

---

## 8. ドメインモデル

### 8.1 User

アプリを利用するユーザー。

主な責務:

- 認証
- 提出履歴の管理

### 8.2 Problem

コーディング問題。

主な責務

- 問題文を保持する
- 難易度を保持する
- テストケースを保持する

### 8.3 TestCase

問題に紐づくテストケース。

主な責務:

- 入力値を保持する
- 期待出力を保持する
- 公開／非公開を管理する

### 8.4 Submission

ユーザーが提出したコード。

主な責務:

- 提出コードを保持する
- 使用言語を保持する
- 採点結果を保持する

### 8.5 ExecutionResult

実行結果。

主な責務:

- 判定結果を保持する
- 実行時間を保持する
- エラー内容を保持する

### 8.6 ProgrammingLanguage

対応言語。

Phase1では以下を対象とする。

```txt
PHP
TypeScript
```

---

## 9. ユビキタス言語

本プロジェクトでは以下の用語を共通言語として利用する。

| 用語 | 意味 |
| -------- | -------- |
| 問題 | ユーザーが解くコーディング問題 |
| 問題文 | 問題の説明 |
| テストケース | 入力と期待出力の組み合わせ |
| サンプルケース | ユーザーに公開するテストケース |
| 非公開ケース | 採点時のみ利用するテストケース |
| 提出 | ユーザーがコードを送信すること |
| 実行 | コードを動作させること |
| 採点 | 出力結果を比較し正誤判定すること |
| 実行結果 | 各テストケースの判定結果 |
| 標準入力 | プログラムへ渡される入力 |
| 標準出力 | プログラムが出力する値 |
| 対応言語 | ユーザーが選択する言語 |
| 難易度 | 問題レベル |
| 提出履歴 | ユーザーの過去提出情報 |

## 10. ER図

Phase1では、以下のエンティティを中心にDB設計を行う。

```mermaid
erDiagram
    users ||--o{ submissions : "has"
    problems ||--o{ test_cases : "has"
    problems ||--o{ submissions : "has"
    subimissions ||--o{ execution_results : "has"
    test_cases ||--o{ execution_results : "used_by"

    users {
      string id PK
      string name
      string email
      datetime email_verified
      string image
      datetime created_at
      datetime updated_at
    }

    problems {
      string id PK
      string title
      string slug
      text description
      string difficulty
      text input_format
      text output_format
      text constraints
      datetime created_at
      datetime updated_at
    }

    test_cases {
      string id PK
      string problem_id FK
      text input
      text expected_output
      boolean is_sample
      int sort_order
      datetime created_at
      datetime updated_at
    }

    submissions {
      string id PK
      string user_id FK
      string problem_id FK
      string language
      text source_code
      string status
      datetime created_at
      datetime updated_at
    }

    execution_results {
      string id PK
      string submission_id FK
      string test_case_id FK
      string status
      text actual_output
      text expected_output
      int execution_time_ms
      int memory_kb
      text error_message
      datetime created_at
    }
  }
  ```

---

## 11. テーブル設計

### 11.1 users

Auth.js で利用するユーザーテーブル。

| カラム | 型 | 必須 | 説明 |
| --- | --- | --- | --- |
| id | String | yes | ユーザーID |
| name | String | no | ユーザー名 |
| email | String | yes | メールアドレス |
| email_verified | DateTime | no | メール認証日時 |
| image | String | no | プロフィール画像 |
| created_at | DateTime | yes | 作成日時 |
| updated_at | DateTime | yes | 更新日時 |

### 11.2 problems

コーディング問題を管理するテーブル。

| カラム | 型 | 必須 | 説明 |
| --- | --- | --- | --- |
| id | String | yes | 問題ID |
| title | String | yes | 問題タイトル |
| slug | String | yes | URL用識別子 |
| description | Text | yes | 問題文 |
| difficulty | String | yes | 難易度 |
| input_format | Text | yes | 入力形式 |
| output_format | Text | yes | 出力形式 |
| constraints | Text | yes | 制約 |
| created_at | DateTime | yes | 作成日時 |
| updated_at | DateTime | yes | 更新日時 |

#### 備考

- `slug` は `/problems/a-plus-b` のようなURLで利用する
- `difficulty` は `easy` / `medium` / `hard` を想定する
- Phase1では `editorial`、`hint`、`tag`、`category` は持たない

### 11.3 test_cases

問題に紐づくテストケースを管理するテーブル。

| カラム | 型 | 必須 | 説明 |
| --- | --- | --- | --- |
| id | String | yes | テストケースID |
| problem_id | String | yes | 問題ID |
| input | Text | yes | 標準入力 |
| expected_output | Text | yes | 期待出力 |
| is_sample | Boolean | yes | サンプルケースかどうか |
| sort_order | Int | yes | 表示順 |
| created_at | DateTime | yes | 作成日時 |
| updated_at | DateTime | yes | 更新日時 |

#### 備考

- `isSample = true` の場合、問題詳細画面に表示する
- `isSample = false` の場合、採点時のみ利用する
- 1つの問題は複数のテストケースを持つ

### 11.4 submissions

ユーザーの提出コードを管理するテーブル。

| カラム | 型 | 必須 | 説明 |
| --- | --- | --- | --- |
| id | String | yes | 提出ID |
| user_id | String | yes | ユーザーID |
| problem_id | String | yes | 問題ID |
| language | String | yes | 使用言語 |
| source_code | Text | yes | 提出コード |
| status | String | yes | 提出ステータス |
| created_at | DateTime | yes | 作成日時 |
| updated_at | DateTime | yes | 更新日時 |

#### 備考

- `language` は `php` / `typescript` を想定する
- `status` は提出全体の採点結果を表す
- 1ユーザーは複数の提出を持つ
- 1問題に対して複数回提出できる

### 11.5 execution_results

各テストケースに対する実行結果を管理するテーブル。

| カラム | 型 | 必須 | 説明 |
| --- | --- | --- | --- |
| id | String | yes | 実行結果ID |
| submission_id | String | yes | 提出ID |
| test_case_id | String | yes | テストケースID |
| status | String | yes | 実行結果ステータス |
| actual_output | Text | no| 実際の出力 |
| expected_output | Text | no | 期待出力 |
| execution_time_ms | Int | no | 実行時間 |
| memory_kb | Int | no | メモリ使用量 |
| error_message | Text | no | エラー内容 |
| created_at | DateTime | yes | 作成日時 |

#### 備考

- 1つの提出は複数の実行結果を持つ
- テストケースごとに `accepted` / `wrong_answer` / `runtime_error` / `timeout` を判定する
- `actual_output` はユーザーコードの出力結果
- `expected_output` は採点時点の期待出力を保存する

---

## 12. 画面設計

### 12.1 トップ画面

パス:

```txt
/
```

役割:

- サービス概要を表示する
- ログイン導線を表示する
- 問題一覧への導線を表示する

### 12.2 ログイン画面

パス:

```txt
/auth/signin
```

役割:

- ユーザーがログインできる
- 認証後、問題一覧またはダッシュボードへ遷移する

### 12.3 問題一覧画面

パス:

```txt
/problems
```

表示内容:

- 問題タイトル
- 難易度
- 解答状況
- 問題詳細へのリンク

利用API:

```http
GET /api/problems
```

### 12.4 問題詳細・解答画面

パス:

```txt
/problems/[problemId]
```

表示内容:

- 問題文
- 入力形式
- 出力形式
- 制約
- サンプルケース
- 言語選択
- Monaco Editor
- 提出ボタン
- 採点結果

利用API:

```http
GET /api/problems/{problemId}
POST /api/submissions
```

### 12.5 ダッシュボード画面

パス:

```txt
/dashboard
```

表示内容:

- 解いた問題数
- 正解済み問題数
- 直近の提出履歴
- 学習進捗

利用API:

```http
GET /api/submissions
```

---

## 13.API設計

### 13.1 問題一覧取得API

```http
GET /api/problems
```

問題一覧を取得する。

#### 認証

任意。

#### レスポンス例

```json
[
  {
    "id": "problem_001",
    "title": "A+B",
    "slug": "a-plus-b",
    "difficulty": "easy",
    "solved": true
  }
]
```

### 13.2 問題詳細取得API

```http
GET /api/problems/{problemId}
```

問題詳細とサンプルケースを取得する。

#### 認証

任意。

#### レスポンス例

```json
{
  "id": "problem_001",
  "title": "A+B",
  "slug": "a-plus-b",
  "description": "2つの整数A,Bが与えられます。A+B を出力してください。",
  "difficulty": "easy",
  "inputFormat": "A B",
  "outputFormat": "A+B の結果を1行で出力してください。",
  "constraints": "1 <= A, B <= 100",
  "sampleTestCases": [
    {
      "id": "test_case_001",
      "input": "1 2",
      "expectedOutput": "3"
    }
  ]
}
```

### 13.3 コード提出API

```http
POST /api/submissions
```

ユーザーが提出したコードを保存し、テストケースに対して採点する。

#### 認証

必須。

#### リクエスト例

```json
{
  "problemId": "problem_001",
  "language": "php",
  "sourceCode": "<?php\n[$a, $b] = array_map('intval', explode('', trim(fgets(STDIN))));\necho $a + $b . PHP_EOL;"
}
```

#### レスポンス例

```json
{
  "submissionId": "submission_001",
  "status": "accepted",
  "results": [
    {
      "testCaseId": "test_case_001",
      "status": "accepted",
      "actualOutput": "3",
      "expectedOutput": "3",
      "executionTimeMs": 10,
      "memoryKb": 1024
    }
  ]
}
```

### 13.4 提出履歴取得API

```http
GET /api/submissions
```

ログインユーザーの提出履歴を取得する。

#### 認証

必須。

#### クエリパラメータ

| パラメータ | 必須 | 説明 |
| --- | --- | --- |
| problemId | no | 指定した問題の提出履歴のみ取得する

#### レスポンス例

```json
[
  {
    "id": "submission_001",
    "problemId": "problem_001",
    "problemTitle": "A+B",
    "language": "php",
    "status": "accepted",
    "createdAt": "2026-06-12T10:00:00.000Z"
  }
]
```

---

## 14. API設計方針

### 14.1 認証方針

問題一覧と問題詳細は、未ログインでも閲覧可能とする。

コード提出と提出履歴取得は、ログイン必須とする。

理由:

- 未ログインユーザーでもアプリの内容を確認できるようにするため
- 提出履歴はユーザーに紐づく情報のため、認証が必要であるため

### 14.2 サンプルケースと非公開ケースの扱い

問題詳細取得APIでは、`is_sample = true` のテストケースのみ返却する。

`is_sample = false` の非公開ケースは、採点時のみ利用し、APIレスポンスに含めない。

### 14.3 ステータス

提出結果と実行結果では、以下のステータスを利用する。

| ステータス | 意味 |
| --- | --- |
| accepted | 正解 |
| wrong_answer | 不正解 |
| runtime_error | 実行時エラー |
| timeout | 実行時間超過 |
| internal_error | システム内部エラー |

### 14.4 対応言語

Phase1では以下の言語に対応する。

| 値 | 言語 |
| --- | --- |
| php | PHP |
| typescript | TypeScript |