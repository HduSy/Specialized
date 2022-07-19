---
date created: 2022-07-15 14:30
date updated: 2022-07-15 14:37
---

创建日期：2022-07-13 16:15:12  
最后修改：2022-07-13 16:15:12

---

> There are no strangers here; Only friends you haven't yet met.  
>—<cite>William Butler Yeats</cite>

# 正文

## 基本概念

### 控制器 Controller

控制器接收处理应用程序特定请求，路由策略控制着哪些控制器接收哪些请求。一个控制器可以有多个路由，不同路由执行不同处理程序。（cli.controller.ts）

#### 创建

`nest -g resource [name]`  
通过 `@Controller()` 和 `class` 创建。

#### 路由

路由映射是将请求绑定到相应控制器，通过设置 `@Controller()` 可选参数，可轻松对一组相关路由进行分组，减少重复代码。

### 提供者 Providers

#### 创建

`nest -g service [name]`  
通过 `@Injectable()` 和 `class` 创建。

#### 概念

由 `@Injectable` 装饰器修饰的普通类，可以是 `service`、`repositry`、`factory`、`helper` 等，在 `constructor` 构造函数中注入，替控制器分担一些额外的更加复杂的处理。

#### 示例

```ts
@Post('/upload/codeblock')  
@UseInterceptors(FileInterceptor('file'))  
async uploadCodeBlock(  
  @UploadedFile() file: TUploadFileField,  
  @BasicAuthUser() username: string,  
  @Body('id', ParseIntPipe) id: number,  
  @Body('bfs', ParseIntPipe) bfs: number  
) {  
  // 检查用户权限  
  await this.cliService.getInfoWithAuth(id, username, { vCodeMode: false });  
  
  // 提取zip文件  
  const {  
    oss: ossFiles,  
    bfs: bfsFiles,  
    json: jsonFile,  
  } = this.cliService.getZipUploadResources(file.buffer, {  
    useBfs: bfs === 1,  
    readHtml: false,  
    prefix: `activity${id}/`,  
  });  
  
  // 分别上传bfs和oss资源  
  const [bfsResult, ossResult] = await Promise.all([  
    this.bfsService.uploadMany(bfsFiles, true),  
    this.ossService.uploadMany(ossFiles),  
  ]);  
  
  // 将上传得到的URL组合起来，下发给用户  
  const fileResultList: Array<{  
    path: string;  
    url: string;  
  }> = [  
    ...this.cliService.mappingUrlAndFile(bfsResult, bfsFiles),  
    ...this.cliService.mappingUrlAndFile(ossResult, ossFiles),  
  ];  
  
  // 上传完文件，增加处理逻辑  
  const codeblockResources = this.cliService.filterCodeBlockResources(  
    fileResultList,  
    jsonFile  
  );  
  await this.actPageService.updateCodeBlockConfig(  
    id,  
    codeblockResources,  
    username  
  );  
  return {  
    files: fileResultList,  
  };  
}
```

### 模块 Modules

#### 创建

`nest -g service [name]`  
通过 `@Module()` 和 `class` 创建。

#### 概念

由 `@Module` 装饰器修饰的普通类，将紧密相关的功能聚合在一起。

| 属性 | 描述 |
| ----------- | ---------- |
| providers | 服务提供者 |
| controllers | 控制器 |
| imports | 引入模块列表，目的是引入其他模块导出的 providers 以本模块使用 |
| exports | 导出由本模块提供的 providers，目的是其他模块引入本模块时，providers 可用，是本模块 providers 的子集 |

##### 功能模块

提供特定功能。

##### 共享模块

多个模块之间可以共享 providers。

##### 全局模块

不引入模块就无法使用该模块下的 providers，有时仅仅想使用特定 providers 而不想引入对应 module，那么就可以以 `@Global` 装饰符修饰 Module，并通过根或核心模块注册。

##### 动态模块

允许自定义模块，动态注册、配置 providers。

#### 示例

```ts
@Module({  
  imports: [  
    ActDbModule,  
    WebActBackendModule,  
    OssModule.register(),  
    BfsModule.register(),  
    HttpModule,  
    StaticServerModule,  
  ],  
  providers: [Logger, CliService],  
  controllers: [CliController, CliNoAuthController],  
})  
export class CliModule {}
```

### 中间件 Middlewares

#### 概念

在路由处理程序之前执行，可访问 req&res 对象，及请求响应周期中的 next() 中间件函数。

#### 作用

