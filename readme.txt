git notebook

1、创建目录 mkdir learngit

git init // 初始化本地仓库

git add readme.txt

git commit -m "wrote a readme file"

ls -ah  // 显示隐藏文件夹

2、
git diff readme.txt
git status

3、
git reset --hard HEAD^ git reset --hard HEAD~100	// 回退上一版本s
git log
git reflog //查看命令历史，以便确定要回到未来的哪个版本

4、stage 先add到暂存区，在commit

5、git跟踪并管理的是修改而非文件，修改必须add后才能被commit
git diff HEAD 可查看工作区和分支的区别

6、git checkout -- readme.txt	// 让这个文件回到最近一次git commit或git add时的状态s
git reset HEAD readme.txt	//git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区

7、git rm -- readme.txt // 删除

8、1)在github创建一个远程库
	2)git remote add origin  https://github.com/yyjh/learngit // 关联一个远程库
	3)git push -u origin master第一次推送master分支的所有内容
	  git push origin master推送最新修改
9、推送免密方法：
	.git/config 增加
	[credential]   
    helper = store
	保存，第一次需要输入用户名密码，输入一次密码后第二次就会记住密码了不会再提示输入用户名及密码
10、从远程库clone
	GitHub默认使用ssh，git clone git@github.com:yyjh/gitskills.git
	实际上，Git支持多种协议还可以用https等其他协议，如https://github.com/yyjh/gitskills.git这样的地址。
	
	使用ssh时报错： Warning: Permanently added the RSA host key for IP address '13.250.177.223' to the list of known 
	hosts.
	git@github.com: Permission denied (publickey).
	fatal: Could not read from remote repository.

	Please make sure you have the correct access rights
	and the repository exists.
	
	需添加公钥 1) 可以用 ssh -T git@github.com去测试一下
			   2) 执行 ssh-keygen -t rsa -b 4096 -C "kunn_zhang@163.com" 产生key
			   3) 开启ssh-agent	执行 eval $(ssh-agent -s)
			   4) 把key加到ssh-agent 执行ssh-add ~/.ssh/id_rsa
			   5) 复制到剪切板 clip < ~/.ssh/id_rsa.pub,添加到github setings里
11、创建分支
	git checkout -b dev
	创建dev分支并切换到dev相当于
	git branch dev
	git checkout dev
	
	git pull <remote> <branch>: Download changes and directly merge/integrate into HEAD 

12、git branch// 查看分支
	git merge  branch// 合并分支
	git branch -d <name>// 删除分支
	git branch -D <name>// 强行删除分支(未被合并的分支)

13、git merge --no-ff -m "merge with no-ff" dev// 禁用Fast forward，本次合并要创建一个新的commit，所以加上-m参数，
	把commit描述写进去
	
14、git stash// 保留工作现场
	git stash pop// 回复现场
	git stash list  git stash apply stash@{0}
	
15、多人协作的工作模式通常是这样：

	1).首先，可以试图用git push origin branch-name推送自己的修改；

	2).如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

	3).如果合并有冲突，则解决冲突，并在本地提交；

	4).没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

	5).如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令
	git branch --set-upstream branch-name origin/branch-name。

16、tag
	git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id
	git tag -a <tagname> -m "blablabla..."可以指定标签信息
	git tag -s <tagname> -m "blablabla..."可以用PGP签名标签
	命令git tag可以查看所有标签
	git log --pretty=oneline --abbrev-commit 查看日志(不带详细信息)
	
17、命令git push origin <tagname>可以推送一个本地标签；

	命令git push origin --tags可以推送全部未推送过的本地标签；

	命令git tag -d <tagname>可以删除一个本地标签；

	命令git push origin :refs/tags/<tagname>可以删除一个远程标签
	
18、non-fast-forward
	推送时出现这个报错
	$ git push origin master
	To https://github.com/yyjh/Notes.git
	! [rejected]        master -> master (non-fast-forward)
	error: failed to push some refs to 'https://github.com/yyjh/Notes.git'
	hint: Updates were rejected because the tip of your current branch is behind
	hint: its remote counterpart. Integrate the remote changes (e.g.
	hint: 'git pull ...') before pushing again.
	hint: See the 'Note about fast-forwards' in 'git push --help' for details.
	
	出现(non-fast-forward)的根本原因是repository已经存在项目且不是你本人提交（我知道是大概率你提交的，但是git只认地址），你commit的项目和远程repo不一样。这时该怎么办呢？很简单，把远端项目拉回本地：

	git pull
	然而pull回来之后，你再push依旧会fail。 
	原因是他们是两个不同的项目，要把两个不同的项目合并，不能简单的git pull。而是

	git pull origin master --allow-unrelated-histories
	这条命令允许了不同项目的合并。 
	好了，pull成功了。 
	接下来
	git push origin master
