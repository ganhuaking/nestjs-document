# 使用 NestJs 的優缺點

[English Version](why/README.md "English Version")

## 優點

NestJs 提供了一個分層架構就像 [Angular](https://angular.io/) 一樣, 主要有以下幾個優點

1. 因為使用相依性入, 讓功能間的相依性低比較好做隔離測試
2. 因為使用相依性入, 低耦合讓功能抽換減少影響範圍
3. 具有一個輔助的命令字元工具 nest-cli, 如同 angular-cli
4. 具有設定好的組態檔案, 讓開發方便測試快速

## 缺點

1. 學習曲線有點高(要熟悉 nestjs 相依性注入語法與概念)
2. 建立正式環境的產品有一系列的設定需要注意(相對於其他 nodejs 後端框架)


**前一個主題:** [什麼是 NestJs](nestjs/README-zh_TW.md "什麼是 NestJs")
**下一個主題:** [如何使用 NestJs](usage/README-zh_TW.md "如何使用 NestJs")