### 文件

配置文件位于根目录 [config.yaml](https://github.com/we7coreteam/w7-rangine-go-skeleton/blob/main/config.yaml)

启动服务时需要使用 -f 参数指定配置文件路径，如果运行一些需要配置的辅助命令时，也需要指定配置文件。比如生成数据库 model 文件时。

当然也可以在 main.go 中采用 go:embed 的形式将配置文件嵌入到程序内部中。
```
//go:embed config.yaml
var ConfigFile []byte

func main() {
	app := app.NewApp(app.Option{
		Name: "w7-rangine-go-skeleton",
		DefaultConfigLoader: func(config *viper.Viper) {
			config.SetConfigType("yaml")
			// 如果你也需要解析yaml配置中的环境变量，需要调用下面方法进行解析
			// ParseConfigContentEnv(b []byte)
			config.ReadConfig(bytes.NewReader(ConfigFile))
		},
	})
}
```

### 环境配置

在开发的过程中，面对不同环境时（开发环境、内测环境、正式环境），需要指定不同的配置信息。基于这样的需求，在配置 config.yaml 文件时，支持采用环境变量的方式来配置。

#### 环境变量默认值

> 此写法是 shell 中的标准写法，也可用于在系统中的环境变量配置

- ${FOO}  表示采用 FOO 这个环境变量的值，如果没有配置则为空
- ${FOO-test} 表示当 FOO 没有定义时，采用 test 这个默认值
- ${FOO:-test} 表示当 FOO 没有定义或为空时，采用 test 这个默认值

#### 示例

```
database:
  default:
    driver: mysql
    user_name: ${MYSQL_USERNAME-root}
    password: ${MYSQL_PASSWORD-123456}
    host: ${MYSQL_ADDRESS-127.0.0.1}
    port: 3306
    db_name: ${MYSQL_DATABASE-test}
    charset: utf8mb4
    prefix: ims_
```


### 最佳实践

#### 本地开发

本地开发时，可指定环境变量的默认值来使用配置。也可以通过 Ide 提供的编译配置中增加环境变量。

#### 线上环境

可以通过以下两种方式注入环境变量

```
export FOO=123 && rangine server:start -f config.yaml 
``` 

或是把环境变量写入 .env 文件中

```
FOO=test
NAME=xiaoli
AGE=16
```

```
source .env && rangine server:start -f config.yaml 
```

（不推荐）也可以将环境变量直接配置到 /etc/profile 或是 /etc/bashrc 中。

#### docker

镜像部署下可采用 -e 的形式将环境变量注入

```
docker run -e FOO=test ....
```