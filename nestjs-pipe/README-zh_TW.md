# NestJs Pipe

[English Version](nestjs-pipe/README.md "English Version")

## 參考資料

[NestJs From Zero to Hero Course: NestJs Pipe](https://www.udemy.com/course/nestjs-zero-to-hero/learn/lecture/26681372#overview)

## 用途

NestJs Pipes 作用在 NestJs Controller Handlers 被呼叫前, 會把要傳入 Handlers 參數先處理過

NestJs Pipes 可以用來做資料格式轉換或是資料格式驗證

NestJs Pipes 會回傳資料不論是原始資料或是修改過的, 這些資料會傳入路由處理函式(i.e 定義在 NestJs Controller 的 Handlers)

NestJs Pipes 會拋出例外. 拋出的例外會由 NestJs 擷取並且轉換成 HTTP 回應給客戶端程式

## NestJs 中一些預設的 Pipes

ValidationPipe: 用來驗證輸入資料格式

ParseIntPipe: 用來驗證輸入格式是數字

## 客製化的 Pipe

可以自訂義 NestJs Pipe 但需要符合以下規則：

1. 必須要用 @Injectable() 裝飾子裝飾這個類別

2. 需要實作 PipeTransform 這個介面. 因此, 每個 NestJs Pipe 都需要執行 transform 這個函式. 這個函式會被 NestJs 底層呼叫去處理路由輸入參數

3. transform 函式接收兩個參數:
  
value: 傳入要處理的原始參數

metadata(非必要參數): 一個物件包含一些參數的驗證資訊或是轉換資訊

## NestJs Pipe 可以使用的層集

NestJs Pipes 可用以下幾種方式取用:

1. Handler 層的 Pipe: 透過 @UsePipes() 裝飾子來使用. 這種 Pipe 會存取到所有路由參數 

```typescript
@Post()
@UsePipes(SomePipe)
createTask(@Body('description') description: string) {
  //..
}
```

1. Parameter 層集的 Pipes: 只有特定被指定到參數會被 Pipe 處理

```typescript
@Post()
createTask(
  @Body('description', somePipe) description  
) {
  //..
}
```

1. 全域的 Pipes: 被定義在應用層集, 並且會處理所有路由的參數

```typescript
async function bootstrap() {
  const app = await NestFactory.create(ApplicationModule);
  app.useGlobalPipes(SomePipe);
  await app.listen(3000);
}
bootstrap();
```

**上一個主題:** [DTO 範例](example-dto/README-zh_TW.md "DTO 範例")
**下一個主題:** [NestJs Pipe 範例](example-nestjs-pipe/README-zh_TW.md "NestJs Pipe 範例")