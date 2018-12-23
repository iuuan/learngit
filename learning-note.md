


<center><h2>Git学习笔记</h2></center>
## Git介绍

- Git是分布式版本控制系统
- 集中式VS分布式：   
   1. 集中式版本控制系统，版本库集中存放在中央服务器，必须要联网才能工作,没有历史版本库。   
   2. 分布式版本控制系统，版本控制系统没有“中央服务器”，每个人电脑上都是一个完整的版本库。   
   3. 分布式系统优势：安全性更高，不需要联网，如果中央服务器故障，任何其他一个开发人员的本地都有最新的带有历史记录的版本库。
- 主要区别在于历史版本库的存放，集中式系统历史版本只存在于中央服务器，而分布式控制系统中每个本地库都有历史记录存放。

## Git命令
***

### Git配置
    $ git config --global user.name "Your Name"
    $ git config --global user.email "email@example.com"  
注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。


### 创建版本库
#### 在当前目录创建git仓库
    $ git init 

#### 把文件添加到仓库
    $ git add <file>
    $ git commit
`git add`可以反复多次使用，添加多个文件，`git commit`可以一次提交很多文件，在`git commit`命令后添加(-m '····')方便从历史记录里找到修改记录。


### 掌握工作区的状态
    $ git status 

### 查看文件修改内容
    $ git diff

### 版本回退
    $ git reset -- hard HEAD^
HEAD指向的版本是当前版本，回到上一版本使用以上命令，如果回退上两个版本使用`HEAD^^`，如果回退版本数较大（如往上50个版本），使用`HEAD~50`。
### 回退指定版本
    $ git reset --hard commit_id
`commit_id`是指定版本号，是由SHA1计算出来的数字

### 查看提交历史
    $ git log   

### 查看命令历史
    $ git reflog

### 工作区、暂存区和版本库
git与其他版本控制系统的不同之处就是有暂存区的概念，工作区就是电脑中能看到的目录，工作区有一个隐藏目录[.git]，这是git的版本库。版本库里有许多东西，最重要的是称为stage的暂存区。   

