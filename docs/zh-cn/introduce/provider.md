### 业务模块

每个一个业务模块都以一个子目录的形式存放于 app 目录下，一个子目录表示一个完整的业务模块（功能）。比如用户管理，附件管理等等。

#### 分包

如果某一个业务模块过于庞大，也可以通过 go mod 的形式进行分包，只不过也需要按照业务模块的形式进行开发，提供 provider.go 文件，在 main 函数中将此模块注册进来。

### 入口文件

#### 注册方法

通过 Register 方法，根据模块的实际需要进行初始化工作，初始化时依赖外部的一些对象或是变量时，需要指定到 Register 方法的参数列表，并在 main 函数注册时将他们传入进来。

初始化时主要负责的操作有以下几点：

- console cmd 命令的注册
- 路由的注册
- 模块中使用的全局对象的初始化
- orm 模型的全局配置
- 等等

#### 暴露方法

在 Export 方法中，返回对外部需要暴露的一些对象。比如系统的 HttpServer 模块需要将 Server 对象暴露给外部使用。

```
func (provider *Provider) Export() *httpserver.Server {
	return provider.server
}
```

### 注册

编好写的 provider 需要在 main 函数将他们注册并启用。

### 完整的示例

```
=============================== main.go =======================================

// 将静态资源获取到 asset 变量中
//go:embed asset/*
var asset embed.FS


func main() {
	app := app.NewApp(app.Option{
		Name: "w7-rangine-go-skeleton",
	})

	// 业务中需要使用 http server，这里需要先实例化
	// 并通过 export 将 httpServer 对象暴露出来
	httpServer := new(http.Provider).Register(app.GetConfig(), app.GetConsole(), app.GetServerManager()).Export()

	// 注册业务 provider，此模块中需要使用 httpServer 和 console, 还有上方嵌入的静态资源
	// 通过 register 方法，将所需要的组件传入到业务模块中
	new(respo.Provider).Register(httpServer, app.GetConsole(), asset)

	app.RunConsole()
}

====================== app/module/provider.go ==================================

type Provider struct {
}

func (provider *Provider) Register(httpServer *http_server.Server, console console.Console, asset embed.FS) {
	// 注册一个 respo:test 命令
	console.RegisterCommand(new(command.Test))
	

	// 注册一些路由
	httpServer.RegisterRouters(func(engine *gin.Engine) {
		
	})

	// 使用静态资源
	file, _ := asset.ReadFile("asset/image/icon.png")

}

// 没有什么需要给外部暴露，也可省略不写
func (provider *Provider) Export() {
	
}
```