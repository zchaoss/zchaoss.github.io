---
title: Git
date: 2020-10-05 15:46:47
tags: Git
categories: Git
index_img: https://i.loli.net/2020/10/03/t5HXQqvL7BepOES.png
---

# Git 

![v2-dad7a6694ad6026308d580ada34703f5_720w](https://i.loli.net/2020/10/03/t5HXQqvL7BepOES.png)

## 基本配置

### 账户信息配置

```css
/*保存在用户目录的.gitconfig文件下*/
git config --global user.name xxx 				#设置用户名
git config --global user.email yyy				#设置邮箱
git config --global push.default simple 		#git push默认提交格式
git config --global core.quotepath false		#防止文件名变成数字
git config --global core.editor "code --wait" 	#使用vscode编辑提交信息
git config --global core.autocrlf input 		#提交到git是自动将换行符转换为lf
```



### 密钥配置

```css
/*生成公钥后，请把id_rsa.pub文件的内容拷贝到github的Settings*/
ssh-keygen -t rsa -b 4096 -C "your_email@example.com" #生成密钥
ssh -T git@github.com 								  #测试是否成功
```



### 别名配置

#### 方法一

```css
/*在.gitconfig设置别名*/
git config --global alias.st status #git st
git config --global alias.co checkout #git co
```

#### 方法二

```css
/*在.bashrc设置别名*/
alias ga=" git add"
alias gc=" git commit v"
```



### 特殊文件配置

		将文件名写入`.gitignore`，系统会忽略里面的文件

## 本地仓库

```css
git init             				#初始化本地仓库
git add .             				#将修改的和新文件提交到暂存区，不包括被删除的文件
git commit -v         				#提交到本地仓库时显示diff的信息
git status -sb        				#以短形式显示修改文件清单，但显示分支名称
git diff			  				#显示工作区和暂存区的差异
git log               				#显示提交记录
git reflog            				#显示历史提交记录
git reset --hard [commentid]        #版本回滚，重置相应的工作区，暂存区以及HEAD
git rebase -i                       #提交信息的增删查改
git commit --amend					#修改最近一次的提交信息
```

```css
/*主要是用于工作区还原，建议使用git restore*/
git checkout -- <file>              #还原已在工作区更改的文件
git reset HEAD -- <file>            #删除已注册到暂存区的文件
```

**ps:**	

1. HEAD指向的是现在使用中的分支的最后一次更新。提交时使用~(tilde)和^(caret)就可以指定某个提交的相对位置。
2. `git reset`和`git revert`的区别 简而言之`reset`会删除之前的commit，HEAD会向后退。`revert`用新的commit来回滚之前的commit，HEAD是向前进的。所以前者`push`需要`git push -f`

## 分支处理

```css
git branch           				 #显示本地分支
git branch -d <branchname>			 #删除分支
git branch -D <branchname>           #强制删除分支
git checkout <branchname>            #切换分支
git checkout -b <branchname>         #创建并切换分支
git merge <branchname>               #将指定分支合并到当前分支
git stash                            #将工作环境封存
git stash pop                        #恢复工作环境，并将stash内容删除
```

```css
git cherry-pick <commitid>           #选择某个分支的提交导入到现有分支
git merge --squash <branchname>		 #将所选分支的的所有提交合并成一个提交，并导入当前分支
git rebase <commitid>				 #使用变基的方式合并
git rebase --continue				 #继续变基
git rebase --abort					 #中止变基	
```

**ps:**	git默认的合并方式是fast-forward合并，若想合并后还能生成新的提交请使用`git merge --no-ff `

## 远程仓库

```css
git remote add <shortname> <url>     #添加一个远程仓库，并指定相应的简写
git push -u <remote> <branch>		 #设置upsteam,将本地仓库上传到远程仓库
git push 							 #将本地仓库上传到远程仓库（设置好upsteam才可简写）
git pull                             #将远程仓库同本地分支合并（git fetch+git merge）
git fetch							 #将远程仓库下载到本地仓库
git clone <url>                      #克隆远程仓库，并创建同名文件夹
```

## 标签

```css
git tag								#查看标签列表		
git tag <tagname>					#添加标签
git tag -a <tagname>				#添加标签注释	
git tag -d <tagname>				#删除标签
```

**后话：**git版本2.28以上，使用`git restore`代替`git checkout`的文件恢复功能，使用`git switch`代替``git checkout`的分支处理功能，可以参考[Git新命令switch和restore](https://yanhaijing.com/git/2020/09/17/git-switch-and-restore/)

***

**参考资料：**

1. [猴子都懂的git入门](https://backlog.com/git-tutorial/cn/stepup/stepup1_1.html)
2. [廖雪峰的git教程](https://www.liaoxuefeng.com/wiki/896043488029600)

**推荐资料:**

1. [图解git](http://marklodato.github.io/visual-git-guide/index-zh-cn.html)
2. [git 简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)
3. [我的git笔记](https://zhuanlan.zhihu.com/p/22609000)