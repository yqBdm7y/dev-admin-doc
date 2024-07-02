# 指南

## 快速上手

dev-admin-web

```bash
pnpm install # 安装依赖
pnpm run dev # 运行
```

dev-admin

```bash
go run main.go
```

## 方法

### 响应

dev-admin调用了devtool中的API库，并使其简化。

你的项目可以调用dev-admin的方法，从而使项目更加简洁。

#### 返回错误

示例：

```go
func (p Platform) Create(c *gin.Context) {
	result := d.Database[d.LibraryGorm]{}.Get().DB.Create(&form)
	if result.Error != nil {
		d.Gin{}.Error(c, dadmin.Err(result.Error))
		return
	}
}
```

我们使用 `d.Gin{}.Error(c, dadmin.Err(err))`这样的方式返回错误

#### 返回成功

```go
d.Gin{}.Success(c, dadmin.Success(form.ID))
```

### 登录

#### 调用

调用可以使用自定义的参数，也可以使用内置的默认参数。

不带参数的调用，使用默认参数

```go
authMiddleware, err := jwt.New(dadmin.LoginNew())
```

带参数的调用，以下只是一个示例，具体根据你的业务逻辑来设置参数

```go
authMiddleware, err := jwt.New(dadmin.LoginNew(
		dadmin.LoginWithTimeout(time.Second*5),
		dadmin.LoginWithMaxRefresh(time.Second*10),
		dadmin.LoginWithCustomFieldAccessToken("accessToken"),
		dadmin.LoginWithCustomFieldRefreshToken("refreshToken"),
		dadmin.LoginWithCustomFieldExpire("expires"),
	))
```

#### 可选参数

##### 加密盐

默认为随机生成的加密盐。

你可以设置自定义的加密盐，只需要在调用的时候附带 `dadmin.LoginWithKey("YOUR_KEY")` 作为参数，其中的YOUR_KEY替换为你需要自定义的加密盐。

```go
authMiddleware, err := jwt.New(dadmin.LoginNew(
		dadmin.LoginWithKey("YOUR_KEY"),
	))
```

##### 访问令牌过期时间

默认为2个小时。

你可以设置自定义的过期时间，只需要在调用的时候附带 `dadmin.LoginWithTimeout(time.Second*5)` 作为参数，其中的 `time.Second*5` 替换为你需要自定义的过期时间。

```go
authMiddleware, err := jwt.New(dadmin.LoginNew(
		dadmin.LoginWithTimeout(time.Second*5),
	))
```

##### 刷新令牌过期时间

默认为24个小时（一天）。

你可以设置自定义的过期时间，只需要在调用的时候附带 `dadmin.LoginWithMaxRefresh(time.Second*10)` 作为参数，其中的 `time.Second*10` 替换为你需要自定义的过期时间。

```go
authMiddleware, err := jwt.New(dadmin.LoginNew(
		dadmin.LoginWithMaxRefresh(time.Second*10),
	))
```

##### 自定义访问令牌的字段名

默认为 `token`。

你可以设置自定义的字段名，只需要在调用的时候附带 `dadmin.LoginWithCustomFieldAccessToken("accessToken")` 作为参数，其中的 `accessToken` 替换为你需要自定义的字段名。

```go
authMiddleware, err := jwt.New(dadmin.LoginNew(
		dadmin.LoginWithCustomFieldAccessToken("accessToken"),
	))
```

##### 自定义刷新令牌的字段名

默认为 `refresh_token`。

你可以设置自定义的字段名，只需要在调用的时候附带 `dadmin.LoginWithCustomFieldRefreshToken("refreshToken")` 作为参数，其中的 `refreshToken` 替换为你需要自定义的字段名。

```go
authMiddleware, err := jwt.New(dadmin.LoginNew(
		dadmin.LoginWithCustomFieldRefreshToken("refreshToken"),
	))
```

##### 自定义令牌过期时间的字段名

默认为 `expire`。

你可以设置自定义的字段名，只需要在调用的时候附带 `dadmin.LoginWithCustomFieldExpire("expires")` 作为参数，其中的 `expires` 替换为你需要自定义的字段名。

```go
authMiddleware, err := jwt.New(dadmin.LoginNew(
		dadmin.LoginWithCustomFieldExpire("expires"),
	))
```
