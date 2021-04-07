# tips from learning git command 

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

2. `$git merge origin/dev` 表示合并`dev`的代码


> 如果merge前， 想要看看fetch的代码和本分支的代码存在哪些差异和修改， 可以用$git diff master origin/dev 来查看差异
> 注意合并dev分支的命令是 $git merge origin/dev 而不是 $git merge origin dev.

## dev分支合并master分支代码的步骤:

假如现在`dev`分支上.

1. `$git fetch origin master` 下载`master`分支上新的代码
2. `$git diff dev origin/master`  查看下载的代码与本地`dev`的差异
3. `$git merge origin/master` 确认无误后合并`master`分支代码


### 注意事项

1.  假如`master`为项目的最终主分支, 拉么其它子分支每次提交代码到自己分支分支时，均应该先执行`fetch`和`merge`最终主分支`master`代码，确认无冲突和错误时，才提交到本分支，这样便于最终主分支每次合并其它分支时都没有冲突和错误存在,减少沟通和修改时间(如果主分支合并者与子分支不是同一个人开发，辣么合并主分支产生冲突和错误时，就需要子分支的同事一起协同才能解决，而这在提交自己分支时先`fetch`和`merge`主分支后可以避免的）

## 借助git tag 标签快速准确回退版本

1.打标签
发布文档版本之后一般建议打一个标签，方便标识和相关操作,
```bash 
$git tag -a v1.0 -m"v1.0 stable version"
```
2.推送标签到远程仓库
当本地打完标签觉得可以了之后，就应该及时推送到远程仓库，
```bash 
$git push origin --tags
```

3.查看有哪些标签

3.1 查看有哪些标签名称
```bash
$git tag
```

3.2 具体查看某个标签的详细信息
```bash 
#$git show <tag-name>
$git show v1.0
```

4.删除标签

4.1 删除本地标签
```bash
# git tag -d <tag-name>
git tag -d v1.0
```

4.2 删除远程仓库的标签
```bash 
$git push origin :v1.0					#方法一
$git push origin :refs/tags/<tag-name>  #方法二
$git push origin –delete <tag-name>     #方法三
```

5.借助标签快速向后回退版本

每次发布版本之前都应该对前一次稳定版本打上一个标签，当新发布版本存在bug时，则可以通过标签快速回退到标签指定的稳定版本,具体实施步骤,

5.1 为当前bug版本新建一个分支
```bash
# $git checkout <tag-name> <new-branch-name>
$git checkout v1.0 bugfix 
# 或者 直接在bug版本目录下， git checkout -b bugfix 
```

5.2 借助tag回退到稳定版本
```bash
$git show <stable-version-tag-name> #查询待回退稳定版本的commit-id(假如commit-id 前六位123456)
$git reset --hard <commit-id> #执行回退
$git push -u origin master #将回退版本推送到远程服务器
```

5.3 合并已修复的bugfix分支
```bash
$git checkout master #回到主分支
$git merge bugfix #合并
```

6.向前回退版本
当执行`$git reset --hard <commit-id>`命令向后回退之后，用`git log`是看不到之前提交的`log`了， 那如果要向前回退应该怎么办？ 可以借助`$git relog`命令来操作, 
```bash 
$git relog #查询待向前回退版本的commit-id
$git reset --hard <commit-id> #执行回退
```


## git config

全局配置和项目配置

全局配置信息在: `~/.gitconfig`

项目配置在项目目录下的： `./.git/config`

`git config --global`操作全局配置, 不带`--global`选项的话，会尝试相对于当前目录的：`./git/config`, 找不到的话，报错。

为各个项目单独配置`user.name`和`user.email`

你可能会在不同的几个项目中工作，各个项目的用户名可能不同，为了保证日志的准确性和提交时无误，最好对各个项目设置`user.name`和`user.email`

```bash
# for global setting
git config --global user.name xxx
git config --global user.email xxx@xxx.com

# for repository
git config user.name xxxx
git config user.email xxxx@xxx.com
```

在本地`git clone`一个远程项目或`git init`一个新项目时， `git`提交默认使用的是全局的`user.name`和`user.email`信息， 可以在当前项目根目录下使用下面的命令构建属于当前项目提交着的名字和邮箱，如下:
```bash 
git config user.name xxx
git config user.email.xxx@xxx.com
```
在当期项目根目录下执行上述两条命令之后，在`.git/config`文件中会新增如下信息， 当然如果直接手动在增加如下信息，而不使用上述命令，效果也是一样的.
```bash
[user]
	name =  xxxxxx
	email = xxxxxxx 
```

> 只有当 user.name 和 user.email 设置和github帐号完全相同，并且在上述设置完成之后git add 添加的提交 才会被github认为是github帐号者提交的，否在被认为是第三方提交.


## 远程仓库回滚

### 删除最后一次提交

使用`revert`或`reset`命令都可以方便的删除最后一次提交， 如下:

```bash
git revert HEAD
git push origin master
# 或者 
git reset --hard HEAD^
git push origin master -f
```
> -f 参数是强制提交，因为reset之后本地库落后于远程库一个版本，因此需要强制提交

使用`revert`和`reset`命令的区别在于，
1.`revert`是放弃指定提交的修改，但是会生成一次新的提交，需要填写提交注释，`以前的历史记录都在`，而reset是指将HEAD指针指到指定提交，历史记录中不会出现放弃的提交记录;
2.`reset`是删除远程仓库的最后一次提交, 并且不保留历史记录。
最直白的区别在于,假设最后一次提交的记录为A, 那用`revert`回退版本之后，A记录依然存在，且会生成一条新的记录; 如用`reset`命令回退则A记录被删除不会存在了。


### 删除历史某次提交

1.先用git log命令在历史记录中查找到想要删除的某次提交的commit id
2.执行以下`rebase`命令（"commit id"替换为想要删除的提交的"commit id"，需要注意最后的^号，意思是commit id的前一次提交）

```bash
git rebase -i "commit id"^
```
3.执行该条命令之后会打开一个编辑框，列出了包含该次提交在内之后的所有提交,然后在编辑框中删除你想要删除的提交所在行，然后保存退出就好啦，如果有冲突的需要解决冲突。接下来，执行以下命令，将本地仓库提交到远程库就完成了

```bash
git push origin master -f
```

### 修改历史某次提交

这种情况的解决方法类似于第二种情况，只需要在第二条打开编辑框之后，将你想要修改的提交所在行的pick替换成edit然后保存退出，这个时候rebase会停在你要修改的提交，然后做你需要的修改，修改完毕之后，执行以下命令

```bash
git add .
git commit --amend
git rebase --continue
```

如果你在之前的编辑框修改了n行，也就是说要对n次提交做修改，则需要重复执行以上步骤n次。
需要注意的是，在执行rebase命令对指定提交修改或删除之后，该次提交之后的所有提交的"commit id"都会改变。

### git pull之后查看别人修改的文件记录
pull之后可以查看别人提交的日志记录或者文件差异
```bash
#查看文件差异有如下方式:
#方式1(查看所有差异)
git diff HEAD^ HEAD
(如果提示zsh: no matches found: HEAD^) 可以尝试修改为,
git diff HEAD\^ HEAD 

#方式2(查看某一个文件的差异)
git diff HEAD^ file-path
git diff HEAD^ src/path/utils.go   查看的是 utils.go 更新后与本地之前之间的差异对比

#查看修改日志记录有如下方式:
#方式1
git show # view the diff of last commit
#方式2
git whatchanged
#方式3
git log --stat --date=relative
#方式4
git log -p path_to_file
```

## 其它
- [git奇淫技巧](https://github.com/521xueweihan/git-tips)
