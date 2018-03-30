git notebook

创建目录 mkdir learngit

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