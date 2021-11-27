# todo-api server DTO

## CreateTasksDTO

### create a folder named "dto" under tasks folder

```shell
cd ~/todo-api-server
mkdir src/tasks/dto
```
### create create-task.dto.ts under the dto folder

for create tasks just need title and description 2 string property

and id with use uuid auto generated

src/tasks/dto/create-task.dto.ts
```typescript
export class CreateTaskDto {
  title: string;
  description: string;
}
```

### install uuid node module

```shell
npm i uuid
```
### implementation for Tasks.createTask with CreateTaskDto as input

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

### implementation for POST /tasks with @Body() CreateTaskDto as input

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

**Previous Topic:** [NestJs DTO](dto/README.md "NestJs DTO")
**Next Topic:** [NestJs Pipe](nestjs-pipe/README.md "NestJs Pipe")