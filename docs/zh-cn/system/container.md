### 支持

Container 组件采用的是 [golobby/container](https://github.com/golobby/container) 组件，通过容器功能，可以全局共享对象。

### 添加一个单例对象

```
type Object struct {
	Name string
}

err := facade.GetContainer().NamedSingleton("name", func() *Object {
	obj := &Object{}
	return obj
})
if err != nil {
	panic(err)
}
```

### 添加一个工厂

```
// 还是刚才那个 Obj 结构

err := facade.GetContainer().NamedTransient("name1", func() *Object {
	obj := &Object{}
	return obj
})
if err != nil {
	panic(err)
}
```

### 获取这个对象

根据不同的名字可以获取到刚才放入容器的对象

```
var obj *Obj
err := facade.GetContainer().NamedResolve(&obj, "name")
```