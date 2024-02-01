### 实例化 App

使用软擎时，需要先在 main.go 中实例化一个 app 对象，并将他运行起来。可以理解为 main 函数将所有 provider 注册并串联在一起。每个独立的 provider 之间并不知道对方的存在，如果需要一些外部依赖，那么 main 函数会将所有需要的组件或是 provider 初始化，再通过 register 函数传递到每个 provider 中。

在软擎中，也会集成一些常用的组件(log event)或是服务（http tcp），假如提供的这些组件并不能很好的胜任你的需要，那么你也可以自行创建组件，在 main 中完成 provider 初始化工作。


当然，一些系统中全局的配置也需要在 main 函数中完成。每个 provider 只负责自己业务涉及到的部分。

```
func main() {
	app := app.NewApp(app.Option{
		Name: "w7-rangine-go-skeleton",
	})

	// 初始化一些组件，系统提供或是自己开发
	// 需要对外部提供对象使用的，通过 export 方法暴露出来
	.....

	// 注册一些业务 provider
	// 业务 provider 中，或多或少的需要用到上面初始化的组件
	// 在注册业务 provider 时，通过 register 方法将所需要的外部组件传入进去
	.....


	app.RunConsole()
}

```

#### app.Option 配置

- Name 指定系统的名称
- Version 指定系统的版本号
- DefaultConfigLoader 用于个性化的载入系统配置

### 门面 facade

#### 获取控制台对象

```
console := facade.GetConsole()
console.RegisterCommand(new(command.Test))
```

#### 获取配置对象

```
config := facade.GetConfig()
config.GetString("database.default.driver")
```

#### 获取容器对象

```
container:= facade.GetContainer()
```


#### 获取日志对象

```
logger := facade.GetLoggerFactory()
```

#### 获取事件对象

```
event := facade.GetEvent()
```

#### 获取Redis对象

```
event := facade.GetRedisFactory()
```

#### 获取数据库对象

```
event := facade.GetDbFactory()
```