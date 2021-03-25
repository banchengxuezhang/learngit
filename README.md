# Git教程 
    参考:
    https://www.liaoxuefeng.com/wiki/896043488029600/897884457270432
## 1.Windows安装Git
    1.下载Git ftp://mi@banchengxuezhang.top/D盘（软件）/我的下载/Git-2.25.1-64-bit.exe
    2.安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！
    3.安装完成后设置用户名和邮箱
    命令:
        git config --global user.name "xxx"
        git config --global user.email "xxx@xx.com"
    注意:--global 这个参数表示全局,设置了这个参数表示这台机器所有git仓库都会使用这个配置,当然也可以对某个仓库指定不同的用户名和Email地址。
## 2.创建版本库
    1. 首先选择一个合适的地方创建一个空目录(项目目录):
        命令如下:
        mkdir xxx 创建目录
        cd xxx 打开并进入目录
        pwd 查看当前目录
        注意:目录名,包括父目录,最好不要含中文
    2.初始化目录,让它变成Git仓库
        git init
        瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。
        如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用ls -ah命令就可以看见。      
    3.在Git目录中创建文件
        touch README.md
    4.将文件添加到Git仓库
        git add README.md
        git add * 提交目录下所有文件
        git add . 提交目录下所有文件
    5.将文件的修改进行提交
         git commit -m "xxx"
    -m "xxx" xxx写的是本次提交的一些说明,这样方便查看改动记录
     git commit命令执行成功后会告诉你，1 file changed：1个文件被改动（我们新添加的README.md文件）；2 insertions：插入了x行内容（README.md有x行内容）。
## 3.查看修改内容并提交
    1.首先我们进行工作,对README.md文件进行修改,通过git status 查看修改状态.
    git status 可以看出xxx文件被修改过了,但是没有提交的修改
    2.如果要看具体修改的内容,可以通过git diff 看看具体修改了什么内容,diff即不同,通过这个命令我们可以看出
    -xxx 删了什么
    +xxx添加了什么内容
    3.提交修改和提交新文件一样是两步
        1.git add xxx
        2.git commit -m "xxx"
## 4.版本回退(时光机)
    1.查看版本的历史记录
        命令:git log 可以看见提交id 提交人 提交时间 提交说明
        git log --pretty=oneline 不显示提交人和提交时间 只显示id和提交说明
        显示结果中,HEAD表示当前版本 上一个版本就是HEAD^ 上上个版本就是HEAD^^ 再往前写成HEAD~100
    2.版本回退命令:
        git reset --hard HEAD^ 回退到上一个版本
        版本回退,内容还原了,使用git log 已经看不见被退掉的版本了
        这个时候 翻记录找到要回退版本的那个id 如 c7cc4cbxxx
        使用 git reset --hard c7cc4cbxxx 回到指定的版本(包括log看不见的,已经被回退了的版本)
    3.Git的版本回退速度非常快，因为Git在内部有个指向当前版本的HEAD指针，当你回退版本的时候，Git仅仅是把HEAD从指向要退到的版本.
    4.通过 git reflog 查看每一次命令 ,找到那个想返回的的版本的id
## 5.工作区和暂存区
    工作区（Working Directory）
    就是你在电脑里能看到的目录，比如我的learngit文件夹就是一个工作区
    版本库（Repository）
    工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
    前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：
        第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
        第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
        因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。
   你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。
   实践出真知:
    1.修改README.md文件
    2.文件夹下新建LICENSE文本 内容随便写
    3.使用git status 查看状态
   Git非常清楚地告诉我们，README.md被修改了，而LICENSE还从来没有被添加过，所以它的状态是Untracked。
   现在，使用两次命令git add，把README.md和LICENSE.txt都添加后，用git status再查看一下：
                    new file:   LICENSE.txt
	                modified:   README.md
   所以，git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支。
   一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的：
        nothing to commit, working tree clean
## 6.管理修改
    现在，假定你已经完全掌握了暂存区的概念。下面，我们要讨论的就是，为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。
    命令:
        cat README.md 查看文件内容
        git diff HEAD -- README.md 命令可以查看工作区和版本库里面最新版本的区别
    每次修改，如果不用git add到暂存区，那就不会加入到commit中。