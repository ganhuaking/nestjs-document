# NestJs Config

[English Version](config/README.md "English Version")

一般來說, 有些機敏資料比如 JWT_SECRET 或是資料庫連線設定, 不會直接寫死在原始碼裏面, 而是使用環境參數的方式在程式執行的時候帶入

NestJs 提供了一個元件叫作 ConfigModule 可以用來讀取環境變數或環境參數設定檔比如 .env 檔案

## 安裝 NestJs config 所需要的套件

```shell
npm i -S @nestjs/config
```

## 建立 .env.stage.dev

```yaml
DB_DATABASE=task-management
DB_USERNAME=admin
DB_PASSWORD=password
DB_HOST=localhost
DB_PORT=5432
PORT=5566
```

## 在 AppModule 引入 ConfigModule

```typescript
import { Module } from '@nestjs/common';
import { TasksModule } from './tasks/tasks.module';
import { AuthModule } from './auth/auth.module';
import { TypeOrmModule } from '@nestjs/typeorm';
import { ConfigModule, ConfigService } from '@nestjs/config';
@Module({
  imports: [
    ConfigModule.forRoot({
      envFilePath: [`.env.stage.${process.env.STAGE}`],
    }),
    TasksModule,
    AuthModule,
    // TypeOrmModule.forRoot({
    //   type: 'postgres',
    //   synchronize: true, // setup for synchronize data structure with db
    //   autoLoadEntities: true, // this will autoload entities from other module
    //   host: `${DB_HOST}`,
    //   port: 5432,
    //   username: `${DB_USERNAME}`,
    //   password: `${DB_PASSWORD}`,
    //   database: `${DB_DATABASE}`,
    // }),
    TypeOrmModule.forRootAsync({
      imports: [ConfigModule],
      inject: [ConfigService],
      useFactory: async (configService: ConfigService) => {
        return {
          type: 'postgres',
          synchronize: true, // setup for synchronize data structure with db
          autoLoadEntities: true, // this will autoload entities from other module
          host: configService.get('DB_HOST'),
          port: configService.get('DB_PORT'),
          username: configService.get('DB_USERNAME'),
          password: configService.get('DB_PASSWORD'),
          database: configService.get('DB_DATABASE'),
        };
      },
    }),
  ],
})
export class AppModule {}
```

## 安裝 @hapi/joi @types/hapi__joi for .env 做設定檔組態檢查

```shell
npm i -S @hapi/joi
npm i -D @types/hapi__joi
```
## 建立 config.schema.ts

```typescript
import * as Joi from '@hapi/joi';

export const configValidationSchema = Joi.object({
  PORT: Joi.number().default(3000).required(),
  STAGE: Joi.string().required(),
  DB_HOST: Joi.string().required(),
  DB_PORT: Joi.number().default(5432).required(),
  DB_USERNAME: Joi.string().required(),
  DB_PASSWORD: Joi.string().required(),
  DB_DATABASE: Joi.string().required(),
});
```

## 設定 validationSchema

```typescript
import { Module } from '@nestjs/common';
import { TasksModule } from './tasks/tasks.module';
import { AuthModule } from './auth/auth.module';
import { TypeOrmModule } from '@nestjs/typeorm';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { configValidationSchema } from './config.schema';
@Module({
  imports: [
    ConfigModule.forRoot({
      envFilePath: [`.env.stage.${process.env.STAGE}`],
      validationSchema: configValidationSchema,
    }),
    TasksModule,
    AuthModule,
    TypeOrmModule.forRootAsync({
      imports: [ConfigModule],
      inject: [ConfigService],
      useFactory: async (configService: ConfigService) => {
        return {
          type: 'postgres',
          synchronize: true, // setup for synchronize data structure with db
          autoLoadEntities: true, // this will autoload entities from other module
          host: configService.get('DB_HOST'),
          port: configService.get('DB_PORT'),
          username: configService.get('DB_USERNAME'),
          password: configService.get('DB_PASSWORD'),
          database: configService.get('DB_DATABASE'),
        };
      },
    }),
  ],
})
export class AppModule {}
```

**上一個主題:** [在 NestJs 使用 TypeORM 的範例](example-typeorm/README-zh_TW.md "在 NestJs 使用 TypeORM 的範例")