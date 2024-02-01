### 支持

Http 组件是采用的 [gin-gonic/gin](https://github.com/gin-gonic/gin) 库，一些使用详细信息可查看其官方文档，[查看文档](https://gin-gonic.com/zh-cn/docs/)。

### 创建

> 在创建 Http 组件时，务必请先将 HttpServer 组件注册到系统中。

```
./bin/rangine make:module --name=home
```

通过命令生成一个名为 home 的业务组件，[查看代码结构](https://github.com/we7coreteam/w7-rangine-go-skeleton/tree/main/app/home)。

#### 目录说明

|目录|说明|
|-|-|
|[app/home/provider.go](https://github.com/we7coreteam/w7-rangine-go-skeleton/blob/main/app/home/provider.go)|模块的 provider 注册入口，里面需要包含 Register、Export 方法。模块中需要的组件在 Register 函数中声明|
|app/home/command| 存放 console 命令|
|app/home/http/controller| 存放 http 服务的控制器，如果有其它服务以此类推 |
|app/home/http/middleware| 存放 http 服务的中间件 |
|app/home/logic| 封装一些业务相关的方法|
|app/home/model| 数据库orm  |

### 将组件注册到系统中

在入口 main.go 文件中，注册刚才生成的组件。

```
func main() {
	app := app.NewApp(app.Option{
		Name: "w7-rangine-go-skeleton",
	})

	// 业务中需要使用 http server，这里需要先实例化
	httpServer := new(http.Provider).Register(app.GetConfig(), app.GetConsole(), app.GetServerManager()).Export()

	// 注册业务 provider，此模块中需要使用 http server 和 console
	new(home.Provider).Register(httpServer, app.GetConsole())
	app.RunConsole()
}
```