- 执行任何代码。
- 对请求和响应对象进行更改。
- 结束请求 - 响应周期。
- 调用堆栈中的下一个中间件函数。
- 如果当前的中间件函数没有结束请求 - 响应周期, 它必须调用 `next()` 将控制传递给下一个中间件函数。否则, 请求将被挂起。

#### 创建

两种方式创建自定义 `Nest` 中间件：

1. 函数中创建；
2. 由 `@Injectable` 装饰的，实现了（`implements`）`NestMiddle` 接口的类。

#### 应用中间件

包含中间件的模块必须实现（`implements`）`NestModule` 接口，通过模块类的 `configure()` 方法来设置。

### 异常过滤器 ExceptionFilter

#### 概念

内置的**异常层**负责处理整个应用程序中的所有抛出的异常，当捕获到未处理的异常时，最终用户将收到友好的响应。
开箱即用，此操作由内置的全局异常过滤器执行，该过滤器处理类型 `HttpException`（及其子类）的异常。

```json
// 兜底
{
   "statusCode": 500,
   "message": "Internal server error"
}
```

#### 示例

`HttpException` 构造函数接受两种类型参数传递：

```ts
import { HttpException, HttpStatus } from '@nestjs/common'
// string, code
@Get('/error')  
testError() {  
  throw new HttpException('Forbidden', HttpStatus.FORBIDDEN)  
}
/** 响应
{
  "statusCode": 403,
  "message": "Forbidden"
}
**/


// obj, code
@Get('/error')  
testError() {  
  throw new HttpException({  
    code: HttpStatus.FORBIDDEN,  
    message: 'This is a custom message.'  
  }, HttpStatus.FORBIDDEN)  
}
/** 响应
{
  "code": 403,
  "message": "This is a custom message."
}
**/
```

#### 自定义异常

通过 `extends HttpException` 实现。

#### 内置 HTTP 异常

- `BadRequestException`
- `UnauthorizedException`
- `NotFoundException`
- `ForbiddenException`
- `NotAcceptableException`
- `RequestTimeoutException`
- `ConflictException`
- `GoneException`
- `PayloadTooLargeException`
- `UnsupportedMediaTypeException`
- `UnprocessableException`
- `InternalServerErrorException`
- `NotImplementedException`
- `BadGatewayException`
- `ServiceUnavailableException`
- `GatewayTimeoutException`

#### 异常过滤器

##### 概念

通过创建由 `@Catch([T])` 装饰符修饰，实现了（`implements`）`ExceptionFilter` 接口的类，来自定义异常处理，对异常层有更细粒度的完全控制权。在指定 `catch(exception: T, host: ArgumentsHost)` 函数签名内实现自定逻辑，其中 `T` 为异常类型。

##### 示例

```ts
import { ArgumentsHost, Catch, ExceptionFilter, HttpException } from '@nestjs/common'  
import {Response,Request} from 'express'  
@Catch(HttpException) // 绑定到元数据到该过滤器  
export class HttpExceptionFiltter implements ExceptionFilter {  
  // 异常对象 exception: T，T：异常类型。  
  catch(exception: HttpException, host: ArgumentsHost): any {  
    const ctx = host.switchToHttp()  
    const response = ctx.getResponse<Response>()  
    const request = ctx.getRequest<Request>()  
    const status = exception.getStatus()  
    response.status(status)  
      .json({  
        statusCode: status,  
        timestamp: new Date().toISOString(),  
        path: request.url  
      })  
  }  
}
```

##### 作用域

路由范围、控制器范围，全局范围。通过 `@UseFilters` 绑定。

### 管道 Pipe

#### 概念

由 `@Injectable()` 装饰，实现了（`implements`）`PipeTransform` 接口的类。功能：

	- 数据转换：将输入数据转换为需要的数据输出；
	- 数据验证：对输入数据进行验证，如果通过则继续，否则抛出异常。

#### 绑定管道

使用装饰符 `@UsePipes()`，范围和异常过滤一样，可以指定是控制器的、特定路由的和全局的，还可以是针对参数的。

#### 内置管道

- `ValidationPipe`
- `ParseIntPipe`
- `ParseBoolPipe`
- `ParseArrayPipe`
- `ParseUUIDPipe`
- `DefaultValuePipe`

#### 对象结构验证

