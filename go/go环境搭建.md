# go环境搭建

- [go环境搭建](#go环境搭建)
  - [go在window下的安装](#go在window下的安装)
    - [配置go proxy](#配置go-proxy)

## go在window下的安装
![https://studygolang.com/articles/24277](https://studygolang.com/articles/24277)
关键点： 
1. goroot gopath 环境变量
2. 路径下设置好文件

### 配置go proxy
$ go env -w GO111MODULE=on
$ go env -w GOPROXY=https://goproxy.cn,direct
>  七牛云提供的代理[https://goproxy.cn/](https://goproxy.cn/)