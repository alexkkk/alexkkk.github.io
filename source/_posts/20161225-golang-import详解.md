---
title: golang-import详解
date: 2016-12-25 22:42:34
tags: Go
categories: Go
---

## 如何import包？
golang中import包比较简单，如下：

```go
import "fmt"    //这种情况会默认在GO的GOPATH目录下进行全局查找
import "./fmt"  //这种情况会在相对目录进行查找
import "/fmt"   //这种情况会在绝对路径进行查找

```

## 如果包重名怎么办？
golang可以很容易地通过别名区分同名的包，如下：
```go
import global_fmt "fmt"
import my_fmt "./fmt"
```

## 包名太长怎么办？
有时候在引入包的时候，包名很长写的很烦，能不能省掉？回答：必须可以啊
```go
import . "fmt"
```
将fmt 启用别名"."，这样就可以直接使用其内容，而不用再加fmt.
例如fmt.Println("")可以直接写成Println("")

## import _ "fmt"是什么鬼？
正常import包`import "fmt"`会执行包种的一些变量的声明和初始化，然后再执行init()函数
`import _ "fmt"`表示只执行`fmt`包里的`init`函数
