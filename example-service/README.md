# todo api server services

Lets start create our TasksService and AuthService

## create tasks service use nestjs-cli

```shell==
nest g service tasks --no-spec
```
**Notice** with --no-spec is for not generate test for service 

![](https://i.imgur.com/dBUS6Ht.png)

## create auth service use nestjs-cli

```shell==
nest g service auth --no-spec
```
**Notice** with --no-spec is for not generate test for service


![](https://i.imgur.com/W1QueEf.png)

## TasksService implementation

### first inject TasksService into TaskController by constructor

```typescript
import { Controller } from '@nestjs/common';
import { TasksService } from './tasks.service';

@Controller('tasks')
export class TasksController {
  constructor(private tasksService: TasksService) {}
}
```

### Implemtation for getAllTasks with simple array first

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

### implementation for GET /tasks routes with previous finished TasksService.getAllTasks method

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
### for enhance type safety lets define task.module.ts

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
and modify TasksService as follow

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
