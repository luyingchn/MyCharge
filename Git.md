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
  
git checkout. HEAD 或者git chekout <file> HEAD之乡的master分支文件替换缓存区和工作区文件 极度危险 
























