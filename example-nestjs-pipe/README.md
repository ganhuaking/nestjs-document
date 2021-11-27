# todo-api server pipe

## ValidationPipe for createTaskDto
### first install class-validator and class-transformer
```shell
npm i class-validator class-transformer@0.4.0 -S
```

[class-validator github](https://github.com/typestack/class-validator)

### add validation add CreateTaskDto

```typesript===
import { IsNotEmpty } from 'class-validator';
export class CreateTaskDto {
  @IsNotEmpty()
  title: string;
  @IsNotEmpty()
  description: string;
}
```

### setup application validationPipe for all incoming request

```typescript
import { ValidationPipe } from '@nestjs/common';
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(3000);
}
bootstrap();
```

**Previous Topic:** [NestJs Pipe](nestjs-pipe/README.md "NestJs Pipe")
**Next Topic:** [NestJs with TypeORM](typeorm/README.md "NestJs with TypeORM")