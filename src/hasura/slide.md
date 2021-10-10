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

# huskyについて

---
<!--
paginate: true
-->

## 目次
- 自己紹介
- huskyとGit hooksについて
- husky v4とhusky v5以降の違い
- husky以外の選択肢
- まとめ

---

# 自己紹介

---

## 自己紹介
- 名前：日髙廉都（ひたかれんと）
- 所属：Integration UNIT 1
- 出身：九州の大分県出身
- 趣味：ボードゲーム、筋トレ

---

# huskyとGit hooksについて

---

## 発表の経緯
- 佐野市の防災アプリ開発でHasuraを使った
- 短期間でAPIを立てることができた
- 便利なサービスなので多くの人に知って欲しい

---

## Hasuraとは
- DBから自動的にGraphQLサーバを構築サービス

![bg right 50%](2021-10-10-14-27-22.png)

---

## サポートされているDBとAPI

![bg right 50%](2021-10-10-14-31-11.png)

---

## Hasuraのメリット
- GraphQL サーバーを実装する手間が省ける
- 途中から始めはじめやすい＆やめやすい
  - スキーマ情報を解析して自動的にGraphQLサーバを構築してくれるので既存のDBをそのまま使える
  - 途中で辞めたとしてもDBはそのまま使い回すことができる
  - ロックインされづらい

---

## Hasuraのデメリット
- GraphQLサーバーを実装する手間が省ける
- 途中から始め安い＆辞めやすい

---

## Hasuraが向いているシステム
- 複雑なロジックが不要な参照系のAPIが必要なサービス
  - 複雑なロジックの処理を行うためには、別途APIを立てたりする必要があり、Hasuraのメリットを活かしづらい
- マイクロサービス
  - Dockerイメージが配布されているので始めやすい
  - 複雑なロジックが必要なアプリではHasuraだけで完結するのは難しいので、複雑なロジックの処理を行うためのAPIを別途立てる必要がある

---