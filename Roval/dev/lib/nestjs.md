
创建日期：2022-07-13 16:15:12
最后修改：2022-07-13 16:15:12
- - -
> There are no strangers here; Only friends you haven't yet met.
> — <cite>William Butler Yeats</cite>

## 正文
### 基本概念
#### 控制器
控制器接收处理应用程序特定请求，路由策略控制着哪些控制器接收哪些请求。一个控制器可以有多个路由，不同路由执行不同处理程序。（cli.controller.ts）
##### 创建
`nest -g resource [name]`
通过 `@Controller()` 和 `class` 创建。
##### 路由
路由映射是将请求绑定到相应控制器，通过设置 `@Controller()` 可选参数，可轻松对一组相关路由进行分组，减少重复代码。

## 参考文献
