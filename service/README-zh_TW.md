# NestJs Service 與 Providers

[English Version](service/README.md "English Version")

## 參考資料

[NestJs From zero to hero Introduction to NestJs Providers and Services](https://www.udemy.com/course/nestjs-zero-to-hero/learn/lecture/26680758#overview)

## NestJs Providers

NestJs Provider 會使用 @Injectable 裝飾子賦予 Provider 特性, 可以在任何 NestJs 元件的建構子透過相性注入來引入

NestJs Provider 類型可以是一個簡單資料物件, 一個類別, 同步或非同步的生成工廠函數

NestJs Provider 必須要在 NestJs Module 被引入來使用(必須引入在 module的 providers 屬性)

NestJs Provider 可以從 NestJs Module 被輸出給其他 NestJs Module 使用(寫在 module 內的 exports 屬性) - 引入該 NestJs Module 的 Module 就可以直接注入, 而不需要再自己的 providers 引入

## NestJs Service

NestJs Service 特性如同 NestJs Provider 一樣. 但是並非所有 NestJs Provider 都是 Service.

當使用 @Injectable() 裝飾子時, NestJs Service 將被生成為 Singleton (也就是該 NestJs 應用內是唯一) 並且需要依附 Module 的 providers 屬性下

通常會把商業邏輯包裝在 Service 這層

## Controler 能夠透過相依性注入引用多個不同的 Service

![](https://i.imgur.com/WKnq4NB.png)

## Module 內使用 Providers 的範例

```typescript
import { TasksController } from './tasks.controller';
import { TasksService } from './tasks.service';
import { LoggerService } from '../shared/logger.service';
@Module({
  controllers: [
    TasksController,
  ],
  providers: [
    TasksService,
    LoggerService,
  ],
})
export class TasksModule {}
```

in TasksController, LoggerService and TasksService could be used through dependency injection.

## NestJs 的相依性注入規則

[NestJS Dependency Injection Udemy Course](https://www.udemy.com/course/nestjs-zero-to-hero/learn/lecture/26680758#overview)

所有 NestJs 內的元件都可以注入有被 @Injectable 裝飾子裝飾的 Provider

我們可以在物件建構子定義要注入的 Provider. NestJs 會幫我們處理相依性注入幫我們要注入的 Provider 生成為物件並且設定為該類別的成員

舉例來說:

```typescript
import { TasksService } from 'tasks.service';

@Controller('/tasks')
export class TasksController {
  constructor(private tasksService: TasksService){}

  @Get()
  async getAllTasks() {
    return await this.tasksService.getAllTaks();
  }
}
```

**上一個主題:** [Controller 範例](example-controller/README-zh_TW.md "Controller 範例")
**下一個主題:** [Service 範例](example-service/README-zh_TW.md "Service 範例")