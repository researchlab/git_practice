#git_demo 

for learning git command.


## 回退单个文件到指定版本步骤:

假如要回退`test.txt`文件
1. `$git log test.txt`  查询`test.txt`文件提交的所有记录（包含commit id 和注释) , 假如通过这条命令查到要回退到`commit_id`为`721c875ded8e4a3ed164c940714fdf78cef8692f`
2. `$git checkout 721c875ded8e4a3ed164c940714fdf78cef8692f test.txt`  这条命令就将`test.txt`文件回退到了上述指定的版本中了. 
3. `$git commit -m"update test.txt"` 通过上述步骤2回退`test.txt`文件之后，不需要再用`git add`添加修改， 而是直接用`git commit -m""`提交修改即可
4. `$git push origin dev -vvv` 这个步骤就是提交到远程库生效.


## 创建新的分支步骤
假如现在在`master`分支上.
1. `$git checkout -b dev` 创建并check到`dev`分支
2. `$git push origin dev -vvv` 从上一步创建`dev`之后，直接通过`git push`命令就可以向远程库提交`dev`以创建远程`dev`分支

