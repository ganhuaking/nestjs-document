# NestJs Config

Usually, something that is sensitive, like JWT_SECRET or Data connection credential we will not hardcode as previous setup int source code, but using environment variable to setup when runtime.

NestJs provide a component called Config that can read environment variable setup from config file, like .env

## install package for use NestJs config

```shell
npm i -S @nestjs/config
```
## make .env.stage.dev file
```yaml
DB_DATABASE=task-management
DB_USERNAME=admin
DB_PASSWORD=password
DB_HOST=localhost
DB_PORT=5432
PORT=5566
```
## add ConfigModule in AppModule
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

## install @hapi/joi @types/hapi__joi for .env file schema check

```shell
npm i -S @hapi/joi
npm i -D @types/hapi__joi
```
## generate config.schema.ts

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

## setup validationSchema config

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

**Previous Topic:** [Example NestJs with TypeORM](example-typeorm/README.md "Example NestJs with TypeORM")