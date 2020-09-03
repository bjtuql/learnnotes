## 创建版本库

- 版本库又名仓库，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

  ```
  mkdir gitlearn
  cd gitlearn
  pwd
  git init
  ```

- 通过 git init命令把这个目录变成可以管理的仓库

## 把文件添加到版本库

- 编写README.md文件

- ```
  git add README.md
  git commit -m "wrote a readme file"
  # -m表示本次提交的说明
  ```

- 更改README.md文件

  ```
  git status
  # git status告诉我们，将要被提交的修改包括README.md
  git diff README.md
  # 查看difference
  git add README.md
  git commit -m "update readme file"
  ```

- 为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，例如

  ```
  git add file1.txt
  git add file2.txt file3.txt
  git commit -m "add 3 files."
  ```

## 版本回退

- 不断对文件进行修改，然后不断提交修改到版本库里

  ```
  # 修改README.md最后一行内容
  git diff README.md
  git status
  git add README.md
  git commit -m "modify the last line"
  git log
  # 告诉我们历史记录
  git log --pretty=oneline
  # 嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数
  ```

- ```
  # 暂存区的回滚
  git reset --hard 版本号
  # 切换到上上⼀个版本，HEAD^^，HEAD^^^上上上⼀个
  git reset HEAD^^
  # 查看每⼀次命令
  $ git reflog
  ```

## 工作区和暂存区

- 工作区是我们电脑里能看到的目录

- 工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

  ![1599135113_1_.jpg](https://i.loli.net/2020/09/03/p6NJknCHwtaWsf9.png)

- 第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

  第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

  因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改

## 撤销修改

- 命令`git checkout -- README.md`意思就是，把`README.md`文件在工作区的修改全部撤销，这里有两种情况：

  一种是`README.md`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

  一种是`README.md`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

  总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

## 暂存区与远程仓库的连接

- ```
  #⽣成ssh密钥
  ssh-keygen -t rsa -C “你的github邮箱”
  #电脑下根据路径查找到密钥
  #github官⽹下setting
  git remote add origin + github仓库⽹站
  git push -u origin master
  ```


## 分支管理

- 当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

- 假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并

- 合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

- 实战

  - ```
    git checkout -b dev
    # git checkout命令加上-b参数表示创建并切换，相当于以下两条命令
    # git branch dev
    # git checkout dev
    ```

  

  - ```
    # 用git branch命令查看当前分支
    # git branch命令会列出所有分支，当前分支前面会标一个*号
    # 我们就可以在dev分支上正常提交，比如对README.md做个修改，修改后提交
    git add README.md
    git commit -m "branch test"
    # 切换回master分支后，再查看一个README.md文件，刚才添加的内容不见了！因为那个提交是在dev分支上，而master分支此刻的提交点并没有变
    # 把dev分支的工作成果合并到master分支上
    git merge dev
    # git merge命令用于合并指定分支到当前分支。合并后，再查看README.md的内容，就可以看到，和dev分支的最新提交是完全一样的。注意到上面的Fast-forward信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。当然，也不是每次合并都能Fast-forward，我们后面会讲其他方式的合并。合并完成后，就可以放心地删除dev分支了
    git branch -d dev
    git branch
    ```

  - 总结：

    ```
    Git鼓励大量使用分支：
    查看分支：git branch
    创建分支：git branch <name>
    切换分支：git checkout <name>或者git switch <name>
    创建+切换分支：git checkout -b <name>或者git switch -c <name>
    合并某分支到当前分支：git merge <name>
    删除分支：git branch -d <name>
    ```

- 当master分支和feature1分支同时提交修改；当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

  解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

  用`git log --graph`命令可以看到分支合并图

## 分支策略

- 在实际开发中，我们应该按照几个基本原则进行分支管理：

  首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

  那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了

- 合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。





