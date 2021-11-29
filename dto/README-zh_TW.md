# NestJs DTO

[English Version](dto/README.md "English Version")

## 資料傳輸物件

資料傳輸物件指的是用來規範傳輸資料格式的物件

資料傳輸物件只做存儲, 讀取, 序列化與反序列化沒有其他額外的行為

因為能夠有效率轉換資料, 在複雜的應用程式內能夠提升資料傳輸效率

在 NestJs 中透過資料傳輸物件也能夠對傳入的資料做驗證 

## 在 NestJs 資料傳輸物件語法定義方式

資料傳輸物件在 NestJs 中可以使用 interface 或是 class 來撰寫

然而, 由於定義成 interface 無法在程式執行時去追蹤, 所以 NestJs 官方建議使用 **class** 來撰寫  

## DTOs 範例

![](https://i.imgur.com/rDYJAH9.png)

**上一個主題:** [Service 範例](example-service/README-zh_TW.md "Service 範例")
**下一個主題:** [DTO 範例](example-dto/README-zh_TW.md "DTO 範例")