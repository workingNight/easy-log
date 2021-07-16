## go session 和  store

缘由：http协议是无状态的，需要一些数据去标识用户身份

服务器使用session id来标识session，session id由服务器负责产生，保证随机性与唯一性，相当于一个随机密钥，避免在握手或传输中暴露用户真实密码

cookie: 会话cookie 和持久cookie,保存在内存和保存在硬盘

session和cookie的区别，cookie是保存在浏览器端的，session是保存在session对象中的，把sessionid返回给浏览器，浏览器每次发请求携带sessionid就行。

如何发送sessionid，一般有2种方法，cookie和url重写。

1. cookie 响应头 Set-Cookie: xxx
2. url重写，给返回用户每个页面的所有url后面都追加session标识符



## session管理设计

- 全局session管理器
- 保证sessionid 的全局唯一性
- 为每个客户关联一个session
- session 的存储(可以存储到内存、文件、数据库等)
- session 过期处理



## go 中cookie

Go语言中通过net/http包中的SetCookie来设置：

```Go
http.SetCookie(w ResponseWriter, cookie *Cookie)
//ResponseWriter 经典响应的对象
```

cookie是一个结构体

```Go
type Cookie struct {
    Name       string
    Value      string
    Path       string
    Domain     string
    Expires    time.Time
    RawExpires string

// MaxAge=0 means no 'Max-Age' attribute specified.
// MaxAge<0 means delete cookie now, equivalently 'Max-Age: 0'
// MaxAge>0 means Max-Age attribute present and given in seconds
    MaxAge   int
    Secure   bool
    HttpOnly bool
    Raw      string
    Unparsed []string // Raw text of unparsed attribute-value pairs //未解析的原生键值对
}
```