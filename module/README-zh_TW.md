# NestJs Module

[English Version](module/README.md)
## 參考文件
[NestJs-Zero-to-Hero Udemy Course](https://www.udemy.com/course/nestjs-zero-to-hero/learn/lecture/26680748#overview)

## 簡介

Module 是 NestJs 應用中的基礎邏輯分割的部份 (i.e. 可以把一個 NestJs 根據不同邏輯切割多個小的 Module)

NestJs 中使用 @Module 裝飾子包裝起來的物件都會被生成為單例, 每個模組都可以一個以上的其他模組所引用 

在專案結構的部份, 通常每個模組會建立一個資料夾, 同模組的原始碼會放在一起

上一個主題中, bootstrap 函式使用了 NestFactory 由 AppModule 產生 app 物件

AppModule 就是這個 NestJs 應用的主要 Module

## 概念

Module 可以由一個以上的其他 Module 所組成

舉例來說:

一個 ForumModule 可能包含了 PostModule, CommentModule, AuthModule 如下圖

![](https://i.imgur.com/qITBFMv.png)

而 PostModule, CommentModule 也可以同時組成一個叫作 UserProfileModule 的 Module 來讓分享給其他 Module 

## NestJs Module 語法結構

以下是 NestJs module 語法結構:

```typescript
@Module({
  imports: [], // for imports other modules
  controllers: [], // for setup controllers for handles http request
  providers: [], // for setup basic service logic
  exports: [], // for shared this module functionality part like Service 
})
export class ModuleName;
```

使用 @Module 裝飾子定義了一個 Module

NestJs 使用 @Module 裝飾子與裡面的一些屬性來處理 Module 的邏輯

## @Module 裝飾子屬性

### providers

用來設定 Module 可用的 Providers

### controllers

設用 Module 內會被初始化的 Controllers

### exports

設定要被輸出給其他 Module 使用的 Providers

### imports

設定 Module 需要引用哪些其他的 Module

## 範例

![](https://i.imgur.com/RB81nFb.png)

上面的結構, 可以使用以下實作來完成

```typescript
@Module({
  imports: [AuthModule, PostModule, CommentModule],
  providers: [ForumService],
  controllers: [ForumController],
  exports: [ForumService],
})
export class ForumModule;
```

**上一個話題:** [NestJs 專案結構](layout/README-zh_TW.md "NestJs 專案結構")
**下一個話題:** [Module 範例](example-module/README-zh_TW.md "Module 範例")