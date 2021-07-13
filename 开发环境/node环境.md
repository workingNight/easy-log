
# node
- [node](#node)
  - [nvm](#nvm)
    - [安装](#安装)
    - [使用](#使用)
  - [node cnpm启用阿里源](#node-cnpm启用阿里源)
  - [参考链接](#参考链接)

## nvm 


### 安装
1. [https://github.com/coreybutler/nvm-windows/releases](https://github.com/coreybutler/nvm-windows/releases)
2. nvm-setup.zip 
3. settings.txt 配置阿里源
   ```
    node_mirror: https://npm.taobao.org/mirrors/node/
    npm_mirror: https://npm.taobao.org/mirrors/npm/
   ```
4. nvm -v 检查一下安装是否成功
5. nvm install  xxx  安装一个版本的node
6. nvm use xxx


### 使用
nvm off                     // 禁用node.js版本管理(不卸载任何东西)
nvm on                      // 启用node.js版本管理
nvm install <version>       // 安装node.js的命名 version是版本号 例如：nvm install 8.12.0
nvm uninstall <version>     // 卸载node.js是的命令，卸载指定版本的nodejs，当安装失败时卸载使用
nvm ls                      // 显示所有安装的node.js版本
nvm list available          // 显示可以安装的所有node.js的版本
nvm use <version>           // 切换到使用指定的nodejs版本
nvm v                       // 显示nvm版本
nvm install stable          // 安装最新稳定版




## node cnpm启用阿里源
npm install -g cnpm --registry=https://registry.npm.taobao.org

## 安装yarn
npm install -g yarn
### 关于执行报错 ，配置权限
get-ExecutionPolicy
set-ExecutionPolicy RemoteSigned
[参考文章](https://blog.csdn.net/zlq_CSDN/article/details/102789989)

## 参考链接
[nvm安装](https://blog.csdn.net/qq_30376375/article/details/115877446)