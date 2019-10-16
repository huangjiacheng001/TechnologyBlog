#多账户ssh-key以及常备Git命令清单


单词释义：
workspace         |      工作区
index || stage   |      暂存区
repository       |      本地仓库区 
remote           |      远程仓库


总结常用命令前先插入一下
Git配置单账户&&多账户ssh-key

因为使用Git最基本需要配个ssh登录的，另外可能我们也不

止一个仓库地址，比如同时存在公司和自己的Gitlab或者

Github，那么这时就需要多账户配置代理


1.生成ssh-key

`root@tecmint:~# ssh-keygen -t rsa -C "Y-Email" -f ~/.ssh/id_rsa`

Tip：-f  ~/.ssh/id_rsa 并不是必须的。如果已经生成过ssh-key，这里小心会覆盖掉先前的，默认是生成 id_rsa，也可以自己命名，比如id_rsa_github或者github_rsa,喜欢就行


2.将生成的id_rsa.pub内容添加到Gitlab或Github的Add an SSH key中的Key中

`root@tecmint:~# ls ~/.ssh`

`**Result:** id_rsa id_rsa.pub Known_hosts`

`root@tecmint:~# cat ~/.ssh/id_rsa.pub`

Tip：填写title可以随意，但是最好表意明确！window系统中操作bash时 Ctrl+Insert 键复制 Shift+Insert 键是粘贴


3.多账户配置代理
假如现在已经执行了上面的操作，已经存在一个ssh-key(这个用在Github上)，现在要配置另一个ssh-key，假设用在Gitlab上。方法如下:  
root@tecmint:~# ssh-keygen -t rsa -C "Y-Email" -f ~/.ssh/id_rsa_gitlab
root@tecmint:~# ls ~/.ssh
**Result:** id_rsa id_rsa.pub id_rsa_gitlab id_rsa_gitlab.pub Known_hosts
root@tecmint:~# cat ~/.ssh/id_rsa_gitlab.pub
接着执行第二部操作，然后需要将新生成的key加入ssh-key里面，执行如下操作：
root@tecmint:~# ssh-agent bash
root@tecmint:~# ssh-add ~/.ssh/id_rsa_gitlab 
Tip: ssh-agent bash 并不是必须的，在ssh-add执行不成功时，需要执行这个命令 

—————————————————————————

重点配置代理
现在就要写一个配置文件了，实现ssh-key的分发对应,配置文件如下图，这里配置了三个key：



首先要生成配置文件
配置文件的作用就是在你处在一个repository时，连接远程库会识别哪个服务器以及哪个用户，然后寻找相应的真正的地址HostName
root@tecmint:~# touch ~/.ssh/config
root@tecmint:~# vim config
配置文件Option：

每个账号单独配置一个Host
每个Host要取一个别名
每个Host主要配置HostName和IdentityFile两个属性即可。
Host的名字原则上可以取自己喜欢的名字，不过这个会影响git相关命令，新配置别名可以在配置文件修改仓库地址相应部分为别名即可,也可以使用 git remote set-url指令，下面有介绍
例如上面的配置最后一个：
Host another_github.com  
# 这样定义的话，命令如下：
# git@后面紧跟的名字改为another_github.com,
> git clone git@another_github.com:tecmint/your_repo.git
Option 讲解
Host github_custom.com #别名
HostName github.com #真实的域名地址
IdentityFile ～/.ssh/id_rsa #私钥地址
RSAAuthentication yes 
PreferredAuthentications github.com #配置登录时用什么权限认证--可设为publickey,password publickey,keyboard-interactive等  
User your-Email #配置使用用户名
测试是否配置成功
root@tecmint:~# ssh -T git@github.com
root@tecmint:~# ssh -T git@another_github.com
使用linux环境连接gitlab仓库时出现`bad owner or permissions on .ssh/config`，执行以下命令：
root@tecmint:~# chown $USER ~/.ssh/config
root@tecmint:~# chmod 644 ~/.ssh/config


如果喜欢看官方文档的可以不用往下看了

1,分支有关命令

* 查看本地分支
  git branch
* 切换回上一个分支
  git checkout - 
* 选择一个commit，合并到当前分支
  git cherry-pick [commit]   
* 查看本地分支信息包含所处版本号，提交信息
  git branch -v  
