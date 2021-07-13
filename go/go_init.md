# go


## go在window下的安装
[https://studygolang.com/articles/24277](https://studygolang.com/articles/24277)    
关键点： 
1. goroot gopath 环境变量
2. 路径下设置好文件

### 配置go proxy
$ go env -w GO111MODULE=on
$ go env -w GOPROXY=https://goproxy.cn,direct
>  七牛云提供的代理[https://goproxy.cn/](https://goproxy.cn/)



## go 模块的理解
go get xxx之后依然还是，下划波浪线，依然没有引入成功。
goproxy.go:7:2: no required module provides package github.com/goproxy/goproxy: go.mod file not found in current directory or any parent directory; see 'go help modules'
>  估计的原因是模块化没有理解透，  go mod?
- 这个go mod 类似 js中的node_modules？ 添加依赖？
- 使用方法，在弄一个新项目的时候先   go mod init you_github.com/projectname
- 声明一个package main  这里将放执行代码的主入口

先把go官网的例子都弄完再说把
- When you need your code to do something that might have been implemented by someone else, you can look for a package that has functions you can use in your code.如果你想写的代码是别人已经实现了的,你可以找到这个包并引入和使用
- pkg.go.dev 可以在这个网站寻找别人已经发布的模块
- go mod tidy 
  ![](C:\Users\qq231\Pictures\md_img)