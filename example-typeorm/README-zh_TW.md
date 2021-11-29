# todo-api-server 使用 TypeORM 範例

[English Version](example-typeorm/README.md "English Version")

## 安裝 typeorm @nestjs/typeorm pg

```shell
npm i -S typeorm @nestjs/typeorm pg
```

## 設定 TypeORM 連線

```typescript
import { Module } from '@nestjs/common';
import { TasksModule } from './tasks/tasks.module';
import { AuthModule } from './auth/auth.module';
import { TypeOrmModule } from '@nestjs/typeorm';
@Module({
  imports: [
    TasksModule,
    AuthModule,
    TypeOrmModule.forRoot({
      type: 'postgres',
      synchronize: true, // setup for synchronize data structure with db
      autoLoadEntities: true, // this will autoload entities from other module
      host: `${DB_HOST}`,
      port: 5432,
      username: `${DB_USERNAME}`,
      password: `${DB_PASSWORD}`,
      database: 'task-management',
    }),
  ],
})
export class AppModule {}
```

這些設定值可以從 [typeorm 官網](https://typeorm.io/#/) 找到

在這個部份有以下幾個重點：

1. TypeOrmModule.forRoot 代表這個設定會套用到全域

2. **type** 屬性代表資料庫類型,  因為使用 Postgresql DB 所以選用 postgres

3. **database** 屬性代表要連線的資料庫名稱, 這個資料庫必須要先建立好否則會連線不到

4. **synchronize** 屬性如果設定為 true, entity 模型會自動同步到與 entity 名稱相同的資料表欄位

5. **autoloadEntity** 屬性如果設為 true, entity 模型可以分散寫在不同的 Module 並且會自動載入主要的 Module
6. 在這個範例我們使用直接寫死參數的方法, 但之後在 [NestJs Config](/config/README.md "NestJs Config") 會使用 NestJs ConfigModule 這個元件來載入環境參數這種比較安全的作法

## 建立 Task Entity

建立 task-status.enum 
```typescript
export enum TaskStatus {
  OPEN = 'OPEN',
  IN_PROGRESS = 'IN_PROGRESS',
  DONE = 'DONE',
}
```

建立 task.entity.ts

```typescript
import { Column, Entity, PrimaryGeneratedColumn } from 'typeorm';
import { TaskStatus } from './task-status.enum';
@Entity()
export class Task {
  @PrimaryGeneratedColumn('uuid')
  id: string;
  @Column()
  title: string;
  @Column()
  description: string;
  @Column()
  status: TaskStatus;
}
```

## Active Record vs Data Mapper

[Active Record vs Data Mapper Pattern](https://github.com/typeorm/typeorm/blob/master/docs/active-record-data-mapper.md)

### Active Record

使用 Active Record 方法, 也就是透過 Entity 模型來直接操作資料庫.

### Data Mapper

使用 Data Mapper 方法, 會除了定義 Entity 模型來設定資料表欄位外, 會額外把存取資料庫的方法定義在獨立出來一個叫作 Repository 的物件. 也就是透過 Repository 來做真正資料庫操作, Entity 模組只是用來規範資料表欄位

## TasksRepository

### 我們為了更抽象化以及方便測試所以使用 Data Mapper 方式

設定 TasksRespotiory

```typescript
import { EntityRepository, Repository } from 'typeorm';
import { Task } from './task.entity';
@EntityRepository(Task)
export class TasksRepository extends Repository<Task> {}
```

要注意的是 @EntityRepository() 裝飾子用來指定 Repository 要對應到哪一個 Entity

### 把 TasksRepository 引入 Task module

```typescript
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { TasksController } from './tasks.controller';
import { TasksRepository } from './tasks.repository';
import { TasksService } from './tasks.service';

@Module({
  imports: [TypeOrmModule.forFeature([TasksRepository])],
  controllers: [TasksController],
  providers: [TasksService],
})
export class TasksModule {}
```

### 使用 @InjectRepository 裝飾子把 TasksRepository 注入 TasksService 

移除原本的 tasks 陣列

```typescript
import { Injectable, NotFoundException } from '@nestjs/common';
import { Task } from './task.entity';
import { TaskStatus } from './task-status.enum';
import { v4 as uuid } from 'uuid';
import { CreateTaskDto } from './dto/create-task.dto';
import { GetTaskDto } from './dto/get-task.dto';
import { DeleteTaskDto } from './dto/delete-task.dto';
import { UpdateTaskDto } from './dto/update-task.dto';
import { GetTasksFilterDto } from './dto/get-tasks-filter.dto';
import { TasksRepository } from './tasks.repository';
import { InjectRepository } from '@nestjs/typeorm';
@Injectable()
export class TasksService {
  constructor(
    @InjectRepository(TasksRepository)
    private tasksRepository: TasksRepository,
  ) {}
}
```

### 實作 TaskRepository.createTask 方法

```typescript
import { EntityRepository, Repository } from 'typeorm';
import { CreateTaskDto } from './dto/create-task.dto';
import { TaskStatus } from './task-status.enum';
import { Task } from './task.entity';
@EntityRepository(Task)
export class TasksRepository extends Repository<Task> {
  async createTask(createTaskDto: CreateTaskDto): Promise<Task> {
    const { title, description } = createTaskDto;
    // create entity
    const task = this.create({
      title,
      description,
      status: TaskStatus.OPEN,
    });
    await this.save(task);
    return task;
  }
}
```
### 修改 TasksService.createTask 與 TasksController POST /tasks handler

TasksService
```typescript
import { Injectable, NotFoundException } from '@nestjs/common';
import { Task } from './task.entity';
import { TaskStatus } from './task-status.enum';
import { v4 as uuid } from 'uuid';
import { CreateTaskDto } from './dto/create-task.dto';
import { GetTaskDto } from './dto/get-task.dto';
import { DeleteTaskDto } from './dto/delete-task.dto';
import { UpdateTaskDto } from './dto/update-task.dto';
import { GetTasksFilterDto } from './dto/get-tasks-filter.dto';
import { TasksRepository } from './tasks.repository';
import { InjectRepository } from '@nestjs/typeorm';
@Injectable()
export class TasksService {
  constructor(
    @InjectRepository(TasksRepository)
    private tasksRepository: TasksRepository,
  ) {}
  async createTask(createTaskDto: CreateTaskDto): Promise<Task> {
    return this.tasksRepository.createTask(createTaskDto);
  }
  async getTaskById(getTaskDto: GetTaskDto): Promise<Task> {
    const { id } = getTaskDto;
    const task = await this.tasksRepository.findOne({ id });
    if (!task) {
      throw new NotFoundException(`getTaskById with id: ${id} not found`);
    }
    return task;
  }
}
```

TasksController
```typescript
import {
  Body,
  Controller,
  Delete,
  Get,
  Param,
  Patch,
  Post,
  Query,
} from '@nestjs/common';
import { CreateTaskDto } from './dto/create-task.dto';
import { DeleteTaskDto } from './dto/delete-task.dto';
import { GetTaskDto } from './dto/get-task.dto';
import { GetTasksFilterDto } from './dto/get-tasks-filter.dto';
import { UpdateTaskDto } from './dto/update-task.dto';
import { Task } from './task.entity';
import { TasksService } from './tasks.service';

@Controller('tasks')
export class TasksController {
  constructor(private tasksService: TasksService) {}
  @Post()
  createTask(@Body() createTaskDto: CreateTaskDto): Promise<Task> {
    return this.tasksService.createTask(createTaskDto);
  }
  @Get('/:id')
  getTaskById(@Param() getTaskDto: GetTaskDto): Promise<Task> {
    return this.tasksService.getTaskById(getTaskDto);
  }
}
```

**上一個主題:** [NestJs with TypeORM](typeorm/README-zh_TW.md "NestJs with TypeORM")
**下一個主題:** [NestJs Config](config/README-zh_TW.md "NestJs Config")