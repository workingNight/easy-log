# typora 小技巧





## windows右键生成md文件

创建一个txt文本，

```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\.md]
@="typora.md"
"icon"="E:\\Typora\\bin\\typora.exe"

[HKEY_CLASSES_ROOT\.md\OpenWithProgids]
"Typora.md"=""
"VSCode.md"=""

[HKEY_CLASSES_ROOT\.md\ShellNew]
"NullFile"=""
```

文件改名.reg后缀

双击执行



## 搭建图库，七牛云图库



