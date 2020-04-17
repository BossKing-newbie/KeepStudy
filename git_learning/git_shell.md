## 1.创建版本库

```git
$ git init
```

## 1.2.把文件添加到仓库中

#### 1.2.1 往仓库添加文件

```git
$ git add readme.txt
```

#### 1.2.2 提交文件到仓库中

```git
$ git commit -m "wrote a readme file"
```

- 简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

- 为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件.

#### 1.3 小结

- 初始化一个Git仓库，使用`git init`命令。

- 添加文件到Git仓库，分两步：


- 使用命令`git add `，注意，可反复多次使用，添加多个文件；
- 使用命令`git commit -m `，完成。

## 2. 版本回退

- ​	在实际工作中，我们脑子里怎么可能记得一个几千行的文件每次都改了什么内容，不然要版本控制系统干什么。版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用`git log`命令查看。

#### 2.1 git log


```git
$ git log

```

- ![avatar](.//image//git_log.png)`	
- git log`命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是`append GPL`，上一次是`add distributed`，最早的一次是`wrote a readme file`。
- 如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：


```git
$ git log --pretty=oneline
```

- 首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。
- 尝试将当前版本回退到上一个版本，就可以使用```git reset```命令

#### 2.2 git reset

- ```git
  $ git reset --hard HEAD^
  ```

- Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。

- `HEAD`指向的版本就是当前版本

#### 2.3 git reflog

- ```git
  $ git reflog
  ```

  ![avatar](./image/git_reflog.png)

- 在Git中，总是有后悔药可以吃的。当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到`append GPL`，就必须找到`append GPL`的commit id。Git提供了一个命令`git reflog`用来记录你的每一次命令：

#### 2.4 小结

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本

## 3. 工作区和暂存区

- Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。

####  3.1 工作区（Working Directory）

- 就是你在电脑里能看到的目录

#### 3.2 版本库

- 工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。
- Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。
- 前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：
- 第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；
- 第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。
- 因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。
- 可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

## 4. 管理修改

- 为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。
- Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交.
- 每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。

## 5. 撤销修改

#### git checkout -- file

- 假如在修改一份文件时，不小心加入某一行，但是在此之前已经将该文件使用```git add```命令加入暂存区，此时可以使用$ ```git checkout -- readme.txt```  丢弃工作区的修改。 
- 命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：
- 一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
- 一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
- 总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。
- 用命令`git reset HEAD `可以把暂存区的修改撤销掉，重新放回工作区：
- `git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。

#### 5.1 小结

- 又到了小结时间。
- 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。
- 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD `，就回到了场景1，第二步按场景1操作。
- 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节，不过前提是没有推送到远程库。
- 在最新版中git reset HEAD 可以使用 git restore --staged

## 6. 删除文件

#### 小结

命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。

## 7.添加远程库

#### 小结

- 首先在github创建仓库

- 然后复制SSH的git地址

- ```git
  $ git remote add origin git@github.com:BossKing-newbie/KeepStudy.git
  ```

- 添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。

- 下一步，就可以把本地库的所有内容推送到远程库上

- ```git
  git push -u origin master
  ```

- ![](.//image//git_github.png)

- 把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

- 由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

- 推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

- ![](.//image//git_origin_github.png)

- 从现在起，只要本地作了提交，就可以通过命令：

- ```$ git push origin master```

- 要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

- 关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

- 此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

- 分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

## 8.从远程库克隆

#### 小结

- 要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。
- Git支持多种协议，包括`https`，但`ssh`协议速度最快。

## 9. 创建与合并分支

- 在版本回退里，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即`master`分支。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

- 一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

- ![Image Text](https://www.liaoxuefeng.com/files/attachments/919022325462368/0)

- 每次提交，`master`分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长。

- 当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

- ![](https://www.liaoxuefeng.com/files/attachments/919022363210080/l)

- 你看，Git创建一个分支很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！

- 不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：

- ![](https://www.liaoxuefeng.com/files/attachments/919022387118368/l)

- 假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：

- 所以Git合并分支也很快！就改改指针，工作区内容也不变！

- 合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

- 首先，我们创建dev分支，然后切换到dev分支：

- ```git
  git checkout -b dev
  ```

- `git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

- ```git
  git branch dev
  git checkout dev
  ```

- 使用git branch命令查看当前分支：

- ```git
  git branch
  ```

- `git branch`命令会列出所有分支，当前分支前面会标一个`*`号。然后，我们就可以在`dev`分支上正常提交。

- 如果dev分支的工作完成，我们就可以切换回master分支：

- ```git
  git checkout master
  ```

- 切换回master分支后，会版本回退到master提交点。

- 把dev分支合并到master分支上，可以使用以下命令：

- ```git
  git merge dev
  ```

- `git merge`命令用于合并指定分支到当前分支。合并后，再查看`readme.txt`的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

- 注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

- 合并完成后，就可以放心地删除`dev`分支了

- ```git
  git branch -d dev
  ```

- 因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

- 实际上，切换分支这个动作，用`switch`更科学。因此，最新版本的Git提供了新的`git switch`命令来切换分支：

- 创建并切换到新的`dev`分支，可以使用：

- ```git
  git switch -c dev
  ```

- 直接切换到已有的`master`分支，可以使用：

- ```git
  git switch master
  ```

- 使用新的`git switch`命令，比`git checkout`要更容易理解。

#### 小结

- Git鼓励大量使用分支：
- 查看分支：`git branch`
- 创建分支：`git branch `
- 切换分支：`git checkout `或者`git switch `
- 创建+切换分支：`git checkout -b `或者`git switch -c `
- 合并某分支到当前分支：`git merge `
- 删除分支：`git branch -d `

## 10.解决冲突

#### 小结

- 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
- 解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。
- 用`git log --graph`命令可以看到分支合并图。

## 11. 分支管理策略

#### Fast forward模式与普通模式合并

- 通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

- 如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

- 在dev分支上进行开发后，切换回master分支，`git switch matser`，准备合并dev分支时，使用普通模式合并：

- ```git
  git merge --no-ff -m "merge with no-ff" dev
  ```

- 合并后使用`git log`可以查看分支历史

#### 分支策略

- 在实际开发中，我们应该按照几个基本原则进行分支管理：
- 首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
- 那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；
- 你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。
- 所以，团队合作的分支看起来就像这样：
- ![](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)

#### 小结

- Git分支十分强大，在团队开发中应该充分应用。
- 合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。