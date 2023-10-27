---
title: NestJs
date: 2023-08-11 20:22:22
categories:
  - web
tags:
  - node
---

# NestJs

NestJS 是运行在服务端的一个 JS 框架。它运行时是 JavaScript，编程时使用TypeScript, 是结合灵活性、可扩展性的框架。并且结合了面向对象编程(OOP)、函数式编程(FP)和函数响应式编程(FRP)

它的特殊之处在于，以前使用 JS 方式写后台代码，现在通过写 TS, 再编译成 JS 运行。既包含了 JS 的灵活性，又有 TS 的约束。
NestJS 也主张的是 MVC 的格式。

- module 的作用是在程序运行时给模块处理依赖。好处是所有模块的依赖都可以在 module 中清晰明了的知道引用还是被引用
- controller 的作用是处理请求，所有的请求会先到 controller，再经 controller 调用其他模块业务逻辑
- service 是真正处理业务逻辑的地方，所有的业务逻辑都会在这里处理。它可经过 module 引用其他模块的service，也可经过 module 暴露出去。

## 开发准备

安装 NestJS cli

```bash
npm i -g @nestjs/cli
```

创建 nest-test 项目

```bash
nest new nest-test
```

安装依赖或更新依赖

```bash
npm install
```

启动

```bash
npm run start
```

## 生成新模块

NestJS cli 也支持用命令行形式来创建，这样就不需要做重复的创建文件的动作了。

```bash
nest g controller students
nest g service students
nest g module students

#########目录结构变成如下
src
  |- app.controller.spec.ts
  |- app.controller.ts     
  |- app.module.ts         
  |- app.service.ts        
  |- main.ts               
  |- students/
        |- students.controller.spec.ts
        |- students.controller.ts     
        |- students.module.ts         
        |- students.service.spec.ts
        |- students.service.ts       
```

编辑文件

```ts
// students.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class StudentsService {
    ImStudent() {
        return 'Im student';
    }
}
```

```ts
// students.controller.ts
import { Controller, Get } from '@nestjs/common';
import { StudentsService } from './students.service';

@Controller('students')
export class StudentsController {
    constructor(private readonly studentsService: StudentsService) {}
  
    @Get('who-are-you')
    whoAreYou() {
        return this.studentsService.ImStudent();
    }
}
```
