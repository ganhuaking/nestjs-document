# NestJs Service And Providers

[中文版](service/README-zh_TW.md "中文版")

## Reference document

[NestJs From zero to hero Introduction to NestJs Providers and Services](https://www.udemy.com/course/nestjs-zero-to-hero/learn/lecture/26680758#overview)

## NestJs Providers

Can be injected  into constructors if decorated as an @Injectable, via dependency injection

Can be a plain value, a class, sync/async factory ...

Providers must be provided to a module for them to be usable

Can be exported from a module - and then be available to other modules that import it

## NestJs Service

Defined as providers. Not all providers are services.

Singleton when wrapped with @Injectable() and provided to a module.

The main source of business logic.

## Controler could import multiple services through dependency injection

![](https://i.imgur.com/WKnq4NB.png)

## Providers in Module Example

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

## Dependency Injection in NestJs

[NestJS Dependency Injection Udemy Course](https://www.udemy.com/course/nestjs-zero-to-hero/learn/lecture/26680758#overview)

Any component within the NestJs ecosystem can inject a provider that is decorated with the @Injectable.

We define the dependencies in the constructor of the class. NestJs will take care of the injection for us, and it will then be available as a class property.

for example:

```typescript==
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

**Previous Topic:** [Example Controller](example-controller/README.md "Example Controller")
**Next Topic:** [Example Service](example-service/README.md "Example Service")