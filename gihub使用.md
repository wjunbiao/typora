1、Git的诞生
Linus创建了Linux，后来不满于集中式的控制系统（SVN，CVS），自己花费两周时间用c语言写了Git。
svn：SVN是subversion的缩写，是一个开放源代码的版本控制系统，通过采用分支管理系统的高效管理，简而言之就是用于多个人共同开发同一个项目，实现共享资源，实现最终集中式的管理。
cvs：CVS是一个C/S系统，是一个常用的代码版本控制软件。主要在开源软件管理中使用。与它相类似的代码版本控制软件有subversion（svn）。

2、集中式vs分布式
集中式版本控制系统目前在用的就是svn（免费），和IBM的ClearCase和微软的VSS。
分布式的除了Git，还有 BitKeeper，还有类似Git的Mercurial和Bazaar等。

3、安装Git
默认安装，然后在Git bash中输入：
$ git config --global user.name “Your Name”
$ git config --global user.email “email@example.com”
验证一下：
查看当前的用户名：git config user.name
查看当前的邮箱：git config user.email





4、基本操作
创建一个空目录：mkdir learngit
切换到D盘：cd /D
切换到上一级：cd ..
切换到根目录：cd \
查看当前目录有什么内容 ：dir
查看当前路径：pwd
把当前目录变成Git 可以管理的仓库：git init
编写一个readme.txt文件：vi readme.txt (退出编写模式使用：wq)
查看readme.txt文件 ： cat readme.txt







5、中级操作
把文件放入仓库两步：
1、git add readme.txt
2、git commit -m "提示内容"

由近到远查看提交日志：git log	或	git log --pretty=online（退不出来按Q）
回退到上一个版本 git reset --hard HEAD^
回退到上上一个版本 git reset --hard HEAD^^
回退到上100个版本写成 git reset --hard HEAD~100
回退到后面的操作：只要上面没有关闭上面的git log 还是可以看见前面的id 的就可以回退到某个版本：git reset --hard 385af3a(版本前面的那几个数字)
但是如果已经关闭了，不知道id是什么了可以用 ：git reflog 查看你的每一次命令，这样就又可以用id 回退到之前的版本了。

写错后丢弃工作区的修改：git checkout -- readme.txt(回到版本库或者暂存区，总之就是最近一次的状态)
把暂存区的修改回退到工作区：git reset HEAD readme.txt

删除文件
在工作区删除：rm readme.txt（发现删错还可以用git checkout -- readme.txt从版本库里拿一份恢复）
在版本库中删除：git rm readme.txt, git commit -m "删除"



6、高级操作
关联远程库：git remote add origin git@github.com:wjunbiao/learn.git
查看关联情况：git remote -v
生成密钥：ssh-keygen -t rsa -b 4096 -C "2300825413@qq.com"
本地库的所有内容推送到远程库：git push -u origin master(第一次加上 -u参数，这样可以把本地的master 分支和远程的master 分支关联起来，之后在提交就不用加-u了)
本地提交：git push origin master
删除远程库（断开连接）：git remote rm origin
克隆远程库到本地：git clone git@github.com:wjunbiao/learn.git
创建分支：git branch dev
切换分支：git checkout dev	/	git switch master
创建并切换分支 ：git branch -b dev 	/	git switch -c dev
查看分支：git branch 
合并分支（指定分支到当前分支）：git merge dev
删除分支：git branch -d dev  
查看分支合并情况：git log --graph --pretty=oneline --abbrev-commit
禁用Fast forward 模式的合并分支：git merge --no-ff -m "提示信息" dev
把当前工作区储藏起来：git stash
把工作区恢复同时删除原来的stash:git stash pop (等同于git stash apply , git  stash drop)。
修复相同的bug :git cherry-pick 4c805e2
推送到其他分支：git push origin dev
创建远程origin 的 dev 分支到本地:git checkout -b dev origin/dev

强制推送并覆盖远程分支的更张（请注意这可能会导致其他开发者的工作丢失）：git push origin master --force-with-lease