* 查看远程分支
  git branch -r 
* 查看所有分支包含本地与远程分支 
  git branch -a 
* 查看所有分支以及分支得相关信息
  git branch -av 
* 新建一个分支，指向指定commit
  git branch [branch name] [commit]     
* 新建指定分支并且与远程分支建立追踪关系
  git branch --track [branch name] [remote-branch] 
* 已经存在分支关联远程分支
  git branch --set-upstream-to=origin/config config 
* 删除本地分支  
  git branch -d [branch name]
* 强制删除本地分支
  git branch -D [branch name]  
* 删除远程主机的分支 
  git push origin --delete [branch name]  
* 删除本地仓库的远程分支  
  git branch -dr [origin/brnach] 
* 删除远程主机远程分支(实际是推送了一个空分支)
  git push origin :[branch name] 
* 删除远程tag
  git push origin --delete tag [tag name] 
* 删除远程tag
  git tag -d [tag name] 首先创建一个空tag   
  git push origin :refs/tags/[tag name] 删除远程tag 
2,配置文件gitconfig

# 当前git配置列表
  git config --list    
# 编辑git配置
  git config -e [--global] 
# 配置提交用户名  
  git config [--global || --local] user.name "your name"  
# 配置提交email
  git config [--global || --local] user.email "your email" 
3,添加、删除文件

# 添加当前目录的所有文件到暂存区
  git add .  
# 添加指定文件到暂存区
  git add [file01] [file02] 
# 添加指定目录到暂存区，包含子目录
  git add [dir]   
# 添加每个变化前，都会要求确认，可以实现同一文件多处变化，分次提交
  git add -p 
# 删除工作区指定文件，并将其添加到暂存区
  git rm [file]
# 删除暂存区指定文件，但保留工作区修改，同时停止追踪文件
  git rm --cached [file] 
# 重命名文件，并将其放入暂存区
  git mv [origin file] [new file] 
4,提交文件

# 提交暂存区文件到repository
  git commit -m "message"  
# 提交暂存区指定文件到repository
  git commit [file01] [file02] -m "message"  
# 提交工作区自上次commit之后的变化，直接到repository
  git commit -am "message"  
# 提交时显示所有diff信息
  git commit -v 
# 使用一次新的提交替代上一次提交，如果上一次代码没有发生变化，用来修改上一次的提交信息 
  git commit --amend -m "message"  
# 重做上一次提交
  git commit --amend [file01] [file02]  
5,打包文件

# 以zip格式讲master分支打包到file.zip文件
git archive --formmat zip --output ../file.zip master 
6,标签（tag是在commit之后增加的）

# 列出所有tag
  git tag
# 创建一个新的tag指定到当前commit
  git tag [tag]
# 指定-a选项创建附注标签
  git tag -a v1.1 -m "version 1.1" 04f654b 
# 创建一个tag到指定commit
  git tag [tag] [commit]  
# 删除指定tag
  git tag -d [tag]  
# 删除远程tag 或者 git push origin --delete tag [tag]
  git push [origin] :refs/tags/[tag]  
# 显示指定tag信息
  git show [tag]  
# 推送指定tag到远程仓库
  git push [origin] [tag]  
# 推送所有tag到远程仓库
  git push [origin] --tags  
# 在Git中你并不能真的检出一个标签，因为它们并不能像分支一样来回移动。
# 如果你想要工作目录与仓库中特定的标签版本完全一样，可以使用 
# 在特定的标签上创建一个新分支
  git checkout -b [branchname] [tagname] 
# 新建一个分支指向一个tag 
  git checkout -b [branch] [tag]  
7,远程同步

# 下载远程仓库所有变动，但是不合并  
  git fetch [remote]  
# 显示所有远程仓库
  git remote -v
# 显示远程仓库信息
  git remote show [origin]  
# 新增一个远程仓库并命名，默认origin
  git remote add [remote shortname] url  
# 取回远程仓库的变化并合并答本地分支
  git pull [remote] [branch]  
# 强制推送，即使有冲突
  git push [remote] --force  
# 推送本地所有分支到远程仓库
  git push [remote] --all  
# 修改远程仓库的URL，配置多代理别名会用到
  git remote set-url origin [new remote url]
