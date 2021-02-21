---

title: GIT 教程
date: {{ date }}
updated: {{date}}
tags: Git
categories: Git

---

重要概念:

- 三方合并原则
- git 提交的本质
- git 操作和指针之间的关系...
- 冲突合并
- 什么时候冲突





### git 分区

- 工作区 (working directory)  暂存区(stage)   版本库

  > 关系:  工作区  add  暂存区  commit  版本库

- `git status `:可以追踪工作区的文件 ,红色文件代表工作区 绿色文件代表暂存区

  

- `git add ...`   使文件加入暂存区，被纳入版本控制，修改时可以被追踪

- 显示 new file,对其更改，删除，更改会显示modified状态，可以再次add把已修改的文件加入暂存区，然后进行提交，通过 checkout可以恢复，也可以通过reset不暂存，不暂存的话就变成了未追踪状态，可以重复以前的操作。 删除和更改操作一样，当删除过后，如果利用reset就会撤销暂存，并且文件无法恢复

- `git commit  -m'注释' `    加入版本库   更改删除发生状况，首先把文件全部提交完后当本地仓库和工作区同步时，则status什么都不显示,当更改后必须重新通过add加入暂存然后再次commit ，或者通过checkout（从暂存区？）恢复文件，删除情况与更改一样

  > `git commit -am '注释'`    即相当于是**`git add .`** 与 **`git commit`** 两步操作的合成

  > **该方式只适用于已被`git`追踪的文件（即文件至少提交过一次）**
  >
  > **当文件第一次提交到暂存区时（此时该文件并未被`git`追踪）不可以使用该命令，而是要分开写，否则会报错**







#### Git 日志

- 主要分为三大类：分支的**提交日志**、文件的**修改日志**、`git`的**操作日志**

- `git log`  查看版本库提交日志 ,提交历史是倒叙的，最新的提交排在最前面

  > `git log -3`最近几条  `git log --graph`  图形化显示提交历史
  >
  > `git log --pretty=oneline`   以一行的形式显示提交历史
  >
  > `git log --all --decorate --oneline --graph`  

- `git blame file_name`   查看修改日志

- `git reflog`  查看操作日志

  > 区别: **`git log`**：只能显示**当前分支**的提交历史，如果进行版本回退，会丢失较后版本的提交信息 这时通过操作日志进行查看..
  >
  > 总体上来说，操作日志包含了修改日志和提交日志，是最全的`git`日志

  

### git 删除、修改、撤销

- `git rm <file>`   **该命令用于删除版本库中的文件**；如果用来删除工作区和暂存区中的文件都会报错

  > 本质上是两步操作 :  删除版本库中的文件, 把删除更改状态加入暂存区
  >
  > `git rm --cached <file>`    只删除暂存区的file文件 ,不在追踪该文件

- `rm <file>`  命令只能用于删除工作区和版本库中的文件, 不能删除暂存区文件

  > 与`git rm`**不同的是**，该指令不会将删除操作纳入**暂存区**。

#### 重命名文件

- `git mv <file1> <file2>`  该命令会把重命名操作状态纳入暂存区
- `mv <file1> <file2>` 系统命令  和git命令区别在于不会将操作提交到暂存区



#### 比较文件

- 系统命令 `diff  file1 file2` 比较本地文件

- 比较**暂存区**和**工作区**中的同一文件

  > `git diff`  该命令自动比较 工作区的修改后的文件 和暂存区的目标文件 

- 比较**版本库**和**工作区**中的同一文件

  > `git diff commit_id`  用于比较指定commit id提交上的`A`文件和工作区中的`A`文件
  >
  > `git diff HEAD`  用于比较最新提交上的`A`文件和工作区中的`A`文件

- 比较**版本库**和**暂存区**中的同一文件

  > `git diff --cached commit_id`  
  >
  > `git diff --cached`     作用和上一部分一样

#### 撤销修改

- **将暂存区恢复到工作区**:

  > 将已经纳入**暂存区**的修改撤销，恢复到**工作区**修改但没有提交的状态，再恢复到修改前

- > `git restore --stage <file>`  将文件从**缓存区**中移动到**工作区**参数`--stage`写成`--staged`效果是一样的
  >
  > `git reset HEAD <file>`  

- **撤销工作区的操作**

  > 撤销工作区中对文件的操作，包括新增、修改、删除等
  >
  > 原理: 使用暂存区的文件进行比对, 对工作区进行回滚, 所有提交到暂存区后就无法改变修改

