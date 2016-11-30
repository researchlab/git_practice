#git_demo 

for learning git command.


## 回退单个文件到指定版本步骤:

假如要回退`test.txt`文件

1. `$git log test.txt`  查询`test.txt`文件提交的所有记录（包含commit id 和注释) , 假如通过这条命令查到要回退到`commit_id`为`721c875ded8e4a3ed164c940714fdf78cef8692f`的`test.txt`文件版本.

2. `$git checkout 721c875ded8e4a3ed164c940714fdf78cef8692f test.txt`  这条命令就将`test.txt`文件回退到了上述指定的版本中了. 

3. `$git commit -m"update test.txt"` 通过上述步骤2回退`test.txt`文件之后，不需要再用`git add`添加修改， 而是直接用`git commit -m""`提交修改即可

4. `$git push origin dev -vvv` 这个步骤就是提交到远程库生效.


> 在第2步 git checkout 要特别注意， 如果其它文件依赖本次回退test.txt中新的修改代码时， 拉么回退test.txt之后 导致其它依赖文件或许也要做相应的修改操作否则会存在问题.


## 创建新的分支步骤:

假如现在在`master`分支上.

1. `$git checkout -b dev` 创建并check到`dev`分支

2. `$git push origin dev -vvv` 从上一步创建`dev`之后，直接通过`git push`命令就可以向远程库提交`dev`以创建远程`dev`分支

也可以基于远程分支创建本地分支命令如下:

1. `$git checkout -b dev origin/master` 本命令表示基于`origin/master`分支来创建`dev`分支


## master分支合并dev分支的代码步骤:

假如现在在`dev`分支上.

1. `$git checkout master` 切换到`master`

2. `$git pull origin dev`  在`master`分支上`pull`下来`dev`分支的代码

3. `$git push origin master -vvv` 在确认`pull`下拉的`dev`分支的代码没有问题之后，通过`push`命令更新远程库`master`分支的代码


但是不建议直接用`pull`来合并其它分支的代码， 而建议使用`fetch`（下载）和`merge`(合并）两个命令做合并操作;步骤:

1. `$git fetch origin dev` 表示将`dev`分支的新代码下载到本地

2. `$git merge origin dev` 表示合并`dev`的代码


> 如果merge前， 想要看看fetch的代码和本分支的代码存在哪些差异和修改， 可以用$git diff master origin/dev 来查看差异
