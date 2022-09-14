# Gin 框架知识汇总




Gin 框架

<!--more-->



## ## 安装

#### 要求

- Go 1.13 及以上版本

#### 安装

```shell
go get -u github.com/gin-gonic/gin
```



#### 引入

```go
import "github.com/gin-gonic/gin"
```



## 基本使用



#### 初始化

```go
 r := gin.Default()
```



#### 路由处理

1. 基本操作

```go
r.GET(relative Path，handler)
r.POST(relative Path，handler)
r.PUT(relative Path，handler)
r.DELETE(relative Path，handler)
```

2. 路由分组

```go
apiv1 := r.Group(relative Path)
{
  apiv1.GET(relative Path，handler)
	apiv1.POST(relative Path，handler)
	apiv1.PUT(relative Path，handler)
	apiv1.DELETE(relative Path，handler)
}
```




