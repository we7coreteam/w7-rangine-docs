### 输出日志

> default 通道可直接使用 log/slog 进行日志输出


```
slog.Info("test log", "临时目录", os.TempDir())
```

### 其它通道日志输出

```
log, _ := facade.GetLoggerFactory().Channel("db")
log.Info("route middleware test, req time: " + time.Now().String())
```

### 最佳实践

写入日志时，会将 debug 级别的日志输出到控制台上显示，并写入到 run.log 日志文件中，

info 级别的日志只会写入到 run.log 文件中。

```
log:
  default:
    driver: stack
    channels:
      - file
      - console
  file:
    driver: file
    path: run.log
    level: info
  console:
    driver: console
    level: debug
```

### 配置日志驱动

#### stack

用于将日志分发到多个通道中

```
driver: stack
channels:
  - file
  - console
```

#### file

日志将写入到文件中，目录位于当前目录下的 runtime/logs 目录下。配置如下：

```
driver: file  	# 指定驱动 file or console
path: run.log # 指定 file 文件名称
level: debug 	# 指定输出日志级别
max_size: 2 	# 指定日志切割大小
```

#### console

日志将写入到 stdout 中，直接在控制台中输出，方便开发时进行调试。


```
driver: console
level: debug 	# 指定输出日志级别
```

#### 自定义驱动

##### 注册

在 main.go 中对日志驱动进行注册

```
app.GetLoggerFactory().RegisterDriver("db", func(config logger.Config) (logger.Driver, error) {
		fields := helper.ValidateAndGetErrFields(config)
		if len(fields) > 0 {
			return nil, errors.New("log config error, reason: fields: " + strings.Join(fields, ","))
		}
		level, err := zapcore.ParseLevel(config.Level)
		if err != nil {
			return nil, err
		}
		atomicLevel := zap.NewAtomicLevelAt(level)
		return &DbDriver{
			levelEnabler: atomicLevel,
		}, nil
	})
```

db_driver.go 

```
type DbDriver struct {
	logger.Driver
	levelEnabler zapcore.LevelEnabler
}
func (d DbDriver) Write(level zapcore.Level, enc zapcore.Encoder, ent zapcore.Entry, fields []zapcore.Field) error {
	if !d.levelEnabler.Enabled(level) {
		return nil
	}
	// 插入数据库记录sql
	return nil
}

func (DbDriver) Sync() error {
	return nil
}
```