- > `git checkout -- <file>`  可以撤销工作区中对`flie`文件的改动操作, `--`后面加上空格, 修改如果通过git add 提交到暂存区,该指令无效
  >
  > `git restore <file>`  可以撤销**工作区**中对`file`文件的操作,效果和上式相同



#### 修改提交注释和作者

- `git commit --amend -m` '修正信息'

  > 当需要为最近一次提交添加大量注释时: `git commit --amend`

- `git commit --amend --author 'Name<email>'`

- > 修改提交注释的同时，会创建了一个新提交替换了原来需要修正的提交, 也就是说日志中只有一次提交而不是两次

- 修改特定提交信息

  > `git rebase -i commit_id` 
  >
  > `git rebase -i HEAD~n`   其中`n`表示需要显示的最近`n`次提交记录

#### 获取帮助

- `git config help ` 跳转网页 获取config命令的说明



### 分支

- 在开发当中，往往需要分工合作,`git`为我们提供的分支功能就能实现这一需求

- > `git branch`  查看当前版本库中的所有分支
  >
  > `git branch -a`  查看所有本地分支，包括本地分支和本地远程分支
  >
  > `git branch -v`   查看所有本地分支上最近一次的提交记录
  >
  > `git branch -av`    查看所有分支的最近一次提交记录
  >
  > `git branch -r`   参数用于单独查看本地远程分支
  >
  > `git branch -vv`   表示查看所有本地分支与远程分支的关联情况

- **本地分支**

- > `git branch <branch_name>`    创建分支
  >
  > `git checkout -b <branch_name>`  创建并切换到新的分支
  >
  > `git checkout <branch_name>`  切换分支
  >
  > `git checkout -`  切换回上次操作的分支：

- **重命名分支**: `git branch -m <oldName> <newName>`

- **删除本地分支:**  `git branch -d new_branch`  

  > 不能删除本地分支 ,
  >
  > 当需要删除的分支上有`master`分支没有的内容，并且删除前没有进行合并（`merge`）时，会报错  提示参数改为 D

- 分支合并

  > `git merge <branch_name>` 这里所讲的分支指的是有公共提交节点的分支
  >
  > 当两分支没有公共提交节点 采用`rebase`进行合并

- `git merge`原理

  >  这种合并方式叫做：`Fast-forward`，**没有冲突**，改变的只是`master`指针指向
  >
  > `git merge --no-ff dev`  禁用Fast-forward模式, 会触发一次提交,而不是改变指针
  >
  > 注意两者的区别

  

- 分支合并原则

- 分支的本质

  > **分支：指向一条`commit`对象链或一条工作记录线的指针**   
  >
  > `HEAD`始终指向当前分支
  >
  > 对于`HEAD`修改的任何操作，都会被`git reflog` 记录下来

- > 我们可以通过`git`的底层命令`symbolic-ref`来实现对`HEAD`文件内容的修改



- 合并冲突

  > 分别对两分支上的`test.txt`文件进行修改，并分别将修改**提交**到各自的分支,会发生合并冲突, 因为 `git`合并时并不知道以哪个分支的修改为标准,不能采用`Fast-forward`方式自动合并

- 解决冲突

  > **选择需要保留的内容，手动解决合并冲突** ,只需要在文件中保留想要的内容即可,
  >
  > 然后使用add把修改提交到暂存区, 通过commit提交完成合并(merge)
  >
  > 一般情况下 我们使用工具解决冲突

- 合并分支的实质就是不同提交的合并，以及`HEAD`和分支指针的移动；



### 远程仓库github、git图形

- `git`裸库 : 没有工作区的`git`仓库 , 只起到代码托管的作用而不需要也不应该修改服务器上的代码。

  > `git init --bare`   创建裸库

- 本地仓库与远程版本库

  > 远程仓库的实质就是本地仓库中的**版本库**对应的**远程版本库**，通常直接叫做**远程仓库**。

- > `git push`：将**本地仓库某分支**上的代码推送到**远程仓库的某分支**上, 推送操作是分支对分支的,而不是将本地仓库的所有分支都推送到远程仓库
  >
  > `git pull`:  将远程仓库文件拉取到本地版本库

