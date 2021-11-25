# NestJs Module
## document reference
[NestJs-Zero-to-Hero Udemy Course](https://www.udemy.com/course/nestjs-zero-to-hero/learn/lecture/26680748#overview)
## Introduction

In NestJs, Module is the basic unit for separating logic in a app.

Modules are singletons, each module could be imported by multiple other modules.

In layout part, usually have a folder per module, containing the modules's components.

In previous section, bootstrap function use NestFactory to generate app instance from AppModule

AppModule is the root module for all NestJs

## Concept

Module could be composed by a group of Module.

for example:

An ForumModule may consist of PostModule, CommentModule, AuthModule

![](https://i.imgur.com/qITBFMv.png)

And PostModule and CommentModule could also be compose of UserProfileModule at the same time

## Structure of NestJs Module
This is the NestJs module syntax structure:
```typescript==
@Module({
  imports: [], // for imports other modules
  controllers: [], // for setup controllers for handles http request
  providers: [], // for setup basic service logic
  exports: [], // for shared this module functionality part like Service 
})
export class ModuleName;
```
define a module by use @Module decorator

NestJs use @Module decorator with metadata to organize the logic

## @Module Decorator Properties

### providers

Array of providers to be available within module via dependency injection

### controllers

Array of controllers to be instantiated within module.

### exports

Array of providers to export to other modules.

### imports

List of modules required by this module.

## Example

![](https://i.imgur.com/RB81nFb.png)

In the above structure, we could implementation with following code:
```typescript
@Module({
  imports: [AuthModule, PostModule, CommentModule],
  providers: [ForumService],
  controllers: [ForumController],
  exports: [ForumService],
})
export class ForumModule;
```
