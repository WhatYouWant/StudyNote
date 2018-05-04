#Git学习笔记

##常用命令

```
$ git branch -d [branchname] 删除本地git分支
$ git branch 查看分支
$ git branch [branchname] 新建分支
$ git checkout [branchname] 切换分支
$ git commit -m '注释' 提交分支
$ git status 获取分支状态
$ git add . 添加当前所有文件
$ git push [remotename] [branchname] 推送branch到remote
$ git branch -a 查看远程分支
```

##查看提交历史 git log
一个常用选项是 -p，用来显示每次提交的内容差异。 -n显示最近n次提交。

git log 常用选项

选项 | 说明
------- | -------
-p | 按补丁格式显示每个更新之间的差异。
--stat | 显示每次更新的文件修改信息
--shortstat | 只显示 --stat中最后的行数修改添加移除统计
--name-only | 仅在提交信息后显示已修改的文件清单
--name-status | 显示新增，修改，删除的文件清单
--abbrev-commit | 仅显示SHA-1的前几个字符，而非所有的40个字符
--relative-date | 使用较短的相对时间显示（比如，“2 weeks ago”）
--graph	| 显示ASCII图形表示的分支合并历史
--pretty | 使用其他格式显示历史提交信息。可用的选项包括online,short,full,fuller和format(后跟指定格式)

限制git log输出的选项

选项 | 说明
------- | -------
-(n) | 仅显示最近的n条提交
--since, --after | 仅显示指定时间之后的提交。
--until, --before | 仅显示指定时间之前的提交。
--author | 仅显示指定作者相关的提交。
--committer | 仅显示指定提交者相关的提交。
--grep | 仅显示含指定关键字的提交
-S | 仅显示添加或移除了某个关键字的提交

##撤销操作
--amend 选项重新提交。
$git commit --amend
例如

```
$git commit -m 'initial commit'
$git add forgotten_file
$git commit --amend
```
最终只会有一个提交，第二次提交将代替第一次提交的结果。

####取消暂存的文件
$git reset HEAD filename
####撤销对文件的修改
$git checkout -- filename

##远程仓库的使用
####查看远程仓库
```
$git remote
$git remote -v
```
####添加远程仓库
```
$git remote add <shortname> <url> 可以使用shortname来替代url
```
例如
```
$git remote add pb https://github.com/paulboone/ticgit
$get fetch pb
```
####从远程仓库中抓取与拉取
$git fetch [remote-name]
git fetch origin 会抓取克隆（或上一次抓取）后新推送的所有工作。 必须注意 git fetch 命令会将数据拉取到你的本地仓库 - 它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作。
可以使用git pull命令来自动的抓取然后合并远程分支到当前分支。
####推送到远程仓库
git push [remote-name] [branch-name]
$git push origin master
####查看远程仓库
git remote show [remote-name]
####远程仓库的移除与重命名
git remote rename [old_name] [new_name]

##打标签
####列出标签
$git tag

特定模式的标签
$git tag -l 'v1.8.5*'
####创建标签
标签类型有两种：轻量标签和附注标签。
一个轻量标签很像一个不会改变的分支-它只是一个特定提交的引用。
附注标签是存储在git数据库中的一个完整对象。
####附注标签
指定-a选项
$git tag -a v1.4 -m 'my version 1.4'
通过git show命令可以看到标签信息与对应的提交信息。
####轻量标签
$git tag v1.4
####后期打标签
$git tag -a v1.2 校验和或部分校验和
####共享标签
$git push origin v1.5
####检出标签
$git checkout -b [branchname] [tagname]

##Git别名
通过git config命令来轻松的为每一个命令设置一个别名。

$git config --global alias.co checkout -->使用co代替checkout命令

$git config --global alias.unstage 'reset HEAD --'

$git config --global alias.last 'log -1 HEAD'

#Git分支 - 分支简介
####参考网址: [https://git-scm.com/book/zh/v2/Git-分支-分支简介]()
Git 的分支，其实本质上仅仅是指向提交对象的可变指针。 Git 的默认分支名字是 master。 在多次提交操作之后，你其实已经有一个指向最后那个提交对象的 master 分支。 它会在每次的提交操作中自动向前移动。
Note:Git 的 “master” 分支并不是一个特殊分支。 它就跟其它分支完全没有区别。 之所以几乎每一个仓库都有 master 分支，是因为 git init 命令默认创建它，并且大多数人都懒得去改动它。
	
