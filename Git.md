## 配置部分

git --version      
查看版本    

git config --global user.name "luy"  

git config --global user.email "luyingchn@gmail.com"    

配置个git账户和邮件    

git config --global alias.br branch    

设置简洁命令br代替branch

git config --global color.ui true

开启输出颜色显示

## 开始
先进入工作目录 cd /luy/workspace

创建 mkdir demo

进入 cd demo

初始化 git init

正常会看到Initialized empty Git repository in ...

ls -af

这会在你的工作区域创建一个隐藏目录 .git(这个目录即Git版本库 Repository)

提交 git commit -m "这是提交说明。"

## .git目录

搜索 git grep "...这个命令还有问题"

在工作区子目录操作，会向上递归查找.git目录，因为这个目录记录了版本库（暂存区状态），可以用下面这个命令查询

git status 如果git工作区外执行命令会报错 fatal：not repository...

打印.git文件路径 git rev-parse --git-dir

打印工作区根目录 git rev-parse --show-toplevel

打印当前路径相对工作区根目录的相对目录 git rev-parse --show-prefix

打印当前路径距离工作区根路径的深度 git rev-parse --show-cdup 

执行init 命令会自动创建.git目录，该目录下存放了库级别的配置文件(即/.git/config)，修改用以下命令

git config -e

另外两种级别分别是全局配置，系统配置

全局即用户目录下的.gitconfig文件（/home/luy/.gitconfig）

git config -e --global

系统即/etc/gitconfig

git config -e --system

注意优先级，最高的是库级别的，这样可以让库 覆盖 用户 覆盖 系统

提交补充

--amend
--allow-empty
-- reset-author

git log --pretty=fuller 查看提交日志

推送 git push origin master

比较本地与暂存区（commit后，push前？）

git diff

查看精简日志

git log --pretty=oneline

查看精简提交状态

git status -s

git clone 备份???

## 暂存区

先说明一点，暂存区即add 进去

commit则到本地库master

这些操作都是本地的代码管理，不依赖服务器和网络

git log --stat 查看变更统计的log

修改文件后的提交 add，commit（不能直接commit）

git diff 比较工作区和暂存区差异，没有add的？

git diff HEAD 比较 工作区和本地库差异 所有文件差异？

所以如果一次修改add操作过，这两种diff结果一样

git status 绿色字表示你已经add到暂存区了，红色的字表示当前修改还没有add进暂存区，或者是新文件（即Untracked）

git diff --staged
git diff --cacehd

比较暂存区和本地库的差异，即add的但没有commit的？

git status 和git diff两个命令来扫描工作区改动，会更新.git/index文件时间戳

## 本地仓库的操作

git reset Head    意思是master树替换index树？？？

git rm --cached <file> 删除暂存区文件
  
git checkout. 或者git checkout -- <file> 暂存区文件代替工作区文件 危险操作
  
git checkout. HEAD 或者git chekout <file> HEAD指向的master分支文件替换缓存区和工作区文件 极度危险 
  
git clean -fd 清除工作区非跟踪文件和目录，新增文件，如果只是修改文件，不会清除
  
## Git Diff
工作区，暂存区，版本库 都对应一套目录树，工作区很直观可以看到

版本库查看可以使用这个命令 git ls-tree -l HEAD

暂存区用 git ls-files -s

或者先将目录树写入Git对象库

git write-tree 获取tree-id

再 git ls-tree -l tree-id ,这里的tree-id可以不用写全

工作区和暂存区比较 git diff

暂存区和HEAD比较 git diff -cached HEAD ，HEAD 可以省略

工作区和HEAD 比较 git diff HEAD, HEAD 可以用master

## git commit -a 
前面提到过提交如果不add，会失败 那么这个-a就可以强制绕过暂存区做提交，然而这样会丢弃暂存区带来的控制能力

## git status

基础命令最后一个就是git status 
这里会涉及到暂存区的处理和master分支机制，为此先暂停一段落

git stash 保存进度

## Git对象

打印提交id

git log -l --pretty=raw

一般，一个提交包含三个id，分别是commit本次提交，tree本次提交对应的目录树，parent上次提交

可以使用下面格式的命令查看类型（-t）或者内容（-p）

git cat-file -t id

git cat-file -p id

注意上面的id需要用sha1哈西串代替，可以写一部分

所以对象库的内部关系，可以描述为 每次commit都对应一个当前id，包含目录树id，上次提交id，通过上次提交id可以追溯上次提交的目录树等，进而形成一个链条，如果追溯到一次提交没有parent id，则这次提交就是第一次提交  然后每次提交对应的tree id，包含至少一个blob对象，每个对象对应一条内容id和内容信息

