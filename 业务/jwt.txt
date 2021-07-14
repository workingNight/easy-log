# jwt

## 是什么
jwt  全称 json web tocken.
由三部分组成： header  payload  signature组成
header 
alg: 使用的算法
typ: JWT  类型

payload： 一些携带的数据。

signature 由header和payload，然后加上screct组成，screct是存放在服务器端的。	
加上这个签名是为了保证token在传输过程中没有被篡改或者损坏

## 为什么
解决传统session机制的一些弊病，具有xxx优点

## session 对比
session 是服务器拿到用户信息数据后，进行生成session数据，然后返回sessionid给客户端，
然后客户端每次请求都携带这个sessionid来表明自己的身份。带来的缺点，每次服务器拿到sesision
id之后要进行查询操作。
sessionid是存储在cookie中的，跨域不友好，需要描述生效domain，也容易被获取后被跨站脚本
伪造请求（csrf)
session在分布式部署中，多机共享session不友好。
session可以主动清除session.
jwt


## 怎么做
推荐的做法：
1. 因为考虑吧到cookie丢失就白哦是身份可以被伪造，故官方建议的使用方式是存放在localStorage
并在请求头中发送
2. 不要存放敏感信息在token里面
3. exp时效不要设定太长
4. 开启http only预防xss攻击
> 补充  如果cookie中设置了httponly属性，那么通过js脚本将无法读取到cookie信息，这样
> 能有效防止xss攻击。
> response.setHeader("Set-Cookie", "cookiename=value; Path=/;Domain=domainvalue;Max-Age=seconds;HTTPOnly");
5.  重播攻击, 要尽量jwt 和 session-id 的过期时间要尽量的短.， 黑名单机制
6.  用https防范中间人攻击

## 零碎
http 头里的 Authorization 加上 Bearer 

编写koa中间件，实现用户认证与授权。一般一些框架都有对应的jwt包，gin也有对应的这个包，
我们主要是需要了解这方面的知识，他解决的问题。和session对比


## 参考文章
[https://juejin.cn/post/6844903918879670279](https://juejin.cn/post/6844903918879670279)
[session和jwt对比](https://bbs.huaweicloud.com/blogs/199487)