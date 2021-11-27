# todo-api-server with TypeORM

## install typeorm @nestjs/typeorm pg

```shell
npm i -S typeorm @nestjs/typeorm pg
```

## setup connection with TypeORM

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
These setup could be found from [typeorm offical document](https://typeorm.io/#/)

In this part, Notice following points:

1. TypeOrmModule.forRoot means this setup will be global

2. **type** property stand for db type, we choose postgres for Postgresql DB

3. **database** property stand for connection database, this database need to created first

4. **synchronize** property if this set to true, entity model will synchronize with table columns 

5. **autoloadEntity** property if this set to true, the entity model could be separated into different module, and autoload load into root module

6. In this example we use hardcode this property into the setup, but later in [NestJs Config](/config/README.md "NestJs Config") could use Configuration Component to load environment variable dynamically and more securely

## create Task Entity

create task-status.enum for TaskStatus enum
```typescript
export enum TaskStatus {
  OPEN = 'OPEN',
  IN_PROGRESS = 'IN_PROGRESS',
  DONE = 'DONE',
}
```
change origin task.model to task.entity.ts

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

Using the Active Record approach, you define all your query methods inside the model itself, and you save, remove, and load objects using model methods.

Simply said, the Active Record pattern is an approach to access your database within your models. You can read more about the Active Record pattern.

### Data Mapper

Using the Data Mapper approach, you define all your query methods in separate classes called "repositories", and you save, remove, and load objects using repositories. In data mapper your entities are very dumb - they just define their properties and may have some "dummy" methods.

Simply said, data mapper is an approach to access your database within repositories instead of models. 

## TasksRepository

### We choose Data Mapper pattern for more abstraction and better test

setup TasksRespotiory

```typescript
import { EntityRepository, Repository } from 'typeorm';
import { Task } from './task.entity';
@EntityRepository(Task)
export class TasksRepository extends Repository<Task> {}
```
Notice that @EntityRepository() decorator specify mapping which entity is Repostiory is for

### load the TasksRepository into the Task module

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

### Inject TasksRepository into TasksService with @InjectRepository decorator
remove origin tasks array
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

### implement createTask method in TaskRepository

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
### change TasksService.createTask TasksController POST /tasks handler

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

**Previous Topic:** [NestJs with TypeORM](typeorm/README.md "NestJs with TypeORM")
**Next Topic:** [NestJs Config](config/README.md "NestJs Config")