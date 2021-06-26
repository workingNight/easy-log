##  git配置别名

配置一个git last，让其显示最后一次提交信息：
git config --global alias.last 'log -1'    

git lg    
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"   

很多人都用co表示checkout，ci表示commit，br表示branch   
git config --global alias.co checkout   
git config --global alias.ci commit  
git config --global alias.br branch  


[参考文章，廖雪峰git](https://www.liaoxuefeng.com/wiki/896043488029600/898732837407424)
