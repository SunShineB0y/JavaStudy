## Git ##
分支操作

	git branch dev 创建dev分支
	git checkout dev 切换到dev分支
	git checkout -b dev 创建并切换到dev分支
	git branch -d dev 删除dev分支

查看分支信息

	git branch 查看本地分支
	git branch -a 查看本地和远程所有分支

远程dev分支拉到本地

	git checkout –b dev origin/dev  创建，同步并切换到dev分支

本地分支推送到远程库
	
	在master分支
	git branch -a  查看全部分支信息
	git branch dev  创建dev分支
	git push origin dev  把dev分支推送到远程
	git branch -a  查看全部分支信息
	
删除远程分支develop

	git push -d origin develop 

暂存修改

	git stash  暂存
	git stash list  查看stash列表
	git stash apply  恢复（不删除stash）
	git stash drop  删除第一条stash
	git stash pop  恢复并删除stash

撤销修改
	
	git checkout -- file

	作用：1.修改后，还没有放到暂存区，使用 撤销修改就回到和版本库一模一样的状态。
	     2.另外一种是readme.txt已经放入暂存区了，接着又作了修改，撤销修改就回到添加暂存区后的状态。

添加到暂存区

	git add Git使用.md

提交到本地库

	git commit -m "本次修改的注释"
	git commit -a -m "本次修改的注释"  对于未添加到缓存区的文件，这条命令直接带着“-a”即可直接提交到本地库

查看log

	git log 查看全部log
	git log --oneline 显示在一行
	git log --oneline -n4 查看最近四条log
	git log --oneline master 查看master分支的log