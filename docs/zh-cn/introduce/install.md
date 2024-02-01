### 安装

> 安装时，请替换版本号为最新版本

```
// 安装工具命令到 /tmp 目录，有需要可以自定义目录
export GOPATH=/tmp && go install github.com/we7coreteam/w7-rangine-go-skeleton@v1.1.0

// 创建项目
/tmp/bin/w7-rangine-go-skeleton make:project --name=github.com/donknap/w7-im-websocket-push --target-dir=../w7-im-websocket-push
```

-  -\-name 指定项目名称可以为 github.com/donknap/w7-im-websocket-push 或是 w7-im-websocket-push
- -\-target-dir 指定要生成的目录，未指定为当前目录

也可以直接将骨架项目直接 clone 下来。 

> clone 骨架项目时，有必要时，将项目中所有的包名替换成你自己的。

```
git clone https://github.com/we7coreteam/w7-rangine-go-skeleton.git
```


### Goland 配置

#### 模块加速

1. 打开菜单 Preferences -> Go -> Go Modules
2. 启用 Enable Go modules integration
3. 配置 Environment: GOPROXY=https://goproxy.cn

#### 编译配置

> 需要注意文件中的 <module name="w7-rangine-go-skeleton" /> 需要改成你自己的项目名称

我们提供了 Goland 的编译配置文件，位于 [.run/rangine.run.xml](https://github.com/we7coreteam/w7-rangine-go-skeleton/blob/main/.run/rangine.run.xml)。运行后将编译可执行文件到 bin 目录，并启动 server。

执行编译后，也可以通过 bin/rangine 命令去执行一些辅助命令。
