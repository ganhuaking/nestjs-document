# NestJs Pipe

## Referenced Document

[NestJs From Zero to Hero Course: NestJs Pipe](https://www.udemy.com/course/nestjs-zero-to-hero/learn/lecture/26681372#overview)

## Usage

Pipes operate on the arguments to be processed by the route handler, just before the handler is called

Pipes can perform data transformation or data validation

Pipes can return data - either orginal or modified - which be passed on to the route handler

Pipes can throw exceptions. Exceptions thrown will be handled by NestJs and parese into an error response.

## Default Pipes in NestJs

ValidationPipe: use for validation data

ParseIntPipe: validation input is number

## Custom Pipe Implementation

Pipes are classes annotated with the @Injectable() decorator

Pipes must implmenent the PipeTransform generic interface. Therefore, every pipe must have a transform() method. This method will be called by NestJs to process the arguments.

The transform() method accepts 2 parameters:

value: the value of the processed arugment
metadata(optional): an object containing metadata about the argument.

Pipes could be consume in different ways:

1. Handler-level pipes: via the @UsePipes() decorator. Such pipe will process all parameters for the incoming requests.

```typescript
@Post()
@UsePipes(SomePipe)
createTask(@Body('description') description: string) {
  //..
}
```
2. Parameter-level pipes: Only the specific parameter for which the pipe has been specified will be processed.

```typescript
@Post()
createTask(
  @Body('description', somePipe) description  
) {
  //..
}
```

3. Global pipes: defined in application level and will be applied to any incomming request.

```typescript
async function bootstrap() {
  const app = await NestFactory.create(ApplicationModule);
  app.useGlobalPipes(SomePipe);
  await app.listen(3000);
}
bootstrap();
```

**Previous Topic:** [Example DTO](example-dto/README.md "Example DTO")
**Next Topic:** [Example NestJs Pipe](example-nestjs-pipe/README.md "Example NestJs Pipe")