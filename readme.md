## git notebook

###文件操作

>mkdir learngit  //创建目录 
git init   // 初始化本地仓库
s -ah  // 显示隐藏文件夹


####普通工作流程
```
git跟踪并管理的是修改而非文件，修改必须先add到暂存区(stage)后才能被commit
git add readme.md
2.git add .  // add所有文件
git commit -m "备注"  // 提交
git push // push到仓库
git push -u origin master第一次推送master分支的所有内容
```

####对比
>git diff
git diff readme.md
git diff 44ef88c8ebac908d58(reflog)
git diff HEAD //可查看工作区和分支的区别
git status  // 文件状态
cat  readme.md //显示文件内容

####历史记录
>git log
git log --pretty=oneline --abbrev-commit 查看日志(不带详细信息)
git reflog // 查看命令历史，以便确定要回到未来的哪个版本

####回退
>git reset --hard HEAD^ 
git reset --hard HEAD~100	// 回退上一版本s
git reset HEAD readme.md	//git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区

*####下拉仓库内容到本地
>git pull  // 下拉
git pull origin master  // 第一次获取远程仓库master分支上的内容
git checkout -- readme.md	// 让这个文件回到最近一次git commit或git add时的状态

####删除
>git rm -- readme.md
git rm -r filefolder

####重命名
> git mv a.txt b.txt
  或 mv a.txt b.txt  git add a.txt

####本地项目与远程git仓库关联流程
+ 新建远程仓库或远程仓库为空
```
git init  // git初始化本地仓库
git remote add origin https://github.com/yyjh/learngit  // 关联一个远程库
git add .  // 将全部文件加入git版本管理 .的意思是将当前文件夹下的全部文件放到版本管理中
git commit -m "注释"  // 提交文件 使用-m 编写注释
git push  // 推送到远程分支
```
+  远程仓库已有文件
```
git init  // git初始化
git remote add origin https://github.com/yyjh/learngit  // 关联一个远程库
git pull origin master  // 获取远程仓库master分支上的内容
git branch --set-upstream-to=origin/master master  // 将当前分支设置为远程仓库的master分支
git add .  // 将全部文件加入git版本管理 .的意思是将当前文件夹下的全部文件放到版本管理中
git commit -m "注释"  // 提交文件 使用-m 编写注释
git push  // 推送到远程分支
```

####推送免密方法：
>.git/config 增加
[credential]
helper = store

保存，第一次需要输入用户名密码，输入一次密码后第二次就会记住密码了不会再提示输入用户名及密码
或者
>git config —global credential.helper store
git config --global user.name "youname"
git config --global user.password "password"
git config --global user.email "aa@qq.com"

####从远程库clone

GitHub默认使用ssh，git clone git@github.com:yyjh/learngit.git
实际上，Git支持多种协议还可以用https等其他协议，如https://github.com/yyjh/learngit.git这样的地址。

使用ssh时报错：
>Warning: Permanently added the RSA host key for IP address '13.250.177.223' to the list of known hosts.
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
Please make sure you have the correct access rights and the repository exists.

需添加公钥 :

	可以用 ssh -T git@github.com去测试一下
	执行 ssh-keygen -t rsa -b 4096 -C "kunn_zhang@163.com" 产生key
	 开启ssh-agent执行 eval $(ssh-agent -s)
	 把key加到ssh-agent 执行ssh-add ~/.ssh/id_rsa
	复制到剪切板 clip < ~/.ssh/id_rsa.pub,添加到github setings里

####分支
>git checkout -b dev
创建dev分支并切换到dev相当于
git branch dev
git checkout dev

>git pull <remote> <branch>: Download changes and directly merge/integrate into HEAD

>git branch// 查看分支
git merge  branch// 合并分支
git branch -d <name>// 删除分支
git branch -D <name>// 强行删除分支(未被合并的分支)

####合并
>git merge --no-ff -m "merge with no-ff" dev  // 禁用Fast forward，本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去

####stash
>git stash// 保留工作现场
git stash pop// 回复现场
git stash list  git stash apply stash@{0}

####tag(标签)

>git tag  // 查看所有标签
git tag <name>  // 用于新建一个标签，默认为HEAD，也可以指定一个commit id
git tag -a <tagname> -m "blablabla..."  // 可以指定标签信息
git tag -s <tagname> -m "blablabla..."  // 可以用PGP签名标签

>git push origin <tagname> // 推送一个本地标签
git push origin --tags  // 推送全部未推送过的本地标签
git tag -d <tagname>  // 删除一个本地标签
git push origin :refs/tags/<tagname>  // 删除一个远程标签

####non-fast-forward

推送时出现这个报错:
> git push origin master
To https://github.com/yyjh/Notes.git
! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/yyjh/Notes.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

出现(non-fast-forward)的根本原因是repository已经存在项目且不是你本人提交（我知道是大概率你提交的，但是git只认地址），你commit的项目和远程repo不一样。这时该怎么办呢？很简单，把远端项目拉回本地：
>git pull

然而pull回来之后，你再push依旧会fail。 
原因是他们是两个不同的项目，要把两个不同的项目合并，不能简单的git pull。而是
>git pull origin master --allow-unrelated-histories

这条命令允许了不同项目的合并。 

好了，pull成功了。 
接下来
> git push origin master 

#### 代理上网导致的连接失败,要设置全局代理

>ssh: connect to host github.com port 22: Connection timed out fatal: Could not read from remote repository.

需要改用https协议。
>git remote rm origin
git remote add orign https://github.com/******.git


 依然报错：fatal: unable to access 'https://github.com/******.git/': Failed to connect to github.com port 443: Timed out

**设置全局代理**
>git config --global http.proxy 172.17.6.133:808

查看是否成功
>git config --get http.proxy 172.17.6.133:808

####git pull报错
>error: The following untracked working tree files would be overwritten by merge:
.editorconfig
.gitattributes
.gitignore
Engine/Binaries/DotNET/GitDependencies.exe

执行
>git clean -d -fx

会删除掉没有add到仓库的文件，操作记得慎重，以免改动文件的丢失。本质上就是操作仓库中没有被追踪的本地文件
>git clean -f -n         # 1
git clean -f            # 2
git clean -fd           # 3
git clean -fX           # 4
git clean -fx           # 5

>选项-n将显示执行（2）时将会移除哪些文件。
该命令会移除所有命令（1）中显示的文件。
如果你还想移除文件件，请使用选项-d。
如果你只想移除已被忽略的文件，请使用选项-X。
 如果你想移除已被忽略和未被忽略的文件，请使用选项-x。	

####RPC failed; curl 18 transfer closed with outstanding read data remaining

+ 加大缓存区
>git config --global http.postBuffer 524288000,这个大约是500M	

+ 少clone一些，–depth 1
>git clone https://github.com/flutter/flutter.git --depth 1

	–depth 1的含义是复制深度为1，就是每个文件只取最近一次提交，不是整个历史版本。

###vim命令

>q! 【强制退出不保存】 q【退出不保存】 wq【退出并保存后面也可以加个！】
Ctrl+放大字体
Ctrl+c 中断 可对出退出换行状态

###Ignore
>touch .ignore
https://github.com/github/gitignore

删除git已经tracking的文件
>git rm -r --cached ignoreFile（ignoreFile就是你想忽略的文件）

