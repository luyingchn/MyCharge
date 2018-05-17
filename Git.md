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
















