# todo-api-server

Lets start create a todo-api-server

[中文版](example-module/README-zh_TW.md "中文版")
## architecture

![](https://i.imgur.com/eV0idop.png)

![](https://i.imgur.com/Osa727R.png)

## api view

### Tasks

| Endpoint          | Method | Description               |
| ----------------- | ------ | ------------------------- |
| tasks/            | GET    | Get tasks (incl. filters) |
| tasks/:id         | GET    | Get a task                |
| tasks/            | POST   | Create a task             |
| tasks/:id         | DELETE | Delete a task             |
| tasks/:id/status/ | PATCH  | Update task status        |

## Auth

| Endpoint          | Method | Description               |
| ----------------- | ------ | ------------------------- |
| auth/signup       | POST   | Sign up                   |
| auth/signin       | POST   | Sign in                   |

## generate task and auth module

### clear not used file on todo-api-server

```shell
cd ~/todo-api-server
rm src/app.controller.ts
rm src/app.service.ts
rm src/app.controller.spec.ts
```

### remove not used import from src/app.module.ts

src/app.module.ts
```typescript
import { Module } from '@nestjs/common';

@Module({
  imports: [],
})
export class AppModule {}
```
### make sure the the app still work

```shell
npm run start:dev
```
will see the green build
![](https://i.imgur.com/G6EAz1m.png)

### generate tasks module with nestjs-cli

```shell
nest g module tasks
```

![](https://i.imgur.com/XPJuHq0.png)


### generate auth module with nestjs-cli

```shell
nest g module auth
```

![](https://i.imgur.com/V7CnuZa.png)


![](https://i.imgur.com/GCN4HWS.png)

**Previous Topic:** [NestJs Module](module/README.md "NestJs Module")
**Next Topic:** [NestJs Controller](controller/README.md "NestJs Controller")