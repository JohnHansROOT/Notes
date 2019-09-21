# Git学习经验

[TOC]
## 安装
百度搜git，[官网](https://git-scm.com/download/win)下载就行

## 入门学习
* 从小白上手吗，跟着[这个博客](https://blog.csdn.net/u010802169/article/details/80490886)做练习吧，包括建立本地库，文件版本管理，生成ssh_key，关联github库，clone库到本地，创建合并分支，解决冲突，基本的就会了
* 这是[git的官方文档](https://git-scm.com/book/en/v2)
* 土豆推荐的[这个教程](https://www.cnblogs.com/tracylxy/p/6429638.html)，不知道咋样还没看过
* 最好熟悉一下linux的命令，特别是vim之类的

##  教训
* 若是要从Github上clone库下来，复制了地址，若报错什么不识别 https 啥的，那肯定是你之前用了Ctrl+V产生了一个隐藏的^（unix没有ctrl+v快捷键），用右键粘贴就好了！参考[这个博客](https://blog.csdn.net/sinat_29891353/article/details/82861472)

## 理解
* 工作区：电脑上的目录，你直接操作的文件所在的文件夹
* 版本库：repository，是隐藏文件夹.git
* 暂存区：repo中的一个文件夹，放置git add 后的文件，还未提交到当前分支（commit）
* 分支：branch，版本改变的历史线路，自动创建master分支，及指向master的指针HEAD；master应该是稳定的成熟的，平时工作应该在分支上进行，成熟后可以提交与主支合并(merge)；分支切换使用git checkout

## 常用的命令
        git config –-global user.name "hwb16"  
        git config --global user.email "hwb16@qq.com" 最开始设置用户名和邮箱作为标识，--global参数是对该机器上所有git仓库
        git init 将当前目录设置为git仓库
        git add 将某文件添加到暂存区中（未提交）
        git commit -m "文件提交" 将暂存区中所有文件提交到仓库，-m是给备注
        git status 查看 git 管理的文件夹下文件的状态
        git diff 查看修改
        git log 查看历史纪录
        git log --pretty=oneline 显示简化版的历史纪录
        git reset --hard HEAD^ 回退到上一个版本，用HEAD^^回退两个版本，用HEAD~~n回退n个版本
        git reset --hard version 回退到version（版本号）
        git reflog 查看版本号
        git restore 可以在commit之前撤销工作区所作的操作
        rm 删除文件（此为bash命令）若再 commit 便会彻底从版本库中删掉
        git checkout -- readme.txt 恢复刚刚删掉的文件
        ssh-keygen -t rsa -C "hwb16@qq.com" 创建SSH key 用于和Github仓库间的传输，会产生一个无尾缀文件和.pub文件，后者为公开公匙
        git remote add origin +库的.git链接 添加远程仓库关联，远程库叫origin
        git push -u origin master 把当前的mater分支推入origin远程库，-u参数是长期关联，以后再推就可以不用再写http那一串了
        git clone https://github.com/JohnHansROOT/testgit 克隆远程库到本地，后面接库的网页地址
        git remote 查看远程库
        git remote -v 查看远程库详细信息
        git branch 查看分支
        git branch dev 新建dev分支
        git checkout dev 切换到dev分支
        git checkout -b dev 新建并切换到dev分支
        git merge dev 合并dev分支到master并删除分支信息，后面可以加 -m 写备注
        git merge --no-ff dev 合并分支并保留分支信息
        git branch -d dev 删除dev分支
        git stash 将工作现场隐藏起来
        git stash list 列出
        git stash apply 恢复现场
        git stash drop 丢弃现场
        git stash pop 恢复并丢弃现场


        
## 常用的操作
* 提交并入当前分支

        git add yourfilename
        git commit -m "your commits"

* 分支合并
    
        git merge --no-ff -m "your commits" fenzhiname
        git branch -d fenzhiname

* 提交至远程仓库

        git remote add origin https://...//*.git
        git push (-u) master origin

* 克隆远程仓库到本地

        git clone https://...

* 将github库和本地库关联
要利用ssh加密传输，因此要用

        ssh-keygen -t rsa -C "hwb16@qq.com"
创建SSH，将pub里的公钥复制，粘贴再github的 settings - SSH and GPG keys - new ssh key，即可