[Joi](https://joi.dev/api/?v=17.6.0) 提供了语义化 API 以非常简单的方式创建 schema，对对象结构进行验证

```ts
import { ObjectSchema } from '@hapi/joi';  
import {  
  ArgumentMetadata,  
  BadRequestException,  
  Injectable,  
  PipeTransform,  
} from '@nestjs/common';  
  
@Injectable()  
export class JoiValidationPipe implements PipeTransform {  
  constructor(private schema: ObjectSchema) {}  
  // 管道必须实现的方法  
  transform(value: any, metadata: ArgumentMetadata) {  
    const { error } = this.schema.validate(value);  
    if (error) {  
      throw new BadRequestException('Validation failed');  
    }  
    return value;  
  }  
}
```

#### 类验证

[[npm#普通对象与类实例间相互转换|class-transformer]]、[[npm#基于装饰器的类型验证|class-validator]] 两个库提供了支持

```ts
import { ArgumentMetadata, BadRequestException, Injectable, PipeTransform } from '@nestjs/common'  
import { plainToClass } from 'class-transformer'  
import { validate } from 'class-validator'  
  
@Injectable()  
export class ClassvalidatePipe implements PipeTransform {  
  async transform(value: any, { metatype }: ArgumentMetadata) {  
    if (!metatype||!this.toValidate(metatype)) return value  
    // This method transforms a plain javascript object to instance of specific class.  
    const object = plainToClass(metatype, value)  
    const errors = await validate(object)  
    if (errors.length) throw new BadRequestException('Validation Failed.')  
    return value  
  }  
  private toValidate(metatype: Function): boolean {  
    const types: Function[] = [String,Boolean, Number, Array, Object]  
    return !types.includes(metatype)  
  }  
}
```

### 守卫 Guards

#### 概念

由 `@Injectable()` 装饰，实现了（`implements`）`CanActivate` 接口的类，类中必须实现 `canActivate` 方法。根据运行时出现的条件（如权限、角色、访问控制列表等）决定是否把该请求丢给路由处理程序处理，即授权。与 pipe 一样，范围可以是控制器、特定路由处理程序、全局的。

#### 示例

```ts
import { CanActivate, ExecutionContext, Injectable } from '@nestjs/common'  
import { Observable } from 'rxjs'  
  
@Injectable()  
export class AuthGuard implements CanActivate {  
  canActivate(context: ExecutionContext): boolean | Promise<boolean> | Observable<boolean> {  
    const request = context.switchToHttp().getRequest()  
    return this.validateRequest(request)  
  }  
  validateRequest(req) {  
  //  守卫处理逻辑  
    return true  
  }  
}
```

`canActivate` 函数的第一个参数继承并扩展了 `ArgumentsHost`：

```ts
export interface ExecutionContext extends ArgumentsHost {  
  getClass<T = any>(): Type<T>;  // 拿到控制器类类型 XxxControllertype
  getHandler(): Function;  // 返回路由处理程序方法引用
}
```

#### 绑定守卫

使用装饰符 `@UseGuards()`，范围和管道、异常过滤一样，可以指定是控制器的、特定路由的和全局的，还可以是针对参数的。

```ts
@Controller('cats')  
@UseGuards(AuthGuard)  
export class CatsController {}
```

#### 反射器

自定装饰器 `@Roles`，利用 `@SetMetadata` 能力，将定制元数据附加到路由处理程序。

```ts
import {SetMetadata} from '@nestjs/common'  
export const Roles = (...roles: string[]) => SetMetadata('roles',roles)
```

 `@Roles` 使用：

```ts
@Post()  
@Roles('admin')  
async create(@Body() createCatDto: CreateCatDto) {  
  this.catsService.create(createCatDto);  
}
```

反射器：

```ts
import { CanActivate, ExecutionContext, Injectable } from '@nestjs/common'  
import { Observable } from 'rxjs'  
import {Reflector} from '@nestjs/core'  
  
@Injectable()  
export class RoleGuard implements CanActivate{  
  constructor(private reflector:Reflector) {  
  }  
  canActivate(context: ExecutionContext): boolean | Promise<boolean> | Observable<boolean> {  
    const roles = this.reflector.get<string[]>('roles', context.getHandler())  
    if (!roles) {  
      return true  
    }  
    const request = context.switchToHttp().getRequest()  
    const user = request.user  
    return this.matchRoles(roles, user.roles)  
  }  
  matchRoles(roles: string[], uroles: string[]) {  
    // 简单或复杂的处理程序  
    return true  
  }  
}
```

## 参考文献

1. [书栈网](https://www.bookstack.cn/read/nestjs-8-zh/README.md)
2.
