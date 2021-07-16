# cookie
一些补充

[TOC]




## 描述
一个存储在浏览器中的小型文本文件，如果是长生命的cookie也可以存在本地文件里面





## cookie是怎么来的

1. 客户端请求
2. 服务器在响应头里面加上Set-Cookie字段
3. 浏览器收到响应后保存下Cookie字段，
4. 之后对该服务器每次请求中都通过Cookie字段将Cookie信息发给服务器




## 字段
Name Value
Expires
Path
Max-Age
Domain
Path
Socure
HTTPOnly
SameSite  {
	Strict 
	Lax
	None
}
Lax模式 post\iframe\ajax\image也不会发送第三方cookie