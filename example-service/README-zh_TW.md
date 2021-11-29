# todo api server Service 範例

讓我們來建構 TasksService 與 AuthService

[English Version](example-service/README.md "English Version")

## 使用 nestjs-cli 建立 tasks Service 

```shell==
nest g service tasks --no-spec
```
**Notice** 使用 --no-spec 代表不要產生 .spec.ts 測試檔 

![](https://i.imgur.com/dBUS6Ht.png)

## 使用 nestjs-cli 建立 auth Service

```shell==
nest g service auth --no-spec
```

**Notice** 使用 --no-spec 代表不要產生 .spec.ts 測試檔


![](https://i.imgur.com/W1QueEf.png)

## 實作 TasksService

### 注入 TasksService 到 TaskController 的 constructor

```typescript
import { Controller } from '@nestjs/common';
import { TasksService } from './tasks.service';

@Controller('tasks')
export class TasksController {
  constructor(private tasksService: TasksService) {}
}
```

### 實作 getAllTasks

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class TasksService {
  private tasks = [];

  getAllTasks() {
    return this.tasks;
  }
}
```

### 使用 TasksService.getAllTasks 來實作 GET /tasks 路由

```typescript
import { Controller, Get } from '@nestjs/common';
import { TasksService } from './tasks.service';

@Controller('tasks')
export class TasksController {
  constructor(private tasksService: TasksService) {}
  @Get()
  getAllTasks() {
    return this.tasksService.getAllTasks();
  }
}
```
### 定義 task.module.ts 來增強資料型別安全性

```typescript
export interface Task {
  id: string;
  title: string;
  description: string;
  status: TaskStatus;
}
enum TaskStaus {
  OPEN = 'OPEN',
  IN_PROGRESS = 'IN_PROGRESS',
  DONE = 'DONE',
}
```

並且修改 TaskService 如下

```typescript
import { Injectable } from '@nestjs/common';
import { Task } from './task.model';

@Injectable()
export class TasksService {
  private tasks: Task[] = [];

  getAllTasks() {
    return this.tasks;
  }
}
```

**上一個主題:** [NestJs Service](service/README-zh_TW.md "NestJs Service")
**下一個主題:** [NestJs DTO](dto/README-zh_TW.md "NestJs DTO")