- `push、pull`完整的写法

  > git push origin srcBranch:destBranch   `srcBranch`表示的是**本地分支**，`destBranch`表示的是**远程分支**
  >
  > git pull origin srcBranch:destBranch   `srcBranch`表示的是**远程分支**，`destBrach`表示的是**本地分支**；
  >
  > 可以这样理解：`srcBranch`表示**从哪里来**，`destBranch`表示**到哪里去**； 

- `git push`

  > 特殊的情况 : 只有一个用户操作远程仓库, 手动的改变远程仓库文件, 而不通过本地推送来改变远程仓库文件,再次推送时会出现错误, 必须先进行pull操作,这时本地才会追踪修改的操作
  >
  > 多人共用同一远程仓库, 用户需要在pull操作之后 再次进行push, 如果直接推送发生错误,原因在于：不能覆盖别的用户做出的文件修改, 需要pull 之后进行merge操作 

- `git pull` 

  > 在进行拉取操作的过程中,会执行合并操作（`merge`）合并操作可能成功也可能失败
  >
  > `pull`操作其实是`fetch`操作和`merge`操作的合成
  
- #### 推送代码

  > `git push -u origin master`    加上`-u`表示将本地的`master`分支与远程的`master`分支**建立关联**；再次推送时就可以直接通过`git push`进行推送了。

- **查看远程仓库地址**

  > `git remote` //只显示远程仓库地址名
  >
  > `git remote -v` //显示远程仓库地址名和对应URL 
  >
  > `git remote show origin` //显示详细信息

- 修改远程仓库

  > 方法一:
  >
  > `git remote rm origin `
  >
  > `git remote add origin https://gitee.com/ahuntsun/gitTest.git`
  >
  > 方法二 :
  >
  > `git remote set-url origin https://163.com`
  >
  > 方法三: 修改.git中的config仓库

- SSH协议建立本地和远程仓库的联系
  

### 版本回退

- 在`git`中永远有后悔药可吃，总是可以回到**版本库**的某一个时刻，这就叫做**版本回退**

- `git reset`   常用的有`--mixed`、`--soft`和`--hard`三种参数

  > `--mixed` 将文件回退到**工作区**，此时会保留**工作区**中的文件，但会丢弃**暂存区**中的文件  为默认值，等同于`git reset`
  >
  > `--soft`：作用为：将文件回退到**暂存区**，此时会保留**工作区**和**暂存区**中的文件；
  >
  > `--hard`：作用为：将文件回退到**修改前**，此时会丢弃**工作区**和**暂存区**中的文件；

- > 使用 `git reset --soft head^ `    **回退一次**
  >
  > `git reset --hard HEAD^^ `  回退二次
  >
  > `git reset --hard HEAD~n`  回退n次
  >
  >  `HEAD`可以更换为分支名
  >
  > 
  >
  > `git reset --hard commit_id`     **该指令的作用为回退到指定的`commit id`  (一般前6位)的提交版本 **
  >
  > 通过`commit id`的回退方式**既可以向前回退，也可以向后回退。**

- `git revert`    不同于`reset`直接通过改变分支指向来进行版本回退，并且不产生新的提交；`revert`是通过额外创建一次提交，来取消分支上指定的某次提交的方式，来实现版本回退的

  > `-e`参数是`--edit`的缩写，为`revert`指令的默认参数，即`git revert -e`等同于`git revert `    在重做过程中，新建一次提交的同时编辑提交信息\
  >
  > `--no-edit`   参数 不编辑新增提交的注释信息
  >
  > `-n`参数是`--no-commit`的简写形式  不进行提交 仅仅把改变放入暂存区,通过手动提交完成回滚操作

- `revert`指令也有多种写法，下面介绍主要的几种。

  > `git revert commit_id`    通过`commit_id`精准地选择想要重做的提交。
  >
  > 重做非最新一次提交，会发生冲突。

  > `git revert HEAD`   ... 和 reset 语法相同

- 撤销`revert`操作  , 再次通过`revert`操作取消上一次的`revert`操作, 即所谓"负负得正"）



- `git checkout`  

  > `git checkout commit_id`    使用`checkout`可以进行版本回退
  >
  > 通过checkout 回退 使组成的`commit`对象链条处于游离状态；
  >
  > - 通过`checkout`进行版本回退会造成游离的提交对象链，需要额外创建一个分支进行保存
  > - 因此，使用`checkout`进行版本回退的思路为，先切换到想要回退的提交版本，再删除进行版本回退的分支`dev`。最后，创建一个新的`dev`分支指向游离的提交对象链，完成分支`dev`的版本回退，简称"偷天换日"；

  

