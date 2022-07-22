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

