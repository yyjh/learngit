git notebook

创建目录 mkdir learngit

git add readme.txt

git commit -m "wrote a readme file"

ls -ah  // 显示隐藏文件夹

2、
git diff readme.txt
git status

3、
git reset --hard HEAD^ git reset --hard HEAD~100	// 回退上一版本
git log
git reflog //查看命令历史，以便确定要回到未来的哪个版本

4、stage 先add到暂存区，在commit