- 由于`checkout`会造成游离的提交对象链，所以，一般不使用`checkout`而是使用`reset`和`revert`进行版本回退

- > 在个人开发上，建议使用`reset`；但是在团队开发中建议使用`revert`，特别是公共的分支（比如`master`)，这样能够完整保留提交历史，方便回溯。



- `git stash`的作用

- > 对没有提交到版本库的，位于工作区或暂存区中游离的修改进行保存，在需要时可进行恢复        这些游离的修改在切换分支时都将会丢弃

- 位于工作区和暂存区中的修改都会被`checkout`操作覆盖  

- > 这种情况在日常开发中很常见，当在`develop`分支上开发新功能的时候，`master`分支出现紧急情况需要切换回去进行修复。但是，当前分支的新功能还没开发完全，贸然切换分支，原来开发的内容就会因被覆盖而丢失，怎么办呢？
  >
  > 使用git stash 暂存修改

- 可见`git stash`可以将当前分支上，位于在工作区或暂存区中的修改，在未提交的情况下进行了保存；并且将分支回退到修改前的状态
  
  > 在切换回该分支时, 需要从stash区域中恢复保存的修改
  >
  
  > `git stash save '注释'`   保存修改时给与注释
  >
  > `git stash list`   查看保存的修改
  
- 主要有三种方法恢复 stash存储的修改

  > `git stash pop`   恢复最新的修改 把修改从stash中删除
  >
  > `git stash apply`    恢复但不删除`stash`中存储的最新修改；
  >
  > > 使用该指令时发生了合并冲突。这是因为，`stash`中保存的每一次修改代表的都是一个版本。
  >
  > `git stash apply stash@{n} `   ，作用为从`stash`中恢复特定的修改，并且不删除`stash`中的该修改。
  >



### git配置  

- `git config--`   三个作用域: system global local  优先级一次递增

  > system: 一台计算机（操作系统）上的所有用户
  >
  > global : 计算机中的某用户创建的所有项目
  >
  > local : 某一特定的版本库

- 通过**`git config --list`：**可以批量查看配置信息(所有的配置信息 包括local global配置)

  > 通过 `cat  .git/config` 查看Local配置   `cat ~/.gitconfig `查看全局配置

- `git config --作用域  --unset <参数名>`    删除配置 

  > 可以用来修改用户名..  

- `.gitignore` 文件中配置不被git追踪的文件

  > 注意在`.gitignore`文件中**一行写一个**文件名；`.gitignore`文件也支持**正则表达式**

- 如果某些文件已经被纳入了版本管理中，那么修改`.gitignore`是无效的,仍然会发生追踪

     > 清除`git`本地缓存，将文件转变为`untrack`状态，然后再提交:
     >
     > git rm -r --cached .



### Git 协作流程和Git pull 常见问题

- `Gitflow` 基于git分支的开发模型

  > 一般有`develop`分支   `test`分支  `master`分支   `bugfix(hotfix)`分支

- `SVN`方式（典型模型）

  > 多个用户对同一个仓库进行推送,主要原因在于每个用户都需要 pull之后再次push,

- git pull 冲突问题

-   `git pull`同源合并冲突

  > 所谓同源，指的是本地仓库与远程仓库中的分支从根提交节点开始，有共同的提交历史

- #### `git pull`不同源合并冲突

  > 所谓不同源，指的是两个仓库中的分支，根提交节点不同，



### Git refspec

- 本地仓库与远程仓库的分支映射关系：`git refspec`, 没有映射的情况下,无法进行git push

  > 通过`git branch -vv`查看本地与远程分支的关联情况

- `git`中其实有三种分支：本地分支、本地远程分支、远程分支

  > 本地远程分支是远程分支的一个镜像，并且在本地仓库与远程仓库之间起到一个桥梁的作用, 当本地仓库中的每一个分支都有与之关联的远程分支之后，本地仓库都会创建对应的**本地远程分支**
  >
  > 比如本地远程分支`origin/develop`就对应着远程分支`develop`。

- 本地远程分支是为了追踪远程分支而存在的，只有在执行`pull`或`push`操作时它的指向才会更新。

- **设置同名远程分支:**

  > `upstream branch`表示上游分支，即远程仓库的分支

