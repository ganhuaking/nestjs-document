# NestJs 專案結構

[English Version](layout/README.md)

## 簡介

在這個段落, 我們將會建立一個全新的 NestJs 專案並且觀察 NestJs 的專案結構

## 建立一個新的專案

使用 nestjs 命令字元工具產生一個名稱為 todo-api-server 的新專案 

```shell
nest new todo-api-server
```

## 專案結構

專案結構如下圖:

![](https://i.imgur.com/nJJmpEL.png)

```yaml
todo-api-server
- node_modules
- src
- test
- .eslintrc.js
- .gitignore
- .prettierrc
- nest-cli.json
- package.json
- package-lock.json
- README.md
- tsconfig.build.json
- tsconfig.json
```
### .eslintrc.js

用來規範語法設定語法提示

### .prettierrc

用來設定格式化原始碼格式

### nest-cli.json

nestjs-cli 執行時所謂用到的設定檔案

### package.json

用來紀錄 nodejs 模組的相依性與專案設定細節

### package-lock.json

一種用來鎖定 nodejs 模組相依性範圍的檔案主要與 npm 管理套件方式有關

### tsconfig.json

typescript 編譯的設定

### tsconfig.build.json

用來增強 tsconfig.json 的設定檔

### src

預設用來存放專案原始碼的目錄資料夾

### test 

預設用來放是 e2e 測試的測試腳本還有設定檔

### .gitignore

用來設定 git 不寫入紀錄的檔案比如 nodejs 模組的執行檔

### node_modules

放置安裝過 nodejs 模組的執行檔

## 原始碼檔案目錄結構

### src/main.ts

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```

這個檔案是 NestJs 應用的執行進入點 (i.e. 也就是 NestJs 是從這隻檔案執行整個專案)

這隻檔案的主要執行邏輯如下：

1 使用 NestFactory 由 AppModule 產生一個 app 物件
 
2 讓 app 物件監聽阜口 3000

### src/app.module.ts

```typescript
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}

```

這個 AppModule 就是整個 NestJs 的主要 Module (最大的那個 Module, 每個 NestJs 應用至少需要有一個 Module)

**重點** 這個類別使用 @Module 裝飾子

**上一個主題:** [如何使用 NestJs](usage/README-zh_TW.md "如何使用 NestJs")
**下一個主題:** [NestJs Module](module/README-zh_TW.md "NestJs Module")