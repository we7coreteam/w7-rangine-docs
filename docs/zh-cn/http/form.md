### 验证支持

数据验证器采用的是 [go-playground/validator](https://github.com/go-playground/validator) 库，一些使用详细信息可查看其官方文档，[查看文档](https://pkg.go.dev/github.com/go-playground/validator/v10)。

### 获取数据

> 在实践开发中，不建议直接通过 gin 对象进行数据获取。

获取数据时，通过定义数据结构 + 验证的形式进行数据绑定。当然你也可以不写任何验证规则，只进行数据获取。


```
Validate(ctx *gin.Context, requestData interface{}) bool
```

验证数据时，会将 Query，Form，Raw 所有的数据都验证。不论你的值是通过哪一种方式提交，都可以正常的验证。需要注意的是，需要保证参数名称的唯一性。

#### 参数

- ctx gin 对象，直接传入即可
- requestData 验证数据结构及规则，成功后，通些此此值返回数据

#### 验证数据Tag定义

定义验证器结构时需要根据不同的数据源指定不同的Tag。

- form 获取 get post 参数，可以使用 default 默认值配置。 
- uri 获取路由中定义的 :id 数据
- json 获取以 Json 形式提交的数据

#### 示例

```
type Home struct {
	controller.Abstract
}

func (home Home) Manifest(ctx *gin.Context) {
	type ParamsValidate struct {
		// url 通过 get 或是 post 传递都可以正常验证
		Url string 	`form:"url,default=test" binding:"required,url"`
		// 获取通过路由定的 id 参数，/somerouter/:id
		Id int 		`uri:id` 
		// 获取通过 json 提交的 name 参数
		Name string 	`json:name`
	}
	params := ParamsValidate{}
	if !home.Validate(ctx, &params) {
		return
	}
	// 验证成功后，值绑定到此变量上
	fmt.Println(params.Url)
}
```