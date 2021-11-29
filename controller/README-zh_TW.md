# NestJs Controller

[English Version](controller/README.md "English Version")

## 參考資料

[Nestjs zero-to-hero Controller](https://www.udemy.com/course/nestjs-zero-to-hero/learn/lecture/26680754#overview)

## 何謂 NestJs Controller

NestJs Controller 主要負責處理 HTTP requests 並且回傳 HTTP response 給客戶端

NestJs Controller 會根據負責處理的資源來綁定一個特定的路由 (舉例來說, 處理 tasks 資源的 Controller 就會綁定 "/tasks" 為路由)

NestJs Controller 會包含一些處理 API 端點的處理函式

NestJs Controller 能夠透過相依性注入來使用在同同一個 Module 的 Provider

## Controller 定義語法

使用 @Controller 裝飾子來賦予類別 NestJs Controller 的特性

```typescript
@Controller('/tasks')
export class TasksController {

}
```
## handler 定義語法

Handler 是 NestJs Controller 類別內的函式, 這些函式會使用 @Get, @Post, @Delete 這些裝飾子來設定要處理的服務端點

```typescript
@Controller('/tasks') 
export class TasksController {
  @Get()
  getAllTasks() {
    // do something
    return ...;
  }
  @Post()
  createTask() {
    // do something
    return ...;
  }
}
```

## request 處理流程

![](https://i.imgur.com/Kbx4taG.png)

## todo-api-server handler 架構

![](https://i.imgur.com/6GbrXKT.png)

**上一個主題:** [Module 範例](example-module/README-zh_TW.md "Module 範例")
**下一個主題:** [Controller 範例](example-controller/README-zh_TW.md "Controller 範例")