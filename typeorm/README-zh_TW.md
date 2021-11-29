# 在 NestJs 使用 TypeORM

[English Version](typeorm/README.md "English Version")

## ORM(物件關聯對映)

一種技術讓我們可以使用物件導向方式來存取或操作資料庫的資料

## 優點

1. 可以把資料操作模型集中在同一個地方 - 相較於 SQL 語法需要對同一個資料表的操作分散再多個不同語法

2. 有許多資料庫操作細節可以自動化 - 資料庫連線, 資料類型對應, 關係對應等等

3. 不需要去寫 SQL 語法

4. 對資料庫做抽象化, 降低對資料庫類型的相依性, 在資料庫類型更換時影響較小

5. 繼承許多 OOP 的優點

## 缺點

1. 必須去學習 ORM 語法, 並非所有 ORM 函式庫都很簡單

2. 效能普通(仍然需要看 ORM 轉換出來 SQL 語句是否能做優化, [N+1 查詢問題](https://dosmanthus.medium.com/rails-n-1-queries-problem-73dfe5f99182)是經典效能問題)

3. 會容易忽略 ORM 背後的 SQL 原理, 可能會造成許多維護的問題

## TypORM

TypeORM 是一個建立在 Nodejs ORM 函式庫支援用 typescript 或是 javascript 撰寫

**上一個主題:** [NestJs Pipe 範例](example-nestjs-pipe/README-zh_TW.md "NestJs Pipe 範例")
**下一個主題:** [在 NestJs 使用 TypeORM 範例](example-typeorm/README-zh_TW.md "在 NestJs 使用 TypeORM 範例")