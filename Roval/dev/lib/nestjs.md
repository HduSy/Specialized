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

## 参考文献
