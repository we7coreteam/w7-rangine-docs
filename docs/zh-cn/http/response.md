## 响应方法

### 成功返回

```
JsonSuccessResponse(ctx *gin.Context)
```

##### 示例

```
func (home Home) Manifest(ctx *gin.Context) {
	home.JsonSuccessResponse(ctx)
	return
}
```

##### 返回 

```
{
    "code": 200,
    "data": "success",
    "msg": ""
}
```

### 有数据的成功返回

```
JsonResponseWithoutError(ctx *gin.Context, data any)
```

##### 示例

```
func (home Home) Manifest(ctx *gin.Context) {
	home.JsonResponseWithoutError(ctx, map[string]string{"name": "XiaoWang"})
	return
}
```

##### 返回 

```
{
    "code": 200,
    "data": {
        "name": "XiaoWang"
    },
    "msg": ""
}
```

### 统一错误返回

```
JsonResponseWithServerError(ctx *gin.Context, err error)
```

##### 示例

```
func (home Home) Manifest(ctx *gin.Context) {
	home.JsonResponseWithServerError(ctx, errors.New("内部错误"))
	return
}
```

##### 返回 

```
{
    "code": 500,
    "data": "",
    "msg": "内部错误"
}
```

### 自定义错误返回

```
JsonResponseWithError(ctx *gin.Context, err error, statusCode int)
```

##### 示例

```
func (home Home) Manifest(ctx *gin.Context) {
	home.JsonResponseWithError(ctx, errors.New("内部错误"), 404)
	return
}
```

##### 返回 

```
{
    "code": 404,
    "data": "",
    "msg": "内部错误"
}
```

### 自定义返回

```
JsonResponse(ctx *gin.Context, data any, error error, statusCode int) 
```

以上所有方法的基础方法

## 自定义响应格式

如果不满足于系统规定的返回数据格式，可以自定义成功和失败时的返回的数据格式。这些配置是全局配置，所以需要在 main.go 中进行注册。

在下面的示例中，发生错误时，返回 http 500 的状态码，并返回 {"error":"错误信息"} 的数据格式。在成功时，则返回 200 的状态码，并将成功数据放到 data 节点中返回。

```
SetErrResponseHandler()

SetSuccessResponseHandler()
```

#### 示例

```
func main() {
	app := app.NewApp()

	response.SetErrResponseHandler(func(ctx *gin.Context, env string, err error, statusCode int) {
		ctx.JSON(500, map[string]interface{}{
			"error": err.Error(),
		})
	})
	
	response.SetSuccessResponseHandler(func(ctx *gin.Context, data any, statusCode int) {
		ctx.JSON(200, map[string]interface{}{
			"data": data,
		})
	})
}
```