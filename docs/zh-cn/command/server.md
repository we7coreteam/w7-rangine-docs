### 启动服务

```
./bin/rangine server:start -f config.yaml
```

- -f 指定配置文件路径
- -e 覆盖配置文件中的某些配置，一般用于配置适配不同环境变量，例如：-e server.http.port=8888，当然也可以直接使用环境变量 -e server.http.port=${HTTP_PORT}

### 热启动服务

```
./reload.sh
```