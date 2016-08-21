#GitHub练习使用笔记
###参考自[廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

##初次使用
| 命令作用                             | 命令代码                               |
| :------------                        |:---------------                        |
| 进入本地文件夹                       | cd [路径]                              |
| 初始化本地git仓库                    | git init                               |
| 添加文件                             | git add *                              |
| 查看添加的不同的文件状态             | git status                             |
| 提交文件变化                         | git commit -m “[提交tag]”              |
| 提交远程仓库url                      | git remote add origin [远程仓库url]    |
| 提交到远程仓库                       | git push origin master                 |


* git add * 是添加.gitignore以外的文件进仓库。没有.gitignore的话默认效果就是全部添加进仓库，也可以git add具体的文件路径


##创建分支
######[链接](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840038939c291467cc7c747b1810aab2fb8863508000)
| 命令作用                             | 命令代码                               |
| :------------                        |:---------------                        |
| 创建并切换到分支(相当与以下两个命令) | git checkout -b [分支名]               |
| 创建分支                             | git branch  [分支名]                   |
| 切换到分支                           | git checkout [分支名]                  |
| 查看分支(当前分支以*开头)            | git branch                             |
| 提交到分支                           | git commit -m “[提交tag]”              |
| 提交到远程仓库分支                   | git push origin [分支名]               |
| 合并分支到当前分支                   | git merge [分支名]                     |
| 删除分支                             | git branch -d [分支名]                 |


* 查看分支：git branch
* 创建分支：git branch <name>
* 切换分支：git checkout <name>
* 创建+切换分支：git checkout -b <name>
* 合并某分支到当前分支：git merge <name>
* 删除分支：git branch -d <name>


##分支冲突
######[链接](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840202368c74be33fbd884e71b570f2cc3c0d1dcf000)
git commit一个新的你想要留下的文件以解决


##分支策略
######[链接](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013758410364457b9e3d821f4244beb0fd69c61a185ae0000)
在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

###BUG分支
######[链接](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137602359178794d966923e5c4134bc8bf98dfb03aea3000)
* 储藏分支：git stash

确定要在哪个分支上修复bug，在该分支上创建临时分支  
假定需要在master分支上修复,创建bug分支  
```
git checkout master  
git checkout -b bug
```
修复完成后，切换回分支，并完成合并  
以master为例  
```
git checkout master
git merge --no-ff -m "[commit info(bug fixed)]" bug
```
切换会原来分支继续本来的工作
```
git checkout dev(切换会原来工作的分支)
git stash list (查看工作区)
一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除
另一种方式是用git stash pop，恢复的同时把stash内容也删了：
```

####修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除
####当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场


##Feature分支
###新功能分支
######[链接](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001376026233004c47f22a16d1f4fa289ce45f14bbc8f11000)
* git branch -d [分支名] 删除分支
* git branch -D [分支名] 强行删除分支

####开发一个新feature，最好新建一个分支
####如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

##多人协作
######[链接](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013760174128707b935b0be6fc4fc6ace66c4f15618f8d000)
* 首先，可以试图用git push origin branch-name推送自己的修改；
* 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
* 如果合并有冲突，则解决冲突，并在本地提交；
* 没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！  
如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。
这就是多人协作的工作模式，一旦熟悉了，就非常简单。


* 查看远程库信息，使用git remote -v；
* 本地新建的分支如果不推送到远程，对其他人就是不可见的；
* 从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
* 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
* 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
* 从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。
