# todo-api server DTO 範例

[English Version](example-dto/README.md "English Version")

## CreateTasksDTO

### 在 src/tasks 資料下建立一個名稱為 dto 的新資料架

```shell
cd ~/todo-api-server
mkdir src/tasks/dto
```
### 在 src/tasks/dto 資料夾下建立 create-task.dto.ts 

建立任務只需要 title 跟 description 這兩個字串屬性

而 id 會使用 uuid 自動建立

所以 CreateTaskDto 如下

src/tasks/dto/create-task.dto.ts
```typescript
export class CreateTaskDto {
  title: string;
  description: string;
}
```

### 安裝 uuid 套件

```shell
npm i uuid -S
```
### 使用 CreateTaskDto 當作輸入參數, 實作 Tasks.createTask

```typescript
import { Injectable } from '@nestjs/common';
import { Task, TaskStatus } from './task.model';
import { v4 as uuid } from 'uuid';
import { CreateTaskDto } from './dto/create-task.dto';
@Injectable()
export class TasksService {
  private tasks: Task[] = [];

  getAllTasks() {
    return this.tasks;
  }

  createTask(createTaskDto: CreateTaskDto): Task {
    const { title, description } = createTaskDto;
    const task: Task = {
      id: uuid(),
      title,
      description,
      status: TaskStatus.OPEN,
    };
    this.tasks.push(task);
    return task;
  }
}

```

###  使用 @Body() CreateTaskDto 當作輸入參數, 實作路由 POST /tasks 

```typescript
import { Body, Controller, Get, Post } from '@nestjs/common';
import { CreateTaskDto } from './dto/create-task.dto';
import { Task } from './task.model';
import { TasksService } from './tasks.service';

@Controller('tasks')
export class TasksController {
  constructor(private tasksService: TasksService) {}
  @Get()
  getAllTasks() {
    return this.tasksService.getAllTasks();
  }
  @Post()
  createTask(@Body() createTaskDto: CreateTaskDto): Task {
    return this.tasksService.createTask(createTaskDto);
  }
}
```

**上一個主題:** [NestJs DTO](dto/README-zh_TW.md "NestJs DTO")
**下一個主題:** [NestJs Pipe](nestjs-pipe/README-zh_TW.md "NestJs Pipe")