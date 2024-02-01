### 注册模块路由

路由注册采用的是 [gin-gonic/gin](https://github.com/gin-gonic/gin) 库，一些使用详细信息可查看其官方文档，[查看文档](https://gin-gonic.com/zh-cn/docs/)。

所有模块内的路由，统一在 provider.go 文件中注册，注册路由时，需要外部传入 HttpServer 对象。通过 httpServer.RegisterRouters() 方法注册路由，如下：

```
func (provider *Provider) Register(httpServer *http_server.Server, console console.Console) {
	// 注册一些路由
	httpServer.RegisterRouters(func(engine *gin.Engine) {
		engine.GET("/home/index", middleware.Home{}.Process, controller.Home{}.Index)
	})
}
```


### 注册全局路由

有些路由，比如 404页面、fav icon 这些属于全局的路由，我们可以在 main.go 中初始化完 HttpServer 对象后，进行全局的路由注册，如下：

```
func main() {
	app := app.NewApp(app.Option{
		Name: "w7-rangine-go-skeleton",
	})

	httpServer := new(http.Provider).Register(app.GetConfig(), app.GetConsole(), app.GetServerManager()).Export()
	
	// 注册一些全局中间件，路由或是其它一些全局操作
	httpServer.Use(middleware.GetPanicHandlerMiddleware())

	httpServer.RegisterRouters(func(engine *gin.Engine) {
		engine.NoRoute(func(context *gin.Context) {
			context.String(404, "404 Not Found")
		})
	})

	app.RunConsole()
}

```