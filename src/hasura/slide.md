---
marp: true
theme: gaia
paginate: true
---
<!--
paginate: false
-->

<style>
h1 {
  padding-top: 6.5rem;
  text-align: center;
  vertical-align: middle;
}
h2 {
  font-size: 36px;
}
section {
    font-family: "Arial", "Hiragino Maru Gothic ProN";
    font-size: 32px;
    /* padding: 40px; */
}
</style>

# Hasuraについて

---
<!--
paginate: true
-->

## 目次
- 自己紹介
- Hasuraとは
- 実際に動かしてみる

---

# 自己紹介

---

## 自己紹介
- 名前：日髙廉都（ひたかれんと）
- 所属：Integration UNIT1 Team3
- 出身：九州の大分県出身
- 趣味：ボードゲーム、将棋、筋トレ

---

## 発表の経緯
- 佐野市の防災アプリ開発でHasuraを使った
- 短期間でAPIを立てることができた
- 便利なサービスなので多くの人に知って欲しい

---

# Hasuraとは

---

## Hasuraとは
- DBから自動的にGraphQL
サーバを構築するサービス

![bg right 100%](2021-10-10-14-27-22.png)

---

## サポートされているDBとAPI

![bg 70%](2021-10-10-14-31-11.png)

---

## Hasuraのメリット
- GraphQL サーバーを実装する手間が省ける
- 途中から始めはじめやすい＆やめやすい
  - スキーマ情報を解析して自動的にGraphQLサーバを構築してくれるので既存のDBをそのまま使える
  - 途中で辞めたとしてもDBはそのまま使い回すことができる
  - ロックインされづらい
- DBクライアントツールにもなる

---

## Hasuraのデメリット
- 外部のAPIを叩いたり、複雑なロジックが必要な場合はHasuraだけでは対応できない
- 機能が多くて全て理解するのが難しい
  - ドキュメントも全て読むのは結構大変
- レスポンスの形式を柔軟に指定できない場合がある

---

## Hasuraが向いているシステム
<!-- - 複雑なロジックが不要な参照系のAPIが必要なサービス
  - 複雑なロジックの処理を行うためには、別途APIを立てる必要があり、Hasuraのメリットを活かしづらい -->
- 単純なDB操作飲み必要なシンプルなサービス
  - Hasuraだけで完結するなら実装コストは低い
- マイクロサービス
  - Dockerイメージが配布されているので始めやすい
  - 複雑なロジックが必要なアプリではHasuraだけで完結するのは難しいので、複雑なロジックの処理を行うためのAPIを別途立てる必要がある

---

# 実際に動かしてみる

---

## docker-compose.yamlを用意する
```json

version: '3.6'
services:
  postgres:
    image: postgres:12
    restart: always
    volumes:
    - db_data:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: postgrespassword
  graphql-engine:
    image: hasura/graphql-engine:v2.0.9
    ports:
    - "8080:8080"
    depends_on:
    - "postgres"
    restart: always
    environment:
      HASURA_GRAPHQL_METADATA_DATABASE_URL: postgres://postgres:postgrespassword@postgres:5432/postgres
      PG_DATABASE_URL: postgres://postgres:postgrespassword@postgres:5432/postgres
      HASURA_GRAPHQL_ENABLE_CONSOLE: "true" # set to "false" to disable console
      HASURA_GRAPHQL_DEV_MODE: "true"
      HASURA_GRAPHQL_ENABLED_LOG_TYPES: startup, http-log, webhook-log, websocket-log, query-log
volumes:
  db_data:

# 参考： https://github.com/hasura/graphql-engine/blob/stable/install-manifests/docker-compose/docker-compose.yaml
```

---
## 起動
```bash
$ docker-compose up -d
```

---
## データベースと接続する
![bg 75%](2021-10-12-01-21-44.png)

---
## テーブル作成
```sql
create table users (
  id SERIAL PRIMARY KEY,
  name TEXT
);
```

---

## データ投入
```sql
insert into users (id, name) values (1, 'ウフル太郎');
insert into users (id, name) values (2, 'ウフル次郎');
insert into users (id, name) values (3, 'ウフル花子');
```

---

## クエリを叩く
```graphql
query {
  users {
    id
    name
  }
}
```

---

## カラムを追加する
![bg 75%](2021-10-12-01-42-25.png)

---

## クエリを叩く
```graphql
query {
  users {
    id
    name
    gender
  }
}
```

---