---

weight: 1
title: "Go 常用库知识"
subtitle: ""
date: 2022-07-22T10:36:26+08:00
lastmod: 2022-07-22T10:36:26+08:00
draft: false
author: ""
authorLink: ""
description: ""
license: ""
images: []

tags: ["Go"]
categories: ["Tips"]

---

记录 Go 常用的一些库相关知识。 

<!--more-->



## time

### time.NewTicker定时器

> NewTicker 会返回一个新的 Ticker

- ticker.C 

  该 Ticker 会包含一个通道字段，并会每隔时间断 d 就向该通道发送当前时间

- ticker.Reset

  重置计时器，使 t 重新计时

- ticker.Stop

  停止计时器



**示例代码**

```shell
package main

import (
    "fmt"
    "time"
)

func main() {

    //创建一个周期性的定时器
    ticker := time.NewTicker(3 * time.Second)
    fmt.Println("当前时间为:", time.Now())

    go func() {
        for {

            //从定时器中获取数据
            t := <-ticker.C
            fmt.Println("当前时间为:", t)

        }
    }()

    for {
        time.Sleep(time.Second * 1)
    }
}
```





## Casbin



`casbin` 是一个强大、高效的访问控制库。

#### 安装方式

版本 `v2`

```shell
go get github.com/casbin/casbin/v2
```



#### 使用方式

`casbin` 将访问控制模型抽象在基于 PERM 模型的配置文件中。如果需要改变授权机制，只需要更改配置文件即可。

配置文件分为 `model.conf` 和 `policy.csv` 

`policy` 定义具体的规则

`request` 是对访问请求的抽象，它的参数与 **e.Enforce()** 函数的参数一一对应

`matcher` 用于将请求和定义的规则一一匹配，生成匹配结果

`effect` 根据匹配结果进行结果汇总，用于决定允许还是拒绝这次请求



#### 示例1(ACL模型)

**model.conf**

```go
[request_definition]
r = sub, obj, act

[policy_definition]
p = sub, obj, act

[matchers]
m = r.sub == p.sub && r.obj == p.obj && r.act == p.act

[policy_effect]
e = some(where (p.eft == allow))
```



**policy.csv**

```go
p, dajun, data1, read
p, lizi, data2, write
```



**main.go**

```go
package main

import (
  "fmt"
  "log"

  "github.com/casbin/casbin/v2"
)

func check(e *casbin.Enforcer, sub, obj, act string) {
  ok, _ := e.Enforce(sub, obj, act)
  if ok {
    fmt.Printf("%s CAN %s %s\n", sub, act, obj)
  } else {
    fmt.Printf("%s CANNOT %s %s\n", sub, act, obj)
  }
}

func main() {
  e, err := casbin.NewEnforcer("./model.conf", "./policy.csv")
  if err != nil {
    log.Fatalf("NewEnforecer failed:%v\n", err)
  }

  check(e, "dajun", "data1", "read")
  check(e, "lizi", "data2", "write")
  check(e, "dajun", "data1", "write")
  check(e, "dajun", "data2", "read")
}
```

**Result**

```shell
$ go run main.go
dajun CAN read data1
lizi CAN write data2
dajun CANNOT write data1
dajun CANNOT read data2
```





#### 示例2（RBAC）

在 `casbin` 中使用 `RBAC` 模型需要在模型文件中添加 `role_definition` 模块,同时修改 `matchers` 模块：

```shell
[role_definition]
g = _, _

[matchers]
m = g(r.sub, p.sub) && r.obj == p.obj && r.act == p.act
```

**policy.csv**

```shell
p, admin, data, read
p, admin, data, write
p, developer, data, read
g, dajun, admin
g, lizi, developer
```

**main.go**

```go
package main

import (
  "fmt"
  "log"

  "github.com/casbin/casbin/v2"
)

func check(e *casbin.Enforcer, sub, obj, act string) {
  ok, _ := e.Enforce(sub, obj, act)
  if ok {
    fmt.Printf("%s CAN %s %s\n", sub, act, obj)
  } else {
    fmt.Printf("%s CANNOT %s %s\n", sub, act, obj)
  }
}

func main() {
  e, err := casbin.NewEnforcer("./model.conf", "./policy.csv")
  if err != nil {
    log.Fatalf("NewEnforecer failed:%v\n", err)
  }

  check(e, "dajun", "data", "read")
  check(e, "dajun", "data", "write")
  check(e, "lizi", "data", "read")
  check(e, "lizi", "data", "write")
}
```

**Result**

```shell
dajun CAN read data
dajun CAN write data
lizi CAN read data
lizi CANNOT write data
```



