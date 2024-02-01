

> 执行部分命令需要使用配置参数时，请通过 -f config.yaml 进行指定

### 生成一个业务模块

```
./bin/rangine make:module [flags]
```

- \-\-name 要创建的模块名称


### 在模块内生成一个 command 命令

```
./bin/rangine make:cmd [flags]
```

- \-\-name 命令名称，一般执行命令为 模块名:命令名
- -\-module-name 模块名称，指定要生成在哪个业务模块内


### 生成数据表 Entity 与 DAO

生成的文件存在于 common/entity 目录，此文件为自动生成，不建议直接修改。使用时在业务中新建 model 文件继承 entity 使用。

```
./bin/rangine make:model [flags]
```

- \-\-table-name 数据表名，不指定时默认为生所有表
- -\-db-channel 指定数据库连接，默认为 default
- -\-yaml-file 根据指定的Yaml配置文件生成 model 及 dao 文件，支持关联


### 查看所有注册路由

```
./bin/rangine route:list
```

### 查看帮助信息

```
./bin/rangine -h
```

### 查看框架版本

```
./bin/rangine version
```


### 编译

```
# osx
make build-osx 

# linux
make build

# windows
make build-windows
```