####创建分支
$git branch testing
	HEAD指针，指向当前所在的本地分支（将HEAD想象为当前分支的别名）。
![](https://git-scm.com/book/en/v2/images/head-to-master.png)

查看各个分支当前所指的对象
$git log --oneline --decorate
####分支切换
```
$git checkout [branchname] ->切换到branchname分支上
```
项目分叉
![](https://git-scm.com/book/en/v2/images/advance-master.png)

查看项目分叉历史
```
$git log --oneline --decorate --graph --all
```
Figure 1.HEAD指向当前所在的分支。
![](https://git-scm.com/book/en/v2/images/head-to-testing.png)

修改test.rb，然后提交
```
$ vim test.rb
$ git commit -a -m 'made a change'
```
Figure 2. HEAD分支随着提交操作自动向前移动
![](https://git-scm.com/book/en/v2/images/advance-testing.png)

切回master分支
```
$ git checkout master
```
	 Note: 分支切换会改变你工作目录中的文件。在切换分支时，一定要注意你工作目录里的文件会被改变。如果是切换到一个较旧的分支，你的工作目录会恢复到该分支最后一次提交时的样子。 如果 Git 不能干净利落地完成这个任务，它将禁止切换分支。

Figure 3.检出时HEAD随之移动
![](https://git-scm.com/book/en/v2/images/checkout-master.png)

稍做修改并提交
```
$vim test.rb
$git commit -a -m 'made other changes'
```
此时，项目提交历史产生分叉
Figure 4.项目分叉历史
![](https://git-scm.com/book/en/v2/images/advance-master.png)

##Git分支-分支的新建与合并
####新建分支
Figure 1. 一个简单提交历史
![](https://git-scm.com/book/en/v2/images/basic-branching-1.png)
新建一个分支并同时切换到那个分支上。

```
$git checkout b iss53
```
它是下面两条命令的简写

```
$git branch [branchname]
$git checkout [branchname]
```
Figure 2.创建一个新分支指针
![](https://git-scm.com/book/en/v2/images/basic-branching-2.png)
继续在iss53分支上工作，做了一些提交。在此过程中，iss53分支在不断的向前推进，因为你已经检出到该分支。

```
$ vim index.html
$ git commit -a -m 'added a new footer [issue 53]'
```
Figure 3.iss53分支随着工作的进展向前推进
![](https://git-scm.com/book/en/v2/images/basic-branching-3.png)
此时需要切换回master修复bug。切换回之前，需要保存当前分支的进度和修补提交。

```
$git checkout master
```
	附：当你切换分支的时候，Git 会重置你的工作目录，使其看起来像回到了你在那个分支上最后一次提交的样子。 Git 会自动添加、删除、修改文件以确保此时你的工作目录和这个分支最后一次提交时的样子一模一样。
需要修复紧急问题，新建一个处理紧急问题的分支（hotfix branch）,在该分支上工作直到问题解决

```
$git checkout -b hotfix
...
```
Figure 4.基于master分支的紧急问题分支hotfix branch
![](https://git-scm.com/book/en/v2/images/basic-branching-4.png)
修复完成后将其合并到master分支。

```
$git checkout master
$git merge hotfix
Updating f42c576...874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
```
	在合并的时候，你应该注意到了"快进（fast-forward）"这个词。 由于当前 master 分支所指向的提交是你当前提交（有关 hotfix 的提交）的直接上游，所以 Git 只是简单的将指针向前移动。 换句话说，当你试图合并两个分支时，如果顺着一个分支走下去能够到达另一个分支，那么 Git 在合并两者的时候，只会简单的将指针向前推进（指针右移），因为这种情况下的合并操作没有需要解决的分歧——这就叫做 “快进（fast-forward）”。
Figure 5.master被快进到hotfix
![](https://git-scm.com/book/en/v2/images/basic-branching-5.png)
合并后就可以删除hotfix分支了,切换回iss53继续工作

```
$git branch -d hotfix
$git checkout iss53
```
Figure 6.继续在iss53上工作
![](https://git-scm.com/book/en/v2/images/basic-branching-6.png)

##分支的合并
已经修复iss53，将其合并到master中

```
$git checkout master
$git merge iss53
```
Figure 7.一次典型合并中所用到的三个快照
![](https://git-scm.com/book/en/v2/images/basic-merging-1.png)

Figure 8.一个合并提交
![](https://git-scm.com/book/en/v2/images/basic-merging-2.png)

####遇到冲突时的分支合并
出现冲突的区段，看起来的样子：

```
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```
这表示 HEAD 所指示的版本（也就是你的 master 分支所在的位置，因为你在运行 merge 命令的时候已经检出到了这个分支）在这个区段的上半部分（======= 的上半部分），而 iss53 分支所指示的版本在======= 的下半部分。 为了解决冲突，你必须选择使用由 ======= 分割的两部分中的一个，或者你也可以自行合并这些内容。



#分支管理
####查看分支

```
$git branch
```
查看每一个分支的最后一次提交

```
$git branch -v
```

--merged与--no-merged可以过滤这个列表中已经合并或尚未合并到当前分支的分支。

#分支开发工作流
####获取远程引用的完整列表

```
$ git remote show (remote)
$ git ls-remote (remote)
```
#远程分支
远程跟踪是远程分支状态的引用。它们以(remote)/(branch)形式命名。例如仓库origin的master分支， origin/master。
Git的clone命令会自动将远程仓库命名为origin，拉取它的所有数据，创建一个指向它的master分支的指针，并且在本地将其命名为origin/master。
	Note：远程仓库名字 “origin” 与分支名字 “master” 一样，在 Git 中并没有任何特别的含义一样。 同时 “master” 是当你运行 git init 时默认的起始分支名字，原因仅仅是它的广泛使用，“origin” 是当你运行 git clone 时默认的远程仓库名字。 如果你运行 git clone -o booyah，那么你默认的远程分支名字将会是 booyah/master。
Figure 1.克隆之后的服务器与本地仓库
![](https://git-scm.com/book/en/v2/images/remote-branches-1.png)

如果你在本地的 master 分支做了一些工作，然而在同一时间，其他人推送提交到 git.ourcompany.com 并更新了它的 master 分支，那么你的提交历史将向不同的方向前进。 也许，只要你不与 origin 服务器连接，你的 origin/master 指针就不会移动。
Figure 2.本地与远程的工作可以分叉
![](https://git-scm.com/book/en/v2/images/remote-branches-2.png)

通过运行git fetch origin命令，可以从远程服务器中抓取本地没有的数据，并且更新本地数据库，移动origin/master指针指向新的，更新后的位置。
![](https://git-scm.com/book/en/v2/images/remote-branches-3.png)

#推送
当你想要公开分享一个分支时，需要将其推送到有写入权限的远程仓库上。 本地的分支并不会自动与远程仓库同步 - 你必须显式地推送想要分享的分支。 
如果希望和别人一起在名为 serverfix 的分支上工作，你可以像推送第一个分支那样推送它。 运行 git push (remote) (branch)

```
$git push origin serverfix
```
也可以运行 git push origin [local-Branchname]:[remote-Brancename]

```
$git push origin serverfix:serverfix
```
如果想让远程仓库上的分支叫做serverfix

```
$git push origin serverfix:awesomebrance
```

####跟踪分支
当克隆一个仓库时，它通常会自动地创建一个跟踪 origin/master 的 master 分支。 然而，如果你愿意的话可以设置其他的跟踪分支 - 其他远程仓库上的跟踪分支，或者不跟踪 master 分支。 最简单的就是之前看到的例子，运行 git checkout -b [branch] [remotename]/[branch]。 

####拉取
当 git fetch 命令从服务器上抓取本地没有的数据时，它并不会修改工作目录中的内容。 它只会获取数据然后让你自己合并。 然而，有一个命令叫作 git pull 在大多数情况下它的含义是一个 git fetch 紧接着一个 git merge 命令。 如果有一个像之前章节中演示的设置好的跟踪分支，不管它是显式地设置还是通过 clone 或 checkout 命令为你创建的，git pull 都会查找当前分支所跟踪的服务器与分支，从服务器上抓取数据然后尝试合并入那个远程分支。
####删除远程分支
删除服务器上的serverfix分支。

```
$git push origin --delete serverfix
```

#Git分支-变基
尽量使用merge吧。

#服务器上的Git-协议













