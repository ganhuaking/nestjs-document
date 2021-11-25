# NestJs Controller

## Reference Document

[Nestjs zero-to-hero Controller](https://www.udemy.com/course/nestjs-zero-to-hero/learn/lecture/26680754#overview)

## What is NestJs Controller Response for

Response for handling incoming requests and returning responses to the client

Bound to a specific path(for examle, "/tasks" for tasks resource)

Contain handlers, which handle endpoints and request methods

Can takes advantage of  dependency injection to consume providers within the same module

## define Controller syntax

decorating a class with the @Controller decorator

```typescript
@Controller('/tasks')
export class TasksController {

}
```
## define handler syntax

Handlers are simply methods within the controller class, decorated with decorator @Get, @Post, @Delete ...

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

## request handle flow

![](https://i.imgur.com/Kbx4taG.png)

## todo-api-server handler layout


![](https://i.imgur.com/6GbrXKT.png)