无论是commit(parent也是commit),tree,blob，都是对象库中的具有id标识的对象，其中id前2位作为objects目录下的子目录名，后38位则是目录下的文件名

## HEAD和master

精简（simple）显示，同时输出分支（branch）名称 git status -s -b 

git branch 则可以显示分支

查log可以用下面命令，发现输出一样

git log -l HEAD

git log -l refs/heads/master

去.git目录查看

find .git -nmae HEAD -o -name master

输出有四个路径，暂不看logs下的，则

./git/HEAD

./git/refs/heads/master （可以简写master）

显示HEAD内容 cat .git/HEAD

ref:refs/heads/master

就是说HEAD指向一个引用，看下master文件内容

cat .git/refs/heads/master

是一个id，用cat-file查看类型和内容，发现是最新一次提交的id，因此可以建立一次历史跟踪链，追踪整个提交历史

.git/refs是保存引用的命名空间，heads目录下的引用称为分支，可以用下面命令显示引用对应提交id

git rev-parse HEAD

git rev-parse refs/heads/master (可以简写master)

所以问题来了，上面一直提到的40位id是什么，下面看

### commit

以最新提交作为comiit

HEAD 对应id的提交内容 git cat-file commit HEAD

内容大小 git cat-file commit HEAD |wc -c     打印出199

拿到大小后执行下面的哈西算法 ( printf "commit 199\000"; git cat-file commit HEAD )| sha1sum

发现这个值正是HEAD 的id

### blob

内容 git cat-file blob HEAD:second.txt

大小: git cat-file blob HEAD: second.txt |wc -c  打印出 24

哈西: ( printf "blob 24\000"; git cat-file blob HEAD:second.txt )| sha1sum

对比： git rev-parse HEAD: second.txt

### tree

大小: git cat-file tree HEAD^(tree) |wc -c 如打印出39

哈西: ( printf "tree 39\000"; git cat-file blob HEAD^(tree) )| sha1sum

对比: git rev-parse HEAD^(tree)

最后，给出访问git库中对象的指令

最近一次提交 HEAD

父提交 ^

父提交的第三个父提交 HEAD^^3 

HEAD^^^^^ 缩写 ~n（即这里5）

暂存区  :path/to/file

## Git重置

commit 操作会让master指向新的提交，git使用reset命令让游标可以指向任意存在的提交id

--hard参数会破坏工作区未提交改动

git reset --hard HEAD^ 指向上一次老的提交

git reset --hard 9e8a761  指向该id的提交（查询提交id用 git log --graph --oneline）

重置后，不能通过查找历史提交的方法找到丢弃的提交id，因为log也丢失了（log没打印，但是对象库中依然存在）

### reflog

带有工作区的版本库都有提供分支日志功能

git config core.logallrefupdates  打印true

直接查看分支日志文件

tail -5 .git/logs/refs/heads/master

reflog提供了操作，其中show命令可以实现与上面那个命令一样的功能，并且会提供一个便捷的表达式master@{n}

git reflog show master |head -5

这时候追踪到了id改变，利用表达式即可重置

git reset --hard master@{2} 重置操作会让版本库的文件替换暂存区和工作区文件，git log也会恢复记录

当然reset的这次记录也会记录在master日志中

### 深入reset

有paths路径的reset命令，不会重置引用以及改变工作区，只会让commit下的文件替换暂存区的文件，相当于add命令的取消操作

没有paths路径的，会重置引用，包括

--hard 替换引用指向；替换暂存区；替换工作区

--soft 只替换引用指向

--mixed （或者省略） 替换引用指向；替换暂存区

## Git检出

一般地，HEAD 文件内容是固定的，即指向分支master，但如果使用以下指令

git checkout master-id

会让HEAD文件内容改成一个具体的id，且id值并非master分支之前的id，是上次提交的id

这种时候如果做提交操作，除了有警告，发现新的提交建立在当前head指向的提交id上

进一步，切换到master分支 git checkout master

上次提交居然没了，包括log也不见了，但是通过上次提交的id可以在版本库中找到，但因为没有分支跟踪，所以一旦reflog中日志过期，这次提交将永远从版本库清除

如果需要保留这次修改，可以使用下面的指令 

git merge current-head-id 

通过日志查看 git log --graph --pretty=oneline 

发现master和要保留的这个提交都作为开发分支，merge操作合并了这次操作，并生成新id

（reset,checkout 都不会产生新id，merge会）

git cat-file -p HEAD 打印出看到两个父提交，因为合并了分支所以都是作为当前提交的父提交

### 深入











































