# todo-api-server

讓我們建立一個 todo-api-server

[English Version](example-module/README.md "English Version")

## 軟體架構

![](https://i.imgur.com/eV0idop.png)

![](https://i.imgur.com/Osa727R.png)

## api 結構

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

## 建立 task 與 auth Module

###  清除 todo-api-server 中沒有用到的檔案

```shell
cd ~/todo-api-server
rm src/app.controller.ts
rm src/app.service.ts
rm src/app.controller.spec.ts
```

### 移除 src/app.module.ts 中沒用到的引用

src/app.module.ts
```typescript
import { Module } from '@nestjs/common';

@Module({
  imports: [],
})
export class AppModule {}
```
### 確認 app 在清除檔案後仍然可以正常運行

```shell
npm run start:dev
```
正常會看到以下正常建立 log
![](https://i.imgur.com/G6EAz1m.png)

### 使用 nestjs-cli 建立 tasks Module

```shell
nest g module tasks
```

![](https://i.imgur.com/XPJuHq0.png)


### 使用 nestjs-cli 建立 tasks Module

```shell
nest g module auth
```

![](https://i.imgur.com/V7CnuZa.png)


![](https://i.imgur.com/GCN4HWS.png)

**上一個主題:** [NestJs Module](module/README-zh_TW.md "NestJs Module")
**下一個主題:** [NestJs Controller](controller/README.md "NestJs Controller")