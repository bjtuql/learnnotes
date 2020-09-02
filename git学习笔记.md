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

  ![](E:\desktop\1599022428(1).jpg)

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

  



