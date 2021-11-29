# NestJs DTO

[中文版](dto/README-zh_TW.md "中文版")

## Data Transfer Object

A DTO is an object that defines how the data will be sent over the network

Do not have any behavior except for storage, retrieval, serialization and deserialization of its own data

Result in increased performance

Can be useful for data validation

## DTO syntax rule in NestJs 

DTO could be used interface or class to be defined

However, it's better to use **class** to defined because interface could not be refered in runtime

## Examples DTOs

![](https://i.imgur.com/rDYJAH9.png)

**Previous Topic:** [Example Service](example-service/README.md "Example Service")
**Next Topic:** [Example DTO](example-dto/README.md "Example DTO")