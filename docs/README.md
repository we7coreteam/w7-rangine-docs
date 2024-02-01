
# 软擎

基于 Golang ^1.19+^ 和 Gin ^1.8+^ 的开发骨架。

### 初衷

Gin 是一款用 Go 编写的很优秀的 HTTP Web 框架。它并不像是传统的 Php 的框架，没有对开发规范、约束做过多的要求。对于第一次接触 Go 语言或是经验不是很丰富的开发人员来说，初次使用的时候，很难对整体项目的一个完善的规划。

这也是我们的初衷，想通过制定一个开发骨架，帮助大家规范、快速的使用 Gin 框架。并在此基础上，提供一些基础的组件，让开发人员可以把重心放在业务上。

Go 并不能像是 Php、Java 可以做一些强制规范，更像是一种习惯上的约定。在软擎框架中，我们也本着“约定大于配置”的理念，构建了一些像是 Provider 的概念，约定了一些函数名称。虽然这些东西并没有强制力，但是我们还是推荐大家在开发的过程中严格的去遵守这些约定。

### 入口 main 函数

在软擎中 main 函数做为入口函数，主要是完成以下几项工作：

1. app 的初始化，app := app.NewApp()
2. 各种组件的初始化或是全局配置。比如：http server、tcp server 等等
3. 注册业务 Provider，根据不同业务在注册时候传入不同的组件对象
4. app.RunConsole() 启动控制台服务

你可以理解 App 是一个基础的控制台服务。我们内置的 http 服务也是以 Provider 的形式通过 Server Manager 注册到 App 中，因此想要启动服务的时候，还需要加附加上启动参数。被启动的服务是在 config.yaml 中进行配置，当然，也可以同时启动多个服务。

```
// 我这里是直接用 goland 编译，指定了运行参数 server:start
/tmp/go_build_github_com_we7coreteam_w7_rangine_go_skeleton server:start
```

### App 

框架的全局对象，包含了一些最基本的属性，配置、容器、日志、事件、控制台、服务管理、Provider管理。这些基本的对象会贯穿在各种不同的 Provider 中。

当然你也可以不使用系统提供的组件，自己去实例化对象。只需要你在定义 Provider 的时候对形参的类型进行定义即可。

如果你想扩展服务，还需要实现 ServerManager 的相关接口。

### Provider（模块）

Provider 在软擎中是一个很广泛的概念，他是软擎提供的一个基础的插件扩展机制。无论是系统中还是业务应用中，都需要采用 Provider 的方式来注册使用。

Provider 通过 Register() 函数进行注册，通过 Export() 函数对外暴露一些对象。

### 目录规范

- app 业务模块的目录，一个子目录表示一个业务模块
  - app/module/provider.go 每个模块的入口文件
  - app/module/command console的cmd命令文件
  - app/module/http/controller http server的控制器
  - app/module/http/middleware http server的中间件
  - app/module/logic 用于封装业务的一些逻辑方法
- config.ymal 项目的配置文件，定义一些服务端口、数据库连接参数
- main.go 项目的主入口文件
- reload.sh 热加载启动脚本
- runtime 存放运行时产生的一些文件
  - runtime/logs 日志文件
- commom 存放一些公用的方法或是结构
  - common/function 存放公共方法
  - common/dao 存放生成的DAO文件
  - common/entity 存放生成的数据库model文件
- asset 存放静态文件
  - asset/image 存放静态图片


### 示例项目

#### im websocket服务端

实现Im消息的接收和推送，接收到的消息推送到队列中，供业务处理。业务主动推送消息后，通知客户端。仓库地址： [https://gitee.com/donknap/w7-im-websocket-push](https://gitee.com/donknap/w7-im-websocket-push)

#### docker镜像制品库

编写项目配置，将项目转换为微擎交付系统可镜像交付的项目。仓库地址： [https://gitee.com/donknap/cd-artifact](https://gitee.com/donknap/cd-artifact)



