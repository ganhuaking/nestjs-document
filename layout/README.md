# NestJs Project Layout

## Introduction

In this section, we will create a whole new nestjs project and see the structure of nestjs project

## Create a new project
Lets use the nest-cli to generate a new nestjs project with name todo-api-server

```shell=
nest new todo-api-server
```
## project layout

layout as follow picture:

![](https://i.imgur.com/nJJmpEL.png)

```yaml==
todo-api-server
- node_modules
- src
- test
- .eslintrc.js
- .gitignore
- .prettierrc
- nest-cli.json
- package.json
- package-lock.json
- README.md
- tsconfig.build.json
- tsconfig.json
```
### .eslintrc.js

use to setup linter for code syntax with certain rule

### .prettierrc

use to formatting the project source code
### nest-cli.json

for nest-cli use to compile source  code and generate source code

### package.json

use to record the node_modules dependency and some script command with npm

### package-lock.json

use to lock the node modules dependency with certain constraint for security

### tsconfig.json

typescript compile setup

### tsconfig.build.json

use to extend the tsconfig.json setup for typescript compile option

### src

This directory is used to store all project source code

### test 

This directory is used to put e2e test code and setup

### .gitignore

for git ignore setup

### node_modules

for nodejs installed module binary

## Src Layout

### src/main.ts

```typescript==
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```
This is the entrypoint to NestJs APP

The main logic about app:

1 use NestFactory create app Instance from AppModule

2 make app Instance listen on port 3000 

### src/app.module.ts

```typescript
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}

```

This is the root module for nestjs

**Notice** this class is use @Module decorator