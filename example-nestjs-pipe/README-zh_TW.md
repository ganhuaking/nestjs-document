# todo-api server Pipe 範例

[English Version](example-nestjs-pipe/README.md "English Version")

## 對 createTaskDto 使用 ValidationPipe 
### 安裝 class-validator 與 class-transformer 這兩個套件

```shell
npm i class-validator class-transformer@0.4.0 -S
```

[class-validator github](https://github.com/typestack/class-validator)

### 替 CreateTaskDto 加入驗證部份

```typescript
import { IsNotEmpty } from 'class-validator';
export class CreateTaskDto {
  @IsNotEmpty()
  title: string;
  @IsNotEmpty()
  description: string;
}
```

### 設定全域 validationPipe 對所有輸入做驗證

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

**上一個主題:** [NestJs Pipe](nestjs-pipe/README-zh_TW.md "NestJs Pipe")
**下一個主題:** [在 NestJs 使用 TypeORM](typeorm/README-zh_TW.md "在 NestJs 使用 TypeORM")