8,撤销—这个平时大家应该会用到很多

# 恢复index中的指定文件到工作区
  git checkout [file]  
# 恢复index中的所有文件到工作区
  git checkout . 
# 恢复指定的commit中指定的文件到index与workspace中
  git checkout [commit] [file]  
# 重置index中的指定文件与上次commit保持一致,工作区保持不变
  git reset [--mixed] [file]  
# 重置指定文件到index与workspace，并且与上次commit保持一致
  git reset --hard [file]  
# 重置到指定commit，但是工作区保持不变
  git reset [commit]  
# 重置到指定commit【与上一个命令指定commit方式不同】，但是工作区保持不变
  git reset HEAD^?   
# 重置index与workspace到指定commit
  git reset --hard [commit]  
# 重置index与workspace到指定commit
  git reset --hard HEAD^?  
# 重置当前HEAD为指定commit，但是index与workspace保持不变
  git reset --keep [commit]  
# 新建一个commit，用来撤销指定commit，指定commit的所有变化将会被新的commit抵消，指定commit的变化将应用到新的commit中
  git revert [commit]  
9,log—查看一些日志以及统计类信息

# 根据关键字搜索提交历史
  git log -S [keyword]  
# 显示commit历史，并显示每次commit的变更文件
  git log --stat  
# 显示指定文件的版本历史，包含文件改名  
  git log --follow [file]  
# 显示指定commit之后的所有变动，其‘提交说明’必须符合搜索条件
  git log [tag] HEAD --grep feature  
# 显示指定commit之后的所有变动，每个commit占据一行
  git log [tag] HEAD --pretty=format:%s  
# 显示指定文件每次commit的diff，可以用来检查更改了什么
  git log -p [file]  
# 显示提交用户以及提交次数
  git shortlog -sn  
# 显示指定文件是谁提交的什么时候提交的
  git blame [file]  
# 显示index与workspace的diff
  git diff 
# 显示index与上一个commit的区别
  git diff --cached [file]  
# 显示worksapce与当前分支最新commit的区别
  git diff HEAD  
# 显示两次提交的diff
  git diff [first-commit] [second-commit]  
# 显示今天写了多少行代码
  git diff --shortstat "@{0 day ago}"  
# 显示某个commit的相关信息，元数据以及内容变化
  git show [commit]  
# 显示指定提交有变动的文件
  git show --name-only [commit]  
# 显示指定提交的文件变动内容
  git show [commit]:[file]  
# 显示最近5次提交  
  git log -5 --pretty --oneline 
# 使用--after或--before来按照日期筛选.
  git log --after="2014-7-1" --before="2014-7-4"
# 按作者
  git log --author="John"
# 按文件,这里的--是告诉Git后面的参数是文件路径而不是branch的名字. 如果后面的文件路径不会和某个branch产生混淆, 你可以省略--.
  git log -- foo.py bar.py   
# 按内容
# 有时你想搜索和新增或删除某行代码相关的commit. 可以使用 －S "<string>". 
# 下面的例子假设你想知道Hello, World!这句话是什么时候加入到项目里去的, 你可以使用下面的命令:
# 如果你想使用正则表达式去匹配而不是字符串, 那么你可以使用-G代替-S.
# 这是一个非常有用的debug工具, 使用他你可以定位所有跟某行代码相关的commit. 
# 甚至可以查看某行是什么时候被copy的, 什么时候移到另外一个文件中去的.
  git log -S "Hello,World!"
 # --graph标记会画出一个ASCII图展示commit历史的分支结构. 通常和--oneline --decorate结合使用:
  git log --graph --oneline --decorate
10,合并commit

# 很多commit毫无用处，需要merge掉以免影响commit树
> git rebase -i origin/master  
  * pick  正常选中
  * reword  选中并且可以修改commit信息
  * edit  选中，rebase会停止，然后允许你修改这个commit
  * squash  选中，会将当前commit与前一个commit合并
  * fixup  与squash一样，但是不保存当前commit的提交信息
  * exec 执行其他shell脚本  
# squash与fixup可以当作命令参数使用  
> git commit --fixup <commit message>  
> git rebase -i --autosquash
或者采用下面的方法：
git reset HEAD~3  
git add .
git commit -am "合并前三次commit"  
git push --force
