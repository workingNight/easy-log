# git



## git 拉取分支并关联远程库
git checkout -b xxx
git branch --set-upstream-to origin/xxx

## sourceTree
导入本机的ssh公钥，要在上面的栏中选中工具-》选项-》然后选择ssh


好像还有一个git push -u origin master是啥作用来着。。。

## git 一点补充

## 把本地项目推到自己的github
git remote add origin git@github.com:yao-yue/go_temp.git
git branch -M main
git push -u origin main
> 关于-M的解释  --move --force 
> --move  Move/rename a branch and the corresponding reflog.
> --force  allow renaming the branch even if the new branch name already exists
> 理解就是强行新建一个分支main，即使已经有了

>关于-u的解释
> -u
>--set-upstream  的简写
>For every branch that is up to date or successfully pushed, add upstream (tracking) reference, used by argument-less git-pull[1] and other commands. For more information, see branch.<name>.merge in git-config[1].


## 创建一个新的仓库并关联
echo "# go_temp" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:yao-yue/go_temp.git
git push -u origin main

## 本地创建一个dev分支和远程dev分支进行关联
git chekcout -b develop
git branch --set-upstream-to origin/develop


## 关于解决冲突和融合
git checkout master
git merge develop
有冲突解决冲突、
git push 
git checkout develop  


## git修改命令行快捷键、配置别名
```
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD'
git config --global alias.last 'log -1'
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

```

## 参考链接
[git文档](https://git-scm.com/docs/git-push)