- 建立本地与远程分支追踪关系的。类型一

  > 方法一: `git push --set-upstream origin <branch> `  : 在远程仓库创键一个与本地分支develop**关联**的同名分支，并将本地分支develop的文件推送到该远程分支上。
  >
  > 方法二 :  `git push -u origin <branch>`  :为本地仓库创建对应的远程分支
  >
  > 使用上述方法，只需设置一次，之后就可以使用简写形式`git push`进行推送
  >
  > 
  >
  > 方法三: `git push origin HEAD`  / `git push origin <branch>`设置对应的远程分支, 因为没有跟踪分支信息, 所以之后无法直接使用git push

  > **设置不同名远程分支:**
  >
  >  `git push --set-upstream origin <branch1>:<branch2>`   其他方法也可以使用类似完整的命令
  >
  > 
  >
  > 注意: 不同名的远程分支进行推送都不可以使用 git push, 因为名字不一样, 必须使用完整的命令的进行推送

  

- 不建立本地与远程分支追踪关系的。类型二

  > `git push origin HEAD`
  >
  > ` git push origin <branch_name>`  
  >
  > 使用该方法,每次都需要使用完整写法

  

- `git push origin master`与`git push -u origin master`的区别

  > 第一次将本地仓库的`master`分支推送到远程仓库的`master`分支上时，使用前者和后者都可以顺利推送
  >
  > 推荐使用 -u 参数,因为之后可以直接进行 git push推送

- `git push -f origin master`  : **强制推送**,直接跳过与远程仓库的`master`分支合并的环节,即以本地的`master`分支内容为准。 慎用该命令, 

  > 如果设置了分支追踪关系, 使用 `git push -f`



- **设置远程分支对应的本地分支** 

  > `git checkout -b <branch> origin/<branch>`    
  >
  > `git checkout --track origin/<branch>`  

  

- **删除远程分支**

  > `git push origin :destBranch`    将空的分支推送到远程分支，这样就能将该远程分支删除
  >
  > `git push origin --delete destBranch`

- `git remote prune origin`   删除无效的远程分支对应的本地远程分支

  > 当远程分支被删除时, 那么对应的本地远程分支就是无效的

  
  
- **重命名分支**
  
  > 本地分支:
  >
  > `git branch -m dev develop`     将本地分支`dev`重命名为`develop`
  >
  > 远程分支: 
  >
  > 无法直接重命名远程分支，只能通过先删除原来的远程分支，再创建一个新名称的分支
  
  
  
### git 标签

- `Git`标签有两种：`轻量级标签`  `带有附注的标签`  , 标签标识的是某一次提交，这次提交可以是任何分支上的任何一次提交。

- 什么时候打标签?  版本发布时,会打上类似v1.0 的标签

- **创建标签:**

  > `git tag <tag_name>`   
  >
  >  `git tag -a <tag_name> -m '注释'`

- 查看标签

  > `git tag`   
  >
  > `git show <tag_name>`   

- 查找标签

  > `git tag -l <tag_name>`    该方式支持正则表达式查找；

- 标签推送到远程

  > `git push origin <tag_name>`  将本地分支上的标签推送到远程仓库
  >
  > 也可以利用此推送多个本地标签
  >
  > `git push origin --tag`   推送本地所有的标签到远程仓库

- 删除标签

  > 远程:
  >
  > `git push origin :<tag_name>`    
  >
  > `git push origin --delete <tag_name>`
  >
  > 本地:
  >
  > `git tag -d <tag_name>`   

- 切换标签

  > `git checkout <tag_name>`   切换标签与使用`reset`进行版本回退十分相似。只不过切换标签只改变了`HEAD`指针的指向，会造成游离的提交。若有需要可以创建一个新分支进行保存。

- Git别名

  > `git config <作用域> alias.<别名> '<命令>'`   设置`git`命令别名
  >
  > 设置完成后，该配置会被写入系统用户的配置文件`gitconfig`中

- 垃圾回收 : git gc 

### Git cherry-pick rebase

- `Git cherry-pick`的作用为移植提交。采用先删除再添加的方法将会很繁琐，而使用`cherry-pick`就能轻松实现这一需求。

  > 将多余的提交通过 cherry-pick 移植提交, 然后删除该分支上多余的提交

- `rebase`有两个意思：**变基**、**衍合**，即变换分支的参考基点

### git子库

- `git`提供了子库的概念。可以将这些子模块存放在不同的仓库中，通过`submodule`或`subtree`实现仓库的嵌套。