![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

将文件往版本库里添加时是分两步执行的:<br>
<ul>
<li>第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区。</li>
<li>第二步是用`git commit`提交修改，实际上就是把暂存区的所有内容提交到当前分支。</li>
</ul>
Git是如何跟踪修改的，每次修改，如果不用git add到暂存区，那就不会加入到commit中。  

### 撤销修改
#### 丢弃工作区的修改
    $ git checkout -- <file>
该命令是将文件在工作去的修改全部撤销，这里有两种情况：
<ol>
	<li>一种是file自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；</li>
	<li>一种是file已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。</li>
</ol>
总之，就是让这个文件回到最近一次git commit或git add时的状态。

#### 丢弃暂存区的修改
改乱了工作区某个文件的内容同时还添加到了暂存区，想丢弃修改时，先使用命令`git reset HEAD <file>`，之后按撤销工作区修改进行操作。
#### 进行了commit命令提交的修改
已经提交了不合适的修改到版本库时，想要撤销修改，使用版本回退命令，前提是没有推送到远程库.

### 删除文件
    $ git rm <file>
当你要删除文件`text.txt`的时候，可以采用命令：`rm test.txt`
这个时候有两种情况：<br>
第一种情况:的确要把`test.txt`删掉，那么可以执行
    `$ git rm test.txt`
    `$ git commit -m "remove test.txt"`
此时文件被删除，且删除记录上传本地库。<br>
第二种情况:误删文件，想恢复，这时候还没有`commit -m "remove test.txt"`，执行`git checkout test.txt`将文件恢复。<br>
如果执行完`git commit -m "remove test.txt"`后就不能用`checkout`恢复了，得用`git reset --hard HEAD^`，再从版本库写回到工作区。<br>
`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

### 远程仓库
#### 创建SSH Key 
    $ ssh-keygen -t rsa -C "youremail@example.com"

#### 关联远程仓库
    $ git remote add origin https://github.com/username/repositoryname.git

#### 推送到远程仓库
    $ git push -u origin master
-u 表示第一次推送master分支的所有内容，此后，每次本地提交后，只要有必要，就可以使用命令`$ git push origin master`推送最新修改。

#### 从远程克隆
    $ git clone https://github.com/usern/repositoryname.git
<strong>注意:</strong>当你不能使用`git@github.com`命令来进行推送和克隆，是因为没有安装密钥。添加私秘钥到`$ ssh-add ~/.ssh/id_rsa`<br>
如果添加失败可以先执行命令<code>$ eval \`ssh-agent\`</code> \`是～键上的那个符号，然后再次添加私秘钥。<br>
用`$ ssh -T git@github.com`判断是否绑定成功。如果返回`successfully`表示成功。


### 分支管理
#### 查看分支
    $ git branch

#### 创建分支
    $ git branch <name>

#### 切换分支
    $　git checkout <name>

#### 创建+切换分支
    $ git checkout -b <name>

#### 合并某分支到当前name分支
    $ git merge <name>

#### 删除分支
    $ git branch -d <name>

#### 强行删除分支
    $ git branch -D <name>
如果要丢弃一个没有被合并过的分支，可以通过以上命令来实现。

#### 查看分支合并图
    $ git log --graph
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
用`git log --graph`命令可以看到分支合并图。

#### 普通模式合并分支
    $ git merge --no-ff -m "description" <branchname>
通常进行分支合并时，git会使用Fast forward模式，删除分支后，分支信息会丢失，可以使用`--on-ff`参数，禁用Fast forward，需要时加上一个`-m`参数把commit描述写进去。这样进行合并后的历史有分支，能看出来曾经做过合并。

#### 保存工作现场
    $ git stash  
#### 查看工作现场
    $ git stash list
#### 恢复工作现场 
    $ git stash pop
#### 在新建分支中修改bug时的推荐操作
转自[教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)讨论区“我有疫苗”用户的[评论](https://www.liaoxuefeng.com/discuss/001409195939432748a2c9fae3846bc98b3c2a547fa321b000/0015444271534391cee28ca27ed474d97b30b2b0ad6b534000)
<pre>
首先理清两个概念：
    未被追踪的文件:指的是新建的文件或文件夹且还没加入到暂存区(新建的还没有被git add)
    未加入到暂存区的文件:指的是已经被追踪过，但是没有加入到暂存区(已经执行过git add/commit的但是这次修改后还没有git add)
    举例:
        readme.md  已经被git add/git commit过，但是呢,我这次只是修改了,而且没有修改完,不能commit;
        test    新建的文件夹,没有被git add/git commit;
        有个bug需要处理,这时候我需要切换分支，去处理bug
        正确的步骤:
            git add test（让git去追踪这个新文件）
            git stash保留现场
   如果不执行这两个命令，那么在修改BUG完成之后git status，就会发现readme.md没有添加到暂存区，同时又多了个test文件,
   但是自己的readme.md没有完成，万万不可以提交，这样导致bug的修改代码也提交不了。
   所以需要git stash，这样在提交修改bug代码的时候 ，就不会看见readme.md 和 test。可以安心提交修改bug的代码。
</pre>

#### 查看远程库信息
    $ git remote -v；
本地新建的分支如果不推送到远程，对其他人就是不可见的；
#### 从本地推送分支
    $ git push origin branch-name
如果推送失败，先用git pull抓取远程的新提交；
#### 在本地创建和远程分支对应的分支
    $ git checkout -b branch-name origin/branch-name
本地和远程分支的名称最好一致；
#### 建立本地分支和远程分支的关联
    $ git branch --set-upstream branch-name origin/branch-name
#### 从远程抓取分支
    $ git pull
如果有冲突，要先处理冲突。

### 标签
git标签是版本库的快照，实际就是某个commit的指针，如果要找到某次版本的commit号，数字复杂不好找，使用tag取一个易于记住和理解的名字就方便许多，它跟某个commit绑在一起。（类似于域名和IP地址的关系？）
#### 新建标签 
    $ git tag<tagname> 
默认为HEAD，也可以指定一个commit id。
#### 查看标签信息
    $ git show <tagname>
#### 创建带有说明的标签
    $git tag -a <tagname> -m <description> <branchname> or commit_id
用-a指定标签名，-m指定说明文字
#### 查看所有标签
    $ git tag
#### 推送某个标签到远程
    $ git push origin <tagname>
#### 一次性推送全部尚未推送到远程的本地标签
    $ git push origin --tags
#### 删除一个本地标签
    $ git tag -d <tagname>
#### 删除一个远程标签
    $ git push origin :refs/tags/<tagname>