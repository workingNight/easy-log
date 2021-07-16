## 看ahooks的readme所得

1. 图片直接用image,而不是 \!\[\]\(url\)

2. 巧用:beer: 这种icon

3. [![NPM version][image-1]][1] [![NPM downloads][image-2]][2] 顶部的这个说明

4. 用## 2级标题， 一个顶级#

5. 中英文切换 English | [简体中文](English | [简体中文\](url\)

   其实就是建2个文件，点击跳转

6. github上的md文件能够解析一些html标签，table，img等。不确定别的地方的img是否ok



### editorconfig

```
# http://editorconfig.org
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

#防止md文件换行失效
[*.md]
trim_trailing_whitespace = false

[Makefile]
indent_style = tab
```



### gitigonre

```
yarn.lock
dist
es
lib
.docz
node_modules
.history
.idea
.vscode
coverage
.doc
.DS_Store
.umi
.umi-production
page
lerna-debug.log
tsconfig.tsbuildinfo
packages/hooks/README.md
可见node_modules是不用写/node_modules
yarn.lock文件也是需要忽略的
```



### npmrc

文件直接配置npm的镜像仓库

registry = https://registry.npm.taobao.org



### gulpfiile.js

```
gulp.series('clean', 'cjs', 'es', 'declaration', 'copyReadme');
```

清除lib es dist目录

const ts = require('gulp-typescript');



