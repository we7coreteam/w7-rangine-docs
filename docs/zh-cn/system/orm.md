### 支持

Orm 组件是采用的 [go-gorm/gorm](https://github.com/go-gorm/gorm) 库，一些使用详细信息可查看其官方文档，[查看文档](https://gorm.io/zh_CN/docs/)。

### 生成

可通过 make:model 命令生成相应表结构的 orm 文件，生成文件位于 /common/dao 与 /common/entity 目录，如果有自定义类型生成文件位于 /common/accessor 。[查看生成命令](https://wiki.w7.com/document/2315/7789#h_Entity_23)

#### 通过配置文件生成

通过配置需要使用 [gorm-gen-yaml](https://github.com/we7coreteam/gorm-gen-yaml) 插件，具体配置文件，请查看[配置文件](https://github.com/we7coreteam/gorm-gen-yaml/blob/main/gen.yaml)。

### 使用

使用 gorm model 时，需要在 main 中先指定 db 对象

```
func main() {
	app := app.NewApp(app.Option{
		Name: "w7-rangine-go-skeleton",
	})

	// .... 省略一些代码

	db, _ := facade.GetDbFactory().Channel("default")
	dao.SetDefault(db)

	// 注册业务 provider，此模块中需要使用 http server 和 console
	new(respo.Provider).Register(httpServer, app.GetConsole())
	app.RunConsole()
}
```

### 简单查询

```
dao.Q.student.where(dao.Q.student.ID.eq(1)).preload(dao.Q.student.user).first()
```