软擎提供一套简单的错误助手函数，位于 err_handler 包下面。

### 检测是否是错误

```
func Found(err error) bool
```

#### 示例

```
if err_handler.Found(err) {
	auth.JsonResponseWithError(ctx, err, 500)
	return
}
```

### 抛出错误

```
func Throw(message string, previous error) error
```

抛出错误时，会自动附加 traceback 

#### 示例

```
func test() error {
	// 错误中包含了 traceback 信息
	return err_handler.Throw("这里是错误，我抛出了", nil)
}
```